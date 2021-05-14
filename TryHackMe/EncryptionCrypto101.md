# Encryption Crypto 101

**Date:** 14, May, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/encryptioncrypto101) to go to the TryHackMe room.

## Key Terms

- **Ciphertext** - The result of encrypting a plaintext, encrypted data

- **Cipher** - A method of encrypting or decrypting data. Modern ciphers are cryptographic, but there are many non cryptographic ciphers like Caesar.

- **Plaintext** - Data before encryption, often text but not always. Could be a photograph or other file

- **Encryption** - Transforming data into ciphertext, using a cipher.

- **Encoding** - NOT a form of encryption, just a form of data representation like base64. Immediately reversible.

- **Key** - Some information that is needed to correctly decrypt the ciphertext and obtain the plaintext.

- **Passphrase** - Separate to the key, a passphrase is similar to a password and used to protect a key.

- **Asymmetric encryption** - Uses different keys to encrypt and decrypt.

- **Symmetric encryption -** Uses the same key to encrypt and decrypt

- **Brute force** - Attacking cryptography by trying every different password or every different key

- **Cryptanalysis** - Attacking cryptography by finding a weakness in the underlying maths

```
Note to self:

Read the difference between
    - Encryption vs Encoding
    - Password vs Passphrase
    
Definition of Cryptanalysis
```

### Are SSH keys protected with a passphrase or a password?
- **Answer:** Passphrase

---

## Why is Encryption important?

- Whenever sensitive user data needs to be stored, it should be encrypted.
- DO NOT encrypt passwords unless you’re doing something like a password manager. Passwords should not be stored in plaintext, and you should use hashing to manage them safely.

### What does SSH stand for?
- **Answer:** Secure Shell

### How do webservers prove their identity?
- **Answer:** certificates

### What is the main set of standards you need to comply with if you store or process payment card details?
- **Answer:** PCI-DSS (Payment Card Industry Data Security Standard)

---

## Crucial Crypto Maths

- There's a little bit of math(s) that comes up relatively often in cryptography. The Modulo operator. 

### What's 30 % 5?
- **Answer:** 0

### What's 25 % 7
- **Answer:** 4

### What's 118613842 % 9091
- **Answer:** 3565

```py
Python 3.8.3 (tags/v3.8.3:6f8c832, May 13 2020, 22:37:02) [MSC v.1924 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> 118613842 % 9091
3565
```

---

### Types of Encryption

1. **Symmetric encryption** uses the same key to encrypt and decrypt the data.
    - **Examples:** DES (Broken) and AES (128 or 256 bit keys are common for AES, DES keys are 56 bits long).

2. **Asymmetric encryption** uses a pair of keys, one to encrypt and the other in the pair to decrypt. These keys are referred to as a public key and a private key.
    - **Examples:** RSA and Elliptic Curve Cryptography (RSA typically uses 2048 to 4096 bit keys.) RSA and Elliptic Curve cryptography are based around different **mathematically difficult (intractable)** problems, which give them their strength.

