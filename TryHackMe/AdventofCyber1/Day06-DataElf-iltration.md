# Day 06 - Data Elf-iltration

**Date:** 27, May, 2021

**Author:** Dhilip Sanjay S

---

### What data was exfiltrated via DNS?
- **Answer:** Candy Cane Serial Number 8491
- **Steps to Reproduce:** 
    - Copy the DNS query value, remove the domain.
    - Use Hex decoding:

```bash
echo 43616e64792043616e652053657269616c204e756d6265722038343931 | xxd -r -p
Candy Cane Serial Number 8491
```

---

## Downlaod, Bruteforce & Extract

- Download the zip file by exporting it in wireshark.
- Use **fcrackzip** for bruteforcing the password of the zip

```bash
$ sfcrackzip -b --method 2 -D -p /usr/share/wordlists/rockyou.txt -v christmaslists.zip 
found file 'christmaslistdan.tx', (size cp/uc     91/    79, flags 9, chk 9a34)
found file 'christmaslistdark.txt', (size cp/uc     91/    82, flags 9, chk 9a4d)
found file 'christmaslistskidyandashu.txt', (size cp/uc    108/   116, flags 9, chk 9a74)
found file 'christmaslisttimmy.txt', (size cp/uc    105/   101, flags 9, chk 9a11)
possible pw found: december ()
```

- Unzip the file:

```bash
$ $ unzip christmaslists.zip 
Archive:  christmaslists.zip
[christmaslists.zip] christmaslistdan.tx password: 
 extracting: christmaslistdan.tx     
  inflating: christmaslistdark.txt   
  inflating: christmaslistskidyandashu.txt  
  inflating: christmaslisttimmy.txt
```

---

### What did Little Timmy want to be for Christmas?
- **Answer:** PenTester
- **Steps to Reproduce:** 

```bash
$ cat christmaslisttimmy.txt 
Dear Santa,
For Christmas I would like to be a PenTester! Not the Bic kind!
Thank you,
Little Timmy.
```

---

### What was hidden within the file?
- **Answer:** RFC527
- **Steps to Reproduce:** 
    - Export the `TryHackMe.jpg` from wireshark.
    - Using `Steghide`, we find the hidden data: (Empty passphrase)

```bash
$ steghide extract -sf TryHackMe.jpg
Enter passphrase: 
wrote extracted data to "christmasmonster.txt".

$ cat christmasmonster.txt

                              ARPAWOCKY
                               RFC527

                    Twas brillig, and the Protocols
                         Did USER-SERVER in the wabe.
                    All mimsey was the FTP,
                         And the RJE outgrabe,

                    Beware the ARPANET, my son;
                         The bits that byte, the heads that scratch;
                    Beware the NCP, and shun
                         the frumious system patch,

                    He took his coding pad in hand;
                         Long time the Echo-plex he sought.
                    When his HOST-to-IMP began to limp
                         he stood a while in thought,

                    And while he stood, in uffish thought,
                         The ARPANET, with IMPish bent,
                    Sent packets through conditioned lines,
                         And checked them as they went,

                    One-two, one-two, and through and through
                         The IMP-to-IMP went ACK and NACK,
                    When the RFNM came, he said "I'm game",
                         And sent the answer back,

                    Then hast thou joined the ARPANET?
                         Oh come to me, my bankrupt boy!
                    Quick, call the NIC! Send RFCs!
                         He chortled in his joy.

                    Twas brillig, and the Protocols
                         Did USER-SERVER in the wabe.
                    All mimsey was the FTP,
                         And the RJE outgrabe.

                                                            D.L. COVILL
                                                            May 1973
```


---