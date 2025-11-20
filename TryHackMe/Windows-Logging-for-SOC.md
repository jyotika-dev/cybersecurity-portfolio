# Windows Logging for SOC - TryHackMe

> Master Windows event logs for threat detection

**Room Link:** https://tryhackme.com/room/windowsloggingforsoc

<img width="1575" height="921" alt="image" src="https://github.com/user-attachments/assets/46a2c2bc-87bc-43bd-ac8c-35bf25f572a5" />

---

## Task 1: Introduction

**Topics:**
- Windows event logs
- Sysmon monitoring
- PowerShell logging
- Log correlation

**No answer needed**

---

## Task 2: What Is Logged

### Log Storage

**Location:** `C:\Windows\System32\winevt\Logs`

**Format:** Binary (.evtx files)

**Common logs:**
- Application.evtx
- Security.evtx
- System.evtx

### Event Viewer

**Open:** Win + R → `eventvwr`

**Key fields:**
- Event ID (unique identifier)
- Date and Time (system time)
- Keywords (success/failure)
- Source (which log)

### Questions

**Q1: Event ID for successful login?**

Format: LogSource / ID

- Answer: `Security / 4624`

---

## Task 3: Security Log - Authentication

### Key Event IDs

- **4624** - Successful Logon
- **4625** - Failed Logon

### Important Fields

- **Logon Type** (3=Network, 10=RDP)
- **Account Name**
- **Workstation Name**
- **Source IP**
- **Logon ID** (unique session)

### RDP Brute Force Detection

**Filter:** Event ID 4625

**Red flags:**
- Multiple failed attempts
- Many usernames tried (spraying)
- Single account targeted (brute force)
- Suspicious workstation names
- Unexpected source IPs

**Follow-up:** Find successful 4624 with same IP.

### Questions

**Q1: IP that performed brute force?**

File: Practice-Security.evtx

Filter for 4625, check source IPs.

- Answer: `10.10.53.248`

---

**Q2: Breached user?**

Find 4624 from attacker's IP.

- Answer: `Administrator`

---

**Q3: Logon ID of malicious RDP login?**

Check Logon Type 10 from attacker.

- Answer: `0x183C36D`

---

## Task 4: Security Log - User Management

### Key Event IDs

- **4720** - User Created
- **4732** - User Added to Group
- **4733** - User Removed from Group

### Event Structure

**Subject:** Who did action (includes Logon ID)

**Object:** Target user/group

**Details:** Changes made

### Hunt Backdoor Users

**Filter:** 4720, 4732

**Red flags:**
- Unknown IT staff action
- Non-working hours
- Suspicious usernames
- Unusual group additions

**Correlate:** Use Logon ID to find login event.

### Questions

**Q1: User created by attacker?**

Filter 4720, check after RDP login.

- Answer: `svc_sysrestore`

---

**Q2: Which two privileged groups added?**

Filter 4732, alphabetical order.

- Answer: `Backup Operators, Remote Desktop Users`

---

**Q3: Does Logon ID match previous task?**

Check Subject's Logon ID in 4732 events.

- Answer: `Yea`

---

## Task 5: Sysmon - Process Monitoring

### Sysmon vs Security Log

**Security 4688:** Basic process creation

**Sysmon Event 1:** Advanced process monitoring

**Location:** Applications & Services → Microsoft → Sysmon → Operational

### Event ID 1 Fields

**Process Info:**
- Image (path)
- CommandLine
- ProcessId

**Parent Info:**
- ParentImage
- ParentCommandLine
- ParentProcessId

**Binary Info:**
- Hashes (MD5, SHA256)
- Signature

**User Context:**
- User
- LogonId

### Analysis Workflow

1. Check Image path (suspicious: C:\Temp, C:\Users\Public)
2. Check process name (random: jqyvpqldou.exe)
3. Check hash on VirusTotal
4. Check parent process
5. Trace back using ParentProcessId
6. Correlate via Logon ID

### Questions

**Q1: Which browser does Sarah use?**

