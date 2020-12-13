# Day 13 - Coal for Christmas

**Date:** 13, December, 2020

**Author:** Dhilip Sanjay S

---

## Kernel Exploits
- Dirty COW (Copy-On-Write) - [CVE-2016-5195]( https://dirtycow.ninja/)
- [Dirty COW - C code](https://github.com/FireFart/dirtycow/blob/master/dirty.c)
---

## Solutions
### What old, deprecated protocol and service is running?
- **Answer:** Telnet

---

### What credential was left for you?
- **Answer:** clauschristmas
- **Steps to Reproduce:** `telnet <MACHINE_IP> <PORT_FROM_NMAP_SCAN>`

---

### What distribution of Linux and version number is this server running?
- **Answer:** Ubuntu 12.04
- **Steps to Reproduce:** `cat /etc/*release`

---

### Who got here first?
- **Answer:** grinch
- **Steps to Reproduce:** `cat cookies_and_milk.txt`

---

### What is the verbatim syntax you can use to compile, taken from the real C source code comments?
- **Answer:** gcc -pthread dirty.c -o dirty -lcrypt

---

### What "new" username was created, with the default operations of the real C source code?
- **Answer:** firefart

---

### What is the MD5 hash output?
- **Answer:** 8b16f00dd3b51efadb02c1df7f8427cc 
- **Steps to Reproduce:** 
    ```bash
    firefart@christmas:/home/santa# cd /root
    firefart@christmas:~# touch coal
    firefart@christmas:~# tree
    .
    |-- christmas.sh
    |-- coal
    `-- message_from_the_grinch.txt

    0 directories, 3 files
    firefart@christmas:~# tree | md5sum
    8b16f00dd3b51efadb02c1df7f8427cc   
    ```
---

[Back to Advent of Cyber 2](/Advent%20of%20Cyber%202) 