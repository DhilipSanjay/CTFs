# Day 21 - Time for some ELForensics

**Date:** 21, December, 2020

**Author:** Dhilip Sanjay S

---

## Powershell Forensics
- To obtain hash of a file: `Get-FileHash -Algorithm MD5 file.txt`
- To scan executable file: `c:\Tools\strings64.exe -accepteula file.exe`
- Alternate Data Streams (ADS) - `Get-Item -Path file.exe -Stream *`
    - Attributes specific to Windows NTFS.
    - Every file has at least one data stream ($DATA) and ADS allows files to contain more than one stream of data.
    - Malware writers have used ADS to hide data in an endpoint, but not all its uses are malicious. 
    - When you download a file from the Internet unto an endpoint there are identifiers written to ADS to identify that it was downloaded from the Internet.

- **Windows Management Instrumentation**, to launch the hidden file - `wmic process call create $(Resolve-Path file.exe:streamname)`


## Solutions

### Read the contents of the text file within the Documents folder. What is the file hash for db.exe?
- **Answer:** 596690FFC54AB6101932856E6A78E3A1
- **Steps to Reproduce:** 
    ```powershell
    PS C:\Users\littlehelper\Documents> Get-Content '.\db file hash.txt'
    Filename:       db.exe
    MD5 Hash:       596690FFC54AB6101932856E6A78E3A1    
    ``` 
---

### What is the file hash of the mysterious executable within the Documents folder?
- **Answer:** 5F037501FB542AD2D9B06EB12AED09F0
- **Steps to Reproduce:** 
    ```powershell
    PS C:\Users\littlehelper\Documents> Get-FileHash -Algorithm MD5 .\deebee.exe

    Algorithm       Hash                                                                   Path
    ---------       ----                                                                   ----
    MD5             5F037501FB542AD2D9B06EB12AED09F0                                       C:\Users\littlehelper\Documents\deebee.exe
    ```
---

### Using Strings find the hidden flag within the executable?
- **Answer:** THM{f6187e6cbeb1214139ef313e108cb6f9}
- **Steps to Reproduce:** 
    ```powershell
    PS C:\Users\littlehelper\Documents> C:\Tools\strings64.exe -accepteula .\deebee.exe | Select-String -Pattern 'THM'

    THM{f6187e6cbeb1214139ef313e108cb6f9}
    ```
---

### What is the flag that is displayed when you run the database connector file?
- **Answer:** THM{088731ddc7b9fdeccaed982b07c297c}
- **Steps to Reproduce:** 
    ```powershell
    PS C:\Users\littlehelper\Documents> Get-Item .\deebee.exe -Stream *

    PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\littlehelper\Documents\deebee.exe::$DATA
    PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\littlehelper\Documents
    PSChildName   : deebee.exe::$DATA
    PSDrive       : C
    PSProvider    : Microsoft.PowerShell.Core\FileSystem
    PSIsContainer : False
    FileName      : C:\Users\littlehelper\Documents\deebee.exe
    Stream        : :$DATA
    Length        : 5632

    PSPath        : Microsoft.PowerShell.Core\FileSystem::C:\Users\littlehelper\Documents\deebee.exe:hidedb
    PSParentPath  : Microsoft.PowerShell.Core\FileSystem::C:\Users\littlehelper\Documents
    PSChildName   : deebee.exe:hidedb
    PSDrive       : C
    PSProvider    : Microsoft.PowerShell.Core\FileSystem
    PSIsContainer : False
    FileName      : C:\Users\littlehelper\Documents\deebee.exe
    Stream        : hidedb
    Length        : 6144

    PS C:\Users\littlehelper\Documents> wmic process call create $(Resolve-Path .\deebee.exe:hidedb)
    Executing (Win32_Process)->Create()
    Method execution successful.
    Out Parameters:
    instance of __PARAMETERS
    {
            ProcessId = 2436;
            ReturnValue = 0;
    };
    ```

- Command prompt output:
    ```
    Choose an option:
    1) Nice List
    2) Naughty List
    3) Exit

    THM{088731ddc7b9fdeccaed982b07c297c}
    ```
---
[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202)