File: Practice-Sysmon.evtx

First process execution events.

- Answer: `Google Chrome`

---

**Q2: File downloaded from browser?**

Check Downloads folder in Image path.

- Answer: `C:\Users\sarah.miller\Downloads\ckjg.exe`

---

**Q3: URL file downloaded from?**

Use Sysmon Event 15 (FileStream) to find URL.

- Answer: `http://gettsveriff.com/bgj3/ckjg.exe`

---

## Task 6: Sysmon - Files and Network

### Additional Event IDs

- **Event 3** - Network Connection
- **Event 11** - File Create
- **Event 13** - Registry Value Set
- **Event 22** - DNS Query

### Correlation

**Use ProcessId** to link events to Event ID 1.

### Network Red Flags

- External IPs on port 80
- Non-standard ports (4444)
- Known malicious IPs
- Suspicious domains (.top, .click)

### File Red Flags

- Staging directories (C:\Temp)
- Scripts (.bat, .ps1)
- Executables (.exe, .com)
- Startup/persistence locations

### Questions

**Q1: File created for persistence?**

Event 11, check Startup folder.

- Answer: `C:\Users\sarah.miller\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\DeleteApp.url`

---

**Q2: C2 server IP:Port?**

Event 3, network connections.

- Answer: `193.46.217.4:7777`

---

**Q3: Domain of malicious IP?**

Event 22, DNS query results.

- Answer: `hkfasfsafg.click`

---

## Task 7: PowerShell Logging

### The Problem

**Single Sysmon event:** powershell.exe launched

**Hidden:** All commands executed inside session

### PowerShell History File

**Location:**
```
C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

**Contains:**
- Every typed command
- Survives reboots
- One file per user
- Plain text format

**Limitations:**
- No command output
- No script content
- Can be deleted manually

### Questions

**Q1: First PowerShell command by Administrator?**

Open Admin's ConsoleHost_history.txt

- Answer: `Get-ComputerInfo`

---

**Q2: When was first command run?**

Check file creation date (Properties).

- Answer: `May 18, 2025`

---

**Q3: Flag in PowerShell history?**

Check other users' history files.

Path: `C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`

- Answer: `THM{it_was_me!}`

---

## Task 8: Conclusion

**No answer needed**

---

## Quick Reference

### Critical Event IDs

**Security:**
- 4624 - Successful login
- 4625 - Failed login
- 4720 - User created
- 4732 - User added to group

**Sysmon:**
- 1 - Process creation
- 3 - Network connection
- 11 - File created
- 13 - Registry changed
- 22 - DNS query

### Logon Types

- **2** - Interactive (keyboard)
- **3** - Network (shares, RDP with NLA)
- **10** - RDP (without NLA)

### Correlation Keys

**Logon ID:** Link activities to login session

**ProcessId:** Link Sysmon events to process

### Detection Patterns

**Brute Force:**
```
Multiple 4625 → Single 4624 from same IP
```

**Backdoor Account:**
```
4624 (login) → 4720 (create user) → 4732 (add to admin)
```

**Malware Execution:**
```
Event 1 (download) → Event 11 (file) → Event 3 (C2)
```

### File Locations

**Suspicious paths:**
- C:\Temp
- C:\Users\Public
- C:\Windows\Temp
- Downloads folder

**Persistence locations:**
- Startup folder
- Registry Run keys
- Scheduled tasks

---

## Key Takeaways

- **4624/4625** most frequent events
- **Logon Type** critical field (3=Network, 10=RDP)
- **Logon ID** correlates activities
- **Sysmon** provides detailed process context
- **ProcessId** links Sysmon events
- **Event 1** foundation for process analysis
- **Parent process** reveals execution chain
- **Network events** show C2 connections
- **File events** reveal persistence
- **PowerShell history** logs commands
- **Multiple users** = multiple history files
- **Correlate across** Security, Sysmon, PowerShell
- **Build timeline** using timestamps and IDs

Master log correlation to trace complete attack chains
Happy hunting! 
