---
{"dg-publish":true,"permalink":"/garden/knowledge-base/arp/","tags":["networking"],"created":"2025-10-14 16:21","updated":"2026-04-18 20:05","dg-note-properties":{"modified_date":"2026-04-18 20:05","creation_date":"2025-10-14 16:21","tags":["networking"]}}
---

# ARP

topic: [[03-Resource/networking/Networking\|Networking]]
tags: #networking

ARP = Address Resolution Protocol

We can send gratuitous ARP entry using scapy, but the other host won't accept it if the IP is not inside the ARP entry, unless we set this kernel option: `sysctl -w net.ipv4.conf.all.arp_accept=1`

There's a way to make sure the entry exists. We can make a ping to the target first using the IP that we want.

```bash
# -I <virtual IP>   <target host>
ping -I 30.0.0.10 30.0.0.2
```

This will write the entry to the ARP table of `30.0.0.2`, then we can send unsolicited ARP response to the `30.0.0.2` to update it to something else entirely.

```python
#!/usr/bin/env python3
# gratuitous_arp.py
# Usage (run as root):
#   sudo python3 gratuitous_arp.py --iface eth0 --src-ip 192.168.1.10 --src-mac 00:11:22:33:44:55 --count 3 --interval 0.5
#
# Sends gratuitous ARP replies (broadcast) announcing that <src-ip> is at <src-mac>.

import argparse
import time
import re
import sys
from scapy.all import Ether, ARP, sendp, conf


def is_mac(s):
    return re.fullmatch(r"([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}", s) is not None


def main():
    p = argparse.ArgumentParser(description="Send gratuitous ARP broadcasts with Scapy")
    p.add_argument(
        "--iface", "-I", required=True, help="Interface to send on (e.g. eth0)"
    )
    p.add_argument(
        "--src-ip",
        "-S",
        required=True,
        help="IP to claim (psrc and pdst for gratuitous ARP)",
    )
    p.add_argument(
        "--src-mac", "-M", required=True, help="MAC to claim (hwsrc and Ethernet src)"
    )
    p.add_argument(
        "--count", "-c", type=int, default=3, help="Number of gratuitous ARPs to send"
    )
    p.add_argument(
        "--interval", "-i", type=float, default=0.5, help="Seconds between packets"
    )
    p.add_argument(
        "--dry-run", action="store_true", help="Show packet and exit without sending"
    )
    args = p.parse_args()

    if not is_mac(args.src_mac):
        print("ERROR: invalid MAC format", file=sys.stderr)
        sys.exit(1)

    conf.iface = args.iface
    eth = Ether(dst="ff:ff:ff:ff:ff:ff", src=args.src_mac)
    # ARP op=2 (is-at / reply). For gratuitous ARP we set pdst == psrc and hwdst = ff:ff:ff...
    arp = ARP(
        op=2,
        psrc=args.src_ip,
        hwsrc=args.src_mac,
        pdst=args.src_ip,
        hwdst="ff:ff:ff:ff:ff:ff",
    )
    pkt = eth / arp

    print(f"Interface: {args.iface}")
    print(f"Announcing: {args.src_ip} is at {args.src_mac}")
    print(f"Packets: {args.count}, interval: {args.interval}s")
    print("Packet summary:")
    pkt.show()

    if args.dry_run:
        print("Dry run: not sending.")
        return

    try:
        for i in range(args.count):
            sendp(pkt, iface=args.iface, verbose=False)
            time.sleep(args.interval)
        print("Sent.")
    except PermissionError:
        print("Permission denied: run as root.", file=sys.stderr)
        sys.exit(2)
    except Exception as e:
        print("Error sending packets:", e, file=sys.stderr)
        sys.exit(3)


if __name__ == "__main__":
    main()
```