---
{"dg-publish":true,"permalink":"/garden/knowledge-base/everything-is-a-file/","tags":["linux"],"created":"2022-10-16 20:00","updated":"2026-01-06 09:35","dg-note-properties":{"modified_date":"2026-01-06 09:35","creation_date":"2022-10-16 20:00","tags":["linux"]}}
---

## Everything is a file

tags: #linux

In Linux, **Everything is a file** even commands like `cat` or `ls` are actually programs located under `/bin/cat` or `/bin/ls`, and in other words, files.

One interesting thing is that `[` itself is a file, and not a shell built-in.

If everything is a file, then what are directories?
Directories are just files with a list of filenames inside. 