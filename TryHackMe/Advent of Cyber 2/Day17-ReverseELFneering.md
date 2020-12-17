# Day 17 - ReverseELFneering

**Date:** 17, December, 2020

**Author:** Dhilip Sanjay S

---

## Introduction to x86-64 Assembly
 - Intel x86-64 instruction set architecture
 - Step in producing an executable file:
    1. Compiler - source code to assembly code *(.s files)*.
    2. Assembler - assembly code to object code *(.o file)*.
    3. Linker - object code to executable file.
- 16 registers
- Register size = 64 bits

## Radare 2 
- [Radare 2 - Github](https://github.com/radareorg/radare2)
    - Disassemble binaries(translate **machine code to assembly**)
    - Debug the binaries

- Basic commands:
    - d (debug)
    - a (analyze)
    - pdf (print disassembly function)
    - db (breakpoints)
    - dc (run until breakpoint)
    - px (show hexdump)
    - ds (execute next instruction)
    - dr (show register value)
    - ood (reopen in debugger mode)

## x86-64 Assembly
- Basic Data Types

    |Initial Data Type | Suffix | Size (bytes)|
    |------------------|--------|-------------|
    |Byte	           |    b   |	1         |
    |Word	           |    w	|   2         |
    |Double Word	   |    l   |   4         |
    |Quad	           |    q	|   8         |
    |Single Precision  |    s   |   4         |
    |Double Precision  |    l   |   8         |


- Memory manipulation using registers:
    - **(Rb, Ri)** = MemoryLocation[Rb + Ri]
    - **D(Rb, Ri)** = MemoryLocation[Rb + Ri + D]
    - **(Rb, Ri, S)** = MemoryLocation(Rb + S * Ri]
    - **D(Rb, Ri, S)** = MemoryLocation[Rb + S * Ri + D]

- Important instructions
    - **leaq source, destination**: this instruction sets destination to the address denoted by the expression in source
    - **addq source, destination**: destination = destination + source
    - **subq source, destination**: destination = destination - source
    - **imulq source, destination**: destination = destination * source
    - **salq source, destination**: destination = destination << source where << is the left bit shifting operator
    - **sarq source, destination**: destination = destination >> source where >> is the right bit shifting operator
    - **xorq source, destination**: destination = destination XOR source
    - **andq source, destination**: destination = destination & source
    - **orq source, destination**: destination = destination | source

- IP (Instruction Pointer )
    - rip (IP 64 bit)
    - eip (IP 32 bit)

## CheatSheets
- [x64 Cheat Sheet](http://cs.brown.edu/courses/cs033/docs/guides/x64_cheatsheet.pdf)
- [R2 Cheatsheet](https://scoding.de/uploads/r2_cs.pdf)

## Solutions
### Initial commands to be executed to analyze
```bash
root@kali: r2 -d ./challenge1
[0x00400a30]> aa 
[ WARNING : block size exceeding max block size at 0x006ba220
[+] Try changing it with e anal.bb.maxsize
 WARNING : block size exceeding max block size at 0x006bc860
[+] Try changing it with e anal.bb.maxsize
[x] Analyze all flags starting with sym. and entry0 (aa)

[0x00400a30]> afl | grep main
0x00400b4d    1 35           sym.main
0x00400de0   10 1007 -> 219  sym.__libc_start_main
0x00403840   39 661  -> 629  sym._nl_find_domain
0x00403ae0  308 5366 -> 5301 sym._nl_load_domain
0x00415ef0    1 43           sym._IO_switch_to_main_get_area
0x0044ce10    1 8            sym._dl_get_dl_main_map
0x00470430    1 49           sym._IO_switch_to_main_wget_area
0x0048f9f0    7 73   -> 69   sym._nl_finddomain_subfreeres
0x0048fa40   16 247  -> 237  sym._nl_unload_domain

[0x00400a30]> pdf @main
            ;-- main:
/ (fcn) sym.main 35
|   sym.main ();
|           ; var int local_ch @ rbp-0xc
|           ; var int local_8h @ rbp-0x8
|           ; var int local_4h @ rbp-0x4
|              ; DATA XREF from 0x00400a4d (entry0)
|           0x00400b4d      55             push rbp
|           0x00400b4e      4889e5         mov rbp, rsp
|           0x00400b51      c745f4010000.  mov dword [local_ch], 1
|           0x00400b58      c745f8060000.  mov dword [local_8h], 6
|           0x00400b5f      8b45f4         mov eax, dword [local_ch]
|           0x00400b62      0faf45f8       imul eax, dword [local_8h]
|           0x00400b66      8945fc         mov dword [local_4h], eax
|           0x00400b69      b800000000     mov eax, 0
|           0x00400b6e      5d             pop rbp
\           0x00400b6f      c3             ret
```

### Setup the appropriate break points
```bash
[0x00400a30]> db 0x00400b58
[0x00400a30]> db 0x00400b66
[0x00400a30]> db 0x00400b69
[0x00400a30]> pdf @main
            ;-- main:
/ (fcn) sym.main 35
|   sym.main ();
|           ; var int local_ch @ rbp-0xc
|           ; var int local_8h @ rbp-0x8
|           ; var int local_4h @ rbp-0x4
|              ; DATA XREF from 0x00400a4d (entry0)
|           0x00400b4d      55             push rbp
|           0x00400b4e      4889e5         mov rbp, rsp
|           0x00400b51      c745f4010000.  mov dword [local_ch], 1
|           0x00400b58 b    c745f8060000.  mov dword [local_8h], 6
|           0x00400b5f      8b45f4         mov eax, dword [local_ch]
|           0x00400b62      0faf45f8       imul eax, dword [local_8h]
|           0x00400b66 b    8945fc         mov dword [local_4h], eax
|           0x00400b69 b    b800000000     mov eax, 0
|           0x00400b6e      5d             pop rbp
\           0x00400b6f      c3             ret
```

### What is the value of local_ch when its corresponding movl instruction is called (first if multiple)?
- **Answer:** 1
- **Steps to Reproduce:** 
    ```bash
    [0x00400b51]> dc
    hit breakpoint at: 400b58
    [0x00400b51]> px @rbp-0xc
    - offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
    0x7ffe5c1bffa4  0100 0000 1890 6b00 0000 0000 4018 4000  ......k.....@.@.
    0x7ffe5c1bffb4  0000 0000 e910 4000 0000 0000 0000 0000  ......@.........
    0x7ffe5c1bffc4  0000 0000 0000 0000 0100 0000 d800 1c5c  ...............\
    0x7ffe5c1bffd4  fe7f 0000 4d0b 4000 0000 0000 0000 0000  ....M.@.........
    0x7ffe5c1bffe4  0000 0000 0600 0000 5500 0000 5000 0000  ........U...P...
    0x7ffe5c1bfff4  0400 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c0004  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c0014  0000 0000 0000 0000 0000 0000 0004 4000  ..............@.
    0x7ffe5c1c0024  0000 0000 f49a 3e57 a2f0 4e59 e018 4000  ......>W..NY..@.
    0x7ffe5c1c0034  0000 0000 0000 0000 0000 0000 1890 6b00  ..............k.
    0x7ffe5c1c0044  0000 0000 0000 0000 0000 0000 f49a 3e98  ..............>.
    0x7ffe5c1c0054  1548 b2a6 f49a 8a46 a2f0 4e59 0000 0000  .H.....F..NY....
    0x7ffe5c1c0064  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c0074  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c0084  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c0094  0000 0000 0000 0000 0000 0000 0000 0000  ................
    ```
---

### What is the value of eax when the imull instruction is called?
- **Answer:** 6
- **Steps to Reproduce:**
    ```bash
    [0x00400b51]> dc
    hit breakpoint at: 400b66
    [0x00400b51]> dr | grep rax
    rax = 0x00000006
    orax = 0xffffffffffffffff
    ```
---

### What is the value of local_4h before eax is set to 0?
- **Answer:** 6
- **Steps to Reproduce:** 
    ```bash
    [0x00400b51]> dc
    hit breakpoint at: 400b69
    [0x00400b51]> px @rbp-0x4
    - offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
    0x7ffe5c1bffac  0600 0000 4018 4000 0000 0000 e910 4000  ....@.@.......@.
    0x7ffe5c1bffbc  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1bffcc  0100 0000 d800 1c5c fe7f 0000 4d0b 4000  .......\....M.@.
    0x7ffe5c1bffdc  0000 0000 0000 0000 0000 0000 0600 0000  ................
    0x7ffe5c1bffec  5500 0000 5000 0000 0400 0000 0000 0000  U...P...........
    0x7ffe5c1bfffc  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c000c  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c001c  0000 0000 0004 4000 0000 0000 f49a 3e57  ......@.......>W
    0x7ffe5c1c002c  a2f0 4e59 e018 4000 0000 0000 0000 0000  ..NY..@.........
    0x7ffe5c1c003c  0000 0000 1890 6b00 0000 0000 0000 0000  ......k.........
    0x7ffe5c1c004c  0000 0000 f49a 3e98 1548 b2a6 f49a 8a46  ......>..H.....F
    0x7ffe5c1c005c  a2f0 4e59 0000 0000 0000 0000 0000 0000  ..NY............
    0x7ffe5c1c006c  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c007c  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c008c  0000 0000 0000 0000 0000 0000 0000 0000  ................
    0x7ffe5c1c009c  0000 0000 0000 0000 0000 0000 0000 0000  ................
    ```
---
[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202) 