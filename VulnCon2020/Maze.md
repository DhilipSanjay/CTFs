# Maze

**Date:** 19, December, 2020

**Author:** Dhilip Sanjay S

---

- Find the directories using **gobuster**
```bash
root@kali: gobuster dir -u http://maze.noobarmy.org/ -w /usr/share/wordlists/dirb/common.txt -t 50 | tee gobusterOutput2.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://maze.noobarmy.org/
[+] Threads:        50
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/12/19 23:00:37 Starting gobuster
===============================================================
/.htpasswd (Status: 403)
/.hta (Status: 403)
/.htaccess (Status: 403)
/index.php (Status: 200)
/projects (Status: 301)
/server-status (Status: 403)
===============================================================
2020/12/19 23:01:22 Finished
===============================================================
```

- Go to `http://maze.noobarmy.org/projects`
    - View source code.
    - You will find this commented:
    ```html    
    <img src="justsomerandomfoldername/image-0.png">
    ```

- Navigate to `http://maze.noobarmy.org/projects/justsomerandomfoldername/image-0.png`
    - You'll find a QR code.
    - By continuing, QR codes will be available till `image-27.png`.
        - **P.S:** 27 is my lucky number
    - Manually doing this is a tedious process. 

- Use `zbar-tools`:
    - `apt-get install zbar-tools`
    - Bash one liner (curl + zbarimg):
        ```bash
        root@kali: for i in {0..27}; do curl http://maze.noobarmy.org/projects/justsomerandomfoldername/image-$i.png > image-$i.png 2>/dev/null && zbarimg image-$i.png 2>/dev/null; done

        QR-Code:Hello
        QR-Code:and
        QR-Code:welcome
        QR-Code:to
        QR-Code:this
        QR-Code:challenge!
        QR-Code:We
        QR-Code:hope
        QR-Code:that
        QR-Code:collecting
        QR-Code:these
        QR-Code:images
        QR-Code:was
        QR-Code:not
        QR-Code:that
        QR-Code:hard
        QR-Code:for
        QR-Code:you,
        QR-Code:anyways
        QR-Code:just
        QR-Code:so
        QR-Code:you
        QR-Code:know
        QR-Code:i
        QR-Code:love
        QR-Code:the
        QR-Code:number
        QR-Code:13
        ```
- Upload the image-13.png or paste the URL in [Exif tool](http://exif.regex.info/exif.cgi). You'll get the following data:
    ```
    XMP Toolkit	Image::ExifTool 12.04
    Creator	Decoded as:
    ihyapba{j@$_7u1$_3i3a_@_j3o_pu@yy3at3?}
    ```
- `ROT 13` decode the `creator` data to get the flag.
    ```
    vulncon{w@$_7h1$_3v3n_@_w3b_ch@ll3ng3?}
    ```