---
{"dg-publish":true,"permalink":"/garden/knowledge-base/vim/","created":"2022-10-24 22:00","updated":"2025-08-27 23:24"}
---

# Vim

related: [[10-Inbox/Neovim\|Neovim]]

## tabs to space

Execute the following while the file is open and in normal mode.

```
:set tabstop=4 shiftwidth=4 expandtab 
:retab
```

### Bash

```
find . -iname '*.md' -type f -exec bash -c 'expand -t 4 "$0" | sponge "$0"' {} \;
```

Vim insert keys

```
i : insert before cursor
a : insert after cursor
s : insert on cursor
```

[How to configure vim for Latex](https://castel.dev/post/lecture-notes-1/)
There are a lot of snippets showcase using Ultisnip and other latex specific things inside.


<kbd>shift</kbd> + <kbd>k</kbd> : show the docs for the function


[How I'm able to take notes in mathematics lectures using LaTeX and Vim | Gilles Castel](https://castel.dev/post/lecture-notes-1/)
Vim and latex

## Append at the end for all lines

```
:%norm A*
```

This is what it means:

```
 %       = for every line
 norm    = type the following commands
 A*      = append '*' to the end of current line
```

source: [regex - How can I add a string to the end of each line in Vim? - Stack Overflow](https://stackoverflow.com/a/599416)



Here are some shortcuts that I always use in Obsidian (as well as in nvim).

- o : go to insert mode and insert a new line below
- O : go to insert mode and insert a new line above
- I : go to insert mode at the beginning of the line
- A : go to insert mode at the end of the line
- v> : this will go to visual mode, then indent the line to the right
- v< : this is similar to above, but indent to the left
- Vyp : duplicate the current line
- yyp : also duplicates the line
- dt' : this will delete till `'` or any other character/symbol that you type after `dt`. This does not delete the `'` itself
- df' : this will delete including `'` character, same as above, you can change to other character/symbol


One more thing that I use rarely but so glad it exists is the ability to do multi-cursor insert.

For instance, if you have a list of items, but you forgot to add `-` in front, what you can do is, go to the start of the line, press Ctrl + V, it will change to column select mode, you can go down `j` to select more lines, then once you're ready, you can do Shift + i and insert `-` or any other character or string you want. This trick is very useful if you need to add something to multiple lines at once.