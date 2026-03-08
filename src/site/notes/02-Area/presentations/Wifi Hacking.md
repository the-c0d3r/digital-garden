---
{"dg-publish":true,"dg-path":"Garden/knowledge-base/presentations/Wifi Hacking.md","permalink":"/garden/knowledge-base/presentations/wifi-hacking/","tags":["presentation","hacking"],"created":"2024-05-19 17:57","updated":"2026-03-08 20:46"}
---

# Wifi Hacking

tags: #presentation, #hacking

---

## WiFi Bandwidth

- 2.4 GHz
- 5 GHz

---

## WiFi Standards

- WEP (terribly insecure)
- WPA/WPA2 (most common)
- WPA3 (latest standard)

---

## WEP

60 bit or 128 bit encryption key = IV (initialization vector) = RC4 key
Vulnerability in the IVs being reused
Guaranteed compromise with **5 mins** capture

---

## WEP hacking

![Wifi Hacking-1.jpg](/img/user/03-Resource/Attachments/Wifi%20Hacking-1.jpg)
1. interface monitor mode with `airmon-ng`

---

![Wifi Hacking-2.jpg](/img/user/03-Resource/Attachments/Wifi%20Hacking-2.jpg)
2. capture WEP AP packets, check for roughly 5000 IVs

---

![Wifi Hacking-3.jpg](/img/user/03-Resource/Attachments/Wifi%20Hacking-3.jpg)
3. crack with `aircrack-ng`

---

## WPA/WPA2

Vulnerability lies in the handshake packets
The handshake packets can be captured and bruteforced

---

Steps
1. Monitor mode on interface
2. capture packets from AP
3. De-auth the connected clients, force handshake
4. Crack handshake with wordlist

---

## WPS

WPS Pin = 8 digit number (fixed)

---

Steps
1. Monitor mode on interface
2. use `reaver`, start bruteforcing pins
3. Using pin to recover current password

---

## Other Wireless Attacks

- Broadcasting thousands of beacons
- kick someone out of wifi by deauth
- forcing people to connect to you

---

## Tools

- aircrack-ng (most classic)
- reaver
- pixie dust
- wifite

---