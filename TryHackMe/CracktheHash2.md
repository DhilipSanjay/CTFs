# Crack the Hash 2

**Date:** 23, April, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/crackthehashlevel2) to go to the TryHackMe room.


## Hash Identification

### Launch Haiti on 741ebf5166b9ece4cca88a3868c44871e8370707cf19af3ceaa4a6fba006f224ae03f39153492853. What kind of hash it is?
- **Answer:** RIPEMD-320
- **Steps to Reproduce:**
```bash
haiti -e 741ebf5166b9ece4cca88a3868c44871e8370707cf19af3ceaa4a6fba006f224ae03f39153492853 
RIPEMD-320
Cisco Type 7
BigCrypt [JtR: bigcrypt]
```

---

### Launch Haiti on 1aec7a56aa08b25b596057e1ccbcb6d768b770eaa0f355ccbd56aee5040e02ee

```bash
$ haiti 1aec7a56aa08b25b596057e1ccbcb6d768b770eaa0f355ccbd56aee5040e02ee
Snefru-256 [JtR: snefru-256]
SHA-256 [HC: 1400] [JtR: raw-sha256]
RIPEMD-256
Haval-256 [JtR: haval-256-3]
GOST R 34.11-94 [HC: 6900] [JtR: gost]
GOST CryptoPro S-Box
SHA3-256 [HC: 17400]
Keccak-256 [HC: 17800] [JtR: raw-keccak-256]
Skein-256 [JtR: skein-256]
Skein-512(256)
```

### What is Keccak-256 Hashcat code?
- **Answer:** 17800
- **Steps to Reproduce:** Refer hashcat man page

### What is Keccak-256 John the Ripper code?
- **Answer:** Raw-Keccak-256
- **Steps to Reproduce:** Refer John the Ripper formats by `john --list=FORMATS`

---

## Wordlists

### Wordlistctl
```bash
wordlistctl search rockyou
--==[ wordlistctl by blackarch.org ]==--

    0 > rockyou-05 (104.00 B)
    1 > rockyou-10 (723.00 B)
    2 > rockyou-15 (1.94 Kb)
    3 > rockyou-20 (4.00 Kb)
    4 > rockyou-25 (7.23 Kb)
    5 > rockyou-30 (12.16 Kb)
    6 > rockyou-35 (19.65 Kb)
    7 > rockyou-40 (31.22 Kb)
    8 > rockyou-45 (49.13 Kb)
    9 > rockyou-50 (75.91 Kb)
    10 > rockyou-55 (115.19 Kb)
    11 > rockyou-60 (170.24 Kb)
    12 > rockyou-65 (244.53 Kb)
    13 > rockyou-70 (344.23 Kb)
    14 > rockyou-75 (478.95 Kb)
    15 > rockyou-withcount (56.02 Mb)
    16 > rockyou (53.29 Mb)
    17 > rockyou-5 (104 B)
```

---

### Which option do you need to add to the previous command to search into local archives instead of remote ones?
- **Answer:** -l

---

### If you run wordlistctl search -l rockyou one more time, what is the path where is stored the wordlist?
- **Answer:** /usr/share/wordlists/passwords/rockyou.txt

---

### Sample commands

```bash
$ wordlistctl search facebook
--==[ wordlistctl by blackarch.org ]==--

    0 > facebook-app (1.76 Mb)
    1 > facebook-bot (1.64 Kb)
    2 > facebook-first (40.72 Mb)
    3 > facebook-firstnames (36.58 Mb)
    4 > facebook-last (51.59 Mb)
    5 > facebook-lastnames (46.46 Mb)
    6 > facebook-names-unique (1.5 Gb)
    7 > facebook-lastfirst (98.54 Mb)
    8 > facebook-firstlast (171.41 Mb)
    9 > facebook-f (154.93 Mb)
    10 > facebook-phished (25.09 Kb)
    11 > facebook-pastebay (500 B)


$ wordlistctl list -g fuzzing
--==[ wordlistctl by blackarch.org ]==--

[+] available wordlists:

   0 > php (3.48 Kb)
   1 > frontpage (233.00 B)
   2 > 1-4_all_letters_a-z (2.36 Mb)
   3 > 3-digits-000-999 (4.00 Kb)
   4 > 4-digits-0000-9999 (50.00 Kb)
   5 > 5-digits-00000-99999 (600.00 Kb)
   6 > 6-digits-000000-999999 (7.00 Mb)
   7 > MSSQL-Enumeration (716.00 B)
   8 > MSSQL (1.06 Kb)
   9 > MySQL-Read-Local-Files (210.00 B)
   10 > MySQL-SQLi-Login-Bypass (374.00 B)
   11 > MySQL (108.00 B)
   12 > NoSQL (566.00 B)
   .
   .
   4551 > extensions-skipfish (406.00 B)
   4552 > fuzz-Bo0oM (62.20 Kb)
   4553 > numeric-fields-only (649.00 B)
   4554 > special-chars (64.00 B)
```

