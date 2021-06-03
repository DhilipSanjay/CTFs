# CC: Radare2

**Date:** 02, June, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/ccradare2) to go to the TryHackMe room.

## Command Line Options

### What flag to you set to analyze the binary upon entering the r2 console (equivalent to running aaa once your inside the console)     
- **Answer:** -A

### How do you enable the debugger?
- **Answer:** -d

### How do you open the file in write mode?
- **Answer:** -w 

### How do you enter the console without opening a file?
- **Answer:** -

---

## Analyzation

### What command "Analyzes Everything" (all functions and their arguments: Same as running with radare with -A)
- **Answer:** aaa

### What command does basic analysis on functions?
- **Answer:** af

### How do you list all functions?
- **Answer:** afl

### How many functions are in the example1 binary?
- **Answer:** 12
- **Steps to Reproduce:** 

```bash
[0x00000530]> afl
0x00000530    1 42           entry0
0x00000560    4 50   -> 44   sym.deregister_tm_clones
0x000005a0    4 66   -> 57   sym.register_tm_clones
0x000005f0    5 50           sym.__do_global_dtors_aux
0x00000520    1 6            sym.imp.__cxa_finalize
0x00000630    4 48   -> 42   entry.init0
0x000004f8    3 23           sym._init
0x000006e0    1 1            sym.__libc_csu_fini
0x000006e4    1 9            sym._fini
0x0000066b    1 7            sym.secret_func
0x00000680    4 93           sym.__libc_csu_init
0x00000660    1 11           main
0x00000000    3 97   -> 123  loc.imp._ITM_deregisterTMCloneTable
```

### What is the name of the secret function in the example1 binary?
- **Answer:** secret_func

---

## Information

### What command shows all the information about the file that you're in?
- **Answer:** ia

### How do you get every string that is present in the binary? 
- **Answer:** izz

### What if you want the address of the main function?
- **Answer:** im

### What character do you add to the end of every command to get the output in JSON format?
- **Answer:** j

### How do you get the entrypoint of the file?
- **Answer:** ie

### What is the secret string hidden in the example2 binary?
- **Answer:** goodjob
- **Steps to Reproduce:** 

```bash
[0x00000530]> izz
[Strings]
nth paddr      vaddr      len size section            type    string
――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x00000034 0x00000034 4   10                      utf16le @8\t@
1   0x00000238 0x00000238 27  28   .interp            ascii   /lib64/ld-linux-x86-64.so.2
[..snip..]
80  0x00002148 0x00000882 8   8                       ascii   goodjob\n
```

---

## Navigating Through Memory

### How do you print out the the current memory address your located at in the binary?
- **Answer:** s

### What command do you use to go to a specific point in memory with the syntax <command> <address>?
- **Answer:** s

### What command would you run to go 5 bytes forward?
- **Answer:** s+5

### What about 12 bytes backward?
- **Answer:** s-12

### How do you undo the previous seek?
- **Answer:** s-

### How would go to the memory address of the main function?
- **Answer:** s main

### What if you wanted to go to the address of the rax register?
- **Answer:** sr rax

---

## Printing

### How would you print the hex output of where you currently are in memory?
- **Answer:** px

### How would you print the disassembly of where you're currently at in memory?
- **Answer:** pd

### What if you wanted the disassembly of the main function?
- **Answer:** pdf @ main

### What command prints out the emoji hexdump? (this is not useful at all I just find it funny)
- **Answer:** pxe

### What if you decided you were too good for rows and you wanted the disassembly in column format?
- **Answer:** pC

### What is the value of the first variable in the main function for the example 3 binary?
- **Answer:** 1
- **Steps to Reproduce:** 
    - Value `1` is being moved to `var_4h`:

```bash
0x00000530]> pdf @main
            ; DATA XREF from entry0 @ 0x54d
┌ 25: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_8h @ rbp-0x8
│           ; var int64_t var_4h @ rbp-0x4
│           0x00000660      55             pushq %rbp
│           0x00000661      4889e5         movq %rsp, %rbp
│           0x00000664      c745fc010000.  movl $1, var_4h
│           0x0000066b      c745f8050000.  movl $5, var_8h
│           0x00000672      b800000000     movl $0, %eax
│           0x00000677      5d             popq %rbp
└           0x00000678      c3             retq
```

### What about the second variable?
- **Answer:** 5
- **Steps to Reproduce:** Value `5` is being moved to `var_8h`

---

## The Mid-term

### How many functions are in the binary?
- **Answer:** 13
- **Steps to Reproduce:** 

