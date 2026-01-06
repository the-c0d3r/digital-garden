---
{"dg-publish":true,"permalink":"/garden/posts/linux-process/","created":"2021-12-21T11:08:42+08:00","updated":"2026-01-05 22:44"}
---

# Linux Process

topics: [[02-Area/programming/Linux\|Linux]]
related: [[Garden/notes/ELF\|ELF]]
tags: #linux 

Source: [Module: Program Interaction | pwn.college](https://pwn.college/modules/interaction)

## Process attributes
- state (running, waiting, stopped, zombie)
- priority (+ other scheduling info)
- parent, siblings, children
- shared resources (files, pipes, sockets)
- virtual memory space
- security context
    - euid, egid
    - saved uid and gid
    - [[Garden/notes/Linux Capabilities\|Linux Capabilities]]

## Process creation
- fork
- clone (more recent)
These two *system calls* will create a nearly exact copy of the calling process.

Usually, `execve` syscall will be called in child process to replace itself with another process.

Example
- execute `/bin/cat` in `bash`
- `bash` *forks* into a new child process
- child process `execve` with `/bin/cat` and thereby executing `/bin/cat`

`execve` will fail if the file does not have **executable permission**.

## Process Loading
Sequential and possibly recursive

1. check the shebang line `#!` if it exists, extracts the interpreter and executes it with the original file as the argument
2. check for file format match with `/proc/sys/fs/binfmt_misc`, if it matches, the kernel executes the interpreter specified with the original file as the argument
    1. [binfmt_misc - Wikipedia](https://en.wikipedia.org/wiki/Binfmt_misc) specifies that this file is usually mounted and you can use it to run `.exe` with `wine` for example, `:DOSWin:M::MZ::/usr/bin/wine:`.
3. If the file is dynamically linked [[Garden/notes/ELF\|ELF]], then kernel reads the interpreter or loader defined in [[Garden/notes/ELF#ELF Program headers\|ELF#ELF Program headers]].
4. If the file is a statically linked ELF, the kernel will load it. 
    1. This looks like the fat binary compilation
5. Other legacy formats are checked

Shebang line
`some-script`
```bash
#!/bin/echo
```

Executing it is the same as this. 
```bash
/bin/echo ./some-script
```
Note: this shebang can also call another script as interpreter.
shebang can also have arguments. But when there is arguments, the original file becomes the very last argument.

This brings about an important concept, [[Garden/posts/Linux Concepts#File Extensions are just hints\|Linux Concepts#File Extensions are just hints]]. Kernel go through the aforementioned process to know how to execute it.

### Dynamically Linked ELFs: interpreter

`readelf -a /bin/cat` to check the elf headers. There should be the interpreter information.
This interpreter information is used in the [[10-Inbox/ROP\|ROP]] or [[10-Inbox/ret2libc\|ret2libc]] bypass during [[02-Area/hacking/Buffer Overflow\|Buffer Overflow]].

```bash
$ readelf -a /bin/cat | grep interpret
      [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]
```
So knowing the *interpreter*, you could also manually run the binary file with the said *interpreter*.

```bash
$ /lib64/ld-linux-x86-64.so.2 /bin/cat
hello
hello
^C
```

Or if you wish, you can change it using `patchelf --set-interpreter` to change the interpreter information.

```bash
[thuyein@codelab-arch tmp]$ readelf -a ./cat | grep interpreter
      [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]
[thuyein@codelab-arch tmp]$ patchelf --set-interpreter /nosuchfileexists ./cat
[thuyein@codelab-arch tmp]$ readelf -a ./cat | grep interpreter
      [Requesting program interpreter: /nosuchfileexists]
[thuyein@codelab-arch tmp]$ ./cat
bash: ./cat: No such file or directory
```

It says no such file or directory, because after `/bin/bash` forks the child process, it will trigger `execve` with the process `/nosuchfileexists`, which does not exist on the system. Therefore, it will dump `ENOENT` exit code, which translates to `No Such file or directory`. Check for `man execve` to see the exit codes.

### Dynamically linked ELFs: loading process

1. program and interpreter loaded by the kernel
2. interpreter locates the libraries
    1. `LD_PRELOAD` env var, `/etc/ld.so.preload`
    2. `LD_LIBRARY_PATH` env var 
    3. `DT_RUNPATH` or `DT_RPATH` in the binary file (can be patched by  `patchelf --set-rpath /some/runpath`)
    4. system wide configs `/etc/ld.so.conf`
    5. `/lib` and `/usr/lib`
3. interpreter loads the libraries
    1. libraries can depend on other libraries, causing more loading
    2. relocation updated: these are updated as necessary due to dependency libraries being loaded into memory, causing the memory layouts to change.

Related: [[02-Area/programming/C/LD PRELOAD Exploit\|LD PRELOAD Exploit]], [[02-Area/hacking/privilege escalation/linux/LD Library\|LD Library]]


Each Linux process has a virtual memory space. It contains:
- the binary
- the libraries
- the "heap" (for dynamically allocated memory)
- the "stack" (for function local variables)
- any memory specifically mapped by the program
- some helper regions
- kernel code in the "upper half" of memory (above `0x8000000000000000` on 64-bit architectures), inaccessible to the process

**Virtual** memory is dedicated for a single process, while **Physical** memory encompasses **Virtual** memory and it is shared by the system.

Check `/proc/self/maps` or `/proc/[PID]/maps` for the memory mapping layout.
```bash
[thuyein@codelab-arch tmp]$ cat /proc/self/maps
561743e9b000-561743e9d000 r--p 00000000 103:08 131499                    /usr/bin/cat
561743e9d000-561743ea2000 r-xp 00002000 103:08 131499                    /usr/bin/cat
561743ea2000-561743ea5000 r--p 00007000 103:08 131499                    /usr/bin/cat
561743ea5000-561743ea6000 r--p 00009000 103:08 131499                    /usr/bin/cat
561743ea6000-561743ea7000 rw-p 0000a000 103:08 131499                    /usr/bin/cat
561744cba000-561744cdb000 rw-p 00000000 00:00 0                          [heap]
7f2bb5525000-7f2bb5862000 r--p 00000000 103:08 145397                    /usr/lib/locale/locale-archive
7f2bb5862000-7f2bb5864000 rw-p 00000000 00:00 0
7f2bb5864000-7f2bb588a000 r--p 00000000 103:08 140619                    /usr/lib/libc-2.33.so
7f2bb588a000-7f2bb59d5000 r-xp 00026000 103:08 140619                    /usr/lib/libc-2.33.so
7f2bb59d5000-7f2bb5a21000 r--p 00171000 103:08 140619                    /usr/lib/libc-2.33.so
7f2bb5a21000-7f2bb5a24000 r--p 001bc000 103:08 140619                    /usr/lib/libc-2.33.so
7f2bb5a24000-7f2bb5a27000 rw-p 001bf000 103:08 140619                    /usr/lib/libc-2.33.so
7f2bb5a27000-7f2bb5a32000 rw-p 00000000 00:00 0
7f2bb5a3b000-7f2bb5a5d000 rw-p 00000000 00:00 0
7f2bb5a5d000-7f2bb5a5e000 r--p 00000000 103:08 140606                    /usr/lib/ld-2.33.so
7f2bb5a5e000-7f2bb5a82000 r-xp 00001000 103:08 140606                    /usr/lib/ld-2.33.so
7f2bb5a82000-7f2bb5a8b000 r--p 00025000 103:08 140606                    /usr/lib/ld-2.33.so
7f2bb5a8b000-7f2bb5a8d000 r--p 0002d000 103:08 140606                    /usr/lib/ld-2.33.so
7f2bb5a8d000-7f2bb5a8f000 rw-p 0002f000 103:08 140606                    /usr/lib/ld-2.33.so
7fffcf8d6000-7fffcf8f8000 rw-p 00000000 00:00 0                          [stack]
7fffcf9e8000-7fffcf9ec000 r--p 00000000 00:00 0                          [vvar]
7fffcf9ec000-7fffcf9ee000 r-xp 00000000 00:00 0                          [vdso]
ffffffffff600000-ffffffffff601000 --xp 00000000 00:00 0                  [vsyscall]
```

## The standard C library

`libc.so` is linked by almost every process.


## Statically Linked binaries: loading

When you have binaries that are statically compiled, the memory map should show that there is no longer loading of third party libraries like `libc.so`. 
However, due to the packing of libraries statically, the binary size will increase drastically.

There is a in-built constructor for every program that is triggered. 

```c
__attribute__((constructor)) void haha()
{
    puts("Hello world!");
}
```
You could have this in any program, and it will execute whatever function is attributed as `constructor`, before executing `main()`.

---

## Process Execution
date: [[00-Journals/journal-personal/2021/2021-12-23\|2021-12-23]] 03:04 PM
source: [Module: Program Interaction | pwn.college](https://pwn.college/modules/interaction)
video: [pwn.college - Program Interaction - Linux Process Execution - YouTube](https://www.youtube.com/watch?v=Vtb5wIlthRg)

Normal [[Garden/notes/ELF\|ELF]] automatically calls `__libc_start_main()` in `libc` library, which will in turn calls the program's `main()` function.

Main function signatures
```c
int main(int argc, void **argv, void **envp);
```
Consists of
- the loaded objects (binaries, libraries)
- command line arguments in `argv`
- environment variables in `envp`

`ls -l` sorting will be affected by `$LANG` variable. Depending on the `$LANG=c` or `$LANG=en_US.UTF-8` for instance, they will sort files differently.

You can use the following code to print out all the env vars. 

```c
int main(int argc, void **argv, void **envp) {
    for (int i = 0; envp[i] != 0; i++) puts(envp[i]);
}
```

Binary *import symbols* have to be resolved using library's *export symbols*. It used to be done on demand in the past to save computation cost, at the cost of security. 


List all the symbols that the binary is importing.
```bash
~ > nm -D /bin/cat
         U abort@GLIBC_2.2.5
         U bindtextdomain@GLIBC_2.2.5
         U calloc@GLIBC_2.2.5
         U close@GLIBC_2.2.5
         U __ctype_b_loc@GLIBC_2.3
         U __ctype_get_mb_cur_max@GLIBC_2.2.5
         U __cxa_atexit@GLIBC_2.2.5
         w __cxa_finalize@GLIBC_2.2.5
         U dcgettext@GLIBC_2.2.5
         U __errno_location@GLIBC_2.2.5
         U error@GLIBC_2.2.5
         U _exit@GLIBC_2.2.5

```


List all the symbols that the binary is exporting
```bash
nm -a /bin/cat
```
On my system, this returns no symbols, I think because the `/bin/cat` binary is stripped.


### Interacting with the environment

Primarily done by `syscalls`. Check `man syscalls` for more info. 

Use `strace` to see the system calls triggered by system.

```bash
~ > strace /bin/cat /etc/passwd
execve("/bin/cat", ["/bin/cat", "/etc/passwd"], 0x7ffd17a92ac8 /* 88 vars */) = 0
brk(NULL)                               = 0x5641666ad000
arch_prctl(0x3001 /* ARCH_??? */, 0x7ffea61626f0) = -1 EINVAL (Invalid argument)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=172688, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 172688, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f2b803ab000
close(3)                                = 0
openat(AT_FDCWD, "/usr/lib/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0`|\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0@\0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0"..., 80, 848) = 80
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0K@g7\5w\10\300\344\306B4Zp<G"..., 68, 928) = 68
newfstatat(3, "", {st_mode=S_IFREG|0755, st_size=2150424, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f2b803a9000
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
mmap(NULL, 1880536, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f2b801dd000
mmap(0x7f2b80203000, 1355776, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x26000) = 0x7f2b80203000
mmap(0x7f2b8034e000, 311296, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x171000) = 0x7f2b8034e000
mmap(0x7f2b8039a000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1bc000) = 0x7f2b8039a000
mmap(0x7f2b803a0000, 33240, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f2b803a0000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f2b801db000
arch_prctl(ARCH_SET_FS, 0x7f2b803aa600) = 0
mprotect(0x7f2b8039a000, 12288, PROT_READ) = 0
mprotect(0x564165131000, 4096, PROT_READ) = 0
mprotect(0x7f2b80404000, 8192, PROT_READ) = 0
munmap(0x7f2b803ab000, 172688)          = 0
brk(NULL)                               = 0x5641666ad000
brk(0x5641666ce000)                     = 0x5641666ce000
openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=3392160, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 3392160, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f2b7fe9e000
close(3)                                = 0
newfstatat(1, "", {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0x1), ...}, AT_EMPTY_PATH) = 0
openat(AT_FDCWD, "/etc/passwd", O_RDONLY) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=2236, ...}, AT_EMPTY_PATH) = 0
fadvise64(3, 0, 0, POSIX_FADV_SEQUENTIAL) = 0
mmap(NULL, 139264, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f2b803b4000
read(3, "root:x:0:0::/root:/bin/bash\nnobo"..., 131072) = 2236
write(1, "root:x:0:0::/root:/bin/bash\nnobo"..., 2236root:x:0:0::/root:/bin/bash
nobody:x:65534:65534:Nobody:/:/usr/bin/nologin
dbus:x:81:81:System Message Bus:/:/usr/bin/nologin
bin:x:1:1::/:/usr/bin/nologin
daemon:x:2:2::/:/usr/bin/nologin
mail:x:8:12::/var/spool/mail:/usr/bin/nologin
ftp:x:14:11::/srv/ftp:/usr/bin/nologin
http:x:33:33::/srv/http:/usr/bin/nologin
systemd-journal-remote:x:982:982:systemd Journal Remote:/:/usr/bin/nologin
systemd-network:x:981:981:systemd Network Management:/:/usr/bin/nologin
systemd-resolve:x:980:980:systemd Resolver:/:/usr/bin/nologin
systemd-timesync:x:979:979:systemd Time Synchronization:/:/usr/bin/nologin
systemd-coredump:x:978:978:systemd Core Dumper:/:/usr/bin/nologin
uuidd:x:68:68::/:/usr/bin/nologin
dhcpcd:x:977:977:dhcpcd privilege separation:/:/usr/bin/nologin
dnsmasq:x:976:976:dnsmasq daemon:/:/usr/bin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/usr/bin/nologin
avahi:x:975:975:Avahi mDNS/DNS-SD daemon:/:/usr/bin/nologin
colord:x:974:974:Color management daemon:/var/lib/colord:/usr/bin/nologin
cups:x:209:209:cups helper user:/:/usr/bin/nologin
flatpak:x:973:973:Flatpak system helper:/:/usr/bin/nologin
gdm:x:120:120:Gnome Display Manager:/var/lib/gdm:/usr/bin/nologin
geoclue:x:972:972:Geoinformation service:/var/lib/geoclue:/usr/bin/nologin
git:x:971:971:git daemon user:/:/usr/bin/git-shell
gnome-initial-setup:x:970:970:GNOME Initial Setup:/run/gnome-initial-setup:/usr/bin/nologin
nm-openconnect:x:969:969:NetworkManager OpenConnect:/:/usr/bin/nologin
nm-openvpn:x:968:968:NetworkManager OpenVPN:/:/usr/bin/nologin
ntp:x:87:87:Network Time Protocol:/var/lib/ntp:/bin/false
openvpn:x:967:967:OpenVPN:/:/usr/bin/nologin
polkitd:x:102:102:PolicyKit daemon:/:/usr/bin/nologin
rtkit:x:133:133:RealtimeKit:/proc:/usr/bin/nologin
tss:x:966:966:tss user for tpm2:/:/usr/bin/nologin
usbmux:x:140:140:usbmux user:/:/usr/bin/nologin
thuyein:x:1000:1000:thuyein:/home/thuyein:/bin/zsh
plugdev:x:1001:1000::/home/plugdev:/bin/zsh
saned:x:964:964:SANE daemon user:/:/usr/bin/nologin
systemd-oom:x:962:962:systemd Userspace OOM Killer:/:/usr/bin/nologin
nvidia-persistenced:x:143:143:NVIDIA Persistence Daemon:/:/usr/bin/nologin
unbound:x:960:960:unbound:/etc/unbound:/usr/bin/nologin
mpd:x:45:45::/var/lib/mpd:/usr/bin/nologin
) = 2236
read(3, "", 131072)                     = 0
munmap(0x7f2b803b4000, 139264)          = 0
close(3)                                = 0
close(1)                                = 0
close(2)                                = 0
exit_group(0)                           = ?
+++ exited with 0 +++
~ >
```

From the following line, we can see that it reads the file specified. 

```
read(3, "root:x:0:0::/root:/bin/bash\nnobo"..., 131072) = 2236
...

~ > du -sb /etc/passwd
2236    /etc/passwd
```
And the file size is also in the `read` syscall. We can also see the `write` call with the same file size to the stdout.

I learned [[02-Area/programming/Linux/man page\|man page]] sections and what the page numbers mean. Check `man 2 syscalls` to get a list of system calls. 

Every system calls has a number. 
You can call it using `syscall(n)` where `n` is the syscall number.


### System calls
Interfaces for process to call into the OS/kernel. There are over 300 syscalls in Linux.

### Signals
A way for OS/kernel to call back into the process.

Signals will pause the process execution and invoke the handler, if custom handler is not defined in the process, then it will call the default action.

`SIGKILL` (signal 9) and `SIGSTOP` (signal 19) (*this backgrounds the process*) cannot be handled.

check `man 7 signal` or `kill -l` to see all the signals.

### Shared memory

Memory to be shared with other processes.
The file system is at `/dev/shm`, here you can create the files and `mmap` it from different processes.

It requires system calls to establish, but once established, communication happens without system calls. 

It seems like this `/dev/shm` thing is related to `tmpfs`. I looked up info on how to create and manually use it in [[02-Area/programming/C\|C]], but I can't really find any info on it. 

## Process Termination

2 ways to terminate
1. receiving un-handled signal
2. calling `exit()` syscall. 

Process will remain in *zombie state* until the parent process `wait()` on it. 