### What is the name of the first wordlist in the usernames category?
- **Answer:** CommonAdminBase64
- **Steps to Reproduce:** 

```bash
wordlistctl list -g usernames
--==[ wordlistctl by blackarch.org ]==--

[+] available wordlists:

   0 > CommonAdminBase64 (1.05 Kb)
   1 > multiplesources-users-fabian-fingerle (164.59 Kb)
   2 > familynames-usa-top1000 (7.12 Kb)
   3 > femalenames-usa-top1000 (6.94 Kb)
   4 > malenames-usa-top1000 (6.68 Kb)
   5 > names (70.94 Kb)
   6 > cirt-default-usernames (6.34 Kb)
   7 > mssql-usernames-nansh0u-guardicore (51.00 B)
   8 > top-usernames-shortlist (112.00 B)
   9 > xato-net-10-million-usernames-dup (5.19 Mb)
   10 > xato-net-10-million-usernames (85.24 Mb)
   11 > random_social_usernamesupd (1.78 Gb)
   12 > adobe_users (1.51 Gb)
   13 > words_usernames (132.66 Mb)
   14 > soundcloud_usernames (1.05 Gb)
   15 > wordpress_usernames (541.57 Mb)
   16 > roblox_usernames (357.58 Mb)
   17 > forbes_users (26.51 Mb)
   18 > combocanem765927USERPASS (2.5 Mb)
   19 > enjin_usernames (39.87 Mb)
   20 > russian_users (620.57 Kb)
   21 > tetrisfriends_usernames (15.81 Mb)
   22 > instagram_usernames (11.03 Mb)
   23 > world_of_warcraft_usernames (9.38 Mb)
   24 > FacebookUsernames (276.85 Mb)
   25 > user passes projekt 1 (45 B)
   26 > DefUserNames (10.31 Kb)
   27 > root_userpass (381 B)
   28 > usernames (1.07 Kb)
   29 > twitter_usernames (84.39 Mb)
   30 > UserPassJay (12.68 Kb)

```

---

## Cracking tools, modes & rules

### Modes
- **Wordlist mode** - Dictionary Attacks
- **Incremental mode** - Bruteforcing all possible combinations of characters
- **Rule mode** - Uses wordlist mode by adding some pattern to the string. (Eg: current year, special characters, etc)

### Mutations
- **Border mutation** - combinations of digits and special symbols at the beginning or end or both.
- **Freak mutation** - replace letters with similar looking special symbols
- **Case mutation** - all variations of uppercase/lowercase letters for any characters.
- **Order mutation** - Character order is reversed.
- **Repetition mutation** - Same group of characters are repeated several times.
- **Vowels mutation** - vowels are omitted or capitalized
- **Strip mutation** - one or several characters are removed.
- **Swap mutation** - some characters are swapped places.
- **Duplicate mutation** - some characters are duplicated.
- **Delimiter mutation** - delimiters are addded between characters.

### 2d5c517a4f7a14dcb38329d228a7d18a3b78ce83
- **Answer:** moonligh56
- **Steps to Reproduce:** Use john

```bash
john hash.txt --format=raw-sha1 --wordlist=/usr/share/wordlists/passwords/10k-most-common.txt --rules=THM01
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA1 [SHA1 256/256 AVX2 8x])
Press 'q' or Ctrl-C to abort, almost any other key for status
moonligh56       (?)
1g 0:00:00:00 DONE (2021-04-23 15:52) 10.00g/s 5653Kp/s 5653Kc/s 5653KC/s hotrats56..jayhawks56
Use the "--show --format=Raw-SHA1" options to display all of the cracked passwords reliably
Session completed
```

