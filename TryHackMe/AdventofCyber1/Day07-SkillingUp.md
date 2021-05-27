# Skilling Up

**Date:** 28, May, 2021

**Author:** Dhilip Sanjay S

---

## Run Nmap

```bash
$ nmap -sC -sV -p 1-1000 10.10.164.203 -oN nmap.out
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-28 00:16 IST

Nmap scan report for 10.10.164.203
Host is up (0.16s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 c3:6e:65:a4:e9:79:74:62:76:74:67:f2:85:e2:73:5b (RSA)
|   256 54:b5:e0:cd:c9:f5:72:19:24:f0:6e:a2:3c:f4:8a:e5 (ECDSA)
|_  256 fa:e0:96:ed:05:00:37:1a:e4:ae:b8:7d:64:68:e3:be (ED25519)
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          44835/tcp   status
|   100024  1          50823/udp   status
|   100024  1          55189/tcp6  status
|_  100024  1          56730/udp6  status
999/tcp open  http    SimpleHTTPServer 0.6 (Python 3.6.8)
|_http-server-header: SimpleHTTP/0.6 Python/3.6.8
|_http-title: Directory listing for /

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.41 seconds
```

---

### How many TCP ports under 1000 are open?
- **Answer:** 3

---

### What is the name of the OS of the host?
- **Answer:** Linux

---

### What version of SSH is running?
- **Answer:** 7.4

---

### What is the name of the file that is accessible on the server you found running?
- **Answer:** interesting.file

```bash
$ curl 10.10.164.203:999
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>Directory listing for /</title>
</head>
<body>
<h1>Directory listing for /</h1>
<hr>
<ul>
<li><a href="interesting.file">interesting.file</a></li>
</ul>
<hr>
</body>
</html>
```

---