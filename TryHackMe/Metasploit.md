# Metasploit

**Date:** 23, February, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/rpmetasploit) to go to the TryHackMe room.

## Metasploit
- Metasploit, an open-source pentesting framework, is a powerful tool utilized by security engineers around the world. 
- Maintained by **Rapid 7**, Metasploit is a collection of not only thoroughly tested exploits but also auxiliary and post-exploitation tools.

## Initializing
- Initialize the database: `msfdb init`
- To view the advance options we can trigger for starting the console: `msfconsole -h`
- Start Metasploit using the command: `msfconsole`
- To check whether we have connected to the database: `db_status`
### We can start the Metasploit console on the command line without showing the banner or any startup information as well. What switch do we add to msfconsole to start it without showing this information? This will include the '-'
- **Answer:** -q
- **Steps to Reproduce:** By running `msfconsole -h`, we get to know that `-q` or `--quiet` option is used to start msfconsole withou banner.

---
### Cool! We've connected to the database, which type of database does Metasploit 5 use? 
- **Answer:** postgresql
- **Steps to Reproduce:**
```bash
msf6 > db_status
[*] Connected to msf. Connection type: postgresql.
```
---
## Commands
- To view the help menu: `help`

### The help menu has a very short one-character alias, what is it?
- **Answer:** ?

### Finding various modules we have at our disposal within Metasploit is one of the most common commands we will leverage in the framework. What is the base command we use for searching?
- **Answer:** search

### Once we've found the module we want to leverage, what command we use to select it as the active module?
- **Answer:** use

### How about if we want to view information about either a specific module or just the active one we have selected?
- **Answer:** info

### Metasploit has a built-in netcat-like function where we can make a quick connection with a host simply to verify that we can 'talk' to it. What command is this?
- **Answer:** connect

### Entirely one of the commands purely utilized for fun, what command displays the motd/ascii art we see when we start msfconsole (without -q flag)?
- **Answer:** banner

### Metasploit supports the use of global variables, something which is incredibly useful when you're specifically focusing on a single box. What command changes the value of a variable globally? 
- **Answer:** setg

### Now that we've learned how to change the value of variables, how do we view them? \
- **Answer:** get

### How about changing the value of a variable to null/no value?
- **Answer:** unset

### What command can we use to set our console output to save to a file?
- **Answer:** spool

###  What command can we use to store the settings/active datastores from Metasploit to a settings file? This will save within your msf4 (or msf5) directory and can be undone easily by simply removing the created settings file.
- **Answer:** save

---
## Modules for every occassion
- Metasploit consists of six core modules that make up the bulk of the tools

### Metasploit Filesystem
- Data
- Documentation
- Lib
- Modules
    1. Auxiliary
    2. Encoders
    3. Exploits
    4. Nops
    5. Payloads
    6. Post
- Plugins
- Scripts
- Tools

## Metasploit Libraries
- Number of MSF libraries allow us to run oru exploits without writing additional code for rudimentary tasks.
1. **REX** - Handles sockets, protocols, text transformations.
1. **MSF::CORE** - provies the basic API
1. **MSF::BASE** - Provides friendly API

## Metasploit modules
1. Exploits - Modules that use payloads
1. Auxiliary - Port scanners, fuzzers, sniffers, etc.
1. Payloads - consists of code that runs remotely
1. Encoders - ensure that payloads make it ot their destination intact
1. Nops - (No OPeration) keep the payload sizes consistent across exploit attempts.
1. Post - post exploitation module

- **Loading Addition module trees**
    1. While starting - `msfconsole -m ~/secret-modules/`
    1. Inside msfconsole - `loadpath /path/to/modules/exploits`

### Easily the most common module utilized, which module holds all of the exploit code we will use?
- **Answer:** exploit

### Used hand in hand with exploits, which module contains the various bits of shellcode we send to have executed following exploitation?
- **Answer:** payload

### Which module is most commonly used in scanning and verification machines are exploitable? This is not the same as the actual exploitation of course.
- **Answer:** auxiliary

### One of the most common activities after exploitation is looting and pivoting. Which module provides these capabilities?
- **Answer:** post

### Commonly utilized in payload obfuscation, which module allows us to modify the 'appearance' of our exploit such that we may avoid signature detection?
- **Answer:** encoder

### Last but not least, which module is used with buffer overflow and ROP attacks?
- **Answer:** nop

### Not every module is loaded in by default, what command can we use to load different modules? 
- **Answer:** load

---
## Move that shell!

## We're in, now what?

## Makin' Cisco proud


---

## Reference Links
- [Offensive Security - Metasploit Unleashed](https://www.offensive-security.com/metasploit-unleashed/metasploit-architecture/)
- [Understanding NOPS](https://security.stackexchange.com/questions/30497/nops-in-metasploit)