### Should you trust DES? Yea/Nay
- **Answer:** Nay
- Listen to [Darknet Diaries - Ep: 12 Crypto Wars](https://darknetdiaries.com/episode/12/)

### What was the result of the attempt to make DES more secure so that it could be used for longer?
- **Answer:** Triple DES

### Is it ok to share your public key? Yea/Nay
- **Answer:** Yea

---

## RSA - Rivest Shamir Adleman

- RSA is based on the mathematically difficult problem of working out the factors of a large number. It’s very quick to multiply two prime numbers together, say 17*23 = 391, but it’s quite difficult to work out what two prime numbers multiply together to make 14351 (113x127 for reference).

```
The key variables that you need to know about for RSA in CTFs are p, q, m, n, e, d, and c.

    - “p” and “q” are large prime numbers, “n” is the product of p and q.
    - The public key is n and e
    - The private key is n and d.
    - “m” is used to represent the message (in plaintext) 
    - “c” represents the ciphertext (encrypted text).
```

- **CTFs involving RSA** - Crypto CTF challenges often present you with a set of these values, and you need to break the encryption and decrypt a message to retrieve the flag.

### p = 4391, q = 6659. What is n?
- **Answer:** 29239669

```py
>>> 4391 * 6659
29239669
```

---

## Establishing Keys Using Asymmetric Cryptography

- A very common use of asymmetric cryptography is exchanging keys for symmetric encryption.
- Asymmetric encryption tends to be slower, so for things like HTTPS symmetric encryption is better.

- **Metaphor time**
- In reality, you need a little more cryptography to verify the person you’re talking to is who they say they are, which is done using digital signatures and certificates
- [How does HTTPS actually work](https://robertheaton.com/2014/03/27/how-does-https-actually-work/)

---

## Digital signatures and Certificates

- **Digital signatures** are a way to prove the authenticity of files, to prove who created or modified them. Using asymmetric cryptography, you produce a signature with your private key and it can be verified using your public key.
    - The simplest form of digital signature would be encrypting the document with your private key, and then if someone wanted to verify this signature they would decrypt it with your public key and check if the files match.
- **Certificates** are also a key use of public key cryptography, linked to digital signatures. 
    - The certificates have a chain of trust, starting with a root CA (certificate authority). Root CAs are automatically trusted by your device, OS, or browser from install. 

### What company is TryHackMe's certificate issued to?
- **Answer:** Cloudflare
- **Steps to Reproduce:**
    - On visiting **tryhackme.com**, click the lock symbol in the search box.
    - And then click on **Certificates** to view them.
    
    ```
    Issued to: sni.cloudflaressl.com

    Issued by:  Cloudflare Inc ECC CA-3

    Valid from ‎11 ‎August ‎2020 to ‎11 ‎August ‎2021
    ```

---

## SSH Authentication

- By default, SSH is authenticated using usernames and passwords in the same way that you would log in to the physical machine.
- SSH configured with public and private key authentication. (SSH keys are RSA keys)
- **SSH Private Keys**
    - Using tools like **John the Ripper**, you can attack an encrypted SSH key to attempt to find the passphrase, which highlights the importance of using a **secure passphrase** and keeping your private key private.
- The `authorized_keys` (note the US English spelling) file in **~/.ssh** directory holds public keys that are allowed to access the server if key authentication is enabled.
- For key based authentication, use `ssh -i keyName user@host`

- **Using SSH keys to get a better shell**
    - SSH keys are an excellent way to “upgrade” a reverse shell, assuming the user has login enabled
    - You can add your own public keys to the `~/.ssh/authorized_keys` file.
    - Leaving an SSH key in authorized_keys on a box can be a useful backdoor, and you don't need to deal with any of the issues of unstabilised reverse shells like Control-C or lack of tab completion.

### What algorithm does the key use?
- **Answer:** RSA

### Crack the password with John The Ripper and rockyou, what's the passphrase for the key?
- **Answer:** delicious
- **Steps to Reproduce:** 

```bash
$ python3 /usr/share/john/ssh2john.py id_rsa
id_rsa:$sshng$1$16$0B5AB4FEB69AFB92B2100435B42B7949$1200$8dce3420285b19a7469a642278a7afab0ab40e28c865ce93fef1351bae5499df5fbf04ddf510e5e407246e4221876b3fbb93931a5276281182b9baf38e0c38a56548f30e7781c77e2bf5940ad9f77265102ab328bb4c6f7fd06e9a3153191dfcddcd9672256608a5bff044fbf33901849aa2c3464e24bb31d6d65160df61848952a79ce660a97b3123fa539754a0e5ffbfba796c98c17b4ca45eeeee1e1c7a45412e26fef9ba8ed48a15c2b60e23a5a525ee2451e03c85145d03b7129740b7ec3babda2f012f1ad21ea8c9ccae7e8eaf95e58fe73159db31785f838de9d960d3d2a528abddad0337490caa73565042ff8c5dc672d2e58402e3449cf0500b0e467300220cee35b528e718eb25fdc7d265042d3dbbe39ed52a445bdd78ad4a9462b374f6ce87c1bd28f1154b52c59db6028187c22cafa5b02eabe27f9a41733a35b6cfc73d83c65febafe8e7568d15b5a5a3340472794a2b6da5cff593649b35299ede7e8a2294ce5812bb5bc9396cc4ae5525620f4e83442c7e181317082e5fd93b29773dd7203e22947b960b2fedbd089ffb88793533dcf195281207e05ada2d284dc69b475e7d561a47d43470d490ec9d847d820eb9db7943dcf133350b6e8b6513ed2deeca6a5105eb496170fd2367b3637e7375891a483511168fe1f3292bcd64e252682865e7da1f1f06ae261a62a0155d3a932cc1976f45c1feaaf183ad86c7ce91795fe45395a73268d3c0e228e24d025c997a936fcb27bb05992ff4b23e050edaaae748b14a80c4ff3145f75436100fc840d107eb97e3da3b8114879e373053f8c4431ffc6feecd167f29a75152ad2e09b8bcaf4eaf92ae7155684c9175e32fe2141b67681c37fa41e791bd71872d49ea52bdea6f54ae6c41eb539ad2ed0c7dedf525ee20460a193a70501d9bc18f42347a4dd62d94e9cac504abb02b7a294efb7e1946014de9051d988c3e23fffcf00f4f5beb3b191f9d01557079cb45e992199d13770060e53f09389caa062cfc675aba02c693ef2c4326a1443aef1987e4c8fa10e11e6d2995faf1f8aa991efffcacea28967f24eabac5467e702d3a2e07a4c56f67801870f7cdb34d9d80116d6ce26b3cfbba9b06d06957911b6c13e37b879593af0c3cb29d2f5a388966876b0a26cadd94e79d97868f9464df6cd67433748f3dabbe5e9ac0eb6dacdfd0cc4219cbbf3bb0fe87fce5b907611bcd1e91a64b1cdab3f26b89f70397e5ddd58e921db7ad69871a6705170b58573eaca996d6cb987210e4d1ea2e098978525be38d8b0717671d651abea0521768a03c1028570a78514727812d7d17946cef6aaca0dddd1e5885f0f7feacfe7a3f70911a6f422f855bac2fd23105114898fe44b532992d841a51e08111be2caa66ab30aa3e89cd99177a53271e9400c79944c2406d605a084875c8b4730f108e2a2cce6251bb4fc27a6f3afd03c289745fb17630a8b0f520ba770ca1455c63ad1db7b21272fc9a5d25fadfdf23a7b021f6d8069e9ca8631dd0e81b182521e7b9efc4632643ac123c1bf8e2ce84576ae0cfc24730d051705bd68958d34a232b11742bce05d2db83029bd631913392fc565e6d8accedf1f9c2ba90c48a773bcc627f99ab1a44897280c2d945a0d8a1270206515dd2fa08f8c34a4150a0ba35ff0d3dbc2c21cd00e09f774a0741d28534eec64ea3

$ john id_rsa_decoded --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
delicious        (id_rsa)
Session completed
```

---

## Explaining Diffie Hellman Key Exchange

- **Key exchange** allows 2 people/parties to establish a set of common cryptographic keys without an observer being able to get these keys. Generally, to establish common symmetric keys.

- **Diffie Hellman Key Exchange** - [Computerphile Video - DH Key Exchange](https://www.youtube.com/watch?v=NmM9HA2MQGI)

- DH Key Exchange is often used alongside RSA public key cryptography, to prove the identity of the person you’re talking to with digital signing. This prevents someone from attacking the connection with a man-in-the-middle attack.

---

## PGP, GPG and AES

- PGP stands for **Pretty Good Privacy**. It’s a software that implements encryption for encrypting files, performing digital signing and more.

- **GnuPG or GPG** is an Open Source implementation of PGP from the GNU project. You may need to use GPG to decrypt files in CTFs. Sometimes, PGP/GPG keys can be protected with passphrases. Check out [gpg manpage](https://www.gnupg.org/gph/de/manual/r1023.html).

- AES, sometimes called **Rijndael** after its creators, stands for **Advanced Encryption Standard**. It was a replacement for DES which had short keys and other cryptographic flaws. - [Computerphile Video AES](https://www.youtube.com/watch?v=O4xNJsjtN6E). AES and DES both operate on blocks of data (a block is a fixed size series of bits).


### You have the private key, and a file encrypted with the public key. Decrypt the file. What's the secret word?
- **Answer:** Pineapple
- **Steps to Reproduce:** 
    - Unzip the zip file:
        
        ```bash
        $ file zip
        zip: Zip archive data, at least v2.0 to extract

        $ unzip zip
        Archive:  zip
        extracting: message.gpg             
        inflating: tryhackme.key
        ```
    
    - Import the GPG key using `gpg --import keyFile`:

        ```bash
        gpg --import tryhackme.key 
        gpg: key FFA4B5252BAEB2E6: public key "TryHackMe (Example Key)" imported
        gpg: key FFA4B5252BAEB2E6: secret key imported
        gpg: Total number processed: 1
        gpg:               imported: 1
        gpg:       secret keys read: 1
        gpg:   secret keys imported: 1
        ```
    
    - Decrypt the message using `gpg -d messageFile`:

        ```bash
        $ gpg -d message.gpg
        gpg: encrypted with 1024-bit RSA key, ID 2A0A5FDC5081B1C5, created 2020-06-30
            "TryHackMe (Example Key)"
        You decrypted the file!
        The secret word is Pineapple.
        ```

---

## The Future - Quantum Computers and Encryption

- Quantum computers will soon be a problem for many types of encryption.
- **Asymmetric and Quantum** - While it’s unlikely we’ll have sufficiently powerful quantum computers until around 2030, once these exist encryption that uses RSA or Elliptical Curve Cryptography will be very fast to break. This is because quantum computers can very efficiently solve the mathematical problems that these algorithms rely on for their strength.

- **AES/Triple DES and Quantum** - AES with 128 bit keys is also likely to be broken by quantum computers in the near future, but 256 bit AES can’t be broken as easily. **Triple DES** is also vulnerable to attacks from quantum computers.

- **Current Recommendations**
    - The NSA recommends using **RSA-3072** or better for asymmetric encryption and **AES-256** or better for symmetric encryption.
    - It’s likely that we will have a new encryption standard before quantum computers become a threat to RSA and AES.

---

## References
- [PCI Website](https://www.pcisecuritystandards.org/)
- [RSA Encryption](https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/)
- [Computerphile Video - DH Key Exchange](https://www.youtube.com/watch?v=NmM9HA2MQGI)
- [Computerphile Video AES](https://www.youtube.com/watch?v=O4xNJsjtN6E)
- [Report on Post Quantum Cryptography](https://nvlpubs.nist.gov/nistpubs/ir/2016/NIST.IR.8105.pdf)
-  "Cryptography Apocalypse" By Roger A. Grimes