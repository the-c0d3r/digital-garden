---
{"dg-publish":true,"permalink":"/garden/notes/elf/"}
---


# [[Garden/notes/ELF\|ELF]]
topic: [[02-Area/programming/Linux\|Linux]]
tags: #programming/linux 

Source: [Module: Program Interaction | pwn.college](https://pwn.college/modules/interaction)
ELF = Executable Linkable Format
Binary file format
Stores the architecture compiled on

Contains program and data
how the program should be loaded

## ELF Program headers

Magic Bytes: `7f 45 4c 46`, which stands for ELF
```python
>>> from binascii import unhexlify
>>> unhexlify("7f454c46")
b'\x7fELF'
```

`readelf -a /bin/cat` displays the elf header information of the file.

```bash
readelf -a /bin/cat

...
Program Headers:
  Type           Offset             VirtAddr           PhysAddr
                 FileSiz            MemSiz              Flags  Align
  PHDR           0x0000000000000040 0x0000000000000040 0x0000000000000040
                 0x00000000000002d8 0x00000000000002d8  R      0x8
  INTERP         0x0000000000000318 0x0000000000000318 0x0000000000000318
                 0x000000000000001c 0x000000000000001c  R      0x1
      [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]
  LOAD           0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000001688 0x0000000000001688  R      0x1000
  LOAD           0x0000000000002000 0x0000000000002000 0x0000000000002000
                 0x00000000000049b1 0x00000000000049b1  R E    0x1000
  LOAD           0x0000000000007000 0x0000000000007000 0x0000000000007000
                 0x0000000000002130 0x0000000000002130  R      0x1000
  LOAD           0x0000000000009a70 0x000000000000aa70 0x000000000000aa70
                 0x0000000000000650 0x00000000000007e8  RW     0x1000
  DYNAMIC        0x0000000000009c18 0x000000000000ac18 0x000000000000ac18
                 0x00000000000001f0 0x00000000000001f0  RW     0x8
  NOTE           0x0000000000000338 0x0000000000000338 0x0000000000000338
                 0x0000000000000040 0x0000000000000040  R      0x8
  NOTE           0x0000000000000378 0x0000000000000378 0x0000000000000378
                 0x0000000000000044 0x0000000000000044  R      0x4
  GNU_PROPERTY   0x0000000000000338 0x0000000000000338 0x0000000000000338
                 0x0000000000000040 0x0000000000000040  R      0x8
  GNU_EH_FRAME   0x0000000000007ed0 0x0000000000007ed0 0x0000000000007ed0
                 0x0000000000000324 0x0000000000000324  R      0x4
  GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000000000 0x0000000000000000  RW     0x10
  GNU_RELRO      0x0000000000009a70 0x000000000000aa70 0x000000000000aa70
                 0x0000000000000590 0x0000000000000590  R      0x1
```

Offset is the offset in the file, then on the right is the VirtAddr address offset. Then on the PhysAddr side, there are flags and alignments. 
Flags indicate "R" or "RW" which is READ or READWRITE

**INTERP**: defines the library that should be used to load this ELF into memory
**LOAD**: defines a part of of the file that should be loaded into memory

## ELF Section Headers

| Section    | Purpose                              |
| ---------- | ------------------------------------ |
| .text      | the executable code of the program   |
| .plt, .got | resolve and dispatch library calls   |
| .data      | pre-initialized global writable data |
| .rodata    | global read only data                |
| .bss       | uninitialized global writable data   | 

Section headers are *not* necessary part of ELF.

You can use a few commands to work with elf. 

```bash
readelf
nm
objdump
objcopy
strip
```