---

## Custom wordlist generation

### ed91365105bba79fdab20c376d83d752
- **Answer:** mOlo$$u$
- **Steps to Reproduce:** 

```bash
$ cat john-local.conf 
[List.Rules:THM01]
$[0-9]$[0-9]

$ john md5.txt --format=Raw-MD5 --wordlist=/usr/share/wordlists/misc/dogs_custom.txt
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
Press 'q' or Ctrl-C to abort, almost any other key for status
mOlo$$u$         (?)
1g 0:00:00:00 DONE (2021-04-23 16:12) 50.00g/s 12100p/s 12100c/s 12100C/s aDvanced..yOrk$hire
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed
```

---

### What is the last word of the list?
- **Answer:** Information
- **Steps to Reproduce:** 

```
cewl -d 2 -w $(pwd)/example.txt https://example.org
CeWL 5.5.0 (Grouping) Robin Wood (robin@digi.ninja) (https://digi.ninja/)

cat example.txt 
Example
Domain
domain
for
use
This
illustrative
examples
documents
You
may
this
literature
without
prior
coordination
asking
permission
More
information
```

---

### TTPassGen

```bash
$ ttpassgen --rule '[?d]{4:4:*}' pin.txt 
# Number pins of size 4

$ ttpassgen --rule '[?l]{1:3:*}' abc.txt
# Characters of length 1 to 3

$ ttpassgen --dictlist 'pin.txt,abc.txt' --rule '$0[-]{1}$1' combination.txt
# Combine them with hypen (-) inbetween

$ wc pin.txt 
10000 10000 50000 pin.txt

$ wc abc.txt 
18278 18278 72384 abc.txt

$ wc combination.txt
# It's 1.64 GB
 182780000  182780000 1637740000 combination.txt
```

### e5b47b7e8df2597077e703c76ee86aee
- **Answer:** 1551-li
- **Steps to Reproduce:** 

```
john combi_hash.txt --format=Raw-MD5 --wordlist=combination.txt
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
Press 'q' or Ctrl-C to abort, almost any other key for status
1551-li          (?)
1g 0:00:00:02 DONE (2021-04-23 16:28) 0.3584g/s 10161Kp/s 10161Kc/s 10161KC/s 1551-g..1551-nz
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed
```

---

## Time to crack the hashes

### 1) b16f211a8ad7f97778e5006c7cecdf31
- **Answer:** Zachariah1234*
- **Steps to Reproduce:** 
    - First download common male namelist using `wordlistctl`.
    ```bash
    $ wordlistctl search male
    --==[ wordlistctl by blackarch.org ]==--

        0 > femalenames-usa-top1000 (6.94 Kb)
        1 > malenames-usa-top1000 (6.68 Kb)
        2 > male-names (26.16 Kb)
        3 > maleNames-password (22.54 Kb)
        4 > kb_rus_noun_male (243.12 Kb)
        5 > top_1000_usa_femalenames_english (6.78 Kb)
        6 > top_1000_usa_malenames_english (6.52 Kb)
        7 > Female (2.96 Mb)
        8 > kb_rus_adj_male (346.63 Kb)
        9 > tr_rus_adj_male (390.73 Kb)

    $ wordlistctl fetch -l male-names -d
    --==[ wordlistctl by blackarch.org ]==--

    [!] male-names.txt.gz already exists -- skipping
    [*] decompressing /usr/share/wordlists/misc/male-names.txt.gz
    [+] decompressing male-names.txt.gz completed
    ```

    - Set up john config file:
    ```bash
    cat john-local.conf 
    [List.Rules:task01]
    c$[0-9!@#$%^&*()_+]$[0-9!@#$%^&*()_+]$[0-9!@#$%^&*()_+]$[0-9!@#$%^&*()_+]$[0-9!@#$%^&*()_+]
    c^[0-9!@#$%^&*()_+]^[0-9!@#$%^&*()_+]^[0-9!@#$%^&*()_+]^[0-9!@#$%^&*()_+]^[0-9!@#$%^&*()_+]
    ```
    
    - Run John
    ```
    $ john --format=Raw-MD5 hash1.txt --wordlist=/usr/share/wordlists/usernames/malenames-usa-top1000.txt --rules=task01

    Using default input encoding: UTF-8
    Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
    Press 'q' or Ctrl-C to abort, almost any other key for status
    0g 0:00:00:06 0.41% (ETA: 19:20:20) 0g/s 7024Kp/s 7024Kc/s 7024KC/s Lane03+4+..Curtis03+50
    Zachariah1234*   (?)
    1g 0:00:00:33 DONE (2021-04-23 18:56) 0.03018g/s 7760Kp/s 7760Kc/s 7760KC/s Alden1234*..Ivan1234(
    Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
    Session completed    
    ```
    

