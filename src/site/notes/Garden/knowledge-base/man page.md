---
{"dg-publish":true,"permalink":"/garden/knowledge-base/man-page/","tags":["linux","commands"],"created":"2021-12-23 15:27","updated":"2026-01-19 10:14"}
---

# [[Garden/knowledge-base/man page\|man page]]

topics: [[02-Area/programming/Linux\|Linux]]

source: [What do the numbers in a man page mean? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/a/3587/262256)
context: I was watching lecture from http://pwn.college for [[Garden/posts/Linux Process\|Linux Process]] and noticed the lecturer using `man` with numbers. I have seen it for a few times, but didn't really look into the difference.

Man page seem to have different sections.

```bash
MANUAL SECTIONS
    The standard sections of the manual include:

    1      User Commands
    2      System Calls
    3      C Library Functions
    4      Devices and Special Files
    5      File Formats and Conventions
    6      Games et. al.
    7      Miscellanea
    8      System Administration tools and Daemons

    Distributions customize the manual section to their specifics,
    which often include additional sections.
```

```bash
$ man 1 printf
$ man 3 printf
$ man -a printf
```

`man 1 printf` shows the `printf` command usage, while `man 3 printf` shows the `printf` C function signature.
`man -a` will show everything about the command.