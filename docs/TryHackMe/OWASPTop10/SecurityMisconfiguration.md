# Security Misconfiguration

**Date:** 31, December, 2020

**Author:** Dhilip Sanjay S

---

## Security Misconfiguration
- Security misconfigurations include:
    - Poorly configured permission s on cloud services like S3 buckets
    - Having unnecessary features enabled, like services, pages, accounts or privileges
    - Default accounts with unchanged passwords
    - Error messages that are overly detailed and allow an attacker to find out more about the system.
    - Not using [HTTP Security Headers](https://owasp.org/www-project-secure-headers/) or revealing too much detail in the Server: HTTP header.

## Scenarios
- Some scenarios of Security Misconfiguration are:
    - Not changing the Default credentials.
    - Directory listing not disabled on the server.
    - Sample applications not removed from the production server.
    - Displaying detailed error messages (E.g: Stack Traces)
    - Cloud service provider has default sharing permissions open to the Internet. This allows sensitive data to be accessed.

## Default passwords
- Dyn (DNS provider) was taken down by DDos Attacks. The flood of traffic mostly came from IOT and networking devices like routers and modems infected by [Mirai Malware](https://en.wikipedia.org/wiki/Mirai_(malware))
- Default passwords of exposed telnet services were used to log in into those devices.

---
## Solution

### Hack into the webapp, and find the flag!
- **Answer:** thm{4b9513968fd564a87b28aa1f9d672e17}
- **Steps to Reproduce:** 
    - Directory listing is not disabled in this server.
    - No flags were found in the source code.
    - May be there is a github repo or some other VCS.
    - On googling `Pensive Notes github`, you will get the following result: [PensiveNotes Github repo](https://github.com/NinjaJc01/PensiveNotes).
    - The default credentials are: `pensive:PensiveNotes`.
    - By logging in using the default credentials, you'll get the flag.
---
