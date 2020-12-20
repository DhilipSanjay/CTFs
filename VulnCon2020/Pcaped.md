# Pcaped

**Date:** 20, December, 2020

**Author:** Dhilip Sanjay S

---

- Download the pcap file and run tshark.
    ```bash
    tshark -r Data.pcapng -T fields -e data.text | grep . > output.txt
    ```

- Filter the new lines. The obtained string is Base64 encoded:
    ```bash
    VGhlIGZsYWcgaXMtPiBCMXRfYnlfQjF0X3YxYV9uYwo=
    ```
    
- Base64 decode the text to obtain the flag. 
    ```bash
    root@kali: echo VGhlIGZsYWcgaXMtPiBCMXRfYnlfQjF0X3YxYV9uYwo= | base64 -d
    The flag is-> B1t_by_B1t_v1a_nc
    ```
    