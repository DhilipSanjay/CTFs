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

```
haiti 1aec7a56aa08b25b596057e1ccbcb6d768b770eaa0f355ccbd56aee5040e02ee
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

### wordlistctl search rockyou
```
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
```
wordlistctl search facebook
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


wordlistctl list -g fuzzing
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
```
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

## Custom wordlist generation

## Time to crack the hashes


## References
- [Rawsec's Cybersecurity Inventory](https://inventory.raw.pm/overview.html)