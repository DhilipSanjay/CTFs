# Day 16 - Help! Where is Santa?

**Date:** 16, December, 2020

**Author:** Dhilip Sanjay S

---

## Solutions

### What is the port number for the web server?
- **Answer:** 8000
- **Steps to Reproduce:** Run Nmap, Rustscan, etc.
    ```bash
    root@kali:nmap MACHINE_IP

    Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-17 22:08 IST
    Nmap scan report for MACHINE_IP
    Host is up (0.18s latency).
    Not shown: 999 closed ports
    PORT     STATE SERVICE
    8000/tcp open  http-alt

    Nmap done: 1 IP address (1 host up) scanned in 2.36 seconds
    ```
---

### Without using enumerations tools such as Dirbuster, what is the directory for the API?  (without the API key)
- **Answer:** /api/
- **Steps to Reproduce:** Write a simple python script
    **Code:**
    ```python
    import requests
    from bs4 import BeautifulSoup as bs
    import sys

    if len(sys.argv) < 2:
            print("SYNTAX: \npython3 findLink.py <MACHINE_IP:PORT>")
            exit(0)

    r = requests.get("http://" + str(sys.argv[1]))
    # print(r.text)
    soup = bs(r.text, 'lxml')
    links = soup.find_all('a')
    #print(links)
    for link in links:
            if "href" in link.attrs and link["href"] != "#" :
                    print(link["href"])
    ```

    **Output:**
    ```bash
    root@kali:python3 findLink.py MACHINE_IP:8000 | uniq
    ../
    https://github.com/BulmaTemplates/bulma-templates/blob/master/templates/hero.html
    https://tryhackme.com
    http://machine_ip/api/api_key
    https://github.com/BulmaTemplates/bulma-templates    
    ```
---

### Find out the correct API key. Remember, this is an odd number between 0-100. After too many attempts, Santa's Sled will block you. 
- **Answer:** 57
- **Steps to Reproduce:**
    **Code:**
    ```python
    import requests
    import sys

    if len(sys.argv) < 2:
            print("SYNTAX: \npython3 findApiKey.py <MACHINE_IP:PORT>")
            exit(0)

    url = "http://" + str(sys.argv[1]) + "/api/"
    for i in range(1, 100, 2):
            r = requests.get(url+str(i))
            print(r.text)
    ```
    
    **Output:**
    ```bash
    root@kali:python3 findApiKey.py MACHINE_IP:8000 > output.txt 
    ```
---

### Where is Santa right now?
- **Answer:** Winter Wonderland, Hyde Park, London
- **Steps to Reproduce:**
    ```bash
    root@kali:cat output.txt | grep -v "Error"
    {"item_id":57,"q":"Winter Wonderland, Hyde Park, London."}
    ```
---
[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202) 