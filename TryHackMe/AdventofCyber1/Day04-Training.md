# Day 04 - Training

**Date:** 27, May, 2021

**Author:** Dhilip Sanjay S

---

### Connect to the Server using SSH

```bash
ssh mcsysadmin@10.10.16.57
The authenticity of host '10.10.16.57 (10.10.16.57)' can't be established.
ECDSA key fingerprint is SHA256:IYNPmKurskzl/7GcTFinzOmOKtpQWPkd4wxR3P3pvvI.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.16.57' (ECDSA) to the list of known hosts.
mcsysadmin@10.10.16.57's password: 
Last login: Wed Dec  4 19:50:16 2019 from 82.34.52.37

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[mcsysadmin@ip-10-10-16-57 ~]$
```


### How many visible files are there in the home directory(excluding ./ and ../)?
- **Answer:** 8
- **Steps to Reproduce:** 

```bash
[mcsysadmin@ip-10-10-16-57 ~]$ ls
file1  file2  file3  file4  file5  file6  file7  file8
```

---

### What is the content of file5?
- **Answer:** recipes
- **Steps to Reproduce:** 

```bash
[mcsysadmin@ip-10-10-16-57 ~]$ cat file5
recipes
```

---

### Which file contains the string ‘password’?
- **Answer:** file6
- **Steps to Reproduce:** 

```bash
mcsysadmin@ip-10-10-16-57 ~]$ grep password *
file6:passwordHpKRQfdxzZocwg5O0RsiyLSVQon72CjFmsV4ZLGjxI8tXYo1NhLsEply
```

---

### What is the IP address in a file in the home folder?
- **Answer:** 10.0.0.05
- **Steps to Reproduce:** 

```bash
[mcsysadmin@ip-10-10-16-57 ~]$ grep "[0-9]*\.[0-9]*\.[0-9]*\.[0-9]*" *
file2:10.0.0.05eXWx4auBc8Swra4aPvIoBre+PRsVgu9GVbGwD33X8bd7TWwlZxzSVYa
```

---

### How many users can log into the machine?
- **Answer:** 3
- **Steps to Reproduce:** 

    - Root user + 2 other users

```bash
[mcsysadmin@ip-10-10-16-57 ~]$ pwd
/home/mcsysadmin
[mcsysadmin@ip-10-10-16-57 ~]$ cd ..
[mcsysadmin@ip-10-10-16-57 home]$ ls
ec2-user  mcsysadmin
```

---

### What is the sha1 hash of file8?
- **Answer:** fa67ee594358d83becdd2cb6c466b25320fd2835
- **Steps to Reproduce:** 

```bash
[mcsysadmin@ip-10-10-16-57 ~]$ sha1sum file8
fa67ee594358d83becdd2cb6c466b25320fd2835  file8
```

---

### What is mcsysadmin’s password hash?
- **Answer:** $6$jbosYsU/$qOYToX/hnKGjT0EscuUIiIqF8GHgokHdy/Rg/DaB.RgkrbeBXPdzpHdMLI6cQJLdFlS4gkBMzilDBYcQvu2ro/
- **Steps to Reproduce:** 

```bash
[mcsysadmin@ip-10-10-16-57 /]$ find / -name shadow.bak 2>/dev/null
/var/shadow.bak

[mcsysadmin@ip-10-10-16-57 /]$ cat /var/shadow.bak 
root:*LOCK*:14600::::::
bin:*:17919:0:99999:7:::
daemon:*:17919:0:99999:7:::
adm:*:17919:0:99999:7:::
lp:*:17919:0:99999:7:::
sync:*:17919:0:99999:7:::
shutdown:*:17919:0:99999:7:::
halt:*:17919:0:99999:7:::
mail:*:17919:0:99999:7:::
operator:*:17919:0:99999:7:::
games:*:17919:0:99999:7:::
ftp:*:17919:0:99999:7:::
nobody:*:17919:0:99999:7:::
systemd-network:!!:18218::::::
dbus:!!:18218::::::
rpc:!!:18218:0:99999:7:::
libstoragemgmt:!!:18218::::::
sshd:!!:18218::::::
rpcuser:!!:18218::::::
nfsnobody:!!:18218::::::
ec2-instance-connect:!!:18218::::::
postfix:!!:18218::::::
chrony:!!:18218::::::
tcpdump:!!:18218::::::
ec2-user:!!:18234:0:99999:7:::
mcsysadmin:$6$jbosYsU/$qOYToX/hnKGjT0EscuUIiIqF8GHgokHdy/Rg/DaB.RgkrbeBXPdzpHdMLI6cQJLdFlS4gkBMzilDBYcQvu2ro/:18234:0:99999:7:::
```

---