# 

**Date:** 28, May, 2021

**Author:** Dhilip Sanjay S

---

## Python Script

- Using **requests** module in python, build a string to fetch the flag

```py
#! /usr/bin/python3

import requests

url = "http://10.10.169.100:3000/"

flag = list()
nxt = ''

while True:
	r = requests.get(url + nxt).json()
	print(r)
	value = r['value']
	nxt = r['next']
	if nxt == 'end':
		break
	flag.append(value)

print(''.join(flag))
```

---

## Script Output

```bash
$ ./getflag.py 
{'value': 's', 'next': 'f'}
{'value': 'C', 'next': 's'}
{'value': 'r', 'next': 'a'}
{'value': 'I', 'next': 'g'}
{'value': 'P', 'next': 'q'}
{'value': 't', 'next': 'n'}
{'value': 'K', 'next': 't'}
{'value': 'i', 'next': 'm'}
{'value': 'D', 'next': 'b'}
{'value': 'd', 'next': 'i'}
{'value': 'end', 'next': 'end'}
sCrIPtKiDd
```

---

### What is the value of the flag?
- **Answer:** sCrIPtKiDd

---

