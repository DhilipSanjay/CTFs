# Intro to x86-64

**Date:** 31, May, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/introtox8664) to go to the TryHackMe room.


## Introduction
- Computers execute machine code, which is encoded as bytes, to carry out tasks on a computer. Since different computers have different processors, the **machine code** executed on these computers is **specific to the processor**.
- **Intel x86-64 instruction set** architecture is commonly used today.
- This machine code is usually produced by a compiler, which takes the source code of a file, and after going through some intermediate stages, produces machine code that can be executed by a computer.
- 16-bit -> 32 bit -> 64 bit (instruction set)
    - All these instruction sets have been created for backward compatibility, so code compiled for 32 bit architecture will run on 64 bit machines. 
- Before an executable file is produced, the source code is first compiled into **assembly(.s files)**, after which the assembler converts it into an **object program(.o files)**, and operations with a linker finally make it an executable. 

### Radare2
- radare2 is a framework for reverse engineering and analysing binaries. 
- It can be used to **disassemble binaries**(translate machine code to assembly, which is actually readable) and debug said binaries(by allowing a user to step through the execution and view the state of the program). 
- To debug the executable: `r2 -d <executable>`
- To analyze all: `aa`
- To set the syntax to AT&T: `e asm.syntax=att`
- Help command: `?`
- To find a list of the functions run: `afl`
    ```bash
    [0x7fa314fcf090]> afl
    0x55824f4fa560    1 42           entry0
    0x55824f6fafe0    1 4124         reloc.__libc_start_main
    0x55824f4fa590    4 50   -> 40   sym.deregister_tm_clones
    0x55824f4fa5d0    4 66   -> 57   sym.register_tm_clones
    0x55824f4fa620    5 58   -> 51   entry.fini0
    0x55824f4fa550    1 6            sym..plt.got
    0x55824f4fa660    1 10           entry.init0
    0x55824f4fa730    1 2            sym.__libc_csu_fini
    0x55824f4fa734    1 9            sym._fini
    0x55824f4fa6c0    4 101          sym.__libc_csu_init
    0x55824f4fa66a    1 78           main
    0x55824f4fa540    1 6            sym.imp.__printf_chk
    0x55824f4fa510    3 23           sym._init
    0x55824f4fa000    3 97   -> 123  map.home_tryhackme_introduction_intro.r_x
    ```
- Print Disassembly Function: `pdf @main`

```bash
[0x7fa314fcf090]> pdf @main
            ;-- main:
/ (fcn) main 78
|   int main (int argc, char **argv, char **envp);
|           ; DATA XREF from entry0 (0x55824f4fa57d)
|           0x55824f4fa66a      4883ec08       subq $8, %rsp
|           0x55824f4fa66e      b902000000     movl $2, %ecx
|           0x55824f4fa673      ba01000000     movl $1, %edx
|           0x55824f4fa678      488d35c90000.  leaq str.value_for_a_is__d_and_b_is__d, %rsi ; 0x55824f4fa748 ; "value for a is %d and b is %d\n"                                                                                                          
|           0x55824f4fa67f      bf01000000     movl $1, %edi
|           0x55824f4fa684      b800000000     movl $0, %eax
|           0x55824f4fa689      e8b2feffff     callq sym.imp.__printf_chk
|           0x55824f4fa68e      b901000000     movl $1, %ecx
|           0x55824f4fa693      ba02000000     movl $2, %edx
|           0x55824f4fa698      488d35c90000.  leaq str.value_of_a_is__d_and_b_is__d, %rsi ; 0x55824f4fa768 ; "value of a is %d and b is %d\n"                                                                                                            
|           0x55824f4fa69f      bf01000000     movl $1, %edi
|           0x55824f4fa6a4      b800000000     movl $0, %eax
|           0x55824f4fa6a9      e892feffff     callq sym.imp.__printf_chk
|           0x55824f4fa6ae      b800000000     movl $0, %eax
|           0x55824f4fa6b3      4883c408       addq $8, %rsp
\           0x55824f4fa6b7      c3             retq
```

- The values on the complete left column are memory addresses of the instructions, and these are usually stored in a structure called the stack. 
- The middle column contains the instructions encoded in bytes(what is usually the machine code)
- The last column actually contains the human readable instructions. 

### List of registers:

| 64 bit | 32 bit |
|---------|----------|
| %rax | %eax |
| %rbx | %ebx |
| %rcx | %ecx |
| %rdx | %edx |
| %rsi | %esi |
| %rdi | %edi |
| %rsp | %esp |
| %rbp | %ebp |
| %r8 | %r8d |
| %r9 | %r9d |
| %r10 | %r10d |
| %r11 | %r11d |
| %r12 | %r12d |
| %r13 | %r13d |
| %r14 | %r14d |
| %r15 | %r15d |

- The first 6 registers are known as general purpose registers. 
- The `%rsp` is the stack pointer and it points to the top of the stack which contains the most recent memory address. The stack is a data structure that manages memory for programs. 
- `%rbp` is a frame pointer and points to the frame of the function currently being executed - every function is executed in a new frame.

### Mov instruction

- To move data using registers, the following instruction is used: `movq source, destination`
    - Transferring **constants**(which are prefixed using the $ operator) e.g. `movq $3 rax` would move the constant 3 to the register
    - Transferring values from/to a **register** e.g. `movq %rax %rbx` which involves moving value from rax to rbx
    - Transferring values from/to **memory** which is shown by putting registers inside **brackets** e.g. `movq %rax (%rbx)` which means move value stored in %rax to memory location represented by %rbx.


### Basic Data Types

|Initial Data Type | Suffix | Size (bytes)|
|------------------|--------|-------------|
|Byte	           |    b   |	1         |
|Word	           |    w	|   2         |
|Double Word	   |    l   |   4         |
|Quad	           |    q	|   8         |
|Single Precision  |    s   |   4         |
|Double Precision  |    l   |   8         |


### Memory manipulation using registers
- **(Rb, Ri)** = MemoryLocation[Rb + Ri]
- **D(Rb, Ri)** = MemoryLocation[Rb + Ri + D]
- **(Rb, Ri, S)** = MemoryLocation(Rb + S * Ri]
- **D(Rb, Ri, S)** = MemoryLocation[Rb + S * Ri + D]