```bash
[0x00000530]> afl
0x00000530    1 42           entry0
0x00000560    4 50   -> 44   sym.deregister_tm_clones
0x000005a0    4 66   -> 57   sym.register_tm_clones
0x000005f0    5 50           sym.__do_global_dtors_aux
0x00000520    1 6            sym.imp.__cxa_finalize
0x00000630    4 48   -> 42   entry.init0
0x000004f8    3 23           sym._init
0x000006f0    1 1            sym.__libc_csu_fini
0x00000679    1 7            sym.midterm_func
0x000006f4    1 9            sym._fini
0x00000680    1 11           sym.secret_func
0x00000690    4 93           sym.__libc_csu_init
0x00000660    1 25           main
0x00000000    3 97   -> 123  loc.imp._ITM_deregisterTMCloneTable
```

### What is the value of the hidden string?
- **Answer:** you_found_me
- **Steps to Reproduce:** 

```bash
[0x00000530]> izz
[Strings]
nth paddr      vaddr      len size section   type    string
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x00000034 0x00000034 4   10             utf16le @8\t@
1   0x00000238 0x00000238 27  28   .interp   ascii   /lib64/ld-linux-x86-64.so.2
2   0x00000361 0x00000361 9   10   .dynstr   ascii   libc.so.6
[..snip..]
82  0x00002178 0x00000887 14  14             ascii   \nyou_found_me\n
```

### What is the return value of secret_func()?
- **Answer:** 4
- **Steps to Reproduce:** 

```bash
[0x00000530]> pdf @ sym.secret_func
┌ 11: sym.secret_func ();
│           0x00000680      55             pushq %rbp
│           0x00000681      4889e5         movq %rsp, %rbp
│           0x00000684      b804000000     movl $4, %eax
│           0x00000689      5d             popq %rbp
└           0x0000068a      c3             retq
```

### What is the value of the first variable set in the main function(in decimal format)?
- **Answer:** 12
- **Steps to Reproduce:** Hex `0xc` -> Decimal `12`

```bash
[0x00000530]> pdf @main
            ; DATA XREF from entry0 @ 0x54d
┌ 25: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_8h @ rbp-0x8
│           ; var int64_t var_4h @ rbp-0x4
│           0x00000660      55             pushq %rbp
│           0x00000661      4889e5         movq %rsp, %rbp
│           0x00000664      c745fc0c0000.  movl $0xc, var_4h
│           0x0000066b      c745f8c00000.  movl $0xc0, var_8h
│           0x00000672      b800000000     movl $0, %eax
│           0x00000677      5d             popq %rbp
└           0x00000678      c3             retq
```

### What about the second one(also in decimal format)?
- **Answer:** 192
- **Steps to Reproduce:** Hex `0xc0` -> Decimal `192`

### What is the next function in memory after the main function?
- **Answer:** midterm_func
- **Steps to Reproduce:** 
    - Look at the next memory after main function.
    - First column denotes the memory locations
    - After `0x00000660` (main), the next function location is `0x00000679` (sym.midterm_func)

```bash
[0x00000530]> afl
0x00000530    1 42           entry0
0x00000560    4 50   -> 44   sym.deregister_tm_clones
0x000005a0    4 66   -> 57   sym.register_tm_clones
0x000005f0    5 50           sym.__do_global_dtors_aux
0x00000520    1 6            sym.imp.__cxa_finalize
0x00000630    4 48   -> 42   entry.init0
0x000004f8    3 23           sym._init
0x000006f0    1 1            sym.__libc_csu_fini
0x00000679    1 7            sym.midterm_func
0x000006f4    1 9            sym._fini
0x00000680    1 11           sym.secret_func
0x00000690    4 93           sym.__libc_csu_init
0x00000660    1 25           main
0x00000000    3 97   -> 123  loc.imp._ITM_deregisterTMCloneTable
```

### How do you get a hexdump of four bytes of the memory address your currently at?
- **Answer:** px 4

---

## Debugging

### How do you set a breakpoint?
- **Answer:** db

### What command is used to print out the values of all the registers?
- **Answer:** dr

### How do you run through the program until the program either ends or you hit the next breakpoint?
- **Answer:** dc

### What if you want to step through the binary one line at a time?
- **Answer:** ds

### How do you go forth 2 lines in the binary?
- **Answer:** ds 2

### How do you list out the indexes and memory addresses of all breakpoints?
- **Answer:** dbi

---

## Visual mode

### How do you enter "graph mode" which allows everything to be organized in nice readable boxes
- **Answer:** vV

### What character do you press to run normal radare commands inside visual mode?
- **Answer:** :

### How do you go back to the regular radare shell(leaving visual mode)?
- **Answer:** q

### What if you want to step through the binary inside Visual mode?
- **Answer:** s

### How do you add a comment?
- **Answer:** ;

---

## Write Mode

### How do you write a string to the current memory address.
- **Answer:** w

### What command lists all write changes?
- **Answer:** wc

### What command modifies an instruction at the current memory address?
- **Answer:** wa

---

## The Final Exam

