# Day 11 - The Rogue Gnome

**Date:** 11, December, 2020

**Author:** Dhilip Sanjay S

---

## Learning Objectives
- Privilege Escalation
    - Horizontal
        - Horizontal privilege escalation attack involves using the intended permissions of a user to abuse a vulnerability to access another user's resources who has similar permissions to you.
    -  Vertical
        - Vertical privilege escalation attack involves exploiting a vulnerability that allows you to perform actions like commands or accessing data acting as a higher privileged account such as an administrator.
- [DVWA](http://www.dvwa.co.uk/)
- [Upgrading Simple shell to interactive shells](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys)

---
- Enumeration
    - Find SSH key
    ```bash
    find / -name id_rsa 2> /dev/null    
    ```

- Privilege Escalation Checklist (Have your own checklist and also refer the standard checklists)
    - [g0tmi1k](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation)
    - [payatu](https://payatu.com/guide-linux-privilege-escalation)

- [SUID Vulnerability](https://gtfobins.github.io/)
    ```bash
    find / -perm -u=s -type f 2>/dev/null    
    ```

- For script kiddies
    - [LinEnum](https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh)
    - Transfer using **Python webserver** or **netcat**
    
- Covering tracks
    - /var/log/auth.log
    - /var/log/syslog
    - /var/log/<service/
        -/var/log/apache2/access.log

- **Note:** Don't shred these files in a real pentesting.

---

## Solutions

### What type of privilege escalation involves using a user account to execute commands as an administrator?
- **Answer:** vertical

---

### What is the name of the file that contains a list of users who are a part of the sudo group?
- **Answer:** sudoers

---

### Enumerate the machine for executables that have had the SUID permission set. Look at the output and use a mixture of GTFObins and your researching skills to learn how to exploit this binary.
- **Answer:** `find / -perm -u=s -type f 2>/dev/null`

---

### What are the contents of the file located at /root/flag.txt?
- **Answer:** thm{2fb10afe933296592}
- **Steps to Reproduce:** Since we have SUID set on `usr/bin/bash`, we can abuse it to get root privilege:
    ```bash
    /bin/bash -p
    ```
- Boom! Now you are root. Go get the flag!
---