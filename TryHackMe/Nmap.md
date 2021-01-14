# Nmap

**Date:** 14, January, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/furthernmap) to go to the TryHackMe room.

# Nmap - Introduction
- Nmap can be used to perform many different kinds of port scan.
- 65535 Ports available
    - System ports (0-1023)
    - User Ports (0-49151)
    - Dynamic and/or Private Ports (49152-65535)
- Depending on how the port responds, it can be determined as being open, closed, or filtered (usually by a firewall)

---
## Questions

### What networking constructs are used to direct traffic to the right application on a server?
- **Answer:** Ports
---
### How many of these are available on any network-enabled computer?
- **Answer:** 65536
---
###  How many of these are considered "well-known"? (These are the "standard" numbers mentioned in the task)
- **Answer:** 1024

---

# Nmap Switches

### What is the first switch listed in the help menu for a 'Syn Scan' 
- **Answer:** -sS
---

### Which switch would you use for a "UDP scan"?
- **Answer:** -sU 

---
### If you wanted to detect which operating system the target is running on, which switch would you use?
- **Answer:** -O

---
### Nmap provides a switch to detect the version of the services running on the target. What is this switch?
- **Answer:** -sV
---

### The default output provided by nmap often does not provide enough information for a pentester. How would you increase the verbosity?
- **Answer:** -v

---

### Verbosity level one is good, but verbosity level two is better! How would you set the verbosity level to two?
- **Answer:** -vv

---

### What switch would you use to save the nmap results in three major formats?
- **Answer:** -oA

---

### What switch would you use to save the nmap results in a "normal" format?
- **Answer:** -oN

---

### A very useful output format: how would you save results in a "grepable" format?
- **Answer:** -oG
---

### Sometimes the results we're getting just aren't enough. If we don't care about how loud we are, we can enable "aggressive" mode. This is a shorthand switch that activates service detection, operating system detection, a traceroute and common script scanning. How would you activate this setting?
- **Answer:** -A

---

### How would you set the timing template to level 5?
- **Answer:** -T5

---

### How would you tell nmap to scan port 80
- **Answer:** -p 80

---
### How would you tell nmap to scan ports 1000-1500?
- **Answer:** -p 1000-1500
---

### How would you tell nmap to scan all ports?
- **Answer:** -p-

---

### How would you activate a script from the nmap scripting library
- **Answer:** --script

---
### How would you activate all of the scripts in the "vuln" category?
- **Answer:** --script=vuln

---

# Basic Scan Types
- When port scanning with Nmap, there are three basic scan types. These are:

    - **TCP Connect Scans (-sT)**
        -  If Nmap sends a TCP request with the SYN flag set to a closed port, the target server will respond with a TCP packet with the RST (Reset) flag set. By this response, Nmap can establish that the port is **closed**.
        - If, however, the request is sent to an open port, the target will respond with a TCP packet with the SYN/ACK flags set. Nmap then marks this port as being **open** (and completes the handshake by sending back a TCP packet with ACK set).
        - Nmap sends a TCP SYN request, and receives nothing back. This indicates that the port is being protected by a firewall and thus the port is considered to be **filtered**.

    - **SYN "Half-open" Scans or "Stealth" scans.(-sS)**
        - These scan require sudo permission. If nmap is not run with sudo permission, then it defaults to TCP scan.
        - If a port is closed then the server responds with a RST TCP packet. If the port is filtered by a firewall then the TCP SYN packet is either dropped, or spoofed with a TCP reset.

    - **UDP Scans (-sU)**
        - UDP connections are stateless.
        - When a packet is sent, there should be no response - if this happens then Nmap refers to the port as being **open|filtered** (it could be firewalled).
        - If it gets a UDP response, then the port is market as **open**.
        - When a packet is sent to a **closed** UDP port, the target should respond with an ICMP (ping) packet containing a message that the port is unreachable.
        - Since UDP scans are slow, the common ports are scanned:  `nmap -sU --top-ports 20 <target>`
---
## Questions

### There are two other names for a SYN scan, what are they?
- **Answer:** Half-Open, Stealth

---

