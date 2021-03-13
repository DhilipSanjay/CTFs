# esab64

**Date:** 13, March, 2021

**Author:** Dhilip Sanjay S

---

## Question
### Was it a car or a cat I saw?
- mxWYntnZiVjMxEjY0kDOhZWZ4cjYxIGZwQmY2ATMxEzNlFjNl13X

## Solution
- As you can guess the name says `base64` in reverse. So, it must be something related to **base64**.

- So, initially I base64 decoded the given string. But it was gibberish:

```bash
cat esab64 | base64 -d
����ىX��H��@΅�����ā��	����L͔X͗]�
```
- May be it had something to do with **reversing**. So, I reversed the string and then base64 decoded it. It gave a string, in which the flag was in reverse.
- On reversing the obtained decoded string, I was able to get the flag.

```py
from base64 import b64decode

string = 'mxWYntnZiVjMxEjY0kDOhZWZ4cjYxIGZwQmY2ATMxEzNlFjNl13X'
rev_string = string[::-1]

decoded_string = b64decode(rev_string)
print("Before reversing the decoded string:" , decoded_string, sep="\n")
print("After reversing: ", decoded_string[::-1], sep="\n")
```

- **Output:**

```bash
Before reversing the decoded string:
b'_}e61e711106bd0db1b78efa894b1125bf{galf'
After reversing: 
b'flag{fb5211b498afe87b1bd0db601117e16e}_'
```

## Solution
flag{fb5211b498afe87b1bd0db601117e16e}