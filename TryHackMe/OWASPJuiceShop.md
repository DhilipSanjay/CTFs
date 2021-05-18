# OWASP Juice Shop

**Date:** 18, May, 2021

**Author:** Dhilip Sanjay S

---

## Let's go on an Adventure

### Question #1: What's the Administrator's email address?
- **Answer:** admin@juice-sh.op
- **Steps to Reproduce:** The reviews show each user's email address. Which, by clicking on the Apple Juice product, shows us the Admin email!

### What parameter is used for searching? 
- **Answer:** q
- **Steps to Reproduce:** Check the search box while performing search: `http://10.10.28.40/#/search?q=hi`

###  What show does Jim reference in his review? 
- **Answer:** Star Trek
- **Steps to Reproduce:** 
    - Click on **Green Smoothie**.
    - Check the reviews

    ```bash
    jim@juice-sh.op

    Fresh out of a replicator.
    ```
    
    - On googling **replicator**, we get the result as `Star Trek`.

---

## Inject the Juice

- There are many types of injection attacks:

| Injection Type | Description |
|---------|----------|
| SQL Injection | SQL Injection is when an attacker enters a malicious or malformed query to either retrieve or tamper data from a database. And in some cases, log into accounts. |
| Command Injection | Command Injection is when web applications take input or user-controlled data and run them as system commands. An attacker may tamper with this data to execute their own system commands. This can be seen in applications that perform misconfigured ping tests. |
| Email Injection | Email injection is a security vulnerability that allows malicious users to send email messages without prior authorization by the email server. These occur when the attacker adds extra data to fields, which are not interpreted by the server correctly. |

### Log into the administrator account!
- **Answer:** 32a5e0f21372bcc1000a6088b93b458e41f0e02a
- **Steps to Reproduce:** 
    - Intercept the request in burpsuite while logging in as the admin.
    - Perform SQL Injection by changing the email address: `' or 1==1--`.
    - This will evaluate as true and login as **userid 0**.

### Log into the Bender account!
- **Answer:** fb364762a3c102b2db932069c0e6b78e738d4066
- **Steps to Reproduce:** 
    - Intercept the request in burpsuite while logging in as the bender.
    - Perform SQL Injection by changing the email address: `bender@juice-sh.op' --`.
    - This will comment out the rest of the SQL query and check only the email address, which happens to be existing in the SQL Table and hence will return True.

---

## Who broke my lock?!

### Bruteforce the Administrator account's password!
- **Answer:** c2110d06dc6f81c67cd8099ff0ba601241f1ac0e
- **Steps to Reproduce:**
    - Foward the login request to intruder in burpsuite
    - Add `$$` to the password in the request body.
    - Use the payload - `/usr/share/seclists/Passwords/Common-Credentials/best1050.txt`
    - Look for 200 status code:
        - 401 (Unauthorized)
        - 200 (OK)
    - The admin's password is `admin123`. Very secure password..hehe!

