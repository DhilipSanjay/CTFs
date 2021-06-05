# Reverse Engineering

**Date:** 05, June, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/reverseengineering) to go to the TryHackMe room.


## Crackme 1

- Analyse the binary's main function using `r2`:

```bash
$ r2 -d crackme1.bin 
Process with PID 1586 started...
= attach 1586 1586
bin.baddr 0x557057800000
Using 0x557057800000                                                                                                                             
asm.bits 64
[0x7f8d71014090]> e asm.syntax=att
[0x7f8d71014090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for vtables
[TOFIX: aaft can't run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7f8d71014090]> afl
0x557057800660    1 42           entry0
0x557057a00fe0    1 4124         reloc.__libc_start_main
0x557057800690    4 50   -> 40   sym.deregister_tm_clones
0x5570578006d0    4 66   -> 57   sym.register_tm_clones
0x557057800720    5 58   -> 51   sym.__do_global_dtors_aux
0x557057800650    1 6            sym.imp.__cxa_finalize
0x557057800760    1 10           entry.init0
0x557057800880    1 2            sym.__libc_csu_fini
0x557057800884    1 9            sym._fini
0x557057800810    4 101          sym.__libc_csu_init
0x55705780076a    6 165          main
0x5570578005e0    3 23           sym._init
0x557057800610    1 6            sym.imp.puts
0x557057800620    1 6            sym.imp.__stack_chk_fail
0x557057800000    6 292  -> 318  map._root_Desktop_CTF_TryHackMe_ReverseEngineering_crackme1.bin.r_x
0x557057800630    1 6            sym.imp.strcmp
0x557057800640    1 6            sym.imp.__isoc99_scanf
[0x7f8d71014090]> pdf @ main
            ; DATA XREF from entry0 @ 0x55705780067d
┌ 165: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_18h @ rbp-0x18
│           ; var int64_t var_14h @ rbp-0x14
│           ; var int64_t var_10h @ rbp-0x10
│           ; var int64_t var_eh @ rbp-0xe
│           ; var int64_t var_8h @ rbp-0x8
│           0x55705780076a      55             pushq %rbp
│           0x55705780076b      4889e5         movq %rsp, %rbp
│           0x55705780076e      4883ec20       subq $0x20, %rsp
│           0x557057800772      64488b042528.  movq %fs:0x28, %rax
│           0x55705780077b      488945f8       movq %rax, var_8h
│           0x55705780077f      31c0           xorl %eax, %eax
│           0x557057800781      488d3d0c0100.  leaq str.enter_password, %rdi ; 0x557057800894 ; "enter password"
│           0x557057800788      e883feffff     callq sym.imp.puts      ; int puts(const char *s)
│           0x55705780078d      8b053d010000   movl str.hax0r, %eax    ; [0x5570578008d0:4]=0x30786168 ; "hax0r"
│           0x557057800793      8945ec         movl %eax, var_14h
│           0x557057800796      0fb705370100.  movzwl 0x5570578008d4, %eax ; [0x5570578008d4:2]=114
│           0x55705780079d      668945f0       movw %ax, var_10h
│           0x5570578007a1      488d45f2       leaq var_eh, %rax
│           0x5570578007a5      4889c6         movq %rax, %rsi
│           0x5570578007a8      488d3df40000.  leaq 0x5570578008a3, %rdi ; "%s"
│           0x5570578007af      b800000000     movl $0, %eax
│           0x5570578007b4      e887feffff     callq sym.imp.__isoc99_scanf ; int scanf(const char *format)
│           0x5570578007b9      488d55ec       leaq var_14h, %rdx
│           0x5570578007bd      488d45f2       leaq var_eh, %rax
│           0x5570578007c1      4889d6         movq %rdx, %rsi
│           0x5570578007c4      4889c7         movq %rax, %rdi
│           0x5570578007c7      e864feffff     callq sym.imp.strcmp    ; int strcmp(const char *s1, const char *s2)
│           0x5570578007cc      8945e8         movl %eax, var_18h
│           0x5570578007cf      837de800       cmpl $0, var_18h
│       ┌─< 0x5570578007d3      7513           jne 0x5570578007e8
│       │   0x5570578007d5      488d3dca0000.  leaq str.password_is_correct, %rdi ; 0x5570578008a6 ; "password is correct"
│       │   0x5570578007dc      e82ffeffff     callq sym.imp.puts      ; int puts(const char *s)
│       │   0x5570578007e1      b800000000     movl $0, %eax
│      ┌──< 0x5570578007e6      eb11           jmp 0x5570578007f9
│      │└─> 0x5570578007e8      488d3dcb0000.  leaq str.password_is_incorrect, %rdi ; 0x5570578008ba ; "password is incorrect"
│      │    0x5570578007ef      e81cfeffff     callq sym.imp.puts      ; int puts(const char *s)
│      │    0x5570578007f4      b800000000     movl $0, %eax
│      │    ; CODE XREF from main @ 0x5570578007e6
│      └──> 0x5570578007f9      488b4df8       movq var_8h, %rcx
│           0x5570578007fd      6448330c2528.  xorq %fs:0x28, %rcx
│       ┌─< 0x557057800806      7405           je 0x55705780080d
│       │   0x557057800808      e813feffff     callq sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       └─> 0x55705780080d      c9             leave
└           0x55705780080e      c3             retq
```

