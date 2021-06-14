# Break It

**Date:** 14, June, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/breakit) to go to the TryHackMe room.

## Bases

### Base 32
- **Answer:** easy_base32
- **Steps to Reproduce:** 

```bash
$ echo MVQXG6K7MJQXGZJTGI====== | base32 -d
easy_base32
```


### Double Bases
- **Answer:** double_bases
- **Steps to Reproduce:** 

```bash
$ echo TVJYWEtZVE1NVlBXRVlMVE1WWlE9PT09 | base64 -d
MRXXKYTMMVPWEYLTMVZQ====
$ echo TVJYWEtZVE1NVlBXRVlMVE1WWlE9PT09 | base64 -d | base32 -d
double_bases
```


### Multiple Bases
- **Answer:** base16_is_hex
- **Steps to Reproduce:** 
    - Use Cyberchef!

```bash
$ echo GM4HOU3VHBAW6OKNJJFW6SS2IZ3VAMTYORFDMUC2G44EQULIJI3WIVRUMNCWI6KGK5XEKZDTN5YU2RT2MR3E45KKI5TXSOJTKZJTC4KRKFDWKZTZOF3TORJTGZTXGNKCOE====== | base32 -d | base58 -d | xxd -r -p | base64 -d
base16_is_hex
```

### Lot of Bases
- **Answer:** that_is_a_lot_of_bases
- **Steps to Reproduce:**
    - Use cyberchef:
        - From Base32
        - From Base85
        - From Base64
        - From Base58
        - From Base85
        - From Base64
        - From Base64

### Insane
- **Answer:** defense_the_base
- **Steps to Reproduce:** 
    - Use cyberchef and dcode.fr
        - From Base32 -> From Base85 -> From Decimal -> From Hex
        - From Base91
        - From Base58 -> From Hex -> From Base64 -> From Decimal -> From Hex

---

## Base and Ciphers

### Base 32 + Rot 13
- **Answer:** make_13_spin
- **Steps to Reproduce:** 

```bash
$ echo PJXHQ4S7GEZV6ZTDOZQQ==== | base32 -d | rot13
make_13_spin
```

### Vigenere cipher
- **Answer:** I luv vigenere cipher
- **Steps to Reproduce:** 
    - From Base32 -> From Base64
    - Use Vigenere cipher with the key obtained!

### Bases and ROTs
- **Answer:** decode_and_rot
- **Steps to Reproduce:** 
    - From Base85 -> ROT47 -> From Base64 -> From Base32 -> ROT13


### Insane
- **Answer:** you are a real code cracker
- **Steps to Reproduce:** 
    - Use cyberchef and dcode.fr
        - From Base32 -> From Base85
        - From Base91
        - From Decimal -> From Hex -> From Base64
        - Note down the Key
        - From Base58 -> Vigenere Decode using the key

---

## Base, Cipher and Bit shift

- Use Hex workshop, Cyberchef

### Moderate
- **Answer:** shift logic like a boss
- **Steps to Reproduce:** 
    - Block Shift Left (Logical shift)
    - From Base64 -> ROT13

### Hard
- **Answer:** shift arithmetic like a boss
- **Steps to Reproduce:** 
    - Bit Rotate Left (8 Bits) (Arithmetic shift)
    - From Base58 -> ROT13

### Insane
- **Answer:** God of shifs
- **Steps to Reproduce:** 
    - Bit Rotate Left (8 Bits) - 3 times
    - From Base64
    - Block Shift Left - 2 times
    - From Base85 -> ROT13

---