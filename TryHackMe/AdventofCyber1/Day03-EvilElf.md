# Evil Elf

**Date:** 26, May, 2021

**Author:** Dhilip Sanjay S

---

- Download the **pcap** file.
- Open it using Wireshark.

### Whats the destination IP on packet number 998?
- **Answer:** 63.32.89.195

---

### What item is on the Christmas list?
- **Answer:** ps4
- **Steps to Reproduce:** 
    - We can't find useful information in SSL encrypted packet.
    - So look for **telnet** protocol (Unencrypted)
    - You'll obtain 3 packets.
    - Open the Telnet data of **Packet 2255** and you'll find:

    ```bash
    echo 'ps4' > christmas_list.txt
    ```
    
---

### Crack buddy's password!
- **Answer:** rainbow
- **Steps to Reproduce:** 
    - Open the Telnet Data of **Packet 2908**
    - You'll find the contents of **/etc/shadow** file.
    - Copy and save the hash of **buddy**. (It is sha512 hash)
    - Use **john or hashcat** to crack the hash

    ```bash
    $ john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt 
    Using default input encoding: UTF-8
    Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
    Cost 1 (iteration count) is 5000 for all loaded hashes
    Press 'q' or Ctrl-C to abort, almost any other key for status
    rainbow          (?)
    1g 0:00:00:00 DONE (2021-05-26 12:28) 5.555g/s 1422p/s 1422c/s 1422C/s carolina..freedom
    Use the "--show" option to display all of the cracked passwords reliably
    Session completed
    ```

---