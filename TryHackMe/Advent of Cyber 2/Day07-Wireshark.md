# Day 7 - The Grinch Really Did Steal Christmas

**Date:** 07, December, 2020

**Author:** Dhilip Sanjay S

---
## Fundamentals
- Introduction to IP
- TCP/IP and UDP/IP
    - Three way handshake
- Wireshark Basics

---
## Solutions

### Open "pcap1.pcap" in Wireshark. What is the IP address that initiates an ICMP/ping?
- **Answer:**  10.11.3.2
- **Steps to reproduce:** 
    - Filter `icmp`.
---

### If we only wanted to see HTTP GET requests in our "pcap1.pcap" file, what filter would we use?
- **Answer:** `http.request.method eq get`
---

### Now apply this filter to "pcap1.pcap" in Wireshark, what is the name of the article that the IP address "10.10.67.199" visited?
- **Answer:** reindeer-of-the-week
- **Steps to reproduce:** 
    - Filter `http`.
    - You can find `post/reindeer-of-the-week` by scrolling through.
---

### Look at the captured FTP traffic; what password was leaked during the login process?
- **Answer:** plaintext_password_fiasco
- **Steps to reproduce:** 
    - Filter `ftp`.
    - Look for Login failed and PASS.
---

### Continuing with our analysis of "pcap2.pcap", what is the name of the protocol that is encrypted?
- **Answer:** SSH
---

### Analyse "pcap3.pcap" and recover Christmas! What is on Elf McSkidy's wishlist that will be used to replace Elf McEager?
- **Answer:** Rubber ducky 
- **Steps to reproduce:** 
    - Export HTTP
    - Select `Christmas.zip`
    - After extracting, you check out the file `elf_mcskidy_wishlist.txt`
---
[Back to Advent of Cyber 2](/Advent%20of%20Cyber%202) 