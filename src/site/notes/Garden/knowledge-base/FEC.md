---
{"dg-publish":true,"permalink":"/garden/knowledge-base/fec/","tags":["definition","networking","flashcards"],"created":"2023-06-10 22:31","updated":"2026-04-18 20:18","dg-note-properties":{"modified_date":"2026-04-18 20:18","creation_date":"2023-06-10 22:31","alias":["Forward Error Correction"],"tags":["definition","networking","flashcards"]}}
---

# Forward Error Correction ([[Garden/knowledge-base/FEC\|FEC]])
topics: [[02-Area/programming/Programming\|Programming]]


A technique to recover the error occurring in data storage, data transfer, without having to retransmit the original data. This is mostly used in Deep Space communication, CD/DVD storage, [[99-Archived/02-Area/work/projects/STRIDE/udpcast\|udpcast]], etc.

FEC works by adding redundant information to the original data, which can be used to reconstruct the lost or corrupted data. The amount of redundant information added depends on the level of error correction required.

There are a few different types of FEC (forward error correction), that works on different type of cases. Some of the common types of FEC are as follows:

1. Block codes: In block codes, a group of bits is taken together and checked for errors. If an error is found, the entire group is retransmitted.
2. Convolutional codes: In convolutional codes, each bit is encoded along with its neighbouring bits. This provides more redundancy and the ability to detect and correct errors in real-time.
3. Turbo codes: Turbo codes are a type of convolutional code that uses multiple encoders and decoders to provide even greater error correction capabilities.
4. Reed-Solomon codes: Reed-Solomon codes are used specifically for correcting errors in data stored on disks, CDs, and other digital media. They can correct errors on a byte-by-byte basis. This is useful in case of burst packet loss.

Overall, the choice of FEC depends on the application and the specific type of error that needs to be corrected.

Reed Solomon codes are used in situations where errors occur in bursts, such as in CD/DVD storage. These codes add extra parity bits to the data, allowing the receiver to recover the original data even if a few bits are lost due to a burst error.

Convolutional codes are used in situations where errors occur randomly, such as in wireless communication. These codes add redundancy by using a sliding window to encode the data, which allows the receiver to correct random errors in the received data.

By using both Reed Solomon and Convolutional codes, the chances of data loss due to errors can be minimized. However, this also means that more redundant information is added, which increases the amount of network bandwidth required for data transmission.

Other types of network coding
- [[03-Resource/networking/RLNC\|RLNC]] (Random Linear Network Coding)

## FEC libraries

### Cli tools