### Reset Jim's password!
- **Answer:** 094fbc9b48e525150ba97d05b942bbf114987257
- **Steps to Reproduce:** 
    - We can reset Jim's flag by using **Forgot password**.
    - The security question for Jim: **Your eldest siblings middle name?**
    - This can be obtained from the [wikipedia page](https://en.wikipedia.org/wiki/James_T._Kirk) on googling: `jim star trek`
    - The family members list:

    ```bash
    George Kirk (father)
    Winona Kirk (mother)
    George Samuel Kirk (brother)
    Tiberius Kirk (grandfather)
    James (maternal grandfather)
    Aurelan Kirk (sister-in-law)
    Peter Kirk (nephew)
    2 other nephews
    ```
    - So, middle name of his elder brother - `Samuel`
    
---

## AH! Don't look!

### Access the Confidential Document!
- **Answer:** edf9281222395a1c5fee9b89e32175f1ccf50c5b
- **Steps to Reproduce:** 
    - You can see the contents of ftp folder by visiting: `http://10.10.28.40/ftp/`
    - View and download the **acquisitions.md** file. `http://10.10.28.40/ftp/acquisitions.md`
    - Now navigate back to the home page.

### Log into MC SafeSearch's account!
- **Answer:** 66bdcffad9e698fd534003fbb3cc7e2b7b55d7f0
- **Steps to Reproduce:** 
    - Watch the [Youtube Video - Rap](https://www.youtube.com/watch?v=v59CX2DiX0Y)
    - He notes that his password is "Mr. Noodles" but he has replaced some "vowels into zeros", meaning that he just replaced the o's into 0's.
    - We now know the password to the `mc.safesearch@juice-sh.op` account is `Mr. N00dles`.

### Download the Backup file! (package.json.bak)
- **Answer:** 
- **Steps to Reproduce:** 
    - Initially, on visiting `http://10.10.28.40/ftp/package.json.bak`, we get the following error:

    ```html
    OWASP Juice Shop (Express ^4.17.1)
    403 Error: Only .md and .pdf files are allowed!

       at verify (/juice-shop/routes/fileServer.js:30:12)
       at /juice-shop/routes/fileServer.js:16:7
       at Layer.handle [as handle_request] (/juice-shop/node_modules/express/lib/router/layer.js:95:5)
       at trim_prefix (/juice-shop/node_modules/express/lib/router/index.js:317:13)
       at /juice-shop/node_modules/express/lib/router/index.js:284:7
       at param (/juice-shop/node_modules/express/lib/router/index.js:354:14)
       at param (/juice-shop/node_modules/express/lib/router/index.js:365:14)
       at Function.process_params (/juice-shop/node_modules/express/lib/router/index.js:410:3)
       at next (/juice-shop/node_modules/express/lib/router/index.js:275:10)
       at /juice-shop/node_modules/serve-index/index.js:135:16
       at FSReqCallback.oncomplete (fs.js:171:21)
    ```

    - To get around this, we'll use **Poison Null Byte**.
    - A Poison Null Byte looks like this: `%00`.
    - Perform URL encoding of `%00` -> `%2500`.
    - Adding this and then a `.md` or `.pdf` to the end will bypass the 403 error!
    - **Why does it work**?
        - A Poison Null Byte is actually a **NULL terminator**. 
        - By placing a NULL character in the string at a certain byte, the string will tell the server to terminate at that point, nulling the rest of the string. 

---

## Who's flying this thing?

- When **Broken Access Control exploits or bugs** are found, it will be categorised into one of two types:

| Type | Description |
|---------|----------|
| Horizontal Privilege Escalation | Occurs when a user can perform an action or access data of another user with the same level of permissions.  |
| Vertical Privilege Escalation | Occurs when a user can perform an action or access data of another user with a higher level of permissions. |

### Access the administration page!
- **Answer:** 946a799363226a24822008503f5d1324536629a0
- **Steps to Reproduce:** 
    - In this file `http://10.10.28.40/main-es2015.js`, we have the following code:

    ```js
    Xs = [
    {
    path: 'administration',
    component: Xi,
    canActivate: [
    _
    ]
    ```

    - This reveals the location of **administration page**.
    
### View another user's shopping basket!
- **Answer:** 41b997a36cc33fbe4f0ba018474e19ae5ce52121
- **Steps to Reproduce:** 
    - Keeping the intercept on, forward the requests until you hit the request: `GET /rest/basket/1 HTTP/1.1`
    - Now change number 1 after `/basket/` to 2.
    - Foward the request.
    - It will now show you the basket of UserID 2

### Remove all 5-star reviews!
- **Answer:** 50c97bcce0b895e446d61c83a21df371ac2266ef
- **Steps to Reproduce:** 
    - Navigate to the `http://10.10.28.40/#/administration` page again and click the bin icon next to the review with 5 stars!

---

## Where did that come from?

- XSS or Cross-site scripting is a vulnerability that allows attackers to run javascript in web applications. 
- Their complexity ranges from easy to extremely hard, as each web application parses the queries in a different way.
- Three major type of XSS Attacks:

| Type | Description |
|---------|----------|
| DOM (Special) | DOM XSS (Document Object Model-based Cross-site Scripting) uses the HTML environment to execute malicious javascript. This type of attack commonly uses the <script></script> HTML tag. |
| Persistent (Server-side) | Persistent XSS is javascript that is run when the server loads the page containing it. These can occur when the server does not sanitise the user data when it is uploaded to a page. These are commonly found on blog posts. |
| Reflected (Client-side) | Reflected XSS is javascript that is run on the client-side end of the web application. These are most commonly found when the server doesn't sanitise search data. |

### Perform a DOM XSS!
- **Answer:** 9aaf4bbea5c30d00a1f5bbcfce4db6d4b0efe0bf
- **Steps to Reproduce:** 
    - Use the following code in the search box:

    ```js
    <iframe src="javascript:alert(`xss`)"> s
    ```
    
###  Perform a persistent XSS!
- **Answer:** 149aa8ce13d7a4a8a931472308e269c94dc5f156
- **Steps to Reproduce:** 
    - We are going to manipulate the last login Ip to get persistent XSS
    - Add the following header in the request : `GET /rest/saveLoginIp HTTP/1.1`

    ```
    True-Client-IP: <iframe src="javascript:alert(`xss`)">
    ```
    
    - The **True-Client-IP** header is similar to the **X-Forwarded-For** header, both tell the server or proxy what the IP of the client is.
    - Due to there being no sanitation in the header we are able to perform an XSS attack.


### Perform a reflected XSS!
- **Answer:** 23cefee1527bde039295b2616eeb29e1edc660a0
- **Steps to Reproduce:** 
    - Visit the **Order History** page in the admin account.
    - Click on the truck icon of any of the orders.
    - You'll be navigated to the page: `http://10.10.28.40/#/track-result?id=id-goes-here`
    - Replace the value to ID to XSS payload: `<iframe src="javascript:alert(`xss`)">`
    - Final address will look like this:

    ```
    http://10.10.28.40/#/track-result?id=<iframe src="javascript:alert(`xss`)">    
    ```
    
    - The server will have a lookup table or database (depending on the type of server) for each tracking ID. As the 'id' parameter is not sanitised before it is sent to the server, we are able to perform an XSS attack.  

---

## Exploration

### Access the /#/score-board/ page
- **Answer:** 7efd3174f9dd5baa03a7882027f2824d2f72d86ess
- **Steps to Reproduce:** Visit `http://10.10.28.40/#/score-board`
    - Here you can see your completed tasks as well as other tasks in varying difficulty.
---

## References

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)