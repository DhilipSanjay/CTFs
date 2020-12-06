# Emdee five for life

**Date:** 06, December, 2020

**Author:** Dhilip Sanjay S

---
- This challenge tests your scripting skills. Because you can't beat the timer manually.
- Initally tried to do **Bash Scripting** using `curl`. But greping and filtering the exact string was difficult. 

 ## Python to the rescue!
- Suddenly, I remembered the following python modules:
    - Requests
    - Beautiful soup
- Alternatively, if you know regex, you can also filter the string:
    - re module
---

## MD5 using Python
- By Googling, I found the `hashlib` module serving the exact purpose.
- **NOTE:** 
    - The arguments must be in bytes. Use `string.encode()`.
    - The encoded data must be in hexadecimal. Use `hexdigest()` function.
---

## Script
```python
import re
import hashlib
import requests
from bs4 import BeautifulSoup

url = 'http://134.209.29.219:30789/'
# Maintain Session
# Because opening another session gives 'Too Slow' output
req = requests.session()
r = req.get(url)

def bs():
        soup = BeautifulSoup(r.text, 'html.parser')
        # print(soup)
        tag = soup.h3
        code = soup.h3.contents[0]
        return code

def rgx():
        regex = re.compile('<.*?>')
        code = re.sub(regex, "", r.text).split()
        code = code[-1].lstrip('string')
        return code

code = bs()
code = rgx()
md5sum = hashlib.md5(code.encode()).hexdigest()

data = {'hash': md5sum} # dict(hash = md5sum)
print(data)
response = req.post(url=url, data=data)

print(BeautifulSoup(response.text, 'html.parser').p.contents[0])
```
---
## Important takeaway - Maintaining single session
- Initially, I didn't use `requests.session()`.
- So, the two requests (to get the string and to post the md5) must open two connections. And this is a overhead in such a time sensitive scenario (TCP Three way handshake). So, the flag couldn't be fetched. 
- By using `requests.session()`, a single connection is maintained throughout the runtime of the script. Thereby making the requests fast.


**P.S:** If you are sure that your script is correct, but still you didn't get the flag, you can try googling! That's how we learn!

---
## Solution
HTB{N1c3_ScrIpt1nG_B0i!}
