# Day 1 - Christmas Crisis

**Date:** 01, December, 2020

**Author:** Dhilip Sanjay S

---
## Fundamentals
- The Web
	- DNS, IP address (Try [Intro to Networking room](https://tryhackme.com/room/introtonetworking))
- HTTP(S)
	- HTTP headers, request, response (Try [Web Fundamentals room](https://tryhackme.com/room/webfundamentals))
- Cookies
	- Editing cookie name and value - Privilege escalation
---
## Solutions
### 1. What is the name of the cookie used for authentication?
- **Answer:** auth
- **Steps to reproduce:**
	- Press `Ctrl+Shift+I` (or) `F12` in browser.
	-  In chrome, go to **Application** Tab and select _Cookies_ under **Storage** section to view the cookies.
	- In Firefox, click on **Storage** tab to view the cookies.

---
### 2. In what format is the value of this cookie encoded?
- **Answer:** Hexadecimal
- **Steps to reproduce:**
	- It contains characters - 1 to 9, a to f.
	- Use [CyberChef](https://gchq.github.io/CyberChef/) to find out the format.

---
### 3. Having decoded the cookie, what format is the data stored in?
- **Answer:** JSON
- **Steps to reproduce:**
	- Use [CyberChef](https://gchq.github.io/CyberChef/) and select **From Hex** option to decode.
	- By seeing the format `{key: value}`, you can identify it's JSON.

---
### 4. Figure out how to bypass the authentication. What is the value of Santa's cookie?
- **Answer:** 7b22636f6d70616e79223a22546865204265737420466573746976616c20436f6d70616e79222c2022757365726e616d65223a2273616e7461227d
- **Steps to reproduce:**
	- Remember, the cookie is used to identify the user account in this web app.
	- So, modify the `username` parameter in the decoded JSON format and encode back **To Hex** using CyberChef.
	- Your modified cookie JSON must look like this:
	```json
	{"company":"The Best Festival Company", "username":"santa"}
	```
> [CWE-565](https://cwe.mitre.org/data/definitions/565.html): Reliance on Cookies without Validation and Integrity Checking
---
### 5. What is the flag you're given when the line is fully active?
- **Answer:** THM{MjY0Yzg5NTJmY2Q1NzM1NjBmZWFhYmQy}
- **Steps to reproduce:**
	- Turn all the options to _Active_ in the web app, the flag will be displayed at the bottom.
	
---