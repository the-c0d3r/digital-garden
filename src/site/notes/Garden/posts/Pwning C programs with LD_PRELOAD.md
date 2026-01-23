---
{"dg-publish":true,"permalink":"/garden/posts/pwning-c-programs-with-ld-preload/","created":"2019-12-29","updated":"2026-01-23 14:55"}
---


# Pwning C programs with LD_PRELOAD

This post will be talking about `LD_PRELOAD` environment variables and what they can be used for. This is usually used in C programming as well as CTF style challenges. It will be a bit easier to understand the examples if you have prior C knowledge and basic Linux knowledge.

To talk about LD_PRELOAD, one needs to be familiar with static and dynamic linking of compiler

#### Static linking
This method of the compilation will produce a fully standalone executable or a library file containing all the function definitions into just one file.

#### Dynamic linking
This method of compilation, however, will not be fully standalone, instead, it will rely on the existence of the statically linked libraries on the system. Normally they will search for directory paths such as `/lib64` or `/lib`.
When you have spent a sufficiently long time on a Linux system, you should have seen error messages while installing packages or running programs that look something like the following, complaining about the shared objects not being found.

```
Error: openssl.so.1 not found
Error: libcrypto.so.1 not found
```
This kind of error is common on Linux, and it can usually be solved by installing the necessary libraries.

GCC will be using dynamic linking by default. It means the program and the library will be loaded to memory during execution time and linked. If you want to compile it as static executable, you have to add `-static` to the compiler options when compiling.

Let's explore some dynamically linked binaries.

![Pwning C programs with LD_PRELOAD-1.png](/img/user/03-Resource/Attachments/Pwning%20C%20programs%20with%20LD_PRELOAD-1.png)

`ldd` command can be used to show the libraries that the binary is linked. In this case, it shows the dependencies of `ls` binary.

You can see the shared objects of the library `ls` requires, for instance, w`libc.so.6`, and the path of the `.so` file on the system. There is one interesting thing about the picture, why does `ls` needs `libpthread`? I can't think of any reason why `ls` would need multi threading.

In CTFs, you might see the following types of c program source code. This is simplified to show the LD_PRELOAD misuse. Let's go ahead and compile the code.

```c
#include <stdio.h>
#include <string.h>

int main(int argc, char **argv) {
    char passwd[] = "p455w0rd";
    if (argc < 2) {
        printf("Usage: %s <password>\n", argv[0]);
        return 0;
    }
    if (strcmp(passwd, argv[1]) == 0) {
        printf("You pwned the process, congratz\n");
        printf("Flag is {LD_PRELOAD_rulz}\n");
        return 1;
    }
    printf("Wrong password!\n");
    printf("Access Denied\n");
    return 0;
}
```

![Pwning C programs with LD_PRELOAD-2.png](/img/user/03-Resource/Attachments/Pwning%20C%20programs%20with%20LD_PRELOAD-2.png)

Notice that many c programs need libc, as it contains the functions like `printf`.
This is a very simple program, that if you execute with a correct password, it will print the flag, else it will print wrong password.

The function we need to mess around is the `strcmp()` function. If this function returns 0, then it will successfully print the flag. So, let's bypass this by misusing `LD_PRELOAD`.

![Pwning C programs with LD_PRELOAD-3.png](/img/user/03-Resource/Attachments/Pwning%20C%20programs%20with%20LD_PRELOAD-3.png)

First of all, save the following code as `hijack.c`. It is a very simple function with the same function name and parameters as the standard `strcmp()` function, but it will do nothing but return 0. This will be overwriting the `strcmp()` later on.

```c
int strcmp(const char *s1, const char *s2) {
    return 0;
}
```

![Pwning C programs with LD_PRELOAD-4.png](/img/user/03-Resource/Attachments/Pwning%20C%20programs%20with%20LD_PRELOAD-4.png)

Compile the code as shared object
```bash
gcc -fPIC -c hijack.c -o hijack.o
gcc -shared -o hijack.so hijack.o
```

Now it is the exciting part, the **exploitation**.

![Pwning C programs with LD_PRELOAD-5.png](/img/user/03-Resource/Attachments/Pwning%20C%20programs%20with%20LD_PRELOAD-5.png)

What this does is that it will define the `LD_PRELOAD` variable, and execute the program with `xxx` as input. In normal cases, this will not print the flag, because the input is not the correct password.

But due to the `LD_PRELOAD`, when the program asks for `strcmp()` function, it loads that function from `hijack.so` file, which of course returns 0. Thus PWNED.

This trick is useful in doing CTF challenges and it can lead to local privilege escalation. It is also useful in trying to do dynamic analysis of malware, etc, because you can easily write a function wrapper to tap into the function calls of the programs.

#### Reference urls
- [Reverse Engineering with LD_PRELOAD](https://www.exploit-db.com/papers/13233/)
- [c - Exploiting SUID files with LD_PRELOAD and IFS - Stack Overflow](https://stackoverflow.com/questions/21068650/exploiting-suid-files-with-ld-preload-and-ifs)
- [The magic of LD_PRELOAD for Userland Rootkits](http://fluxius.handgrep.se/2011/10/31/the-magic-of-ld_preload-for-userland-rootkits/)
- [Sudo LD_PRELOAD Linux Privilege Escalation](http://touhidshaikh.com/blog/?p=827)
- [How I got root with Sudo](https://www.securusglobal.com/community/2014/03/17/how-i-got-root-with-sudo/)


