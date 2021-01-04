# Insufficent Logging & Monitoring

**Date:** 04, January, 2021

**Author:** Dhilip Sanjay S

---

## Insufficent Logging
- When web applications are set up, every action performed by the user should be logged.
- **Logging** is important because in the event of an incident, the attackers actions can be traced. - This can be used to assess the risk and impact.
- The bigger impacts of not logging the user actions include:
    - **Regulatory damage:** if an attacker gains access to personally identifiable user information and there is no record of this - users are affected and the applications owners are subject to fines or severe actions depending on the regulations.
    - **Risk of further attacks:** The presence of an attacker may be undetected without logging. Thus attacker can launch further attacks, by stealing credentials, attacking infrastructure and more.

## Information stored in logs
- HTTP status codes
- Time stamps
- Usernames
- API endpoints/ page locations
- IP addresses

- **Note:** Ensure that logs are stored securely and multiple copies of thses logs are stored at different locations.

## Suspicious Activities
- Multiple unauthorised attempts for a particular action. (in admin pages)
- Requests from anomalous IP address or locations. (someone else is trying to access a particular user's account - can have falase positive rate)
- Use of automated tools (can be identified using user-agent headers or speed of requests)
- Common payloads (XSS, SQLi, etc)

- **Note:** The suspicious activity needs to be rated according tot he impact level. Higher impact actions need to be responded sooner.

---

## Solutions

### What IP address is the attacker using?
- **Answer:** 49.99.13.16
- **Steps to Reproduce:** Look out for *Unauthorised* access.

---

### What kind of attack is being carried out?
- **Answer:** Brute Force
- **Steps to Reproduce:** Trying combinations of usernames and passwords to gain access to users' accounts.

---