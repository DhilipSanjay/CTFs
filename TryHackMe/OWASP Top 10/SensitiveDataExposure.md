# Sensitive Data Exposure

**Date:** 28, December, 2020

**Author:** Dhilip Sanjay S

---

- Sensitive data directly linked to customers.
    - Man in the Middle attack (The data can be captured due to weak encrption)
    - Directly on the web server itself.

- Types of Databases
    - SQL - MySQL, MariaDB
    - NoSQL - MongoDB
    - Flat File DB - SQLite

- Sensitive Data Exposure happens when the flat files are stored in the root directory of the website (i.e) one of the files that a suer connecting to the website is able to access.

- SQLite commands
    - `sqlite3 <database-name>` - To access a database.
    - `.tables` - To view the tables.
    - `PRAGMA table_info(customers);` - To see the table information.
    - `SELECT * FROM customers;` - To dump all the information from the table.


## Solutions

### What is the name of the mentioned directory?
- **Answer:** /assets
- **Steps to Reproduce:** 
    - Go to login page and view the source code. `Ctrl + U`
    ```html
    <!-- Must remember to do something better with the database than store it in /assets... -->
    ```
---

### Navigate to the directory you found in question one. What file stands out as being likely to contain sensitive data?
- **Answer:** webapp.db

---

### Use the supporting material to access the sensitive data. What is the password hash of the admin user?
- **Answer:** 6eea9b7ef19179a06954edd0f6c05ceb
- **Steps to Reproduce:** `sqlite3 webapp.db`
    ```sql
    sqlite> .tables
    sessions  users 

    sqlite> PRAGMA table_info(users);
    0|userID|TEXT|1||1
    1|username|TEXT|1||0
    2|password|TEXT|1||0
    3|admin|INT|1||0

    sqlite> select * from users;
    4413096d9c933359b898b6202288a650|admin|6eea9b7ef19179a06954edd0f6c05ceb|1
    23023b67a32488588db1e28579ced7ec|Bob|ad0234829205b9033196ba818f7a872b|1
    4e8423b514eef575394ff78caed3254d|Alice|268b38ca7b84f44fa0a6cdc86e6301e0|0
    ```
---

### What is the admin's plaintext password?
- **Answer:** qwertyuiop
- **Steps to Reproduce:** Use hashcat or crackstation.

---

### Login as the admin. What is the flag?
- **Answer:** THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}
- **Steps to Reproduce:** Type the username as `admin` and password as `qwertyuiop`.

---