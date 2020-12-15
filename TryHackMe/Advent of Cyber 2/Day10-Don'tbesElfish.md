# Day 10 - Don't be sElfish!

**Date:** 10, December, 2020

**Author:** Dhilip Sanjay S

---

## SMB

## Solutions

### 1) Using enum4linux, how many users are there on the Samba server?
- **Answer:** 3
- **Steps to Reproduce:** 
    - `enum4linux -S <MACHINE_IP>`
---
### 2) Now how many "shares" are there on the Samba server?
- **Answer:** 4
- **Steps to Reproduce:** 
    - `enum4linux -U <MACHINE_IP>`
    - `smbclient -L 10.10.167.69`

---
### 3) Use smbclient to try to login to the shares on the Samba server. What share doesn't require a password?
- **Answer:** tbfc-santa
- **Steps to Reproduce:** After running the previous command, you can see the following output:
    ```bash
    [+] Attempting to map shares on 10.10.194.140
    //10.10.194.140/tbfc-hr Mapping: DENIED, Listing: N/A
    //10.10.194.140/tbfc-it Mapping: DENIED, Listing: N/A
    //10.10.194.140/tbfc-santa      Mapping: OK, Listing: OK
    //10.10.194.140/IPC$    [E] Can't understand response:
    NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*
    ```
    - Only tbfc-santa doesn't require any password.

---
### 4) Log in to this share, what directory did ElfMcSkidy leave for Santa?
- **Answer:** jingle-tunes
- **Steps to Reproduce:** Type `ls` to see all the files and folders available 

---
[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202) 