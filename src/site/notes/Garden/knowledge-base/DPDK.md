---
{"dg-publish":true,"permalink":"/garden/knowledge-base/dpdk/","created":"2022-06-03T13:26:20+08:00","updated":"2024-09-23T19:13:04+08:00","dg-note-properties":{"modified_date":"2024-09-23T19:13:04+08:00","creation_date":"2022-06-03T13:26:20+08:00"}}
---


# [[Garden/knowledge-base/DPDK\|DPDK]]

topics: [[02-Area/work/definitions/DPI Engine\|DPI Engine]]
related: [[02-Area/programming/General/NUMA\|NUMA]], [[02-Area/programming/Linux\|Linux]]
tags: #networking

## Data Plane Development Kit
Created by Intel for high speed packet processing with zero-copy and poll mode driver.

This allows processing of packets without any copy from receiving to the transmitting.

## Advantage
![dpdk-comparison.png\|463](/img/user/03-Resource/Attachments/images/dpdk-comparison.png)
Main advantage compared to the traditional kernel based processing is that with DPDK, the userspace is able to directly process the packet, without the kernel being involved, and without costing a copy. Another major difference is that the DPDK is using Poll mode driver, whereas the kernel is handling network packets using [[10-Inbox/IRQ\|IRQ]]s. 

Related notes
- [[99-Archived/02-Area/work/projects/ncore/DPDK bind unbind\|DPDK bind unbind]]
- DPDK in vm: [Docker+openvswitch+dpdk小实验 | Datawine's Blog](https://datawine.github.io/docker-ovs-dpdk-vnf-exp.html)


## Windows

install `msys2 mingw64`
Install this package `pacman -S mingw-w64-x86_64-gcc`
Then follow the instruction at [3. Compiling the DPDK Target from Source — Data Plane Development Kit 23.07.0 documentation](https://doc.dpdk.org/guides/windows_gsg/build_dpdk.html)


## DPDK testpmd

```bash
dpdk-testpmd -l 0-1 -n 4 -- -i --portmask=0x1 --forward-mode=txonly
start tx_first
stop
show port stats all
```
This will generate traffic and send it to the interface. Useful to test if the interface is working or not.


```bash
dpdk-testpmd -l 11-16 -a $PCI_ADDR -- --forward-mode='rxonly' --mbuf-size=10000 --max-pkt-len=9600 --stats-period=3
```
This will check the speed of receiving on the interface.