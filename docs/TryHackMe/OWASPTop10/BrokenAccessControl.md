# Broken Access Control

**Date:** 30, December, 2020

**Author:** Dhilip Sanjay S

---

## Broken Access Control
- If a website visitor is able to access the protect pages that they are not authorised to view, the access controls are broken.
- Regular visitor accessing protected pages can lead to:
    - View sensitive information
    - Accessing unotherized functionality

- Some of the attack scenarios are:
    - Changing the `user account identifier` to some other account's value. If this is not properly verified, the attacker can access any user's account.
    ```
    http://example.com/app/accountInfo?acct=notmyacct
    ```
    
    - An attacker simply force browses to target URLs. Admin rights are required for access to the admin page.
    ```
    http://example.com/app/getappInfo
    http://example.com/app/admin_getappInfo
    ```

- Broken access control allows attackers to bypass authorization which can allow them to view sensitive data or perform tasks as if they were a privileged user.

## IDOR
- Insecure Direct Object Reference - act of exploiting a misconfiguration in the way user input is handled, to access unauthorized resources.
- It is a type of access control vulnerability.
- **Example:**
    ```
     https://example.com/bank?account_number=1234.
    ```
    - An attacker changes the `account_number=1235` and if the site is incorrectly configured, then he would access to someone else's bank information.

---

### What is the flag?
- **Answer:** flag{fivefourthree}
- **Steps to Reproduce:** 
    - After logging in, change the note parameter's value to 0.
    ```
    http://10.10.178.93/note.php?note=0
    ```
---