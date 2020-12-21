# Day 20 - PowershELlF to the rescue

**Date:** 20, December, 2020

**Author:** Dhilip Sanjay S

---

## Powershell
- PowerShell is built on top of the .NET Common Language Runtime (CLR), and accepts and returns .NET objects
- Powershell cmdlets are case-insensitive.
    - Set-Location (cd)
    - Get-ChildItem (ls)
        - Some options that can be used:
            - -Path
            - -File / -Directory
            - -Filter
            - -Recurse
            - -Hidden
            - -ErrorAction SilentlyContinue

    - Get-Content (cat)
    - Measure-Object -Word (wc)
        - Can be piped: `Get-Content -Path file.txt | Measure-Object -Word`

    - To get the exact position of a string within a file: `(Get-Content -Path file.txt)[index]` 

    - To search a particular file for a pattern: `Select-String`


## Solutions

### Search for the first hidden elf file within the Documents folder. Read the contents of this file. What does Elf 1 want?
- **Answer:** 2 front teeth
- **Steps to Reproduce:** 
    ```powershell
    PS C:\Users\mceager> Set-location Documents 
    PS C:\Users\mceager\Documents> Get-ChildItem -File -Hidden  


        Directory: C:\Users\mceager\Documents


    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -a-hs-        12/7/2020  10:29 AM            402 desktop.ini
    -arh--       11/18/2020   5:05 PM             35 e1fone.txt


    PS C:\Users\mceager\Documents> Get-content e1fone.txt 
    All I want is my '2 front teeth'!!!

    ```
---

### Search on the desktop for a hidden folder that contains the file for Elf 2. Read the contents of this file. What is the name of that movie that Elf 2 wants?
- **Answer:** Scrooged
- **Steps to Reproduce:** 
    ```powershell
    PS C:\Users\mceager> Set-Location Desktop
    PS C:\Users\mceager\Desktop> Get-Childitem -Hidden 

    Directory: C:\Users\mceager\Desktop
    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d--h--        12/7/2020  11:26 AM                elf2wo
    -a-hs-        12/7/2020  10:29 AM            282 desktop.ini

    PS C:\Users\mceager\Desktop> Set-Location elf2wo
    PS C:\Users\mceager\Desktop\elf2wo> Get-Childitem -File


    Directory: C:\Users\mceager\Desktop\elf2wo


    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -a----       11/17/2020  10:26 AM             64 e70smsW10Y4k.txt


    PS C:\Users\mceager\Desktop\elf2wo> Get-content .\e70smsW10Y4k.txt
    I want the movie Scrooged <3!
    ```
--- 

### Search the Windows directory for a hidden folder that contains files for Elf 3. What is the name of the hidden folder? (This command will take a while)
- **Answer:** 3lfthr3e
- **Steps to Reproduce:** 
    ```powershell
    PS C:\>Set-Location Windows\System32
    PS C:\Windows\System32> Get-ChildItem -Directory -Hidden -Filter "*3*"


    Directory: C:\Windows\System32


    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d--h--       11/23/2020   3:26 PM                3lfthr3e
    ```

---

### How many words does the first file contain?
- **Answer:** 9999
- **Steps to Reproduce:**
    ```powershell
    PS C:\Windows\System32> Set-Location .\3lfthr3e\
    PS C:\Windows\System32\3lfthr3e> Get-Childitem -Hidden


        Directory: C:\Windows\System32\3lfthr3e


    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -arh--       11/17/2020  10:58 AM          85887 1.txt
    -arh--       11/23/2020   3:26 PM       12061168 2.txt

    PS C:\Windows\System32\3lfthr3e> Get-Content 1.txt | Measure-Object -Word

    Lines Words Characters Property 
    ----- ----- ---------- --------
        9999

    ```
---

### What 2 words are at index 551 and 6991 in the first file?
- **Answer:** Red Ryder
- **Steps to Reproduce:** 
    ```powershell
    PS C:\Windows\System32\3lfthr3e> (Get-Content 1.txt)[551]
    Red

    PS C:\Windows\System32\3lfthr3e> (Get-Content 1.txt)[6991]
    Ryder 

    PS C:\Windows\System32\3lfthr3e> (Get-Content 1.txt)[551,6991]
    Red
    Ryder

    PS C:\Windows\System32\3lfthr3e> Get-Content 1.txt | Select-Object -Index 551,6991
    Red
    Ryder
    ```
---

### This is only half the answer. Search in the 2nd file for the phrase from the previous question to get the full answer. What does Elf 3 want? (use spaces when submitting the answer)
- **Answer:** red ryder bb gun
- **Steps to Reproduce:** 
    ```powershell
    PS C:\Windows\System32\3lfthr3e> Select-String 2.txt -Pattern 'redryder'

    2.txt:558704:redryderbbgun

    PS C:\Windows\System32\3lfthr3e> Get-Content 2.txt | Select-String -Pattern 'redryder'

    redryderbbgun 
    ```
---
[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202) 