---

### 2) 7463fcb720de92803d179e7f83070f97
- **Answer:** Angelita35!
- **Steps to Reproduce:** 
    - First download common female namelist using `wordlistctl`.
    ```bash
    $ wordlistctl search female
    --==[ wordlistctl by blackarch.org ]==--

        0 > femalenames-usa-top1000 (6.94 Kb)
        1 > top_1000_usa_femalenames_english (6.78 Kb)

    $ wordlistctl fetch -l femalenames-usa-top1000 
    --==[ wordlistctl by blackarch.org ]==--

    [*] downloading femalenames-usa-top1000.txt to /usr/share/wordlists/usernames/femalenames-usa-top1000.txt.part
    [+] downloading femalenames-usa-top1000.txt completed
    ```
    
    - Set up john config rule:
    ```bash
    [List.Rules:task02]
    c$[0-9!@#$%^&*()_+]$[0-9!@#$%^&*()_+]$[0-9!@#$%^&*()_+]
    c^[0-9!@#$%^&*()_+]^[0-9!@#$%^&*()_+]^[0-9!@#$%^&*()_+]
    ```
    
    - Run john
    ```bash
    $ john --format=Raw-MD5 hash2.txt --wordlist=/usr/share/wordlists/usernames/femalenames-usa-top1000.txt --rules=task02

    $ john --format=Raw-MD5 hash2.txt --show
    ?:Angelita35!

    1 password hash cracked, 0 left
    ```

---

### 3) f4476669333651be5b37ec6d81ef526f
- **Answer:** Tl@xc@l@ncing0
- **Steps to Reproduce:** 
    - First download Mexico town namelist using `wordlistctl`.

    ```bash
    # Download towns_mx word list
    $ wordlistctl search towns
    --==[ wordlistctl by blackarch.org ]==--

        0 > towns_us (289.81 Kb)
        1 > towns_ca (28.92 Kb)
        2 > towns_nl (20.74 Kb)
        3 > towns_gb (68.59 Kb)
        4 > towns_mx (13.34 Kb)
        5 > towns_de (60.43 Kb)
   
    $ wordlistctl fetch -l towns_mx -d
    --==[ wordlistctl by blackarch.org ]==--

    [*] downloading towns_mx.dic.gz to /usr/share/wordlists/misc/towns_mx.dic.gz.part
    [+] downloading towns_mx.dic.gz completed
    [*] decompressing /usr/share/wordlists/misc/towns_mx.dic.gz
    [+] decompressing towns_mx.dic.gz completed

    # Download cities wordlist too!!
    wordlistctl search cities
    --==[ wordlistctl by blackarch.org ]==--

        0 > cities (939.91 Kb)
        1 > us-cities (207.04 Kb)
        2 > rus_cities_translit (204.51 Kb)
        3 > rus_cities_kbchange (189.72 Kb)
        4 > us_cities (202.19 Kb)
        5 > indian-cities (183.08 Kb)
    
    wordlistctl fetch -l cities 
    --==[ wordlistctl by blackarch.org ]==--

    [*] downloading cities.txt to /usr/share/wordlists/misc/cities.txt.part
    [+] downloading cities.txt completed

    ```

    - Clean the wordlist. Remove spaces and change everything to lowercase.

    ```bash
    cat cities.txt | sed -r 's/\s+//g' | tr '[:upper:]' '[:lower:]' > cities_final.txt

    ```   


    - Run john along with `l33t` rule and the newly generated wordlist.

    ```bash
    john --format=Raw-MD5 hash3.txt --wordlist=/usr/share/wordlists/misc/cities_final.txt --rules=l33t
    Using default input encoding: UTF-8
    Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
    Press 'q' or Ctrl-C to abort, almost any other key for status
    Tl@xc@l@ncing0   (?)
    1g 0:00:00:02 DONE (2021-04-23 20:11) 0.4237g/s 728949p/s 728949c/s 728949C/s Vestv@g0y@..C0r0nelvivid@
    Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
    Session completed
    ```

