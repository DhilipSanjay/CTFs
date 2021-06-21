# Juicy Details

**Date:** 21, June, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/juicydetails) to go to the TryHackMe room.


## Reconnaissance

### What tools did the attacker use?
- **Answer:** nmap, hydra, sqlmap, curl, feroxbuster
- **Steps to Reproduce:** `grep -v Firefox access.log  | more`

---

### What endpoint was vulnerable to a brute-force attack?
- **Answer:** /rest/user/login
- **Steps to Reproduce:** `grep Hydra access.log`

---

### What endpoint was vulnerable to SQL injection?
- **Answer:** /rest/products/search
- **Steps to Reproduce:** `grep sqlmap access.log`

---

### What parameter was used for the SQL injection?
- **Answer:** q 

---

### What endpoint did the attacker try to use to retrieve files? (Include the /)
- **Answer:** /ftp
- **Steps to Reproduce:** 
    - `grep feroxbuster access.log`
    - `cat vsftpd.logls`
    - FTP is commonly used for file retrievals.

---

## Stolen Data

### What section of the website did the attacker use to scrape user email addresses?
- **Answer:** Product Reviews
- **Steps to Reproduce:** 
    - In the **access.log** we can find many request with different ids to the Product review: `GET /rest/products/6/reviews HTTP/1.1" 200 170 "http://192.168.10.4/`

---

### Was their brute-force attack successful? If so, what is the timestamp of the successful login? 
- **Answer:** Yay, 11/Apr/2021:09:16:31 +0000
- **Steps to Reproduce:** 

```bash
$ grep Hydra access.log | grep 200
::ffff:192.168.10.5 - - [11/Apr/2021:09:16:31 +0000] "POST /rest/user/login HTTP/1.0" 200 831 "-" "Mozilla/5.0 (Hydra)"
```


---

### What user information was the attacker able to retrieve from the endpoint vulnerable to SQL injection?
- **Answer:** email, password
- **Steps to Reproduce:** 
    - Check the last few lines of the result: `grep -i select access.log | grep 200`

---

### What files did they try to download from the vulnerable endpoint?
- **Answer:** www-data.bak, coupons_2013.md.bak
- **Steps to Reproduce:** 

```bash
$ grep download vsftpd.log -i
Sun Apr 11 09:35:45 2021 [pid 8154] [ftp] OK DOWNLOAD: Client "::ffff:192.168.10.5", "/www-data.bak", 2602 bytes, 544.81Kbyte/sec
Sun Apr 11 09:36:08 2021 [pid 8154] [ftp] OK DOWNLOAD: Client "::ffff:192.168.10.5", "/coupons_2013.md.bak", 131 bytes, 3.01Kbyte/sec
```


---

### What service and account name were used to retrieve files from the previous question?
- **Answer:** ftp, anonymous
- **Steps to Reproduce:** Check `vsftpd.log` file

---

### What service and username were used to gain shell access to the server?
- **Answer:** ssh, www-data
- **Steps to Reproduce:** 

```bash
grep accepted -i auth.log 
Apr 11 09:41:19 thunt sshd[8260]: Accepted password for www-data from 192.168.10.5 port 40112 ssh2
Apr 11 09:41:32 thunt sshd[8494]: Accepted password for www-data from 192.168.10.5 port 40114 ssh2
```

---