### Can Nmap use a SYN scan without sudo permissions?
- **Answer:** N

---

### If a UDP port doesn't respond to an Nmap scan, what will it be marked as?
- **Answer:** open|filtered

---
### When a UDP port is closed, by convention the target should send back a "port unreachable" message. Which protocol would it use to do so?
- **Answer:** ICMP

---

# Less common scan types

- Additionally there are several less common port scan types which are **stealthier** than SYN scan. These are:

    - **TCP Null Scans (-sN)**
        - When the TCP request is sent with **no flags** set at all. As per the RFC, the target host should respond with a `RST` if the port is **closed**.
    - **TCP FIN Scans (-sF)**
        - A request is sent with the **FIN flag** (usually used to gracefully close an active connection). Once again, Nmap expects a `RST` if the port is **closed**.
    - **TCP Xmas Scans (-sX)**
        -  Xmas scans (-sX) send a **malformed TCP packet** and expects a `RST` response for **closed** ports. 
        - It's referred to as an xmas scan as the flags that it sets (PSH, URG and FIN) give it the appearance of a _blinking christmas tree_ when viewed as a packet capture in Wireshark.

- The expected response for open ports with these scans is also identical, and is very similar to that of a UDP scan. These scans will only ever identify ports as being `open|filtered`, `closed`, or `filtered`. 

- Microsoft Windows (and a lot of Cisco network devices) are known to respond with a `RST` to any **malformed TCP packet** -- regardless of whether the port is actually open or not. This results in **all ports** showing up as being **closed**.

- That said, the goal here is, of course, **firewall evasion**. Many firewalls are configured to drop incoming TCP packets to blocked ports which have the SYN flag set (thus blocking new connection initiation requests). By sending requests which do not contain the SYN flag, we effectively bypass this kind of firewall. 

---
## Questions

### Which of the three shown scan types uses the URG flag?
- **Answer:** Xmas

---
### Why are NULL, FIN and Xmas scans generally used?
- **Answer:** Firewall Evasion

---

### Which common OS may respond to a NULL, FIN or Xmas scan with a RST for every port?
- **Answer:** Microsoft Windows

---

# ICMP (or "ping") scanning
- To see which IP addresses contain active hosts, and which do not - we use Nmap to perform **ping sweep**.
- `nmap -sn 192.168.0.1-254` or `nmap -sn 192.168.0.0/24`
- The `-sn` switch tells Nmap not to scan any ports -- forcing it to rely purely on ICMP packets (or ARP requests on a local network) to identify targets.

## Questions

### How would you perform a ping sweep on the 172.16.x.x network (Netmask: 255.255.0.0) using Nmap? (CIDR notation)
- **Answer:** nmap -sn 172.16.0.0/16

---


## NSE Scripts
- Nmap Scripting Engine (NSE) scripts are written in **Lua programming language** - scanning for vulnerabilities, automating exploits.
- Categories of scripts:
    1. **safe** - won't affect the target.
    1. **intrusive** - Not safe (likely to affect the target).
    1. **vuln** - scan for vulnerabilities.
    1. **exploit** - attempt to exploit a vulnerability
    1. **auth** - attempt to bypass authentication for running services.
    1. **brute** - attempt to bruteforce credentials for running services.
    1. **discovery** - attempt to query running services for further information about the network. (query SNMP server)
- Multiple scripts can be run simultaneously in this fashion by separating them by a comma. 
- Some scripts requires arguments. They can be given with the `--script-args` nmap switch. 
- The arguments are separated by commas, and connected to the corresponding script with periods (i.e.  `<script-name>.<argument>`).