- [Redupe: Forward Error Correction](https://hack.systems/2018/05/16/redupe/), [GitHub - rescrv/redupe: redupe implements forward error correction](https://github.com/rescrv/redupe)
    - unable to compile, got c99 standard issue
- [tahoe-lafs/zfec: zfec -- an efficient, portable erasure coding tool](https://github.com/tahoe-lafs/zfec)
    - Cli tool with C and, python binding
    - only work for files. It can only encode stdin to files, send the files over, then use `unzfec` to merge it back. This won't work for kafka streaming.
    - We could write files with exact lines, then zfec it, transfer the files, then merge it back. But how to solve the problem of when to do the `unzfec`?
- [GitHub - WojciechMigda/zfex: zfex — an efficient, portable erasure coding tool](https://github.com/WojciechMigda/zfex)
    - This is very recent, can hit up to 4G/s, improvement on the zfec. But this one only work on file
- rsbep
    - [GitHub - opencoff/rsbep: Add Reed Solomon Error correction to archives](https://github.com/opencoff/rsbep)
    - Seem to be less efficient, as it does not use "erasure codes"
- Hackernews link that contains a few more other similar tools
    - [Zfec – Efficient, portable erasure coding tool | Hacker News](https://news.ycombinator.com/item?id=12976168)

### Libraries

- ~~[raptorq/main.py](https://github.com/cberner/raptorq/blob/master/examples/main.py)~~
    - source: [Hacker News](https://news.ycombinator.com/item?id=21908371)
    - very fast rust based python binding for RaptorQ implementation for FEC
    - only works for python or rust
- ~~[GitHub - sleepybishop/nanorq: fast compact raptorq (rfc6330) codec in c](https://github.com/sleepybishop/nanorq)~~
    - it says with single core, it can hit multi gigabit speed
    - cannot compile, `oblas_avx.c:56:7: warning: implicit declaration of function ‘_mm256_loadu2_m128i’; did you mean ‘_mm256_loadu_si256’? [-Wimplicit-function-declaration]`
- [GitHub - catid/longhair: Longhair : O(N^2) Cauchy Reed-Solomon Block Erasure Code for Small Data](https://github.com/catid/longhair)
    - last updated 2 years ago
    - written in C++ with C wrapper
    - encode k=23, m=10 encode speed=1064 MB/s

## Cauchy Caterpillar

[GitHub - catid/CauchyCaterpillar: Cauchy Caterpillar : O(N^2) Short-Window Streaming Erasure Code](https://github.com/catid/CauchyCaterpillar)

This is something that superseds the shorthair thingy
[GitHub - catid/shorthair: Shorthair : Generational Block Streaming Erasure Codes](https://github.com/catid/shorthair)
There is a limit on data rate (< 2000 packets/second)
[GitHub - catid/leopard: Leopard-RS : O(N Log N) MDS Reed-Solomon Block Erasure Code for Large Data](https://github.com/catid/leopard)

## Wirehair

[GitHub - catid/wirehair: Wirehair : O(N) Fountain Code for Large Data](https://github.com/catid/wirehair)
- written in C++ with C wrapper
- may require N+1, N+2 or more to recover, on avg, N+0.02
- test result
    - n=1000, size=1300, encoder create (925 mbps), encode (7778 mbps), decode (740 mbps), overhead piece = 0.0185 (more than n) on my laptop
    - n=1000, size=1300, encoder create (773 mbps), encode (6252 mbps), decode (621 mbps), overhead piece = 0.0185 (more than n) on 223 server
- `g++ -o test2 test2.c -lwirehair`
- can handle without order, but block ID has to be the same, if not it will be out of order
- cannot handle data corruption, but it can handle missing packet. This can be handled by either decompression failure or udp checksum failure

Requirements
- data must be in exact block size
- block id must be given, either from header or from sequential receiving ID
- [GitHub - catid/cm256: Fast GF(256) Cauchy MDS Block Erasure Codec in C](https://github.com/catid/cm256)
    - last updated 2 years ago
    - written in C++ with C wrapper
- [librecast/lcrq: C implementation of RFC6330 RaptorQ Codes](https://codeberg.org/librecast/lcrq)
    - This is by a project called librecast, which is ipv6 multicast
    - [Librecast - Decentralisation and Privacy with Multicast](https://librecast.net/roadmap.html)
    - looks like it has completed the features that we need, like server, client
    - this is quite an ambitious project, I think this requires bi-directional
- [quiet/libcorrect: C library Convolutional codes and Reed-Solomon](https://github.com/quiet/libcorrect)
    - RS from this one cannot be used, as it requires a lot of work: source:  [network simulation help for reed solomon codes](https://github.com/quiet/libcorrect/issues/11), and only supports 255 bytes of payload
- [unisync/unicast.c at main - unisync - Codeberg.org](https://codeberg.org/librecast/unisync/src/branch/main/src/unicast.c)
    - librecast project's unicast code
- [https://fenrirproject.org/Luker/libRaptorQ/-/wikis/libRaptorQ-RaptorQ.pdf](https://fenrirproject.org/Luker/libRaptorQ/-/wikis/libRaptorQ-RaptorQ.pdf)
    - how raptorQ works
- [Why DF Raptor is better than Reed Solomon for Streaming Applications.pdf](https://www.qualcomm.com/content/dam/qcomm-martech/dm-assets/documents/Why_DF_Raptor_is_better_than_Reed_Solomon_for_Streaming_Applications.pdf)
- [libRaptorQ: RaptorQ RFC6330 C++11 implementation](https://github.com/LucaFulchir/libRaptorQ)
    - need to implement a way to tell the receiver how long is each packet/block
- [liberasurecode: Erasure Code API library written in C](https://github.com/openstack/liberasurecode)
- [schifra: C++ Reed Solomon Error Correcting Library](https://github.com/ArashPartow/schifra) : NOGO
    - C++ library that seems promising, with example usage code and docs
    - Need to purchase license for commercial usage: [Schifra Reed-Solomon Error Correcting Code Library](https://www.schifra.com/license.html)
- [OpenFEC.org](http://openfec.org/library_content.html)
    - list of libraries for the FEC stuff
    - this only support Reed Solomon and LDPC
- [GitHub - wangyu-/UDPspeeder](https://github.com/wangyu-/UDPspeeder)
    - This has implementation of Reed Solomon, easily copied
    - this implementation requires C++
- [libsathelper/reedsolomon.cpp](https://github.com/opensatelliteproject/libsathelper/blob/master/src/reedsolomon.cpp)
    - code using the libcorrect
- [Sci-Hub | | 10.1109/ICC.2018.8422948](https://sci-hub.se/10.1109/ICC.2018.8422948)
    - Raptor Code UDP (cannot find source code)
- [GitHub - lorinder/TvRQ: Test vector RaptorQ](https://github.com/lorinder/TvRQ)
    - library for raptorq in C, also has python binding
- [AFF3CT - A Fast Forward Error Correction Toolbox](http://aff3ct.github.io/)
- [Flute](https://crates.io/crates/flute)
    - Rust based udpcast type, but also have support for python binding
- [RFC 9223: Real-Time Transport Object Delivery over Unidirectional Transport (ROUTE)](https://www.rfc-editor.org/rfc/rfc9223): a new protocol with FEC frames built-in
    - [route · gpac/gpac Wiki · GitHub](https://github.com/gpac/gpac/wiki/route) : multimedia streaming tool that implements ROUTE protocol
- [SMPTE Standard - Unidirectional Transport of Constant Bit Rate MPEG-2 Transport Streams on IP Networks](https://ieeexplore.ieee.org/document/7291740)
- [GitHub - cea-sec/hairgap: A set of tools to transfer data over a unidirectional network link (typically a network diode).](https://github.com/cea-sec/hairgap)
    - [GitHub - catid/wirehair: Wirehair : O(N) Fountain Code for Large Data](https://github.com/catid/wirehair)
- [GitHub - 5G-MAG/rt-libflute: File Delivery over Unidirectional Transport (FLUTE)](https://github.com/5G-MAG/rt-libflute): C++

### Tuning guides

- [Ethernet — Performance Tuning on Linux](https://cromwell-intl.com/open-source/performance-tuning/ethernet.html)
- [How to achieve low latency with 10Gbps Ethernet](https://blog.cloudflare.com/how-to-achieve-low-latency/)

### [[Garden/knowledge-base/FEC\|FEC]] modes

- [[03-Resource/networking/RLNC\|Random Linear Network Coding]]
- LDPC

Options
- Use [libcorrect](https://github.com/quiet/libcorrect) then implement it on udpperf or another dpdk
- Use [raptorq/main.py](https://github.com/cberner/raptorq/blob/master/examples/main.py) and implement it in python code
    - this will be better, lead to better control over the streaming, compression, etc



[Error correction for UDP transmission? : r/learnpython](https://www.reddit.com/r/learnpython/comments/ceqrli/error_correction_for_udp_transmission/)
useful info on the implementation

> the error distance must be below the group size at all times to prevent data loss. I would rather suggest to encode the RS data within the chunks to prevent multiple dropped packets in a row automatically resulting in data loss. Like with the [reedsolomon](https://github.com/tomerfiliba/reedsolomon) library, that allows you to set the chunksize to the MTU of your network packet (I would go a little lower in case you're sending this over the internet, like 1300 or so). Then the amount of failed packets as a total counts, not in what pattern they appeared.

### Generated Flashcards

#flashcards

What is Forward Error Correction? :: A technique to recover the error occurring in data storage, data transfer, without having to retransmit the original data.
<!--SR:!2024-08-08,2,230-->

What are Block codes? :: In block codes, a group of bits is taken together and checked for errors. If an error is found, the entire group is retransmitted.

What are Convolutional codes? :: In convolutional codes, each bit is encoded along with its neighbouring bits. This provides more redundancy and the ability to detect and correct errors in real-time.

What are Turbo Codes? :: Turbo codes are a type of convolutional code that uses multiple encoders and decoders to provide even greater error correction capabilities.

When do Reed Solomon Codes apply?:: Reed Solomon codes are used specifically for correcting errors in data stored on disks, CDs, and other digital media. They can correct errors on a byte-by-byte basis


Which type of FEC works on burst lost cases? :: Reed Solomon
<!--SR:!2024-08-08,2,230-->

Which type of FEC works on random packet lost cases? :: Convolutional code
<!--SR:!2023-06-13,3,250-->

What are the benefits of using both types of FEC together? :: Minimise chances of data loss
<!--SR:!2026-01-24,2,230-->

What are some drawbacks when using both types of FEC together?:: Incur more network bandwidth
<!--SR:!2024-08-14,8,250-->