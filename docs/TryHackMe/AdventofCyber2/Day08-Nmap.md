# What's Under the Christmas Tree?

**Date:** 08, December, 2020

**Author:** Dhilip Sanjay S

---

## Introduction to Nmap
- [PTES](http://www.pentest-standard.org/index.php/Main_Page)
- TCP Scanning
    - Connect Scan - `nmap -sT <ip>`
        - Administrative privileges required.
        - Berkley Socket API
        - Will be logged

    - SYN Scan - `nmap -sS <ip>`
        - After the **SYN/ACK** is received from the remote host, a **RST** packet is sent by the host that we are scanning from (never completing the connection).

- UDP Scanning - `nmap -sU <ip>`
- Nmap Timing Templates (T0-T5)
- Nmap's Scripting Engine (NSE) - `nmap --script ftp-proftpd-backdoor -p 21 <ip_address>`
    - Scripts can be used to automate various actions such as:
        - Exploitation
        - Fuzzing
        - Bruteforcing

## Defending against Nmap Scans
- Attempt to hide a service by changing its port number to something other than the standard (such as changing SSH from port 22 to 2222)
- Open-source Intrusion Detection **(IDS)** & Prevention Systems **(IPS)**
    - [Snort](https://www.snort.org/)
    - [Suricata](https://suricata-ids.org/)
- Services on firewall such as [pfSense](https://www.pfsense.org/)

#### [Emerging Threats](https://rules.emergingthreats.net/)

---
## Solutions

### When was Snort created?
- **Answer:** 1998

---
### Using Nmap on 10.10.137.242, what are the port numbers of the three services running?  (Please provide your answer in ascending order/lowest -> highest, separated by a comma)
- **Answer:** 80,2222,3389
- **Note:** 3389 - ms-wbt-server (Microsoft Windows Based Terminal - RDP)
---

### Use Nmap to determine the name of the Linux distribution that is running, what is reported as the most likely distribution to be running?
- **Answer:** Ubuntu
- `nmap -A MACHINE-IP`
---

### Use Nmap's Network Scripting Engine (NSE) to retrieve the "HTTP-TITLE" of the webserver. Based on the value returned, what do we think this website might be used for?
- **Answer:** Blog
- `nmap --script http-title.nse MACHINE-IP`
---

### Other Scans
- To run all the vuln scripts - `nmap --script vuln MACHINE-IP`
- -A option is equivalent to -O, -sV, -sC 
---