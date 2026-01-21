---
{"dg-publish":true,"permalink":"/garden/posts/wireguard/","tags":["software","config"],"created":"2023-12-12T11:02:27+08:00","updated":"2026-01-21 15:10"}
---

# [[Garden/posts/wireguard\|wireguard]]

topics: [[03-Resource/networking/Networking\|Networking]]
related: [[02-Area/programming/Linux/Kernel parameters\|Kernel parameters]]


## Installation

[Installation - WireGuard](https://www.wireguard.com/install/)

Centos: `sudo yum install kmod-wireguard wireguard-tools`

## Generate key pair

```
wg genkey | tee private.key | wg pubkey > public.pub
```

## Example config

```
cd /etc/wireguard/
# wg0.conf
[Interface]
PrivateKey = XXXX
Address = 10.0.1.21/32
ListenPort = 51821
PreUp = sysctl -w net.ipv4.ip_forward=1

# remote settings for Host B
[Peer]
PublicKey = XXXX
Endpoint = 10.0.2.22:51822
AllowedIPs = 192.168.1.1/32   # which IP to allow access
```

## Optimisation

Changing to BBR improves CWD

```bash
sudo sysctl -w net.ipv4.tcp_congestion_control=bbr
```

source: [WireGuard Performance Tuning | Pro Custodibus](https://www.procustodibus.com/blog/2022/12/wireguard-performance-tuning/)

## NAT passthrough

- [GitHub - alex14fr/wgsig: NAT traversal and endpoint discovery protocol for Wireguard](https://github.com/alex14fr/wgsig)
    - only works if you can have a centralised server
- [GitHub - malcolmseyd/natpunch-go: NAT puncher for Wireguard mesh networking.](https://github.com/malcolmseyd/natpunch-go)
    - also needs server
- [How we achieved NAT traversal with WireGuard | NordVPN](https://nordvpn.com/blog/achieving-nat-traversal-with-wireguard/)

## Configuration

Basically generate key, generate the `wg0.conf` then up the service.
Can have only one endpoint on each pair.

## softwares

- [GitHub - DefGuard/defguard: Zero-Trust access management with true WireGuard® 2FA/MFA](https://github.com/DefGuard/defguard)
- [GitHub - axllent/wireguard-vanity-keygen: WireGuard vanity key generator](https://github.com/axllent/wireguard-vanity-keygen)
- [[03-Resource/networking/tailscale\|tailscale]]