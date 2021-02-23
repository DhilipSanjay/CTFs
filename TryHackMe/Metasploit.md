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

### Easily the most common module utilized, which module holds all of the exploit code we will use?
- **Answer:** exploit

### Used hand in hand with exploits, which module contains the various bits of shellcode we send to have executed following exploitation?
- **Answer:** 

---

## Move that shell!

## We're in, now what?

## Makin' Cisco proud