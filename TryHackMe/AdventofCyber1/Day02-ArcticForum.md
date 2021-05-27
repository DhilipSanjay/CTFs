# Arctic Forum

**Date:** 26, May, 2021

**Author:** Dhilip Sanjay S

---

### What is the path of the hidden page?
- **Answer:** /SysAdmin
- **Steps to Reproduce:** Run Gobuster/Dirbuster

```bash
$ gobuster dir -u http://10.10.8.131:3000 -t 50 -w /usr/share/wordlists/dirb/common.txt | tee gobuster.out
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.8.131:3000
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/05/26 12:04:49 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 302) [Size: 27] [--> /home]
/Admin                (Status: 302) [Size: 27] [--> /home]
/ADMIN                (Status: 302) [Size: 27] [--> /home]
/assets               (Status: 301) [Size: 179] [--> /assets/]
/css                  (Status: 301) [Size: 173] [--> /css/]   
/Home                 (Status: 302) [Size: 28] [--> /login]   
/home                 (Status: 302) [Size: 28] [--> /login]   
/js                   (Status: 301) [Size: 171] [--> /js/]    
/Login                (Status: 200) [Size: 1713]              
/logout               (Status: 302) [Size: 28] [--> /login]   
/login                (Status: 200) [Size: 1713]              
/SysAdmin             (Status: 200) [Size: 1733]              
/sysadmin             (Status: 200) [Size: 1733]              
                                                              
===============================================================
2021/05/26 12:05:16 Finished
===============================================================
```

---

### What is the password you found?
- **Answer:** defaultpass
- **Steps to Reproduce:** 
    - By visiting the SysAdmin page, we see this comment:

    ```html
    <!--
    Admin portal created by arctic digital design - check out our github repo
    -->
    ```
    
    - Google `arctic digital design github`
    - Open their github repository and you'll find:

    ```md
    arctic digital design used for advent of cyber

    Previous versions of this software have been shipped out. The credentials to log in are:

    username: admin
    password: defaultpass
    ** the login portal accepts usernames instead of emails **
    ```
    
---

### What do you have to take to the 'partay'
- **Answer:** Eggnog
- **Steps to Reproduce:** 
    - Login using the defualt credentials
    - You'll find the following inside the admin panel:

    ```html
     Hey all - Please don't forget to BYOE(Bring Your Own Eggnog) for the partay!!
    ```

---