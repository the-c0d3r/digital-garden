---
{"dg-publish":true,"permalink":"/garden/posts/hugepages/","tags":["#programming/linux","#config"],"created":"2021-10-22T09:42:50+08:00","updated":"2026-01-06 13:36"}
---


# Hugepages

topic: [[02-Area/programming/Linux\|Linux]]
related: [[02-Area/programming/Linux/CPU Isolation\|CPU Isolation]]
tags: #programming/linux , #config


## What is hugepages?
Memory is managed in pages by the OS. On most systems, a page is 4KiB.
CPU has built in mechanism called "Memory Management Unit" that manages the list of pages in hardware. 

Transparent Lookaside Buffer (TLB) is a small hardware cache of virtual-to-physical page mappings. 

When CPU request for translation of mapping, if TLB can provide the value, then it is fast. If TLB cannot provide the value, it is called TLB miss, then CPU has to fallback to software based mapping, which is slower.

The TLB size is fixed, so the only thing you can affect is the size of the pages. This is where the HugePages comes in.

Hugepage is a memory page that is larger than 4KiB. Typically it is 2MiB or 1 GiB on x86_64 architectures. In order for the hugepages to be used, the code / application needs to be aware of it and utilise it. 

Transparent Huge Pages (THP) is a mechanism to automate the management of huge pages without application knowledge, but they have a limitation, they are limited to 2MiB page sizes, and they can lock memory pages. 

For this reason, high performance applications tend to utilise the pre-allocated hugepages rather than transparent hugepages. And usually when configuring for huge pages, we also disable the transparent hugepages.


## Configuring hugepages

Check for hugepages support. 
```bash
cat /proc/cpuinfo | grep pdpe1gb
```

We need to modify the configuration of GRUB bootloader which is stored in this file `/etc/default/grub`, to the line starting with `GRUB_CMDLINE_LINUX`

> Make a copy and **Append** the following to the end.

Adjust the hugepage size and hugepages as required.
Then follow to the next section to mkconfig a new grub.

```
transparent_hugepage=never nosoftlockup mce=ignore_ce default_hugepagesz=1G hugepagesz=1G hugepages=16
```

Then do [[02-Area/programming/Linux/Grub#mkconfig\|Grub#mkconfig]].

## Check current huagepage info

```
grep -i huge /proc/meminfo
```

## Check current boot parameters

```bash
cat /proc/cmdline
```

## Check process taking hugepage

Command: `sudo grep huge /proc/*/numa_maps`
Do note that sometimes it doesn't have any match but the hugepage will say it is used.

```bash
$ sudo grep huge /proc/*/numa_maps
/proc/4131/numa_maps:80000000 default file=/anon_hugepage\040(deleted) huge anon=4 dirty=4 N0=3 N1=1
/proc/4131/numa_maps:581a00000 default file=/anon_hugepage\040(deleted) huge anon=258 dirty=258 N0=150 N1=108
/proc/4131/numa_maps:7f6c40400000 default file=/anon_hugepage\040(deleted) huge
/proc/4131/numa_maps:7f6ce5000000 default file=/anon_hugepage\040(deleted) huge anon=1 dirty=1 N0=1

# getting the process name

$ ps 4131
  PID TTY      STAT   TIME COMMAND
 4131 ?        Sl     1:08 /var/lib/jenkins/java/bin/java -jar slave.jar
$ ps 4153
  PID TTY      STAT   TIME COMMAND
 4153 ?        Sl     1:09 /var/lib/jenkins/java/bin/java -jar slave.jar
```


## Hugepages on runtime

source: [8.3.3.3. Enabling 1 GB huge pages for guests at boot or runtime Red Hat Enterprise Linux 6 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/virtualization_tuning_and_optimization_guide/sect-virtualization_tuning_optimization_guide-memory-huge_pages-1gb-runtime)

```bash
echo 4 > /sys/devices/system/node/node0/hugepages/hugepages-1048576kB/nr_hugepages
echo 1024 > /sys/devices/system/node/node0/hugepages/hugepages-2048kB/nr_hugepages
```
Allocates 4x 1G hugepages and 1024x 2mb hugepages

## Hugepages in VM
Need to have `pdpe1gb` in cpu flags `cat /proc/cpuinfo`
Host need to have the cpu flag, as well as support for huge page, and that same size hugepage allocated.
source: [centos - Hugepagesize is not increasing to 1G in VM - Stack Overflow](https://stackoverflow.com/questions/44379170/hugepagesize-is-not-increasing-to-1g-in-vm)


## Stats info

```
HugePages_Total is the size of the pool of huge pages.
HugePages_Free  is the number of huge pages in the pool that are not yet
                allocated.
HugePages_Rsvd  is short for "reserved," and is the number of huge pages for
                which a commitment to allocate from the pool has been made,
                but no allocation has yet been made.  Reserved huge pages
                guarantee that an application will be able to allocate a
                huge page from the pool of huge pages at fault time.
HugePages_Surp  is short for "surplus," and is the number of huge pages in
                the pool above the value in /proc/sys/vm/nr_hugepages. The
                maximum number of surplus huge pages is controlled by
                /proc/sys/vm/nr_overcommit_hugepages.
```
source: [memory - Linux Huge Pages Usage Accounting - Server Fault](https://serverfault.com/a/438608)

---

## Troubleshooting

1. Check if the current `/proc/cmdline` has the parameters you expect it to have.
