# Reversing ELF

**Date:** 05, June, 2021

**Author:** Dhilip Sanjay S

---

## Crackme 1

```bash
$ ./crackme1
flag{not_that_kind_of_elf}
```

---

## Crackme 2

- Analyze the binary using `r2`:

```bash
$ r2 -A crackme2
[..snip..]
0x080484db      6874860408     pushl $str.super_secret_password ; 0x8048674 ; "super_secret_password" ; const char *s2
[..snip..]
```

- Enter the super secret password to obtain the flag:

```bash
$ ./crackme2 super_secret_password
Access granted.
flag{if_i_submit_this_flag_then_i_will_get_points}
```

---

## Crackme 3

- Analyze the binary using `r2`:

```bash
$ r2 -A crackme3
[..snip..]
0x08048597      c74424048b8e.  movl $str.ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ, var_4h ; [0x8048e8b:4]=0x79426a5a ; "ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ=="   
[..snip..]
```

- Base64 decode the string:

```bash
$ echo ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ== | base64 -d
f0r_y0ur_5ec0nd_le55on_unbase64_4ll_7h3_7h1ng5
```

- Enter the password:

```bash
$ ./crackme3 f0r_y0ur_5ec0nd_le55on_unbase64_4ll_7h3_7h1ng5
Correct password!
```

---

## Crackme 4

- Analyze the binary using `r2` by setting breakpoint in `sym.compare_pwd` function:

```bash
$ r2 -A crackme4 pass
[..snip..]
[0x004006d5]> px  @ rax
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffd72914920  6d79 5f6d 3072 335f 7365 6375 7233 5f70  my_m0r3_secur3_p                                                                        
0x7ffd72914930  7764 0000 0000 0000 003c 6330 eb8b 76cc  wd.......<c0..v.
0x7ffd72914940  6049 9172 fd7f 0000 5907 4000 0000 0000  `I.r....Y.@.....
0x7ffd72914950  584a 9172 fd7f 0000 0000 0000 0200 0000  XJ.r............
0x7ffd72914960  6007 4000 0000 0000 0a4d dd10 9f7f 0000  `.@......M......
0x7ffd72914970  584a 9172 fd7f 0000 0000 0000 0200 0000  XJ.r............
0x7ffd72914980  1607 4000 0000 0000 0000 0000 0000 0000  ..@.............
0x7ffd72914990  0000 0000 0000 0000 2486 3fa8 cf87 3e97  ........$.?...>.
0x7ffd729149a0  4005 4000 0000 0000 0000 0000 0000 0000  @.@.............
0x7ffd729149b0  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd729149c0  2486 1f34 6d62 c468 2486 793f f5a6 0068  $..4mb.h$.y?...h
0x7ffd729149d0  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd729149e0  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd729149f0  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffd72914a00  80c1 fb10 9f7f 0000 0000 0000 0000 0000  ................
0x7ffd72914a10  0000 0000 0000 0000 4005 4000 0000 0000  ........@.@.....
[..snip..]
```

- Enter the password to verify:

```bash
$ ./crackme4 my_m0r3_secur3_pwd 
password OK
```

---

## Crackme 5

- Analyze the binary using `r2` by setting breakpoint before the function call to `sym.strcmp_`:

```bash
$ r2 -A crackme5 
[..snip..]
[0x0040082f]> px @ rdx
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffcd4a040f0  4f66 646c 4453 417c 3374 5862 3332 7e58  OfdlDSA|3tXb32~X                                                                        
0x7ffcd4a04100  3374 5840 7358 6034 7458 747a 0000 0000  3tX@sX`4tXtz....
0x7ffcd4a04110  1042 a0d4 fc7f 0000 00e9 8c6a 5f29 e66a  .B.........j_).j
0x7ffcd4a04120  d008 4000 0000 0000 0aad 1b96 dd7f 0000  ..@.............
0x7ffcd4a04130  1842 a0d4 fc7f 0000 0000 0000 0200 0000  .B..............
0x7ffcd4a04140  7307 4000 0000 0000 0000 0000 0000 0000  s.@.............
0x7ffcd4a04150  0000 0000 0000 0000 17e0 3f51 75db 9aa7  ..........?Qu...
0x7ffcd4a04160  e005 4000 0000 0000 0000 0000 0000 0000  ..@.............
0x7ffcd4a04170  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffcd4a04180  17e0 ffc2 b572 6358 17e0 1919 c2f7 2158  .....rcX......!X
0x7ffcd4a04190  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffcd4a041a0  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffcd4a041b0  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffcd4a041c0  8021 3a96 dd7f 0000 0000 0000 0000 0000  .!:.............
0x7ffcd4a041d0  0000 0000 0000 0000 e005 4000 0000 0000  ..........@.....
0x7ffcd4a041e0  1042 a0d4 fc7f 0000 0000 0000 0000 0000  .B..............
[..snip..]
```

- Enter the password to verify:

```bash
$ ./crackme5
Enter your input:
OfdlDSA|3tXb32~X3tX@sX`4tXtz
Good game
```

---

## Crackme 6

- Analyze the binary using `r2`.
- You'll find a function named `sym.my_secure_test`. Print the disassembled function:

