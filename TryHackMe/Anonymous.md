# Anonymous

**Date:** 10, December, 2020

**Author:** Dhilip Sanjay S

---

### Enumerate the machine.  How many ports are open?
- **Answer:** 4
- **Steps to Reproduce:** `nmap <MACHINE_IP>`

---

### What service is running on port 21?
- **Answer:** FTP

---

### What service is running on ports 139 and 445?
- **Answer:** SMB

---

### There's a share on the user's computer.  What's it called?
- **Answer:** pics
- **Steps to Reproduce:** `enum4linux -S <MACHINE_IP>`

---

### user.txt
- **Steps:** 
    - Place the code for reverse shell in script file `clean.sh`.
    - Listen for incoming connections using `netcat`.

---

### root.txt
- **Steps to Previlege Escalation:** 
    - Abusing SUID for Linux Previlege Escalation.
    - Once you have the reverse shell, [SUID3NUM](https://github.com/Anon-Exploiter/SUID3NUM) script can be used to find the vulnerabilities.
---