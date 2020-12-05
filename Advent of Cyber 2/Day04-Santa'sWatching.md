# Day 4 - Santa's Watching

**Date:** 04, December, 2020

**Author:** Dhilip Sanjay S

---
## Learning Objectives
- Fuzzing (fancy bruteforcing). Wordlists can be context sensitive.
- [Gobuster/Dirbuster](https://github.com/OJ/gobuster)
- [wfuzz](https://github.com/xmendez/wfuzz)


## Solutions
### Given the URL "http://shibes.xyz/api.php", what would the entire wfuzz command look like to query the "breed" parameter using the wordlist "big.txt" (assume that "big.txt" is in your current directory)
- **Answer:**
```bash
wfuzz -c -z file,big.txt http://shibes.xyz/api.php/breed=FUZZ
```
---
### Use GoBuster to find the API directory. What file is there?
- **Answer:** site-log.php
- **Steps to reproduce:** 
- Run Go buster and save the output in a file for later reference.
```bash
gobuster dir -u http://10.10.238.210/ -w /usr/share/wordlists/dirb/big.txt -t 50 -x php,txt,html | tee GobusterOutput.txt
```

---
### Fuzz the date parameter on the file you found in the API directory. What is the flag displayed in the correct post?
- **Answer:** THM{D4t3_AP1}
- **Steps to reproduce:** 
    - The output of the correct date will have a different size of output character/word. 
    - You can also use `grep` to filter it out.
```bash
wfuzz -c -z file,big.txt --hh 0 http://MACHINE-IP/api/site-log.php?date=FUZZ

curl http://MACHINE-IP/api/site-log.php?date=20201125
```
- **One-liner:**
```bash
curl http://MACHINE-IP/api/site-log.php?date=`wfuzz -c -z file,big.txt --hh 0 http://MACHINE-IP/api/site-log.php?date=FUZZ 2>/dev/null| grep "1 W" | awk '{print $(NF-1)}'| xargs`
```
- **P.S:** Just learning how to craft one liners!

---
[Back to Advent of Cyber 2](/Advent%20of%20Cyber%202) 