```bash
$ r2 -A crackme6 hello
[..snip..]
[0x7f44d1da62e9]> pdf @ sym.my_secure_test
            ; CALL XREF from sym.compare_pwd @ 0x4006e4
┌ 340: sym.my_secure_test (int64_t arg1);
│           ; var int64_t var_8h @ rbp-0x8
│           ; arg int64_t arg1 @ rdi
│           0x0040057d      55             pushq %rbp
│           0x0040057e      4889e5         movq %rsp, %rbp
│           0x00400581      48897df8       movq %rdi, var_8h           ; arg1
│           0x00400585      488b45f8       movq var_8h, %rax
│           0x00400589      0fb600         movzbl (%rax), %eax
│           0x0040058c      84c0           testb %al, %al
│       ┌─< 0x0040058e      740b           je 0x40059b
│       │   0x00400590      488b45f8       movq var_8h, %rax
│       │   0x00400594      0fb600         movzbl (%rax), %eax
│       │   0x00400597      3c31           cmpb $0x31, %al             ; 49
│       │   ;-- panel.addr:
│      ┌──< 0x00400599      740a           je 0x4005a5
│      │└─> 0x0040059b      b8ffffffff     movl $0xffffffff, %eax      ; -1
│      │┌─< 0x004005a0      e92a010000     jmp 0x4006cf
│      └──> 0x004005a5      488b45f8       movq var_8h, %rax
│       │   0x004005a9      4883c001       addq $1, %rax
│       │   0x004005ad      0fb600         movzbl (%rax), %eax
│       │   0x004005b0      84c0           testb %al, %al
│      ┌──< 0x004005b2      740f           je 0x4005c3
│      ││   0x004005b4      488b45f8       movq var_8h, %rax
│      ││   0x004005b8      4883c001       addq $1, %rax
│      ││   0x004005bc      0fb600         movzbl (%rax), %eax
│      ││   0x004005bf      3c33           cmpb $0x33, %al             ; 51
│     ┌───< 0x004005c1      740a           je 0x4005cd
│     │└──> 0x004005c3      b8ffffffff     movl $0xffffffff, %eax      ; -1
│     │┌──< 0x004005c8      e902010000     jmp 0x4006cf
│     └───> 0x004005cd      488b45f8       movq var_8h, %rax
│      ││   0x004005d1      4883c002       addq $2, %rax
│      ││   0x004005d5      0fb600         movzbl (%rax), %eax
│      ││   0x004005d8      84c0           testb %al, %al
│     ┌───< 0x004005da      740f           je 0x4005eb
│     │││   0x004005dc      488b45f8       movq var_8h, %rax
│     │││   0x004005e0      4883c002       addq $2, %rax
│     │││   0x004005e4      0fb600         movzbl (%rax), %eax
│     │││   0x004005e7      3c33           cmpb $0x33, %al             ; 51
│    ┌────< 0x004005e9      740a           je 0x4005f5
│    │└───> 0x004005eb      b8ffffffff     movl $0xffffffff, %eax      ; -1
│    │┌───< 0x004005f0      e9da000000     jmp 0x4006cf
│    └────> 0x004005f5      488b45f8       movq var_8h, %rax
│     │││   0x004005f9      4883c003       addq $3, %rax
│     │││   0x004005fd      0fb600         movzbl (%rax), %eax
│     │││   0x00400600      84c0           testb %al, %al
│    ┌────< 0x00400602      740f           je 0x400613
│    ││││   0x00400604      488b45f8       movq var_8h, %rax
│    ││││   0x00400608      4883c003       addq $3, %rax
│    ││││   0x0040060c      0fb600         movzbl (%rax), %eax
│    ││││   0x0040060f      3c37           cmpb $0x37, %al             ; 55
│   ┌─────< 0x00400611      740a           je 0x40061d
│   │└────> 0x00400613      b8ffffffff     movl $0xffffffff, %eax      ; -1
│   │┌────< 0x00400618      e9b2000000     jmp 0x4006cf
│   └─────> 0x0040061d      488b45f8       movq var_8h, %rax
│    ││││   0x00400621      4883c004       addq $4, %rax
│    ││││   0x00400625      0fb600         movzbl (%rax), %eax
│    ││││   0x00400628      84c0           testb %al, %al
│   ┌─────< 0x0040062a      740f           je 0x40063b
│   │││││   0x0040062c      488b45f8       movq var_8h, %rax
│   │││││   0x00400630      4883c004       addq $4, %rax
│   │││││   0x00400634      0fb600         movzbl (%rax), %eax
│   │││││   0x00400637      3c5f           cmpb $0x5f, %al             ; 95
│  ┌──────< 0x00400639      740a           je 0x400645
│  │└─────> 0x0040063b      b8ffffffff     movl $0xffffffff, %eax      ; -1
│  │┌─────< 0x00400640      e98a000000     jmp 0x4006cf
│  └──────> 0x00400645      488b45f8       movq var_8h, %rax
│   │││││   0x00400649      4883c005       addq $5, %rax
│   │││││   0x0040064d      0fb600         movzbl (%rax), %eax
│   │││││   0x00400650      84c0           testb %al, %al
│  ┌──────< 0x00400652      740f           je 0x400663
│  ││││││   0x00400654      488b45f8       movq var_8h, %rax
│  ││││││   0x00400658      4883c005       addq $5, %rax
│  ││││││   0x0040065c      0fb600         movzbl (%rax), %eax
│  ││││││   0x0040065f      3c70           cmpb $0x70, %al             ; 112
│ ┌───────< 0x00400661      7407           je 0x40066a
│ │└──────> 0x00400663      b8ffffffff     movl $0xffffffff, %eax      ; -1
│ │┌──────< 0x00400668      eb65           jmp 0x4006cf
│ └───────> 0x0040066a      488b45f8       movq var_8h, %rax
│  ││││││   0x0040066e      4883c006       addq $6, %rax
│  ││││││   0x00400672      0fb600         movzbl (%rax), %eax
│  ││││││   0x00400675      84c0           testb %al, %al
│ ┌───────< 0x00400677      740f           je 0x400688
│ │││││││   0x00400679      488b45f8       movq var_8h, %rax
│ │││││││   0x0040067d      4883c006       addq $6, %rax
│ │││││││   0x00400681      0fb600         movzbl (%rax), %eax
│ │││││││   0x00400684      3c77           cmpb $0x77, %al             ; 119
│ ────────< 0x00400686      7407           je 0x40068f
│ └───────> 0x00400688      b8ffffffff     movl $0xffffffff, %eax      ; -1
│ ┌───────< 0x0040068d      eb40           jmp 0x4006cf
│ ────────> 0x0040068f      488b45f8       movq var_8h, %rax
│ │││││││   0x00400693      4883c007       addq $7, %rax
│ │││││││   0x00400697      0fb600         movzbl (%rax), %eax
│ │││││││   0x0040069a      84c0           testb %al, %al
│ ────────< 0x0040069c      740f           je 0x4006ad
│ │││││││   0x0040069e      488b45f8       movq var_8h, %rax
│ │││││││   0x004006a2      4883c007       addq $7, %rax
│ │││││││   0x004006a6      0fb600         movzbl (%rax), %eax
│ │││││││   0x004006a9      3c64           cmpb $0x64, %al             ; 100
│ ────────< 0x004006ab      7407           je 0x4006b4
│ ────────> 0x004006ad      b8ffffffff     movl $0xffffffff, %eax      ; -1
│ ────────< 0x004006b2      eb1b           jmp 0x4006cf
│ ────────> 0x004006b4      488b45f8       movq var_8h, %rax
│ │││││││   0x004006b8      4883c008       addq $8, %rax
│ │││││││   0x004006bc      0fb600         movzbl (%rax), %eax
│ │││││││   0x004006bf      84c0           testb %al, %al
│ ────────< 0x004006c1      7407           je 0x4006ca
│ │││││││   0x004006c3      b8ffffffff     movl $0xffffffff, %eax      ; -1
│ ────────< 0x004006c8      eb05           jmp 0x4006cf
│ ────────> 0x004006ca      b800000000     movl $0, %eax
│ │││││││   ; XREFS: CODE 0x004005a0  CODE 0x004005c8  CODE 0x004005f0  CODE 
│ │││││││   ; XREFS: CODE 0x0040068d  CODE 0x004006b2  CODE 0x004006c8  
│ └└└└└└└─> 0x004006cf      5d             popq %rbp
└           0x004006d0      c3             retq
[..snip..]
```

