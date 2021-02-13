# Broken Authentication

**Date:** 27, December, 2020

**Author:** Dhilip Sanjay S

---

- Authentication and session management - core components of modern web applications.
- Authentication allows users to gain access to web applications by verifying their identities.
- Due to stateless nature of HTTP(S), a session cookie is needed.
- Some common flaws in Authentication mechanisms:
    - Brute Force Attacks
    - Use of weak credentials
    - Weak session cookies - Predictable values.
- To mitigate these flaws:
    - Automatic lockout after a certain number of attempts
    - Enforce strong password policy
    - Implement Multi factor Authentication

- **Re-registration of existing user** - sometimes gives the same rights as the re-registered user (like admin).

---
## Solutions

### What is the flag that you found in darren's account?
- **Answer:** fe86079416a21a3c99937fea8874b667
- **Steps to Reproduce:** 
    - Register with username " darren". - Notice the space.
    - Login into the newly registered darren account.

--- 

### What is the flag that you found in arthur's account?
- **Answer:** d9ac0f7db4fda460ac3edeb75d75e16e
- **Steps to Reproduce:** Same as before.

---