### Important instructions
- **leaq source, destination**: this instruction sets destination to the address denoted by the expression in source (Load Effective Address - It doesn't mov the contents!)
- **addq source, destination**: destination = destination + source
- **subq source, destination**: destination = destination - source
- **imulq source, destination**: destination = destination * source
- **salq source, destination**: destination = destination << source where << is the left bit shifting operator
- **sarq source, destination**: destination = destination >> source where >> is the right bit shifting operator
- **xorq source, destination**: destination = destination XOR source
- **andq source, destination**: destination = destination & source
- **orq source, destination**: destination = destination | source

---

## If statements

- If statements use 3 important instructions in assembly:
    - `cmpq source2, source1`: it is like computing `source1-source2` without setting destination
        - **Example:** `cmpl var_4h, %eax` (Compare value in eax with var_4h) 
    - `testq source2, source1`: it is like computing `source1&source2` without setting destination
    - Jump instructions

### Types of Jumps

| Jump Type | Description |
|---------|----------|
| jmp | Unconditional |
| je | Equal/Zero | 
| jne | Not Equal/Not Zero |
| js | Negative | 
| jns | Nonnegative |
| jg | Greater |
| jge | Greater or Equal |
| jl | Less |
| jle | Less or Equal |
| ja | Above(unsigned) |
| jb | Below(unsigned) |

- The last 2 values of the table refer to unsigned integers. 
- Unsigned integers cannot be negative while signed integers represent both positive and negative values. 
- Since the computer needs to differentiate between them, it uses different methods to interpret these values. 
- For signed integers, it uses something called the **two’s complement representation** and for unsigned integers it uses** normal binary calculations**. 

### Analyzing Jump
- To set a breakpoint: `db 0x<Address>`
- To run the program until breakpoint: `dc`
- To view the value of the registers at breakpoint: `dr`
- To view the value in the variable: `px @location`
- To seek/move onto the next instruction: `ds`


{% hint style="info" %}
- `popq` instruction involves popping a value of the stack and reading it.
- `retq` instruction sets this popped value to the current instruction pointer.
{% endhint %}


## Analysing if2

```bash
tryhackme@ip-10-10-239-255:~/if-statement$ r2 -d if2
Process with PID 1351 started...
= attach 1351 1351
bin.baddr 0x55af30dec000
Using 0x55af30dec000
asm.bits 64
 -- A C program is like a fast dance on a newly waxed dance floor by people carrying razors - Waldi Ravens
[0x7eff1fc24090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[Warning: Invalid range. Use different search.in=? or anal.in=dbg.maps.x
Warning: Invalid range. Use different search.in=? or anal.in=dbg.maps.x
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[TOFIX: aaft cant run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7eff1fc24090]> afl
0x55af30dec4f0    1 42           entry0
0x55af30fecfe0    1 4124         reloc.__libc_start_main
0x55af30dec520    4 50   -> 40   sym.deregister_tm_clones
0x55af30dec560    4 66   -> 57   sym.register_tm_clones
0x55af30dec5b0    5 58   -> 51   entry.fini0
0x55af30dec4e0    1 6            sym.imp.__cxa_finalize
0x55af30dec5f0    1 10           entry.init0
0x55af30dec6b0    1 2            sym.__libc_csu_fini
0x55af30dec6b4    1 9            sym._fini
0x55af30dec640    4 101          sym.__libc_csu_init
0x55af30dec5fa    5 68           main
0x55af30dec4b8    3 23           sym._init
[0x7eff1fc24090]> pdf @main
/ (fcn) main 68
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_ch @ rbp-0xc
|           ; var int32_t var_8h @ rbp-0x8
|           ; var int32_t var_4h @ rbp-0x4
|           ; DATA XREF from entry0 (0x55af30dec50d)
|           0x55af30dec5fa      55             pushq %rbp
|           0x55af30dec5fb      4889e5         movq %rsp, %rbp
|           0x55af30dec5fe      c745f4000000.  movl $0, var_ch
|           0x55af30dec605      c745f8630000.  movl $0x63, var_8h      ; 'c' ; 99
|           0x55af30dec60c      c745fce80300.  movl $0x3e8, var_4h     ; 1000
|           0x55af30dec613      8b45f4         movl var_ch, %eax
|           0x55af30dec616      3b45f8         cmpl var_8h, %eax
|       ,=< 0x55af30dec619      7d0e           jge 0x55af30dec629
|       |   0x55af30dec61b      8b45f8         movl var_8h, %eax
|       |   0x55af30dec61e      3b45fc         cmpl var_4h, %eax
|      ,==< 0x55af30dec621      7d0d           jge 0x55af30dec630
|      ||   0x55af30dec623      8365f864       andl $0x64, var_8h
|     ,===< 0x55af30dec627      eb07           jmp 0x55af30dec630
|     ||`-> 0x55af30dec629      8145f4b00400.  addl $0x4b0, var_ch
|     ||    ; CODE XREF from main (0x55af30dec627)
|     ``--> 0x55af30dec630      816dfce70300.  subl $0x3e7, var_4h
|           0x55af30dec637      b800000000     movl $0, %eax
|           0x55af30dec63c      5d             popq %rbp
\           0x55af30dec63d      c3             retq
[0x7eff1fc24090]> db 0x55af30dec619
[0x7eff1fc24090]> db 0x55af30dec621
[0x7eff1fc24090]> db 0x55af30dec627
[0x7eff1fc24090]> dc
hit breakpoint at: 55af30dec619
[0x55af30dec619]> pdf @main
/ (fcn) main 68
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_ch @ rbp-0xc
|           ; var int32_t var_8h @ rbp-0x8
|           ; var int32_t var_4h @ rbp-0x4
|           ; DATA XREF from entry0 (0x55af30dec50d)
|           0x55af30dec5fa      55             pushq %rbp
|           0x55af30dec5fb      4889e5         movq %rsp, %rbp
|           0x55af30dec5fe      c745f4000000.  movl $0, var_ch
|           0x55af30dec605      c745f8630000.  movl $0x63, var_8h      ; 'c' ; 99
|           0x55af30dec60c      c745fce80300.  movl $0x3e8, var_4h     ; 1000
|           0x55af30dec613      8b45f4         movl var_ch, %eax
|           0x55af30dec616      3b45f8         cmpl var_8h, %eax
|           ;-- rip:
|       ,=< 0x55af30dec619 b    7d0e           jge 0x55af30dec629
|       |   0x55af30dec61b      8b45f8         movl var_8h, %eax
|       |   0x55af30dec61e      3b45fc         cmpl var_4h, %eax
|      ,==< 0x55af30dec621 b    7d0d           jge 0x55af30dec630
|      ||   0x55af30dec623      8365f864       andl $0x64, var_8h
|     ,===< 0x55af30dec627 b    eb07           jmp 0x55af30dec630
|     ||`-> 0x55af30dec629      8145f4b00400.  addl $0x4b0, var_ch
|     ||    ; CODE XREF from main (0x55af30dec627)
|     ``--> 0x55af30dec630      816dfce70300.  subl $0x3e7, var_4h
|           0x55af30dec637      b800000000     movl $0, %eax
|           0x55af30dec63c      5d             popq %rbp
\           0x55af30dec63d      c3             retq
```

### Tracing the assembly code
- Initially, 
    - `var_ch` will have 0
    - `var_4h` will have 1000
    - `var_8h` will have 99 
- First `jge` will fail (`var_ch > var_8h? -> FALSE`)
- Second `jge` will also fail (`var_8h > var_4h? -> FALSE`)
- Perform `and` operation on `var_8h` with `0x64`
- Perform **unconditional jmp**
- Perform `sub` operation on `var_4h` with `0x3e7`
- Clear `eax`
- Pop `rbp` and return


### What is the value of var_8h before the popq and ret instructions?
- **Answer:** 96
- **Steps to Reproduce:**
    - Hex `60` -> Decimal `96`

```bash
[0x55af30dec619]> px @rbp-0x8
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffd679037e8  6000 0000 0100 0000 40c6 de30 af55 0000  `.......@..0.U..                                                                        
0x7ffd679037f8  973b 851f ff7e 0000 0100 0000 0000 0000  .;...~..........
0x7ffd67903808  d838 9067 fd7f 0000 0080 0000 0100 0000  .8.g............
0x7ffd67903818  fac5 de30 af55 0000 0000 0000 0000 0000  ...0.U..........
0x7ffd67903828  c069 d794 5837 5c52 f0c4 de30 af55 0000  .i..X7\R...0.U..
0x7ffd67903838  d038 9067 fd7f 0000 0000 0000 0000 0000  .8.g............
0x7ffd67903848  0000 0000 0000 0000 c069 5768 c599 f806  .........iWh....
0x7ffd67903858  c069 c96e ef69 fc04 0000 0000 fd7f 0000  .i.n.i..........
0x7ffd67903868  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd67903878  3337 c31f ff7e 0000 3896 c11f ff7e 0000  37...~..8....~..
0x7ffd67903888  f33b 0700 0000 0000 0000 0000 0000 0000  .;..............
0x7ffd67903898  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd679038a8  f0c4 de30 af55 0000 d038 9067 fd7f 0000  ...0.U...8.g....
0x7ffd679038b8  1ac5 de30 af55 0000 c838 9067 fd7f 0000  ...0.U...8.g....
0x7ffd679038c8  1c00 0000 0000 0000 0100 0000 0000 0000  ................
0x7ffd679038d8  7c57 9067 fd7f 0000 0000 0000 0000 0000  |W.g............
```

### What is the value of var_ch before the popq and ret instructions?
- **Answer:** 0
- **Steps to Reproduce:**

```bash
[0x55af30dec619]> px @rbp-0xc
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffd679037e4  0000 0000 6000 0000 0100 0000 40c6 de30  ....`.......@..0                                                                        
0x7ffd679037f4  af55 0000 973b 851f ff7e 0000 0100 0000  .U...;...~......
0x7ffd67903804  0000 0000 d838 9067 fd7f 0000 0080 0000  .....8.g........
0x7ffd67903814  0100 0000 fac5 de30 af55 0000 0000 0000  .......0.U......
0x7ffd67903824  0000 0000 c069 d794 5837 5c52 f0c4 de30  .....i..X7\R...0
0x7ffd67903834  af55 0000 d038 9067 fd7f 0000 0000 0000  .U...8.g........
0x7ffd67903844  0000 0000 0000 0000 0000 0000 c069 5768  .............iWh
0x7ffd67903854  c599 f806 c069 c96e ef69 fc04 0000 0000  .....i.n.i......
0x7ffd67903864  fd7f 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd67903874  0000 0000 3337 c31f ff7e 0000 3896 c11f  ....37...~..8...
0x7ffd67903884  ff7e 0000 f33b 0700 0000 0000 0000 0000  .~...;..........
0x7ffd67903894  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd679038a4  0000 0000 f0c4 de30 af55 0000 d038 9067  .......0.U...8.g
0x7ffd679038b4  fd7f 0000 1ac5 de30 af55 0000 c838 9067  .......0.U...8.g
0x7ffd679038c4  fd7f 0000 1c00 0000 0000 0000 0100 0000  ................
0x7ffd679038d4  0000 0000 7c57 9067 fd7f 0000 0000 0000  ....|W.g........
```

### What is the value of var_4h before the popq and ret instructions?
- **Answer:** 1
- **Steps to Reproduce:**

```bash
[0x55af30dec619]> px @rbp-0x4
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffd679037ec  0100 0000 40c6 de30 af55 0000 973b 851f  ....@..0.U...;..                                                                        
0x7ffd679037fc  ff7e 0000 0100 0000 0000 0000 d838 9067  .~...........8.g
0x7ffd6790380c  fd7f 0000 0080 0000 0100 0000 fac5 de30  ...............0
0x7ffd6790381c  af55 0000 0000 0000 0000 0000 c069 d794  .U...........i..
0x7ffd6790382c  5837 5c52 f0c4 de30 af55 0000 d038 9067  X7\R...0.U...8.g
0x7ffd6790383c  fd7f 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd6790384c  0000 0000 c069 5768 c599 f806 c069 c96e  .....iWh.....i.n
0x7ffd6790385c  ef69 fc04 0000 0000 fd7f 0000 0000 0000  .i..............
0x7ffd6790386c  0000 0000 0000 0000 0000 0000 3337 c31f  ............37..
0x7ffd6790387c  ff7e 0000 3896 c11f ff7e 0000 f33b 0700  .~..8....~...;..
0x7ffd6790388c  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd6790389c  0000 0000 0000 0000 0000 0000 f0c4 de30  ...............0
0x7ffd679038ac  af55 0000 d038 9067 fd7f 0000 1ac5 de30  .U...8.g.......0
0x7ffd679038bc  af55 0000 c838 9067 fd7f 0000 1c00 0000  .U...8.g........
0x7ffd679038cc  0000 0000 0100 0000 0000 0000 7c57 9067  ............|W.g
0x7ffd679038dc  fd7f 0000 0000 0000 0000 0000 8257 9067  .............W.g
```


### What operator is used to change the value of var_8h, input the symbol as your answer(symbols include +, -, *, /, &, |):
- **Answer:** &
- **Steps to Reproduce:**
    - The following line in the code denotes that `and` operation is perfomed:

    ```bash
    0x55af30dec623      8365f864       andl $0x64, var_8h
    ```

---

## Loops

- A quicker way to examine the loop would be to add a break point to `cmpl` instruction and running dc. Since this is a loop, the program will always break at the `cmpl` instruction(because this instruction checks the condition before executing what is inside the loop)


### Analyzing loop2

```bash
tryhackme@ip-10-10-239-255:~/loops$ r2 -d loop2
Process with PID 1365 started...
= attach 1365 1365
bin.baddr 0x560f6d102000
Using 0x560f6d102000
asm.bits 64
 -- Don't do this.
[0x7f948671a090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[Warning: Invalid range. Use different search.in=? or anal.in=dbg.maps.x
Warning: Invalid range. Use different search.in=? or anal.in=dbg.maps.x
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[TOFIX: aaft can't run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7f948671a090]> afl
0x560f6d1024f0    1 42           entry0
0x560f6d302fe0    1 4124         reloc.__libc_start_main
0x560f6d102520    4 50   -> 40   sym.deregister_tm_clones
0x560f6d102560    4 66   -> 57   sym.register_tm_clones
0x560f6d1025b0    5 58   -> 51   entry.fini0
0x560f6d1024e0    1 6            sym.imp.__cxa_finalize
0x560f6d1025f0    1 10           entry.init0
0x560f6d1026b0    1 2            sym.__libc_csu_fini
0x560f6d1026b4    1 9            sym._fini
0x560f6d102640    4 101          sym.__libc_csu_init
0x560f6d1025fa    4 66           main
0x560f6d1024b8    3 23           sym._init
[0x7f948671a090]> pdf @main
/ (fcn) main 66
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_ch @ rbp-0xc
|           ; var int32_t var_8h @ rbp-0x8
|           ; var int32_t var_4h @ rbp-0x4
|           ; DATA XREF from entry0 (0x560f6d10250d)
|           0x560f6d1025fa      55             pushq %rbp
|           0x560f6d1025fb      4889e5         movq %rsp, %rbp
|           0x560f6d1025fe      c745f4140000.  movl $0x14, var_ch      ; 20
|           0x560f6d102605      c745f8160000.  movl $0x16, var_8h      ; 22
|           0x560f6d10260c      c745fc000000.  movl $0, var_4h
|           0x560f6d102613      c745fc040000.  movl $4, var_4h
|       ,=< 0x560f6d10261a      eb13           jmp 0x560f6d10262f
|      .--> 0x560f6d10261c      8365f402       andl $2, var_ch
|      :|   0x560f6d102620      d17df8         sarl $1, var_8h
|      :|   0x560f6d102623      8b55fc         movl var_4h, %edx
|      :|   0x560f6d102626      89d0           movl %edx, %eax
|      :|   0x560f6d102628      01c0           addl %eax, %eax
|      :|   0x560f6d10262a      01d0           addl %edx, %eax
|      :|   0x560f6d10262c      8945fc         movl %eax, var_4h
|      :|   ; CODE XREF from main (0x560f6d10261a)
|      :`-> 0x560f6d10262f      837dfc63       cmpl $0x63, var_4h      ; 'c'
|      `==< 0x560f6d102633      7ee7           jle 0x560f6d10261c
|           0x560f6d102635      b800000000     movl $0, %eax
|           0x560f6d10263a      5d             popq %rbp
\           0x560f6d10263b      c3             retq
[0x7f948671a090]> db 0x560f6d10262f
[0x7f948671a090]> dc
hit breakpoint at: 560f6d10262f
```


### What is the value of var_8h on the second iteration of the loop?
- **Answer:** 5
- **Steps to Reproduce:**

```bash
[0x560f6d10261c]> px @rbp-0x8
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffce2e95e38  0500 0000 2400 0000 4026 106d 0f56 0000  ....$...@&.m.V..                                                                        
0x7ffce2e95e48  979b 3486 947f 0000 0100 0000 0000 0000  ..4.............
0x7ffce2e95e58  285f e9e2 fc7f 0000 0080 0000 0100 0000  (_..............
0x7ffce2e95e68  fa25 106d 0f56 0000 0000 0000 0000 0000  .%.m.V..........
0x7ffce2e95e78  3681 8993 77cd 9fa8 f024 106d 0f56 0000  6...w....$.m.V..
0x7ffce2e95e88  205f e9e2 fc7f 0000 0000 0000 0000 0000   _..............
0x7ffce2e95e98  0000 0000 0000 0000 3681 a963 85d2 78fb  ........6..c..x.
0x7ffce2e95ea8  3681 97e9 3e1b a8fb 0000 0000 fc7f 0000  6...>...........
0x7ffce2e95eb8  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffce2e95ec8  3397 7286 947f 0000 38f6 7086 947f 0000  3.r.....8.p.....
0x7ffce2e95ed8  c1e6 0600 0000 0000 0000 0000 0000 0000  ................
0x7ffce2e95ee8  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffce2e95ef8  f024 106d 0f56 0000 205f e9e2 fc7f 0000  .$.m.V.. _......
0x7ffce2e95f08  1a25 106d 0f56 0000 185f e9e2 fc7f 0000  .%.m.V..._......
0x7ffce2e95f18  1c00 0000 0000 0000 0100 0000 0000 0000  ................
0x7ffce2e95f28  7f77 e9e2 fc7f 0000 0000 0000 0000 0000  .w..............
```

### What is the value of var_ch on the second iteration of the loop?
- **Answer:** 0
- **Steps to Reproduce:**

```bash
[0x560f6d10261c]> px @rbp-0xc
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffce2e95e34  0000 0000 0500 0000 2400 0000 4026 106d  ........$...@&.m                                                                        
0x7ffce2e95e44  0f56 0000 979b 3486 947f 0000 0100 0000  .V....4.........
0x7ffce2e95e54  0000 0000 285f e9e2 fc7f 0000 0080 0000  ....(_..........
0x7ffce2e95e64  0100 0000 fa25 106d 0f56 0000 0000 0000  .....%.m.V......
0x7ffce2e95e74  0000 0000 3681 8993 77cd 9fa8 f024 106d  ....6...w....$.m
0x7ffce2e95e84  0f56 0000 205f e9e2 fc7f 0000 0000 0000  .V.. _..........
0x7ffce2e95e94  0000 0000 0000 0000 0000 0000 3681 a963  ............6..c
0x7ffce2e95ea4  85d2 78fb 3681 97e9 3e1b a8fb 0000 0000  ..x.6...>.......
0x7ffce2e95eb4  fc7f 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffce2e95ec4  0000 0000 3397 7286 947f 0000 38f6 7086  ....3.r.....8.p.
0x7ffce2e95ed4  947f 0000 c1e6 0600 0000 0000 0000 0000  ................
0x7ffce2e95ee4  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffce2e95ef4  0000 0000 f024 106d 0f56 0000 205f e9e2  .....$.m.V.. _..
0x7ffce2e95f04  fc7f 0000 1a25 106d 0f56 0000 185f e9e2  .....%.m.V..._..
0x7ffce2e95f14  fc7f 0000 1c00 0000 0000 0000 0100 0000  ................
0x7ffce2e95f24  0000 0000 7f77 e9e2 fc7f 0000 0000 0000  .....w..........
```

### What is the value of var_8h at the end of the program?
- **Answer:** 2
- **Steps to Reproduce:**

```bash
[0x56306911761c]> px @rbp-0x8
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffd6d2fdf28  0200 0000 6c00 0000 4076 1169 3056 0000  ....l...@v.i0V..                                                                        
0x7ffd6d2fdf38  977b 0c15 3b7f 0000 0100 0000 0000 0000  .{..;...........
0x7ffd6d2fdf48  18e0 2f6d fd7f 0000 0080 0000 0100 0000  ../m............
0x7ffd6d2fdf58  fa75 1169 3056 0000 0000 0000 0000 0000  .u.i0V..........
0x7ffd6d2fdf68  e858 4309 eaa5 d24c f074 1169 3056 0000  .XC....L.t.i0V..
0x7ffd6d2fdf78  10e0 2f6d fd7f 0000 0000 0000 0000 0000  ../m............
0x7ffd6d2fdf88  0000 0000 0000 0000 e858 435b 97ad 481f  .........XC[..H.
0x7ffd6d2fdf98  e858 5d13 d05d c41e 0000 0000 fd7f 0000  .X]..]..........
0x7ffd6d2fdfa8  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd6d2fdfb8  3377 4a15 3b7f 0000 38d6 4815 3b7f 0000  3wJ.;...8.H.;...
0x7ffd6d2fdfc8  c182 0700 0000 0000 0000 0000 0000 0000  ................
0x7ffd6d2fdfd8  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd6d2fdfe8  f074 1169 3056 0000 10e0 2f6d fd7f 0000  .t.i0V..../m....
0x7ffd6d2fdff8  1a75 1169 3056 0000 08e0 2f6d fd7f 0000  .u.i0V..../m....
0x7ffd6d2fe008  1c00 0000 0000 0000 0100 0000 0000 0000  ................
0x7ffd6d2fe018  7ff7 2f6d fd7f 0000 0000 0000 0000 0000  ../m............
```

### What is the value of var_ch at the end of the program?
- **Answer:** 0
- **Steps to Reproduce:**

```bash
[0x56306911761c]> px @rbp-0xc
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffd6d2fdf24  0000 0000 0200 0000 6c00 0000 4076 1169  ........l...@v.i                                                                        
0x7ffd6d2fdf34  3056 0000 977b 0c15 3b7f 0000 0100 0000  0V...{..;.......
0x7ffd6d2fdf44  0000 0000 18e0 2f6d fd7f 0000 0080 0000  ....../m........
0x7ffd6d2fdf54  0100 0000 fa75 1169 3056 0000 0000 0000  .....u.i0V......
0x7ffd6d2fdf64  0000 0000 e858 4309 eaa5 d24c f074 1169  .....XC....L.t.i
0x7ffd6d2fdf74  3056 0000 10e0 2f6d fd7f 0000 0000 0000  0V..../m........
0x7ffd6d2fdf84  0000 0000 0000 0000 0000 0000 e858 435b  .............XC[
0x7ffd6d2fdf94  97ad 481f e858 5d13 d05d c41e 0000 0000  ..H..X]..]......
0x7ffd6d2fdfa4  fd7f 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd6d2fdfb4  0000 0000 3377 4a15 3b7f 0000 38d6 4815  ....3wJ.;...8.H.
0x7ffd6d2fdfc4  3b7f 0000 c182 0700 0000 0000 0000 0000  ;...............
0x7ffd6d2fdfd4  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd6d2fdfe4  0000 0000 f074 1169 3056 0000 10e0 2f6d  .....t.i0V..../m
0x7ffd6d2fdff4  fd7f 0000 1a75 1169 3056 0000 08e0 2f6d  .....u.i0V..../m
0x7ffd6d2fe004  fd7f 0000 1c00 0000 0000 0000 0100 0000  ................
0x7ffd6d2fe014  0000 0000 7ff7 2f6d fd7f 0000 0000 0000  ....../m........
```

---

## Crackme 1

### Analyzing crackme1

```bash
tryhackme@ip-10-10-239-255:~/crackme$ r2 -d crackme1
Process with PID 1428 started...
= attach 1428 1428
bin.baddr 0x55c9599d7000
Using 0x55c9599d7000
asm.bits 64
 -- What about taking a break? Here, have this nice 0xCC.
[0x7f3b337a8090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[Warning: Invalid range. Use different search.in=? or anal.in=dbg.maps.x
Warning: Invalid range. Use different search.in=? or anal.in=dbg.maps.x
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[TOFIX: aaft cant run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7f3b337a8090]> afl
0x55c9599d76f0    1 42           entry0
0x55c959bd7fe0    1 4124         reloc.__libc_start_main
0x55c9599d7720    4 50   -> 40   sym.deregister_tm_clones
0x55c9599d7760    4 66   -> 57   sym.register_tm_clones
0x55c9599d77b0    5 58   -> 51   entry.fini0
0x55c9599d76e0    1 6            sym..plt.got
0x55c9599d77f0    1 10           entry.init0
0x55c9599d7990    1 2            sym.__libc_csu_fini
0x55c9599d7994    1 9            sym._fini
0x55c9599d7920    4 101          sym.__libc_csu_init
0x55c9599d77fa   10 280          main
0x55c9599d7650    3 23           sym._init
0x55c9599d7680    1 6            sym.imp.puts
0x55c9599d7690    1 6            sym.imp.strlen
0x55c9599d76a0    1 6            sym.imp.__stack_chk_fail
0x55c9599d7000    2 25           map.home_tryhackme_crackme_crackme1.r_x
0x55c9599d76b0    1 6            sym.imp.strcmp
0x55c9599d76c0    1 6            sym.imp.strtok
0x55c9599d76d0    1 6            sym.imp.__isoc99_scanf
[0x7f3b337a8090]> pdf @main
/ (fcn) main 280
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_54h @ rbp-0x54
|           ; var int32_t var_50h @ rbp-0x50
|           ; var int32_t var_4ch @ rbp-0x4c
|           ; var int32_t var_48h @ rbp-0x48
|           ; var int32_t var_40h @ rbp-0x40
|           ; var int32_t var_38h @ rbp-0x38
|           ; var int32_t var_30h @ rbp-0x30
|           ; var int32_t var_28h @ rbp-0x28
|           ; var int32_t var_12h @ rbp-0x12
|           ; var int32_t var_8h @ rbp-0x8
|           ; arg int32_t arg_40h @ rbp+0x40
|           ; DATA XREF from entry0 (0x55c9599d770d)
|           0x55c9599d77fa      55             pushq %rbp
|           0x55c9599d77fb      4889e5         movq %rsp, %rbp
|           0x55c9599d77fe      4883ec60       subq $0x60, %rsp        ; '`'
|           0x55c9599d7802      64488b042528.  movq %fs:0x28, %rax     ; [0x28:8]=-1 ; '(' ; 40
|           0x55c9599d780b      488945f8       movq %rax, var_8h
|           0x55c9599d780f      31c0           xorl %eax, %eax
|           0x55c9599d7811      488d3d900100.  leaq str.enter_your_password, %rdi ; 0x55c9599d79a8 ; "enter your password"
|           0x55c9599d7818      e863feffff     callq sym.imp.puts      ; int puts(const char *s)
|           0x55c9599d781d      488d45ee       leaq var_12h, %rax
|           0x55c9599d7821      4889c6         movq %rax, %rsi
|           0x55c9599d7824      488d3d910100.  leaq 0x55c9599d79bc, %rdi ; "%s"
|           0x55c9599d782b      b800000000     movl $0, %eax
|           0x55c9599d7830      e89bfeffff     callq sym.imp.__isoc99_scanf ; int scanf(const char *format)
|           0x55c9599d7835      c745ac000000.  movl $0, var_54h
|           0x55c9599d783c      488d057c0100.  leaq 0x55c9599d79bf, %rax ; "127"
|           0x55c9599d7843      488945c0       movq %rax, var_40h
|           0x55c9599d7847      488d05750100.  leaq str.01., %rax      ; 0x55c9599d79c3 ; u"01.\u7257\u6e6f\u2067\u6150\u7373\u6f77\u6472\u5900\u756f\u7627\u2065\u6f67\u2074\u6874\u2065\u6f63\u7272\u6365\u2074\u6170\u7373\u6f77\u6472\u0100\u031b\u3c3b"                                      
|           0x55c9599d784e      488945c8       movq %rax, var_38h
|           0x55c9599d7852      488d056a0100.  leaq str.01., %rax      ; 0x55c9599d79c3 ; u"01.\u7257\u6e6f\u2067\u6150\u7373\u6f77\u6472\u5900\u756f\u7627\u2065\u6f67\u2074\u6874\u2065\u6f63\u7272\u6365\u2074\u6170\u7373\u6f77\u6472\u0100\u031b\u3c3b"                                      
|           0x55c9599d7859      488945d0       movq %rax, var_30h
|           0x55c9599d785d      488d05610100.  leaq 0x55c9599d79c5, %rax ; u"1.\u7257\u6e6f\u2067\u6150\u7373\u6f77\u6472\u5900\u756f\u7627\u2065\u6f67\u2074\u6874\u2065\u6f63\u7272\u6365\u2074\u6170\u7373\u6f77\u6472\u0100\u031b\u3c3b"                                                      
|           0x55c9599d7864      488945d8       movq %rax, var_28h
|           0x55c9599d7868      488d45ee       leaq var_12h, %rax
|           0x55c9599d786c      4889c7         movq %rax, %rdi
|           0x55c9599d786f      e81cfeffff     callq sym.imp.strlen    ; size_t strlen(const char *s)
|           0x55c9599d7874      8945b0         movl %eax, var_50h
|           0x55c9599d7877      488d45ee       leaq var_12h, %rax
|           0x55c9599d787b      488d35450100.  leaq 0x55c9599d79c7, %rsi ; "."
|           0x55c9599d7882      4889c7         movq %rax, %rdi
|           0x55c9599d7885      e836feffff     callq sym.imp.strtok    ; char *strtok(char *s1, const char *s2)
|           0x55c9599d788a      488945b8       movq %rax, var_48h
|       ,=< 0x55c9599d788e      eb4e           jmp 0x55c9599d78de
|      .--> 0x55c9599d7890      8b45ac         movl var_54h, %eax
|      :|   0x55c9599d7893      4898           cltq
|      :|   0x55c9599d7895      488b54c5c0     movq -0x40(%rbp, %rax, 8), %rdx
|      :|   0x55c9599d789a      488b45b8       movq var_48h, %rax
|      :|   0x55c9599d789e      4889d6         movq %rdx, %rsi
|      :|   0x55c9599d78a1      4889c7         movq %rax, %rdi
|      :|   0x55c9599d78a4      e807feffff     callq sym.imp.strcmp    ; int strcmp(const char *s1, const char *s2)
|      :|   0x55c9599d78a9      8945b4         movl %eax, var_4ch
|      :|   0x55c9599d78ac      8345ac01       addl $1, var_54h
|      :|   0x55c9599d78b0      837db400       cmpl $0, var_4ch
|     ,===< 0x55c9599d78b4      7413           je 0x55c9599d78c9
|     |:|   0x55c9599d78b6      488d3d0c0100.  leaq 0x55c9599d79c9, %rdi ; "Wrong Password"
|     |:|   0x55c9599d78bd      e8befdffff     callq sym.imp.puts      ; int puts(const char *s)
|     |:|   0x55c9599d78c2      b8ffffffff     movl $0xffffffff, %eax  ; -1
|    ,====< 0x55c9599d78c7      eb33           jmp 0x55c9599d78fc
|    |`---> 0x55c9599d78c9      488d35f70000.  leaq 0x55c9599d79c7, %rsi ; "."
|    | :|   0x55c9599d78d0      bf00000000     movl $0, %edi
|    | :|   0x55c9599d78d5      e8e6fdffff     callq sym.imp.strtok    ; char *strtok(char *s1, const char *s2)
|    | :|   0x55c9599d78da      488945b8       movq %rax, var_48h
|    | :|   ; CODE XREF from main (0x55c9599d788e)
|    | :`-> 0x55c9599d78de      48837db800     cmpq $0, var_48h
|    | :,=< 0x55c9599d78e3      7406           je 0x55c9599d78eb
|    | :|   0x55c9599d78e5      837dac03       cmpl $3, var_54h
|    | `==< 0x55c9599d78e9      7ea5           jle 0x55c9599d7890
|    |  `-> 0x55c9599d78eb      488d3de60000.  leaq str.You_ve_got_the_correct_password, %rdi ; 0x55c9599d79d8 ; "You've got the correct password"                                                                                                                                                
|    |      0x55c9599d78f2      e889fdffff     callq sym.imp.puts      ; int puts(const char *s)
|    |      0x55c9599d78f7      b800000000     movl $0, %eax
|    |      ; CODE XREF from main (0x55c9599d78c7)
|    `----> 0x55c9599d78fc      488b4df8       movq var_8h, %rcx
|           0x55c9599d7900      6448330c2528.  xorq %fs:0x28, %rcx
|       ,=< 0x55c9599d7909      7405           je 0x55c9599d7910
|       |   0x55c9599d790b      e890fdffff     callq sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
|       `-> 0x55c9599d7910      c9             leave
\           0x55c9599d7911      c3             retq
```


### What is the password?
- **Answer:** 127.0.0.1
- **Steps to Reproduce:** 

```bash
0x563362eb283c      488d057c0100.  leaq 0x563362eb29bf, %rax ; rsi ; "127"
|           0x563362eb2843      488945c0       movq %rax, var_40h
|           0x563362eb2847      488d05750100.  leaq str.01., %rax      ; 0x563362eb29c3 ; u"01.\u7257\u6e6f\u2067\u6150\u7373\u6f77\u6472\u5900\u756f\u7627\u2065\u6f67\u2074\u6874\u2065\u6f63\u7272\u6365\u2074\u6170\u7373\u6f77\u6472\u0100\u031b\u3c3b"                                      
|           0x563362eb284e      488945c8       movq %rax, var_38h
|           0x563362eb2852      488d056a0100.  leaq str.01., %rax      ; 0x563362eb29c3 ; u"01.\u7257\u6e6f\u2067\u6150\u7373\u6f77\u6472\u5900\u756f\u7627\u2065\u6f67\u2074\u6874\u2065\u6f63\u7272\u6365\u2074\u6170\u7373\u6f77\u6472\u0100\u031b\u3c3b"                                      
|           0x563362eb2859      488945d0       movq %rax, var_30h
|           0x563362eb285d      488d05610100.  leaq 0x563362eb29c5, %rax ; u"1.\u7257\u6e6f\u2067\u6150\u7373\u6f77\u6472\u5900\u756f\u7627\u2065\u6f67\u2074\u6874\u2065\u6f63\u7272\u6365\u2074\u6170\u7373\u6f77\u6472\u0100\u031b\u3c3b"
```

---

## Crackme 2

### Analyzing crackme2

```bash
tryhackme@ip-10-10-239-255:~/crackme$ r2 -d crackme2
Process with PID 1571 started...
= attach 1571 1571
bin.baddr 0x55f3112a4000
Using 0x55f3112a4000
asm.bits 64
 -- We feed trolls
[0x7fa1f9f92090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[Warning: Invalid range. Use different search.in=? or anal.in=dbg.maps.x
Warning: Invalid range. Use different search.in=? or anal.in=dbg.maps.x
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[TOFIX: aaft cant run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7fa1f9f92090]> afl
0x55f3112a46f0    1 42           entry0
0x55f3114a4fe0    1 4124         reloc.__libc_start_main
0x55f3112a4720    4 50   -> 40   sym.deregister_tm_clones
0x55f3112a4760    4 66   -> 57   sym.register_tm_clones
0x55f3112a47b0    5 58   -> 51   entry.fini0
0x55f3112a46e0    1 6            sym..plt.got
0x55f3112a47f0    1 10           entry.init0
0x55f3112a4990    1 2            sym.__libc_csu_fini
0x55f3112a4994    1 9            sym._fini
0x55f3112a4920    4 101          sym.__libc_csu_init
0x55f3112a47fa   12 283          main
0x55f3112a4650    3 23           sym._init
0x55f3112a4680    1 6            sym.imp.puts
0x55f3112a4690    1 6            sym.imp.fread
0x55f3112a46a0    1 6            sym.imp.strlen
0x55f3112a46b0    1 6            sym.imp.__stack_chk_fail
0x55f3112a4000    2 25           map.home_tryhackme_crackme_crackme2.r_x
0x55f3112a46c0    1 6            sym.imp.fopen
0x55f3112a46d0    1 6            sym.imp.__isoc99_scanf
[0x7fa1f9f92090]> pdf @main
/ (fcn) main 283
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_44h @ rbp-0x44
|           ; var int32_t var_40h @ rbp-0x40
|           ; var int32_t var_3ch @ rbp-0x3c
|           ; var int32_t var_38h @ rbp-0x38
|           ; var int32_t var_2eh @ rbp-0x2e
|           ; var int32_t var_23h @ rbp-0x23
|           ; var int32_t var_18h @ rbp-0x18
|           ; DATA XREF from entry0 (0x55f3112a470d)
|           0x55f3112a47fa      55             pushq %rbp
|           0x55f3112a47fb      4889e5         movq %rsp, %rbp
|           0x55f3112a47fe      53             pushq %rbx
|           0x55f3112a47ff      4883ec48       subq $0x48, %rsp        ; 'H'
|           0x55f3112a4803      64488b042528.  movq %fs:0x28, %rax     ; [0x28:8]=-1 ; '(' ; 40
|           0x55f3112a480c      488945e8       movq %rax, var_18h
|           0x55f3112a4810      31c0           xorl %eax, %eax
|           0x55f3112a4812      488d358f0100.  leaq 0x55f3112a49a8, %rsi ; "r"
|           0x55f3112a4819      488d3d900100.  leaq str.home_tryhackme_install_files_secret.txt, %rdi ; 0x55f3112a49b0 ; "/home/tryhackme/install-files/secret.txt"                                                                                                                               
|           0x55f3112a4820      e89bfeffff     callq sym.imp.fopen     ; file*fopen(const char *filename, const char *mode)
|           0x55f3112a4825      488945c8       movq %rax, var_38h
|           0x55f3112a4829      488b55c8       movq var_38h, %rdx
|           0x55f3112a482d      488d45d2       leaq var_2eh, %rax
|           0x55f3112a4831      4889d1         movq %rdx, %rcx
|           0x55f3112a4834      ba0b000000     movl $0xb, %edx         ; 11
|           0x55f3112a4839      be01000000     movl $1, %esi
|           0x55f3112a483e      4889c7         movq %rax, %rdi
|           0x55f3112a4841      e84afeffff     callq sym.imp.fread     ; size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream)
|           0x55f3112a4846      8945c4         movl %eax, var_3ch
|           0x55f3112a4849      837dc400       cmpl $0, var_3ch
|       ,=< 0x55f3112a484d      7916           jns 0x55f3112a4865
|       |   0x55f3112a484f      488d3d830100.  leaq str.Error_Reading_File, %rdi ; 0x55f3112a49d9 ; "Error Reading File"
|       |   0x55f3112a4856      e825feffff     callq sym.imp.puts      ; int puts(const char *s)
|       |   0x55f3112a485b      b8ffffffff     movl $0xffffffff, %eax  ; -1
|      ,==< 0x55f3112a4860      e995000000     jmp 0x55f3112a48fa
|      |`-> 0x55f3112a4865      488d3d800100.  leaq str.Please_enter_password, %rdi ; 0x55f3112a49ec ; "Please enter password"
|      |    0x55f3112a486c      e80ffeffff     callq sym.imp.puts      ; int puts(const char *s)
|      |    0x55f3112a4871      488d45dd       leaq var_23h, %rax
|      |    0x55f3112a4875      4889c6         movq %rax, %rsi
|      |    0x55f3112a4878      488d3d830100.  leaq str.11s, %rdi      ; 0x55f3112a4a02 ; "%11s"
|      |    0x55f3112a487f      b800000000     movl $0, %eax
|      |    0x55f3112a4884      e847feffff     callq sym.imp.__isoc99_scanf ; int scanf(const char *format)
|      |    0x55f3112a4889      c745bc090000.  movl $9, var_44h
|      |    0x55f3112a4890      c745c0000000.  movl $0, var_40h
|      |,=< 0x55f3112a4897      eb33           jmp 0x55f3112a48cc
|     .---> 0x55f3112a4899      8b45bc         movl var_44h, %eax
|     :||   0x55f3112a489c      4898           cltq
|     :||   0x55f3112a489e      0fb65405d2     movzbl -0x2e(%rbp, %rax), %edx
|     :||   0x55f3112a48a3      8b45c0         movl var_40h, %eax
|     :||   0x55f3112a48a6      4898           cltq
|     :||   0x55f3112a48a8      0fb64405dd     movzbl -0x23(%rbp, %rax), %eax
|     :||   0x55f3112a48ad      38c2           cmpb %al, %dl
|    ,====< 0x55f3112a48af      7413           je 0x55f3112a48c4
|    |:||   0x55f3112a48b1      488d3d4f0100.  leaq str.Wrong_Password, %rdi ; 0x55f3112a4a07 ; "Wrong Password"
|    |:||   0x55f3112a48b8      e8c3fdffff     callq sym.imp.puts      ; int puts(const char *s)
|    |:||   0x55f3112a48bd      b8ffffffff     movl $0xffffffff, %eax  ; -1
|   ,=====< 0x55f3112a48c2      eb36           jmp 0x55f3112a48fa
|   |`----> 0x55f3112a48c4      836dbc01       subl $1, var_44h
|   | :||   0x55f3112a48c8      8345c001       addl $1, var_40h
|   | :||   ; CODE XREF from main (0x55f3112a4897)
|   | :|`-> 0x55f3112a48cc      837dbc00       cmpl $0, var_44h
|   | :|,=< 0x55f3112a48d0      7e17           jle 0x55f3112a48e9
|   | :||   0x55f3112a48d2      8b45c0         movl var_40h, %eax
|   | :||   0x55f3112a48d5      4863d8         movslq %eax, %rbx
|   | :||   0x55f3112a48d8      488d45dd       leaq var_23h, %rax
|   | :||   0x55f3112a48dc      4889c7         movq %rax, %rdi
|   | :||   0x55f3112a48df      e8bcfdffff     callq sym.imp.strlen    ; size_t strlen(const char *s)
|   | :||   0x55f3112a48e4      4839c3         cmpq %rax, %rbx
|   | `===< 0x55f3112a48e7      72b0           jb 0x55f3112a4899
|   |  |`-> 0x55f3112a48e9      488d3d260100.  leaq str.Correct_Password, %rdi ; 0x55f3112a4a16 ; "Correct Password"
|   |  |    0x55f3112a48f0      e88bfdffff     callq sym.imp.puts      ; int puts(const char *s)
|   |  |    0x55f3112a48f5      b800000000     movl $0, %eax
|   |  |    ; CODE XREFS from main (0x55f3112a4860, 0x55f3112a48c2)
|   `--`--> 0x55f3112a48fa      488b4de8       movq var_18h, %rcx
|           0x55f3112a48fe      6448330c2528.  xorq %fs:0x28, %rcx
|       ,=< 0x55f3112a4907      7405           je 0x55f3112a490e
|       |   0x55f3112a4909      e8a2fdffff     callq sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
|       `-> 0x55f3112a490e      4883c448       addq $0x48, %rsp        ; 'H'
|           0x55f3112a4912      5b             popq %rbx
|           0x55f3112a4913      5d             popq %rbp
\           0x55f3112a4914      c3             retq
```

### What is the correct password?
- **Answer:** dwperuc3sv
- **Steps to Reproduce:** 


```bash
0x55f3112a4819      488d3d900100.  leaq str.home_tryhackme_install_files_secret.txt, %rdi ; 0x55f3112a49b0 ; "/home/tryhackme/install-files/secret.txt"
```

- Reading the `secret.txt`:

```bash
tryhackme@ip-10-10-26-167:~/crackme$ cat /home/tryhackme/install-files/secret.txt
vs3curepwd
```

- The password is checked in reverse. So, reverse the secret!

---

## References

- [Radare2 - Github](https://github.com/radareorg/radare2)
- [AT&T vs Intel Syntax](http://web.mit.edu/rhel-doc/3/rhel-as-en-3/i386-syntax.html)
- Cheatsheets:
    - [Radare2 Docs](https://github.com/radareorg/radare2/blob/master/doc/intro.md)
    - [Yet another radare2 cheatsheet](https://gist.github.com/williballenthin/6857590dab3e2a6559d7)
    - [Learning Radare in Practice](https://web.archive.org/web/20180312191821/http://www.radare.org/get/THC2018.pdf)
