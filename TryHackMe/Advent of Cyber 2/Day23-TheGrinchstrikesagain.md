# Day 23 - The Grinch strikes again!

**Date:** 23, December, 2020

**Author:** Dhilip Sanjay S

---

## Fundamentals
- Volume Shadow Copy Service (VSS) - used to create a consistent shadow copy or snapshot.
- Task Scheduler - automatically perform routine tasks (actions) based on triggers.
- Disk Management - system utility in Windows that enables you to perform advanced storage tasks.
    - If you don't specify a drive letter for a partition, it will not be visible in windows explorer.


## Solutions

### Decrypt the fake 'bitcoin address' within the ransom note. What is the plain text value?
- **Answer:** nomorebestfestivalcompany
- **Steps to Reproduce:** 
    - Base64 decode : `bm9tb3JlYmVzdGZlc3RpdmFsY29tcGFueQ==`

---

### At times ransomware changes the file extensions of the encrypted files. What is the file extension for each of the encrypted files?
- **Answer:** .grinch
- **Steps to Reproduce:** You can find many files with this extension. 

---

### What is the name of the suspicious scheduled task?
- **Answer:** opidsfsdf
- **Steps to Reproduce:** Check in Task Scheduler.

---

### Inspect the properties of the scheduled task. What is the location of the executable that is run at login?
- **Answer:** C:\Users\Administrator\Desktop\opidsfsdf.exe
- **Steps to Reproduce:** Check in Task Scheduler.

---

### There is another scheduled task that is related to VSS. What is the ShadowCopyVolume ID?
- **Answer:** 7a9eea15-0000-0000-0000-010000000000
- **Steps to Reproduce:** Check in Task Scheduler. Open the one with the name `ShadowCopyVolume{...}`.

---

### Assign the hidden partition a letter. What is the name of the hidden folder?
- **Answer:** confidential
- **Steps to Reproduce:** Once you enable `View Hidden Items` in `Z:\Backup`, you'll able to see this folder.

---

### Right-click and inspect the properties for the hidden folder. Use the 'Previous Versions' tab to restore the encrypted file that is within this hidden folder to the previous version. What is the password within the file?
- **Answer:** m33pa55w0rdIZseecure!

---

[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202)