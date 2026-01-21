---
{"dg-publish":true,"permalink":"/garden/posts/wireguard/","tags":["software","config"],"created":"2023-12-12T11:02:27+08:00","updated":"2026-01-21 15:10"}
---

# [[Garden/posts/wireguard\|wireguard]]

topics: [[03-Resource/networking/Networking\|Networking]]
related: [[02-Area/programming/Linux/Kernel parameters\|Kernel parameters]]

General steps
1. installation
2. generate key pair
3. generate config file
4. generate service file

## Installation

[Installation - WireGuard](https://www.wireguard.com/install/)

Centos: `sudo yum install elrepo-release epel-release && sudo yum install kmod-wireguard wireguard-tools`

## Generate key pair

```bash
cd /etc/wireguard
wg genkey | tee private.key | wg pubkey > public.pub
```

## Config file

`vi /etc/wireguard/wg0.conf`

```toml
[Interface]
PrivateKey = ${wg_private}
# wireguard vpn interface IP
Address = ${wg_ip}/32
ListenPort = 51820
PreUp = sysctl -w net.ipv4.ip_forward=1

[Peer]
PublicKey = ${wg_peer_key}
# peer's public IP address to connect to
Endpoint = ${wg_peer_ip}   
# if the peer has dynamic IP, then peer can connect back to this host. Simply remove this Endpoint line. 
AllowedIPs = ${wg_peer_allowed_ip}
# peer's wireguard ip address to be allowed
PersistentKeepalive = 60
# this allows NAT punch through
```

## Service

```bash
cat > /etc/systemd/system/wg-quick@wg0.service <<EOF
[Unit]
Description=WireGuard via wg-quick(8) for %I
After=network-online.target nss-lookup.target
Wants=network-online.target nss-lookup.target
PartOf=wg-quick.target
Documentation=man:wg-quick(8)
Documentation=man:wg(8)
Documentation=https://www.wireguard.com/
Documentation=https://www.wireguard.com/quickstart/
Documentation=https://git.zx2c4.com/wireguard-tools/about/src/man/wg-quick.8
Documentation=https://git.zx2c4.com/wireguard-tools/about/src/man/wg.8

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/bin/wg-quick up %i
ExecStop=/usr/bin/wg-quick down %i
ExecReload=/bin/bash -c 'exec /usr/bin/wg syncconf %i <(exec /usr/bin/wg-quick strip %i)'
Environment=WG_ENDPOINT_RESOLUTION_RETRIES=infinity

[Install]
WantedBy=multi-user.target
EOF

systemctl enable wg-quick@wg0.service
systemctl start wg-quick@wg0.service
```

Ensure the firewall allows `51280/udp` on the public interface.

## Check status

```bash
wg show

interface: wg0
  public key: ...
  private key: (hidden)
  listening port: 51820

peer: ...
  endpoint: <PEER_PUBLIC_IP>:51820
  allowed ips: <PEER WG IP>/32
  latest handshake: 1 minute, 33 seconds ago
  transfer: 944 B received, 2.70 KiB sent
  persistent keepalive: every 1 minute
```

If you see latest handshake, it's working fine.

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