---

### 4) a3a321e1c246c773177363200a6c0466a5030afc
- **Answer:** DavIDgUEtTApAn
- **Steps to Reproduce:**
    - Use John with `NT` rule, Raw-SHA1

    ```bash
    john --format=Raw-SHA1 hash4.txt --wordlist=hash4_name.txt --rules=NT
    Using default input encoding: UTF-8
    Loaded 1 password hash (Raw-SHA1 [SHA1 256/256 AVX2 8x])
    Press 'q' or Ctrl-C to abort, almost any other key for status
    DavIDgUEtTApAn   (?)
    1g 0:00:00:00 DONE (2021-04-23 20:27) 8.333g/s 38333p/s 38333c/s 38333C/s DavIDgUEttapAn..DavIDgUEtTAPAn
    Use the "--show --format=Raw-SHA1" options to display all of the cracked passwords reliably
    Session completed
    ```

---

### 5) d5e085772469d544a447bc8250890949
- **Answer:** 
- **Steps to Reproduce:** 
    - Use `Lyricpass` to generate song list of the favourite singer `Adele`.

    ```bash
    $ lyricpass.py -a Adele
    [+] Looking up artist Adele
    [+] Found 345 songs for artists Adele
    [+] All done! 345/345...       

    Raw lyrics: raw-lyrics-2021-04-23-20.30.50
    Passphrases: wordlist-2021-04-23-20.30.50 
    ```
    
    - Use John with rule `r` (for reversed character order)

    ```bash
    john --format=Raw-MD5 hash5.txt --wordlist=raw-lyrics-2021-04-23-20.30.50 --rules=r
    Using default input encoding: UTF-8
    Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
    Press 'q' or Ctrl-C to abort, almost any other key for status
    uoy ot miws ot em rof peed oot ro ediw oot si revir oN (?)
    1g 0:00:00:00 DONE (2021-04-23 20:40) 16.66g/s 6400p/s 6400c/s 6400C/s tnew yrots eht woh si sihT..egdirb eht rednu retaw tnia evol ruo taht yaS
    Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
    Session completed
    ```

---

