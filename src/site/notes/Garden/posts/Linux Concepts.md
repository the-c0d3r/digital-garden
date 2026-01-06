---
{"dg-publish":true,"permalink":"/garden/posts/linux-concepts/","tags":["linux"]}
---

# Linux Concepts

topics: [[02-Area/programming/Linux\|Linux]]

**All the concepts to understand about Linux**

I have been thinking to write about [[02-Area/programming/Linux\|Linux]] for some time now. Each time I thought about it, I gave up, because there is just too many things to talk about. However, that should not be the approach I take toward life. So I decided to change it and write about Linux, concept by concept.

## Background

I have been using [[02-Area/programming/Linux\|Linux]] since around 2010. Back then I only know [[02-Area/programming/Linux\|Linux]] as an operating system, such as Ubuntu 9.04, Backtrack 4, etc. Since then, I have been consistently using [[02-Area/programming/Linux\|Linux]], all through my University days and up to my working days. Linux has been in my life for more than 12 years. Over time, I have picked up quite a few things about it, and I thought what's the use of knowledge, if I don't share it.

I have used quite a few distributions of Linux over the years
- Debian based such as Ubuntu, Debian, Kali Linux, Backtrack, ParrotOS, Linux Mint
- BSD based such as OpenBSD, FreeBSD
- RHEL based such as Centos, Fedora, RHEL
- Arch based such as Manjaro, Arch, Black Arch
- Other more obscure Linux such as Slackware, Beini/XiaoPan (wifi cracking OS), KolibriOS (entire ISO is just a tiny 2MB)

The one that impressed me is the KolibriOS, back then I attempted to run this on my android phone, using some application called Virtual PC or something. I downloaded the ISO which is only 2MB, and I managed to boot it up using the aforementioned emulator software. The OS is quite impressive, given the size of it. It has GUI, it even has built in apps and even a few games.

I also explored a few Linux distros with the intent of learning cyber security / hacking. I tried a variety of it, since the times of BackTrack4 (or was it 3? I can't recall). Many of them are just built to be used for hacking / cyber security. I really loved the way BackTrack5 looked at that time, with the cool wallpaper and very good looking, transparent gnome terminal theme.

One thing leads to another and suddenly found myself learning about bash scripting, [[02-Area/programming/General/Vim\|Vim]], [[10-Inbox/Tmux\|Tmux]], screen, etc. All these things have propelled me towards a career where I was able to make full use of my skills of Linux, Hacking, and Programming.

## Distributions

Linux is not an OS, but a kernel. When people say Linux OS, they usually mean OS powered by Linux Kernel, such as the popular distribution Ubuntu.
There are many different types of distributions, such as the few major distributions that I have listed above.

Distributions are mostly just Linux kernel with a few different software and packages. So what makes a distribution? After trying out many of these "distributions", I have come to the following conclusion.

A Linux distribution consists of
- a Linux Kernel (of course)
- a Desktop Environment
- a process manager (init.d, systemd, etc)
- a Package Manager
- slightly different ways of doing things
- and a group of people willing to devote their free time to develop and give it back for free.

Basically, if you are familiar with one distribution of Linux, you should be able to easily pick up the other distributions. The commands are usually the same with some differences in the way you install packages, and the way you configure things.

For example, Arch is notorious for being difficult to install (trust me, it isn't).

## [[Garden/notes/Everything is a file\|Everything is a file]]

In Linux, you can change anything you want, very easily
You don't like a desktop environment, you change it,

## Free vs Free

[[Free as in Freedom\|Free as in Freedom]] vs [[Free as in Free Beer\|Free as in Free Beer]]
Free as in Freedom of speech, where you can speak up whatever idea you have (however not without consequences). Free as in free beer, where someone can enjoy a cold one.

FOSS [[Free Opensource Software\|Free Opensource Software]]
Free and Opensource does not imply [[Free as in Free Beer\|Free as in Free Beer]], it just means that the source code is given, and anyone is free to change whatever they want with it.


[[Garden/posts/Linux Process\|Linux Process]]
[[Garden/notes/ELF\|ELF]] file format

## File Extensions are just hints

File extensions are just hints for the human user. Kernel only cares about the shebang line or the file header to execute it with.
More info on [[Garden/posts/Linux Process#Process Loading\|Linux Process#Process Loading]]

## Linux is a kernel

Linux is a kernel that provides a platform for user land programs to run.

## Linux kernel structure

- [[Linux Rings\|Linux Rings]]
- Linux [[Kernel Space\|Kernel Space]] vs Linux [[User Space\|User Space]]