- Combine all the hex values that is being used for comparison. 
- Decode the hex value:

```py
>>> bytes.fromhex('313333375f707764').decode('utf-8')
'1337_pwd'
```

- Enter the password to verify:

```bash
$ ./crackme6 1337_pwd
password OK
```

---

## Crackme 7

- Analyze the binary using `r2`:

```bash
$ r2 -d crackme7
[..snip..]
│ ││││ │└─> 0x08048662      8b45f4         mov eax, dword [var_ch]
│ ││││ │    0x08048665      3d697a0000     cmp eax, 0x7a69
│ ││││ │┌─< 0x0804866a      7517           jne 0x8048683
│ ││││ ││   0x0804866c      83ec0c         sub esp, 0xc
│ ││││ ││   0x0804866f      68bc880408     push str.Wow_such_h4x0r_    ; 0x80488bc ; "Wow such h4x0r!"
│ ││││ ││   0x08048674      e8f7fcffff     call sym.imp.puts           ; int puts(const char *s)
│ ││││ ││   0x08048679      83c410         add esp, 0x10
│ ││││ ││   0x0804867c      e825000000     call sym.giveFlag
[..snip..]
```

- We can see that, other than the option 1,2 and 3, there is another option: `0x7a69`
- Use python to decode the hex value:

```py
>>> int('7a69', 16)
31337
```

- Enter the option as `31337`:

```bash
$ ./crackme7
Menu:

[1] Say hello
[2] Add numbers
[3] Quit

[>] 31337
Wow such h4x0r!
flag{much_reversing_very_ida_wow}
```

---

## Crackme 8

- Analyze the binary using `r2`.
- You'll find that `atoi()` function is used before comparing with the password.
- `atoi()` will convert the string to integer (if numbers are in the string, else it'll return 0)

```bash
$ r2 -d crackme8 1234
0x080484dc      e89ffeffff     call sym.imp.atoi           ; int atoi(const char *str)
0x080484e1      83c410         add esp, 0x10
0x080484e4      3d0df0feca     cmp eax, 0xcafef00d
0x080484e9      7417           je 0x8048502
```

- Use `signed int, big endian` in struct to decode the hex value.
- P.S: Unsigned decoded int doesn't work!

``py
>>> import struct
>>> struct.unpack('>i', bytes.fromhex('cafef00d'))
(-889262067,)
```

- Enter the password to verify the flag:

```bash
$ ./crackme8 -889262067
Access granted.
flag{at_least_this_cafe_wont_leak_your_credit_card_numbers}
```


---