### 6) 377081d69d23759c5946a95d1b757adc
- **Answer:** +17215440375
- **Steps to Reproduce:** 
    - Use pnwgen to with prefix - `+1721` (Sint Maarten code) - Refer [Wikipedia](https://en.wikipedia.org/wiki/Sint_Maarten)

    ```bash
    $ ./pnwgen.py
    INFO:--------------------------------
    INFO:Creating a wordlist file...

    Choose the number of digits in generated raw output:
    (min 4, max 10, 7 (by default) - press ENTER)

    >>> 7
    INFO:7 digits raw output was choosed

    INFO:generating +1721***
    INFO:Finished!!!
    ```
    
    - Use john with `raw-md5` format.

    ```bash
    $ john --format=Raw-MD5 hash6.txt --wordlist=phoneno.txt 
    Using default input encoding: UTF-8
    Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
    Press 'q' or Ctrl-C to abort, almost any other key for status
    +17215440375     (?)
    1g 0:00:00:00 DONE (2021-04-23 20:55) 1.030g/s 5608Kp/s 5608Kc/s 5608KC/s +17215440128..+17215440511
    Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
    Session completed

    ```
    

---

### 7) ba6e8f9cd4140ac8b8d2bf96c9acd2fb58c0827d556b78e331d1113fcbfe425ca9299fe917f6015978f7e1644382d1ea45fd581aed6298acde2fa01e7d83cdbd
- **Answer:** !@#redrose!@#
- **Steps to Reproduce:** Use hashcat with `17600` (SHA3-512)
```
$ hashcat -m 17600 hash7.txt /usr/share/wordlists/rockyou.txt

ba6e8f9cd4140ac8b8d2bf96c9acd2fb58c0827d556b78e331d1113fcbfe425ca9299fe917f6015978f7e1644382d1ea45fd581aed6298acde2fa01e7d83cdbd:!@#redrose!@#
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: SHA3-512
Hash.Target......: ba6e8f9cd4140ac8b8d2bf96c9acd2fb58c0827d556b78e331d...83cdbd
Time.Started.....: Fri Apr 23 16:41:07 2021 (18 secs)
Time.Estimated...: Fri Apr 23 16:41:25 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   839.1 kH/s (0.98ms) @ Accel:1024 Loops:1 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests
Progress.........: 14342165/14344385 (99.98%)
Rejected.........: 3093/14342165 (0.02%)
Restore.Point....: 14341141/14344385 (99.98%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: !J389aM544b! -> !8ZnEp

Started: Fri Apr 23 16:40:40 2021
Stopped: Fri Apr 23 16:41:26 2021

```
---

### 9f7376709d3fe09b389a27876834a13c6f275ed9a806d4c8df78f0ce1aad8fb343316133e810096e0999eaf1d2bca37c336e1b7726b213e001333d636e896617
- **Answer:** hackinghackinghackinghacking
- **Steps to Reproduce:**
    - Refer [Blake 2](https://www.blake2.net/) and [Wireguard](https://en.wikipedia.org/wiki/WireGuard)

    - Use CeWL to scrape the words from the website.
    ```bash
    $ cewl -d 2 -w hash8_scrapped.txt http://<MACHINE_IP>/rtfm.re/en/sponsors/index.html
    CeWL 5.4.8 (Inclusion) Robin Wood (robin@digi.ninja) (https://digi.ninja/)

    $ wc hash8_scrapped.txt 
    587  587 4700 hash8_scrapped.txt
    ```

    - Generate the wordlist with 1,2,3,4 and 5 repetition of the words using python script.
    ```py
    file1 = open('hash8_scrapped.txt', 'r')
    file2 = open('hash8_final.txt', 'w')

    while True:
        l = file1.readline()
        if not l:
            break
        l = l.strip()
        for i in range(1, 6):
            file2.write(l*i+'\n')

    file1.close()
    file2.close()
    ```
    
    - Use john with `Raw-Blake2`
    ```bash
    john --format=Raw-Blake2 hash8 -wordlist=hash8_final.txt 
    Using default input encoding: UTF-8
    Loaded 1 password hash (Raw-Blake2 [BLAKE2b 512 128/128 AVX])
    Press 'q' or Ctrl-C to abort, almost any other key for status
    hackinghackinghackinghacking (?)
    1g 0:00:00:00 DONE (2021-04-23 21:33) 33.33g/s 10666p/s 10666c/s 10666C/s LyonLyon..manymanymanymanymany
    Use the "--show" option to display all of the cracked passwords reliably
    Session completed
    ```
    

---

### 8) $6$kI6VJ0a31.SNRsLR$Wk30X8w8iEC2FpasTo0Z5U7wke0TpfbDtSwayrNebqKjYWC4gjKoNEJxO/DkP.YFTLVFirQ5PEh4glQIHuKfA/
- **Answer:** kakashi1
- **Steps to Reproduce:** Use john or HashCat with `sha512crypt`

```
john --format=sha512crypt hash9.txt --wordlist=/usr/share/wordlists/rockyou.txtUsing default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Press 'q' or Ctrl-C to abort, almost any other key for status
kakashi1         (?)
1g 0:00:00:17 DONE (2021-04-23 16:34) 0.05701g/s 1590p/s 1590c/s 1590C/s mothers..citlali
Use the "--show" option to display all of the cracked passwords reliably
Session completed

```

---

## References
- [Rawsec's Cybersecurity Inventory](https://inventory.raw.pm/overview.html)
- [John the ripper rules](https://www.openwall.com/john/doc/RULES.shtml)