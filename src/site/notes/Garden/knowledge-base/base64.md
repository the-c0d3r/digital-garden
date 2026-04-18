---
{"dg-publish":true,"permalink":"/garden/knowledge-base/base64/","tags":["linux","commands"],"created":"2025-09-12 10:49","updated":"2026-04-18 20:09","dg-note-properties":{"modified_date":"2026-04-18 20:09","creation_date":"2025-09-12 10:49","tags":["linux","commands"]}}
---

# base64

```bash
base64 test.txt -w0 > test-base64encoded.txt
base64 -d test-base64encoded.txt > test-output.txt

diff test.txt test-output.txt
```

`-w0` allows for a very long base64 string, instead of each lines with fixed length

## Padding

Base64 encoding increases file size by about 33%. The math works like this:
- Every 3 bytes (24 bits) of binary data are split into 4 groups of 6 bits.
- Each 6-bit group is represented as 1 ASCII character (1 byte).
- Thus, 3 raw bytes → 4 encoded bytes.

That’s 4/3 = 1.333…, or roughly a one-third increase.