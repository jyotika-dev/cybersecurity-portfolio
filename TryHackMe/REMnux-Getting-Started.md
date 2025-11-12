# REMnux: The Basics - TryHackMe

> Learn about REMnux, a specialized Linux distro for malware analysis

**Room Link:** https://tryhackme.com/room/remnuxbasics

<img width="1777" height="896" alt="image" src="https://github.com/user-attachments/assets/ab76aaf5-7679-483a-9a1a-02cb3b7f036b" />


---

## Task 1: Introduction

### What is REMnux?

REMnux is a specialized Linux distribution for malware analysis.

**Pre-installed tools:**
- Volatility (memory analysis)
- YARA (malware detection)
- Wireshark (network analysis)
- oledump (file analysis)
- INetSim (network simulation)

**Benefits:**
- Sandbox environment
- No manual installations needed
- Safe malware analysis
- All tools ready to use

**No answer needed**

---

## Task 2: Machine Access

### Setup

Machine boots in split-screen view (2-3 minutes).

**Files location:** `/home/ubuntu/Desktop/tasks`

**No answer needed**

---

## Task 3: File Analysis

### oledump.py

Tool for analyzing OLE2 files (Office documents).

**OLE2 files:**
- Word documents
- Excel spreadsheets
- PowerPoint presentations

**Basic usage:**
```bash
oledump.py file.xlsx
```

**Select data stream:**
```bash
oledump.py -s 1 file.xlsx
```

### Questions

**What Python tool analyzes OLE2 files?**
- Answer: `oledump.py`

---

**What tool parameter allows you to select a particular data stream?**
- Answer: `-s`

---

**What command is commonly used for downloading files from the internet?**

PowerShell download command.

- Answer: `Invoke-WebRequest`

---

**What file was being downloaded using the PowerShell script?**
- Answer: `Doc-3737122pdf.exe`

---

**Where will the downloaded file be stored?**
- Answer: `$TempFile`

---

**Scan possible_malicious.docx in /home/ubuntu/Desktop/tasks/agenttesla/. How many data streams?**

```bash
oledump.py /home/ubuntu/Desktop/tasks/agenttesla/possible_malicious.docx
```

- Answer: `16`

---

**At what data stream number does the tool indicate a macro present?**

Look for "M" indicator in output.

- Answer: `8`

---

## Task 4: Fake Network to Aid Analysis

### INetSim

Tool that simulates internet services for safe malware analysis.

**Purpose:**
- Simulate real network
- Capture malware network behavior
- Safe testing environment

### Usage

**Start INetSim:**
```bash
sudo inetsim
```

**Download with wget:**
```bash
sudo wget https://MACHINE_IP/flag.txt --no-check-certificate
```

**Stop INetSim:**
```bash
sudo pkill inetsim
```

### Questions

**Download and scan flag.txt. What is the flag?**

```bash
sudo wget https://MACHINE_IP/flag.txt --no-check-certificate
cat flag.txt
```

- Answer: `Tryhackme{remnux_edition}`

---

**Based on the report, what URL Method was used to get flag.txt?**

Check INetSim report after stopping the service.

- Answer: `GET`

---

## Task 5: Memory Investigation

### Volatility 3

Tool for memory forensics - analyzes memory dumps.

**Location:** `/home/ubuntu/Desktop/tasks/Wcry_memory_image/`

### Common Plugins

**windows.pstree.PsTree:**
```bash
vol3 -f wcry.mem windows.pstree.PsTree
```
Lists process tree by parent ID.

**windows.pslist.PsList:**
```bash
vol3 -f wcry.mem windows.pslist.PsList
```
Lists all active processes.

**windows.cmdline.CmdLine:**
```bash
vol3 -f wcry.mem windows.cmdline.CmdLine
```
Shows process command-line arguments.

**windows.filescan.FileScan:**
```bash
vol3 -f wcry.mem windows.filescan.FileScan
```
Scans for file objects.

**windows.dlllist.DllList:**
```bash
vol3 -f wcry.mem windows.dlllist.DllList
```
Lists loaded modules.

**windows.psscan.PsScan:**
```bash
vol3 -f wcry.mem windows.psscan.PsScan
```
Scans for processes.

**windows.malfind.Malfind:**
```bash
vol3 -f wcry.mem windows.malfind.Malfind
```
Finds injected code.

### Strings Extraction

**ASCII strings:**
```bash
strings wcry.mem > wcry.strings.ascii.txt
```

**Unicode little-endian:**
```bash
strings -e l wcry.mem > wcry.strings.unicode_little_endian.txt
```

**Unicode big-endian:**
```bash
strings -e b wcry.mem > wcry.strings.unicode_big_endian.txt
```

### Questions

**What plugin lists processes in a tree based on their parent process ID?**
- Answer: `PsTree`

---

**What plugin is used to list all currently active processes?**
- Answer: `PsList`

---

**What Linux utility tool can extract ASCII and Unicode strings?**
- Answer: `strings`

---

**By running vol3 with Malfind, what is the first process suspected of having injected code?**

```bash
vol3 -f wcry.mem windows.malfind.Malfind
```

Look at first process listed.

- Answer: `csrss.exe`

---

**What is the second process suspected of having injected code?**

Second process in Malfind output.

- Answer: `winlogon.exe`

---

**By running vol3 with DllList, what is the file path of @WanaDecryptor@.exe?**

```bash
vol3 -f wcry.mem windows.dlllist.DllList | grep WanaDecryptor
```

Look for the directory path.

- Answer: `C:\Intel\ivecuqmanpnirkt615`

---

## Task 6: Conclusion

### What We Learned

✅ REMnux VM for malware analysis
✅ oledump.py for Office documents
✅ INetSim for network simulation
✅ Volatility for memory analysis
✅ Strings for text extraction

**No answer needed**

---

## Quick Reference

### oledump.py Commands

```bash
oledump.py file.docx              # List streams
oledump.py -s 1 file.docx         # View stream 1
oledump.py -s 8 -v file.docx      # Decompress macro
```

### INetSim Commands

```bash
sudo inetsim                      # Start service
sudo pkill inetsim                # Stop service
```

### Volatility 3 Commands

```bash
vol3 -f mem.dump windows.pstree.PsTree    # Process tree
vol3 -f mem.dump windows.pslist.PsList    # Process list
vol3 -f mem.dump windows.cmdline.CmdLine  # Command lines
vol3 -f mem.dump windows.filescan.FileScan # Files
vol3 -f mem.dump windows.malfind.Malfind  # Injected code
```

### Strings Commands

```bash
strings file.mem > output.txt              # ASCII
strings -e l file.mem > output.txt         # Little-endian
strings -e b file.mem > output.txt         # Big-endian
```

---

## Plugin Summary

| Plugin | Purpose |
|--------|---------|
| PsTree | Process tree by parent |
| PsList | List active processes |
| CmdLine | Command-line arguments |
| FileScan | Scan for files |
| DllList | List loaded modules |
| PsScan | Scan for processes |
| Malfind | Find injected code |

---

## Key Takeaways

- **REMnux** is specialized for malware analysis
- **oledump.py** analyzes Office documents
- **-s** selects data stream
- **Invoke-WebRequest** downloads files in PowerShell
- **INetSim** simulates network services
- **Volatility** analyzes memory dumps
- **PsTree** shows process hierarchy
- **PsList** lists all processes
- **strings** extracts text from files
- **Malfind** detects code injection
- **csrss.exe** was first suspicious process
- **All tools pre-installed** in REMnux

REMnux makes malware analysis easier with pre-configured tools and safe sandbox environment
Happy analyzing
