---
{"dg-publish":true,"permalink":"/garden/knowledge-base/grep-highlight/","tags":["commands","linux"],"created":"2024-09-23 15:37","updated":"2026-04-04 16:17","dg-note-properties":{"modified_date":"2026-04-04 16:17","creation_date":"2024-09-23 15:37","tags":["commands","linux"]}}
---

# grep highlight

topic: [[02-Area/programming/Linux\|Linux]]

```bash
grep --color=always -E '<pattern>'
tail -F /var/log/dpi.log | grep --color=always -E '[0-9]+Gbps|throughput:'
```

This will make it highlight the matching text only, but still show the full line. 