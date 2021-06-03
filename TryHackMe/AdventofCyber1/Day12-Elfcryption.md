# Day 12 - Elfcryption

**Date:** 02, June, 2021

**Author:** Dhilip Sanjay S

---

## GPG
- GPG uses the AES algorithm to encrypt your files
- To encrypt a file:
    - `gpg -c data.txt`

- To decrypt a file:
    - `gpg -d data.txt`
- Decrypting requires the password

## Open SSL

- To generate a private key we use the following command (8912 creates the key 8912 bits long): 
    - `openssl genrsa -aes256 -out private.key 8912`

- To generate a public key we use our previously generated private key: 
    - `openssl rsa -in private.key -pubout -out public.key`

- Lets now encrypt a file (plaintext.txt) using our public key: 
    - `openssl rsautl -encrypt -pubin -inkey public.key -in plaintext.txt -out encrypted.txt`

- Now, if we use our private key, we can decrypt the file and get the original message: 
    - `openssl rsautl -decrypt -inkey private.key -in encrypted.txt -out plaintext.txt`


---

## Questions

### What is the md5 hashsum of the encrypted note1 file?
- **Answer:** 24cf615e2a4f42718f2ff36b35614f8f
- **Steps to Reproduce:** 

```bash
md5sum note1.txt.gpg 
24cf615e2a4f42718f2ff36b35614f8f  note1.txt.gpg
```

---

### Where was elf Bob told to meet Alice?
- **Answer:** 
- **Steps to Reproduce:** 
    - The gpg key is `25daysofchristmas`

```bash
gpg -d note1.txt.gpg 
gpg: AES.CFB encrypted data
gpg: encrypted with 1 passphrase
I will meet you outside Santa's Grotto at 5pm!
```

---

### Decrypt note2 and obtain the flag!
- **Answer:** 
- **Steps to Reproduce:** 
    - The passphrase for private.key is `hello`

```bash
$ openssl rsautl -decrypt -inkey private.key -in note2_encrypted.txt -out note2.txt 
Enter pass phrase for private.key:

$ cat note2.txt 
THM{ed9ccb6802c5d0f905ea747a310bba23}
```

---