### What is the password that outputs the you win! message?
- **Answer:** oekZ_Z_j
- **Steps to Reproduce:** 
    - If `youdidit` was compared with `youdidit`, then the message must be printed.
    - But the value in the user input seems to be changed at the breakpoint (at strcmp)

    ```bash
    [0x7f9a10920090]> pdf @main
                ; DATA XREF from entry0 @ 0x559ffa2006bd
    ┌ 102: int main (int argc, char **argv, char **envp);
    │           ; var int64_t var_11h @ rbp-0x11
    │           ; var int64_t var_8h @ rbp-0x8
    │           0x559ffa200835      55             push rbp
    │           0x559ffa200836      4889e5         mov rbp, rsp
    │           0x559ffa200839      4883ec20       sub rsp, 0x20
    │           0x559ffa20083d      488b150c0820.  mov rdx, qword [reloc.stdin] ; [0x559ffa401050:8]=0
    │           0x559ffa200844      488d45ef       lea rax, [var_11h]
    │           0x559ffa200848      be09000000     mov esi, 9
    │           0x559ffa20084d      4889c7         mov rdi, rax
    │           0x559ffa200850      e80bfeffff     call sym.imp.fgets      ; char *fgets(char *s, int size, FILE *stream)
    │           0x559ffa200855      488d45ef       lea rax, [var_11h]
    │           0x559ffa200859      4889c7         mov rdi, rax
    │           0x559ffa20085c      e86fffffff     call sym.get_password
    │           0x559ffa200861      488945f8       mov qword [var_8h], rax
    │           0x559ffa200865      488b45f8       mov rax, qword [var_8h]
    │           0x559ffa200869      488d35a40000.  lea rsi, str.youdidit   ; 0x559ffa200914 ; "youdidit"
    │           0x559ffa200870      4889c7         mov rdi, rax
    │           0x559ffa200873      e8f8fdffff     call sym.imp.strcmp     ; int strcmp(const char *s1, const char *s2)
    │           0x559ffa200878      85c0           test eax, eax
    │       ┌─< 0x559ffa20087a      7518           jne 0x559ffa200894
    │       │   0x559ffa20087c      488d359a0000.  lea rsi, str.You_win_   ; 0x559ffa20091d ; "You win!"
    │       │   0x559ffa200883      488d3d9c0000.  lea rdi, [0x559ffa200926] ; "%s"
    │       │   0x559ffa20088a      b800000000     mov eax, 0
    │       │   0x559ffa20088f      e8bcfdffff     call sym.imp.printf     ; int printf(const char *format)
    │       └─> 0x559ffa200894      b800000000     mov eax, 0
    │           0x559ffa200899      c9             leave
    └           0x559ffa20089a      c3             ret
    [0x7f9a10920090]> db 0x559ffa200873
    [0x7f9a10920090]> dc
    youdidit
    hit breakpoint at: 0x559ffa200873
    [0x559ffa200873]> dr
    rax = 0x559ffac296b0
    rbx = 0x00000000
    rcx = 0x559ffac296c0
    rdx = 0x559ffac296b7
    r8 = 0x559ffac296b0
    r9 = 0x7f9a108fbbe0
    r10 = 0xfffffffffffff28d
    r11 = 0x00000020
    r12 = 0x559ffa2006a0
    r13 = 0x00000000
    r14 = 0x00000000
    r15 = 0x00000000
    rsi = 0x559ffa200914
    rdi = 0x559ffac296b0
    rsp = 0x7ffd5d05f350
    rbp = 0x7ffd5d05f370
    rip = 0x559ffa200873
    rflags = 0x00000202
    orax = 0xffffffffffffffff
    ```

    - Registers having different value (even on entering `youdidit`) as input:

    ```bash
    [0x559ffa200873]> px 8 @ 0x559ffa200914
    - offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
    0x559ffa200914  796f 7564 6964 6974                      youdidit                                                                     
    [0x559ffa200873]> px 8 @ 0x559ffac296b0
    - offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
    0x559ffac296b0  8379 7f6e 736e 737e                      .y.nsns~ 
    ```

    - By performing hex subtraction:

    ```bash
    83797f6e736e737e − 796f756469646974 = a0a0a0a0a0a0a0a
    ```
    
    - So we need to subtract the value `a0a0a0a0a0a0a0a` from the original hex value of `youdidit`:

    ```bash
    796f756469646974 − a0a0a0a0a0a0a0a = 6f656b5a5f5a5f6a
    ```
    
    - Convert the hex value `6f656b5a5f5a5f6a` to ascii:

    ```bash
    $ echo 6f656b5a5f5a5f6a | xxd -r -p
    oekZ_Z_j
    ```
    
    - Run the binary to verify the password:

    ```bash
    $ ./the_final_exam 
    oekZ_Z_j
    You win!
    ```
    
---