- The password is `hax0r`, which is being used in `strcmp`:

```bash
$ ./crackme1.bin 

enter password
hax0r
password is correct
```

---

## Crackme 2

- Analyse the binary's main function using `r2`:

```bash
r2 -d crackme2.bin 
Process with PID 1613 started...
= attach 1613 1613
bin.baddr 0x562627400000
Using 0x562627400000
asm.bits 64
[0x7f0470e5c090]> e asm.syntax=att
[0x7f0470e5c090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for vtables
[TOFIX: aaft can't run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7f0470e5c090]> afl
0x562627400610    1 42           entry0
0x562627600fe0    1 4124         reloc.__libc_start_main
0x562627400640    4 50   -> 40   sym.deregister_tm_clones
0x562627400680    4 66   -> 57   sym.register_tm_clones
0x5626274006d0    5 58   -> 51   sym.__do_global_dtors_aux
0x562627400600    1 6            sym.imp.__cxa_finalize
0x562627400710    1 10           entry.init0
0x562627400810    1 2            sym.__libc_csu_fini
0x562627400814    1 9            sym._fini
0x5626274007a0    4 101          sym.__libc_csu_init
0x56262740071a    6 122          main
0x5626274005a0    3 23           sym._init
0x5626274005d0    1 6            sym.imp.puts
0x5626274005e0    1 6            sym.imp.__stack_chk_fail
0x562627400000    2 25           map._root_Desktop_CTF_TryHackMe_ReverseEngineering_crackme2.bin.r_x
0x5626274005f0    1 6            sym.imp.__isoc99_scanf
[0x7f0470e5c090]> pdf @ main
            ; DATA XREF from entry0 @ 0x56262740062d
┌ 122: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_ch @ rbp-0xc
│           ; var int64_t var_8h @ rbp-0x8
│           0x56262740071a      55             pushq %rbp
│           0x56262740071b      4889e5         movq %rsp, %rbp
│           0x56262740071e      4883ec10       subq $0x10, %rsp
│           0x562627400722      64488b042528.  movq %fs:0x28, %rax
│           0x56262740072b      488945f8       movq %rax, var_8h
│           0x56262740072f      31c0           xorl %eax, %eax
│           0x562627400731      488d3dec0000.  leaq str.enter_your_password, %rdi ; 0x562627400824 ; "enter your password"
│           0x562627400738      e893feffff     callq sym.imp.puts      ; int puts(const char *s)
│           0x56262740073d      488d45f4       leaq var_ch, %rax
│           0x562627400741      4889c6         movq %rax, %rsi
│           0x562627400744      488d3ded0000.  leaq 0x562627400838, %rdi ; "%d"
│           0x56262740074b      b800000000     movl $0, %eax
│           0x562627400750      e89bfeffff     callq sym.imp.__isoc99_scanf ; int scanf(const char *format)
│           0x562627400755      8b45f4         movl var_ch, %eax
│           0x562627400758      3d7c130000     cmpl $0x137c, %eax
│       ┌─< 0x56262740075d      750e           jne 0x56262740076d
│       │   0x56262740075f      488d3dd50000.  leaq str.password_is_valid, %rdi ; 0x56262740083b ; "password is valid"
│       │   0x562627400766      e865feffff     callq sym.imp.puts      ; int puts(const char *s)
│      ┌──< 0x56262740076b      eb0c           jmp 0x562627400779
│      │└─> 0x56262740076d      488d3dd90000.  leaq str.password_is_incorrect, %rdi ; 0x56262740084d ; "password is incorrect"
│      │    0x562627400774      e857feffff     callq sym.imp.puts      ; int puts(const char *s)
│      │    ; CODE XREF from main @ 0x56262740076b
│      └──> 0x562627400779      b800000000     movl $0, %eax
│           0x56262740077e      488b55f8       movq var_8h, %rdx
│           0x562627400782      644833142528.  xorq %fs:0x28, %rdx
│       ┌─< 0x56262740078b      7405           je 0x562627400792
│       │   0x56262740078d      e84efeffff     callq sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       └─> 0x562627400792      c9             leave
└           0x562627400793      c3             retq
```