- **Searching for scripts**
    - Search using [Nmap Website](https://nmap.org/nsedoc/)
    - Search local storage - `/usr/share/nmap/scripts`. Two ways:
        - `grep "ftp" /usr/share/nmap/scripts/script.db`
        - `ls -l /usr/share/nmap/scripts/*ftp*`

- **Installing Scripts**
    - `sudo apt update && sudo apt install nmap`
    - `sudo wget -O /usr/share/nmap/scripts/<script-name>.nse https://svn.nmap.org/nmap/scripts/<script-name>.nse`

## Questions
### What language are NSE scripts written in?
- **Answer:** Lua

---

### Which category of scripts would be a very bad idea to run in a production environment?
- **Answer:** intrusive

---

### What optional argument can the ftp-anon.nse script take?
- **Answer:** maxlist
- **Steps to Reproduce:** 
    - Visit the URL.
    ```bash
    nmap --script-help ftp-anon.nse
    Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-14 14:33 IST

    ftp-anon
    Categories: default auth safe
    https://nmap.org/nsedoc/scripts/ftp-anon.html
    Checks if an FTP server allows anonymous logins.

    If anonymous is allowed, gets a directory listing of the root directory
    and highlights writeable files.
    ```

---

### What is the filename of the script which determines the underlying OS of the SMB server?
- **Answer:** smb-os-discovery.nse
- **Steps to Reproduce:** 
    ```bash
    grep smb.*os /usr/share/nmap/scripts/script.db 
    Entry { filename = "smb-flood.nse", categories = { "dos", "intrusive", } }
    Entry { filename = "smb-os-discovery.nse", categories = { "default", "discovery", "safe", } }
    Entry { filename = "smb-vuln-conficker.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
    Entry { filename = "smb-vuln-cve2009-3103.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
    Entry { filename = "smb-vuln-ms06-025.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
    Entry { filename = "smb-vuln-ms07-029.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
    Entry { filename = "smb-vuln-ms08-067.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
    Entry { filename = "smb-vuln-ms10-054.nse", categories = { "dos", "intrusive", "vuln", } }
    Entry { filename = "smb-vuln-regsvc-dos.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
    ```
---

### Read through this script. What does it depend on?
- **Answer:** smb-brute
- **Steps to Reproduce:** 
    ```bash
    grep dependencies /usr/share/nmap/scripts/smb-os-discovery.nse 
    dependencies = {"smb-brute"}
    ```
---


# Firewall Evasion
- Typical Windows host will, with its default firewall, block all ICMP packets. Nmap will register a host with this firewall configuration as dead and not bother scanning it at all.
- `-Pn` switch - tells Nmap to not bother pinging the host before scanning it.
- Nmap will always treat the target host as being alive. (effectively bypassing the ICMP block)
- If you're already directly on the local network, Nmap can also use **ARP requests** to determine host activity.
- Some useful switches:
    - `-f` - Used to fragment the packets into smaller pieces (less likely that the packets will be detected by a firewall or IDS).
    - `--mtu <number>` - Accepts a maximum transmission unit size to use for the packets sent. (it must be a multiple of 8).
    - `--scan-delya <time>ms` - used to add delay between packets sent. Useful if the **network is unstable**, to evade any **time-based firewall/IDS triggers**.
    - `--badsum` - used to generate invalid checksum for packets. Can be used to **determine the presence of a firewall/IDS**. Usually this packet would be dropped, however, firewalls may potenitally respond automatically, without bothering to check the checksum of the packet.

## Questions
### Which simple (and frequently relied upon) protocol is often blocked, requiring the use of the -Pn switch?
- **Answer:** ICMP

---
### Which Nmap switch allows you to append an arbitrary length of random data to the end of packets?
- **Answer:** --data-length
- **Note:** In Nmap help,  `--data-length <num>: Append random data to sent packets`.

---

# Practical

### Does the target respond to ICMP ping requests?
- **Answer:** N
- **Steps to Reproduce:** 
    ```bash
    nmap 10.10.78.135
    Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-14 15:00 IST
    Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
    Nmap done: 1 IP address (0 hosts up) scanned in 3.10 seconds
    ```
---

### Perform an Xmas scan on the first 999 ports of the target -- how many ports are shown to be open or filtered?
- **Answer:** 999
- **Steps to Reproduce:** 
```bash
nmap -sX -Pn -p 0-999 --vv 10.10.78.135
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-14 15:12 IST
Initiating Parallel DNS resolution of 1 host. at 15:12
Completed Parallel DNS resolution of 1 host. at 15:12, 0.02s elapsed
Initiating XMAS Scan at 15:12
Scanning 10.10.78.135 [1000 ports]
XMAS Scan Timing: About 15.50% done; ETC: 15:16 (0:02:49 remaining)
XMAS Scan Timing: About 30.50% done; ETC: 15:16 (0:02:19 remaining)
XMAS Scan Timing: About 58.50% done; ETC: 15:16 (0:01:23 remaining)
Completed XMAS Scan at 15:16, 201.25s elapsed (1000 total ports)
Nmap scan report for 10.10.78.135
Host is up, received user-set.
All 1000 scanned ports on 10.10.78.135 are open|filtered because of 1000 no-responses

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 201.37 seconds
           Raw packets sent: 2000 (80.000KB) | Rcvd: 0 (0B)
```
---

### There is a reason given for this -- what is it?
- **Answer:** No response
- **Steps to Reproduce:**
    - In the nmap output, there is a reason mentioned - All 1000 scanned ports on 10.10.78.135 are open|filtered because of 1000 **no-responses**.

---

### Perform a TCP SYN scan on the first 5000 ports of the target -- how many ports are shown to be open?
- **Answer:** 5
- **Steps to Reproduce:** 
```bash
nmap -Pn -sT -p 0-5000 --vv 10.10.78.135
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-14 15:21 IST
Initiating Parallel DNS resolution of 1 host. at 15:21
Completed Parallel DNS resolution of 1 host. at 15:21, 0.01s elapsed
Initiating Connect Scan at 15:21
Scanning 10.10.78.135 [5001 ports]
Discovered open port 21/tcp on 10.10.78.135
Discovered open port 53/tcp on 10.10.78.135
Discovered open port 3389/tcp on 10.10.78.135
Discovered open port 80/tcp on 10.10.78.135
Discovered open port 135/tcp on 10.10.78.135
Completed Connect Scan at 15:22, 33.76s elapsed (5001 total ports)
Nmap scan report for 10.10.78.135
Host is up, received user-set (0.20s latency).
Scanned at 2021-01-14 15:21:41 IST for 34s
Not shown: 4996 filtered ports
Reason: 4996 no-responses
PORT     STATE SERVICE       REASON
21/tcp   open  ftp           syn-ack
53/tcp   open  domain        syn-ack
80/tcp   open  http          syn-ack
135/tcp  open  msrpc         syn-ack
3389/tcp open  ms-wbt-server syn-ack

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 33.88 seconds
```
---

### Deploy the ftp-anon script against the box. Can Nmap login successfully to the FTP server on port 21?
- **Answer:** Y
- **Steps to Reproduce:** 
    ```bash
    nmap -Pn --script ftp-anon.nse 10.10.78.135
    Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
    Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-14 15:23 IST
    Stats: 0:00:31 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
    NSE Timing: About 80.00% done; ETC: 15:23 (0:00:05 remaining)
    Stats: 0:00:40 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
    NSE Timing: About 80.00% done; ETC: 15:23 (0:00:07 remaining)
    Nmap scan report for 10.10.78.135
    Host is up (0.18s latency).
    Not shown: 995 filtered ports
    PORT     STATE SERVICE
    21/tcp   open  ftp
    | ftp-anon: Anonymous FTP login allowed (FTP code 230)
    |_Cant get directory listing: TIMEOUT
    53/tcp   open  domain
    80/tcp   open  http
    135/tcp  open  msrpc
    3389/tcp open  ms-wbt-server

    Nmap done: 1 IP address (1 host up) scanned in 43.28 seconds
    ```
    - Login using ftp username: `anonymous` and empty password.
    ```bash
    ftp 10.10.78.135
    Connected to 10.10.78.135.
    220-FileZilla Server 0.9.60 beta
    220-written by Tim Kosse (tim.kosse@filezilla-project.org)
    220 Please visit https://filezilla-project.org/
    Name (10.10.78.135:root): anonymous
    331 Password required for anonymous
    Password:
    230 Logged on
    Remote system type is UNIX.
    ftp> ls
    200 Port command successful
    150 Opening data channel for directory listing of "/"
    226 Successfully transferred "/"
    ```
    

---
