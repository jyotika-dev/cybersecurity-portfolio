# FlareVM: The Basics - TryHackMe

> Learn about FlareVM, a toolkit for reverse engineering and malware analysis

**Room Link:** https://tryhackme.com/room/flarevmbasics

<img width="1784" height="825" alt="image" src="https://github.com/user-attachments/assets/cc28fceb-618e-4576-acdc-e4dc7ab20c13" />


---

## Task 1: Introduction

### What is FlareVM?

**FLARE** = Forensics, Logic Analysis, and Reverse Engineering

Created by FireEye's FLARE Team for:
- Reverse engineers
- Malware analysts
- Incident responders
- Forensic investigators
- Penetration testers

**No answer needed**

---

## Task 2: Arsenal of Tools

### Key Tools in FlareVM

**x64dbg:**
- Open-source debugger
- x64 and x32 formats
- Binary analysis

**CFF Explorer:**
- Analyze PE files
- Edit executables
- View file structure

**Process Hacker:**
- Memory editor
- Process watcher
- System monitoring

**FTK Imager:**
- Disc image acquisition
- Forensic analysis
- Evidence collection

**HxD:**
- Hex editor
- View binary files
- Edit raw data

### Questions

**Which tool is an Open-source debugger for binaries in x64 and x32 formats?**
- Answer: `x64dbg`

---

**What tool is designed to analyze and edit Portable Executable (PE) files?**
- Answer: `CFF Explorer`

---

**Which tool is considered a sophisticated memory editor and process watcher?**
- Answer: `Process Hacker`

---

**Which tool is used for Disc image acquisition and analysis for forensic use?**
- Answer: `FTK Imager`

---

**What tool can be used to view and edit a binary file?**
- Answer: `HxD`

---

## Task 3: Commonly Used Tools

### Investigation Tools

**FLOSS:**
- Formerly FLARE Obfuscated String Solver
- Extracts obfuscated strings
- Malware analysis

**Process Explorer:**
- Process monitoring
- Process tree view
- Detailed process info

**Procmon (Process Monitor):**
- Records system activity
- File system monitoring
- Registry monitoring

**PEStudio:**
- Static analysis tool
- No need to run files
- Examines PE properties

### Questions

**Which tool was formerly known as FLARE Obfuscated String Solver?**
- Answer: `FLOSS`

---

**Which tool offers in-depth insights into the active processes running on your computer?**
- Answer: `Process Explorer`

---

**By using Process Explorer, under what process can we find smss.exe?**

Process hierarchy: System → smss.exe

- Answer: `System`

---

**Which powerful Windows tool is designed to help you record issues with your system's apps?**
- Answer: `Procmon`

---

**Which tool can be used for Static analysis or studying executable file properties without running the files?**
- Answer: `PEStudio`

---

**Using PEStudio to open cryptominer.bin in Desktop\Sample, what is the sha256 value?**

Open file in PEStudio and check hash section.

- Answer: `E9627EBAAC562067759681DCEBA8DDE8D83B1D813AF8181948C549E342F67C0E`

---

**Using PEStudio to open cryptominer.bin, how many functions does it have?**

Check functions section in PEStudio.

- Answer: `102`

---

**What tool can generate file hashes for integrity verification?**
- Answer: `CFF Explorer`

---

**Using CFF Explorer to open possible_medusa.txt in Desktop\Sample, what is the MD5?**

Open in CFF Explorer and check file info.

- Answer: `646698572AFBBF24F50EC5681FEB2DB7`

---

**Use CFF Explorer on possible_medusa.txt. In DOS Header Section, what is the e_magic value?**

DOS Header → e_magic field

- Answer: `5A4D`

---

## Task 4: Analyzing Malicious Files

### File Analysis with PEStudio

**Entropy:**
- Measures randomness
- High entropy = likely packed/encrypted
- Range: 0-8

**Manifest:**
- Execution level requirements
- Permissions needed

### Questions

**Using PEStudio, open windows.exe. What is the entropy value?**

Check entropy section - high entropy indicates packing.

- Answer: `7.999`

---

**Using PEStudio, open windows.exe, go to manifest (administrator section). What is the requestedExecutionLevel value?**

Manifest → requestedExecutionLevel

- Answer: `requireAdministrator`

---

**Which function allows the process to use the OS shell to execute other processes?**

Function related to shell execution.

- Answer: `set_UseShellExecute`

---

**Which API starts with R and indicates the executable uses cryptographic functions?**

Rijndael = AES encryption algorithm

- Answer: `RijndaelManaged`

---

**What is the Imphash of cobaltstrike.exe?**

Import hash - unique identifier for imported functions.

- Answer: `92EEF189FB188C541CBD83AC8BA4ACF5`

---

**What is the defanged IP address to which cobaltstrike.exe is connecting?**

Defanged format: dots and brackets

- Answer: `47[.]120[.]46[.]210`

---

**What is the destination port number used by cobaltstrike.exe?**

Check network connections.

- Answer: `81`

---

**What is the parent process of cobaltstrike.exe?**

Use Process Explorer to view process tree.

- Answer: `explorer.exe`

---

## Task 5: Conclusion

**No answer needed**

---

## Quick Reference

### Core Tools

| Tool | Purpose |
|------|---------|
| x64dbg | Debugger for x64/x32 |
| CFF Explorer | PE file analysis |
| Process Hacker | Memory/process monitor |
| FTK Imager | Forensic imaging |
| HxD | Hex editor |
| FLOSS | String extraction |
| Process Explorer | Process monitoring |
| Procmon | System activity monitor |
| PEStudio | Static PE analysis |

### File Analysis Concepts

| Concept | Description |
|---------|-------------|
| Entropy | Randomness (0-8, high = packed) |
| PE | Portable Executable format |
| Imphash | Import hash identifier |
| e_magic | DOS header signature (5A4D = MZ) |
| Manifest | Execution requirements |

### Common PE Analysis Steps

1. **Check file hashes** (MD5, SHA256)
2. **View entropy** (detect packing)
3. **Examine imports** (API calls)
4. **Check strings** (URLs, IPs)
5. **Analyze manifest** (permissions)
6. **Monitor behavior** (dynamic analysis)

---

## Key Takeaways

- **FlareVM** = toolkit by FireEye FLARE Team
- **x64dbg** debugs binaries
- **CFF Explorer** analyzes PE files
- **Process Hacker** monitors processes
- **FTK Imager** for forensics
- **HxD** edits hex/binary
- **FLOSS** extracts obfuscated strings
- **Process Explorer** shows process tree
- **smss.exe** runs under System
- **Procmon** records system activity
- **PEStudio** for static analysis
- **5A4D** = MZ header signature
- **High entropy (7.999)** = packed file
- **requireAdministrator** = needs admin rights
- **Imphash** identifies imported functions
- **Defanged IPs** use brackets [.]
- **explorer.exe** often parent of user processes

FlareVM provides everything needed for malware analysis and reverse engineering in one convenient package
Happy analyzing!