- The hex `0x137c` is compared with the input.
- So, we must enter the hex value in int.
- Use python to find the hex value:

```py
>>> int('137c', 16)
4988
```

- The password is `4988`

```bash
$ ./crackme2.bin 
enter your password
4988
password is valid
```

---

## Crackme 3

- Analyse the binary's main function using `r2`:

```bash
$ r2 -d crackme3.bin 
Process with PID 1678 started...
= attach 1678 1678
bin.baddr 0x5607e0800000
Using 0x5607e0800000
asm.bits 64
[0x7f4bfcd31090]> e asm.syntax=att
[0x7f4bfcd31090]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for vtables
[TOFIX: aaft can't run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7f4bfcd31090]> afl
0x5607e0800610    1 42           entry0
0x5607e0a00fe0    1 4124         reloc.__libc_start_main
0x5607e0800640    4 50   -> 40   sym.deregister_tm_clones
0x5607e0800680    4 66   -> 57   sym.register_tm_clones
0x5607e08006d0    5 58   -> 51   sym.__do_global_dtors_aux
0x5607e0800600    1 6            sym.imp.__cxa_finalize
0x5607e0800710    1 10           entry.init0
0x5607e0800840    1 2            sym.__libc_csu_fini
0x5607e0800844    1 9            sym._fini
0x5607e08007d0    4 101          sym.__libc_csu_init
0x5607e080071a    9 170          main
0x5607e08005a0    3 23           sym._init
0x5607e08005d0    1 6            sym.imp.puts
0x5607e08005e0    1 6            sym.imp.__stack_chk_fail
0x5607e0800000    2 25           map._root_Desktop_CTF_TryHackMe_ReverseEngineering_crackme3.bin.r_x
0x5607e08005f0    1 6            sym.imp.__isoc99_scanf
[0x7f4bfcd31090]> pdf @ main
            ; DATA XREF from entry0 @ 0x5607e080062d
┌ 170: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_28h @ rbp-0x28
│           ; var int64_t var_23h @ rbp-0x23
│           ; var int64_t var_21h @ rbp-0x21
│           ; var int64_t var_20h @ rbp-0x20
│           ; var int64_t var_8h @ rbp-0x8
│           0x5607e080071a      55             pushq %rbp
│           0x5607e080071b      4889e5         movq %rsp, %rbp
│           0x5607e080071e      4883ec30       subq $0x30, %rsp
│           0x5607e0800722      64488b042528.  movq %fs:0x28, %rax
│           0x5607e080072b      488945f8       movq %rax, var_8h
│           0x5607e080072f      31c0           xorl %eax, %eax
│           0x5607e0800731      66c745dd617a   movw $0x7a61, var_23h   ; 'az'
│           0x5607e0800737      c645df74       movb $0x74, var_21h     ; 't' ; 116
│           0x5607e080073b      488d3d120100.  leaq str.enter_your_password, %rdi ; 0x5607e0800854 ; "enter your password"
│           0x5607e0800742      e889feffff     callq sym.imp.puts      ; int puts(const char *s)
│           0x5607e0800747      488d45e0       leaq var_20h, %rax
│           0x5607e080074b      4889c6         movq %rax, %rsi
│           0x5607e080074e      488d3d130100.  leaq 0x5607e0800868, %rdi ; "%s"
│           0x5607e0800755      b800000000     movl $0, %eax
│           0x5607e080075a      e891feffff     callq sym.imp.__isoc99_scanf ; int scanf(const char *format)
│           0x5607e080075f      c745d8000000.  movl $0, var_28h
│       ┌─< 0x5607e0800766      eb2f           jmp 0x5607e0800797
│      ┌──> 0x5607e0800768      8b45d8         movl var_28h, %eax
│      ╎│   0x5607e080076b      4898           cltq
│      ╎│   0x5607e080076d      0fb65405e0     movzbl -0x20(%rbp, %rax), %edx
│      ╎│   0x5607e0800772      8b45d8         movl var_28h, %eax
│      ╎│   0x5607e0800775      4898           cltq
│      ╎│   0x5607e0800777      0fb64405dd     movzbl -0x23(%rbp, %rax), %eax
│      ╎│   0x5607e080077c      38c2           cmpb %al, %dl
│     ┌───< 0x5607e080077e      7413           je 0x5607e0800793
│     │╎│   0x5607e0800780      488d3de40000.  leaq str.password_is_incorrect, %rdi ; 0x5607e080086b ; "password is incorrect"
│     │╎│   0x5607e0800787      e844feffff     callq sym.imp.puts      ; int puts(const char *s)
│     │╎│   0x5607e080078c      b800000000     movl $0, %eax
│    ┌────< 0x5607e0800791      eb1b           jmp 0x5607e08007ae
│    │└───> 0x5607e0800793      8345d801       addl $1, var_28h
│    │ ╎│   ; CODE XREF from main @ 0x5607e0800766
│    │ ╎└─> 0x5607e0800797      837dd802       cmpl $2, var_28h
│    │ └──< 0x5607e080079b      7ecb           jle 0x5607e0800768
│    │      0x5607e080079d      488d3ddd0000.  leaq str.password_is_correct, %rdi ; 0x5607e0800881 ; "password is correct"
│    │      0x5607e08007a4      e827feffff     callq sym.imp.puts      ; int puts(const char *s)
│    │      0x5607e08007a9      b800000000     movl $0, %eax
│    │      ; CODE XREF from main @ 0x5607e0800791
│    └────> 0x5607e08007ae      488b4df8       movq var_8h, %rcx
│           0x5607e08007b2      6448330c2528.  xorq %fs:0x28, %rcx
│       ┌─< 0x5607e08007bb      7405           je 0x5607e08007c2
│       │   0x5607e08007bd      e81efeffff     callq sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       └─> 0x5607e08007c2      c9             leave
└           0x5607e08007c3      c3             retq
```

- The password is being compared character by character in a loop.
- By debugging the program with breakpoints, we find that the password is `azt`

```bash
$ ./crackme3.bin 
enter your password
azt
password is correct
```

---