# Web Fundamentals
**Date:** 02, December, 2020

**Author:** Dhilip Sanjay S

[Click Here](https://tryhackme.com/room/webfundamentals) to go the TryHackMe room.

---
## HTTP VERBS
- There are 9 HTTP Verbs:
    - GET
    - POST
    - PUT
    - UPDATE
    - DELETE
    - TRACE
    - PATCH
    - CONNECT
    - OPTIONS

## Status codes
- 418 - I'm a teapot 

---
## Solutions
### 1. What's the GET flag?
- **Answer:** thm{162520bec925bd7979e9ae65a725f99f}
- **Steps to reproduce:**
```
curl 10.10.45.155:8081/ctf/get
```

---
### 2. What's the POST flag?
- **Answer:** thm{3517c902e22def9c6e09b99a9040ba09}
- **Steps to reproduce:**
```
curl -X POST 10.10.45.155:8081/ctf/post --data "flag_please"
```
> Options used along with CURL:
> - -X FLAG
> - --data "some text"

---
### 3. What's the "Get a cookie" flag?
- **Answer:** thm{91b1ac2606f36b935f465558213d7ebd}
- **Steps to reproduce:**
    - Visit http://machine-ip:8081/ctf/getcookie
    - Press ```Ctrl+Shift+I``` (or) ```F12``` in browser.
	-  In chrome, go to **Application** Tab and select _Cookies_ under **Storage** section to view the cookies.
	- In Firefox, click on **Storage** tab to view the cookies.

---
### 4. What's the "Set a cookie" flag?
- **Answer:** thm{c10b5cb7546f359d19c747db2d0f47b3}
- **Steps to reproduce:**
    - In firefox, lick on **Storage** tab and then click ```+``` symbol to add the cookie with name and value as **flagpls**.
    - In chrome, use console to add cookie.
    ```js
    document.cookie = "flagpls = flagpls"
    ```
    - Visit http://machine-ip:8081/ctf/sendcookie