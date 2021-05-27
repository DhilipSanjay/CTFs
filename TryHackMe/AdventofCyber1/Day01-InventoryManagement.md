# Day 1 - Inventory Management

**Date:** 25, May, 2021

**Author:** Dhilip Sanjay S

---

### What is the name of the cookie used for authentication?
- **Answer:** authid
- **Steps to Reproduce:** 
    - Checkout the Storage tab in browser
---

### If you decode the cookie, what is the value of the fixed part of the cookie?
- **Answer:** v4er9ll1!ss
- **Steps to Reproduce:** 
    - Create accounts with various usernames.
    - URL decode and base64 decode the **cookie** value.
    - You'll find that the cookie value is always **username** + **v4er9ll1!ss**

---

### After accessing his (mcinventory) account, what did the user mcinventory request?
- **Answer:** firewall
- **Steps to Reproduce:** 
    - Change the username value of decoded cookie to `mcinventory`.
    - Base64 encode the value
    - URL encode the value
    - Change the cookie in Browser Storage Tab.
    - Refresh the page to access mcinventory's account.
---