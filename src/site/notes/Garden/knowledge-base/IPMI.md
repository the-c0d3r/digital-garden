---
{"dg-publish":true,"permalink":"/garden/knowledge-base/ipmi/","created":"2022-10-26T22:03+08:00","updated":"2026-01-15 15:40"}
---

# IPMI

This is a tool to interact with the out of bound management interface of servers, such as iLO (HPE), iDRAC (dell) and BMC (supermicro). 

You can use it to do the following for iLO/iDRAC/BMC
1. reset admin password
2. configure out of bound network

## Installation

```sh
yum install -y ipmitool
```

## IP

```bash
ipmitool lan print 1
```

## Resetting

```bash
# get the ID of the user
ipmitool user list 1
ipmitool user set password <ID> <new password>
ipmitool user set password 2 testabc
# ---
# find out available users
ipmitool -I open user list 1
# set password, where X is the ID of the user from above user list
ipmitool user set password X new-pass 
```

> [!note]
> password need to be 16 to 20 in some servers like HPE

```bash
# set ip address to static
ipmitool lan set 1 ipsrc static 

# set default gateway ip
ipmitool lan set 1 defgw ipaddr xxx.xxx.xxx.xxx 
# set default gateway MAC address
ipmitool lan set 1 defgw macaddr xx:xx:xx:xx:xx:xx 
# set BMC ip address
ipmitool lan set 1 ipaddr xxx.xxx.xxx.xxx 
# set BMC net mask
ipmitool lan set 1 netmask xxx.xxx.xxx.xxx 

# reset iLO to take effect
ipmitool mc reset cold


# templates
ipmitool lan set 1 ipaddr 10.0.1.2
ipmitool lan set 1 netmask 255.255.255.0
ipmitool lan set 1 defgw ipaddr 10.0.1.254

ipmitool lan set 1 ipaddr 10.0.1.3
ipmitool lan set 1 netmask 255.255.255.0
ipmitool lan set 1 defgw ipaddr 10.0.1.254
```

## iLO through ssh tunnel

```bash
IPMI="10.10.11.11"
HOST="192.168.1.100"
sudo ssh -L 22:$IPMI:22 -L 23:$IPMI:23 -L 80:$IPMI:80 -L 443:$IPMI:443 -L 17988:$IPMI:17988 -L 9300:1$IPMI:9300 -L 17990:$IPMI:17990 -L 3002:$IPMI:3002 root@10.10.11.11
```

## Hardware logs

```bash
ipmitool sel elist
```

[Reverse Engineering Supermicro IPMI – peterkleissner.com](https://peterkleissner.com/2018/05/27/reverse-engineering-supermicro-ipmi/)