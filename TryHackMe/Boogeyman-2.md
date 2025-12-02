# BoogeyMan 2 - TryHackMe

> Memory forensics and malicious document analysis

**Room Link:** https://tryhackme.com/room/boogeyman2

<img width="1581" height="925" alt="image" src="https://github.com/user-attachments/assets/b8bf3cd5-6f7c-48e0-b847-12accef77a83" />

---

## Investigation

**Artifacts:**
- Phishing email
- Malicious document
- Memory dump (WKSTN-2961.raw)

**Tools:**
- Volatility3
- LibreOffice
- md5sum
- strings

**Location:** Desktop/Artefacts folder

---

## Task: Email and Document Analysis

### Email Investigation

Open email file in Berkeley Mailbox.

**Q1: Email used to send phishing email?**

- Answer: `westaylor23@outlook.com`

---

**Q2: Email of victim employee?**

- Answer: `maxine.beck@quicklogisticsorg.onmicrosoft.com`

---

**Q3: Name of attached malicious document?**

- Answer: `Resume_WesleyTaylor.doc`

---

**Q4: MD5 hash of malicious attachment?**

**Command:**
```bash
md5sum Resume_WesleyTaylor.doc
```

- Answer: `52c4384a0b9e248b95804352ebec6c5b`

---

### Macro Analysis

Open document in LibreOffice.

Check AutoOpen macro module.

**Q5: URL to download stage 2 payload from macro?**

Check macro source code.

Downloads update.png.

- Answer: `https://files.boogeymanisback.lol/aa2a9c53cbb80416d3b47d85538d9971/update.png`

---

**Q6: Process that executed stage 2 payload?**

Check macro execution method.

- Answer: `wscript.exe`

---

**Q7: Full file path of stage 2 payload?**

Macro saves to ProgramData.

- Answer: `C:\ProgramData\update.js`

---

## Task: Memory Forensics

### Process Analysis

**List processes:**
```bash
vol -f WKSTN-2961.raw windows.pslist
```

**Q8: PID of process that executed stage 2?**

Find wscript.exe PID.

- Answer: `4260`

---

**Q9: Parent PID of stage 2 process?**

Check PPID of wscript.exe.

- Answer: `1124`

---

**Q10: URL to download malicious binary?**

Same URL, change .png to .exe.

- Answer: `https://files.boogeymanisback.lol/aa2a9c53cbb80416d3b47d85538d9971/update.exe`

---

**Q11: PID of malicious C2 process?**

Check process list for updater.exe.

Spawned by wscript.exe.

- Answer: `6216`

---

**Q12: Full file path of C2 process?**

**Command:**
```bash
vol -f WKSTN-2961.raw windows.cmdline
```

Find updater.exe path.

- Answer: `C:\Windows\Tasks\updater.exe`

---

### Network Analysis

**Check connections:**
```bash
vol -f WKSTN-2961.raw windows.netscan
```

**Q13: IP and port of C2 connection? (Format: IP:port)**

Find updater.exe network connection.

- Answer: `128.199.95.189:8080`

---

**Q14: Full file path of email attachment in memory?**

Check cmdline output for .doc file.

INetCache location.

- Answer: `C:\Users\maxine.beck\AppData\Local\Microsoft\Windows\INetCache\Content.Outlook\WQHGZCFI\Resume_WesleyTaylor (002).doc`

---

### Persistence Mechanism

**Extract scheduled tasks:**
```bash
strings WKSTN-2961.raw | grep schtasks
```

**Q15: Full command for persistent access?**

Look for /Create and /TN Updater.

- Answer: `schtasks /Create /F /SC DAILY /ST 09:00 /TN Updater /TR 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -NonI -W hidden -c \"IEX ([Text.Encoding]::UNICODE.GetString([Convert]::FromBase64String((gp HKCU:\Software\Microsoft\Windows\CurrentVersion debug).debug)))\"'`

---

## Quick Reference

### Attack Flow

```
Phishing Email
    ↓
Resume_WesleyTaylor.doc
    ↓
AutoOpen Macro
    ↓
Download update.png
    ↓
Save as update.js
    ↓
Execute via wscript.exe
    ↓
Download update.exe
    ↓
Save as updater.exe
    ↓
C2 Connection
    ↓
Scheduled Task Persistence
```

### Volatility3 Commands

**List processes:**
```bash
vol -f file.raw windows.pslist
```

**Command lines:**
```bash
vol -f file.raw windows.cmdline
```

**Network connections:**
```bash
vol -f file.raw windows.netscan
```

**Process tree:**
```bash
vol -f file.raw windows.pstree
```

### Process Relationships

```
WINWORD.EXE (1124)
    ↓
wscript.exe (4260) - Stage 2
    ↓
updater.exe (6216) - C2
```

### Key Artifacts

**Email:**
- Sender: westaylor23@outlook.com
- Victim: maxine.beck@quicklogisticsorg.onmicrosoft.com
- Attachment: Resume_WesleyTaylor.doc

**Payloads:**
- Stage 1: Resume_WesleyTaylor.doc (macro)
- Stage 2: update.js (downloads update.exe)
- Stage 3: updater.exe (C2 beacon)

**Persistence:**
- Scheduled task: Updater
- Time: Daily at 09:00
- Executes: PowerShell with registry payload

### Macro Analysis

**Document:** Resume_WesleyTaylor.doc

**Module:** AutoOpen

**Action:**
1. Download update.png
2. Save as update.js
3. Execute with wscript.exe

**URL:** https://files.boogeymanisback.lol/

### Memory Forensics Tips

**Finding PIDs:**
- Use windows.pslist
- Check parent-child relationships
- Note suspicious process names

**Network activity:**
- windows.netscan shows connections
- Filter by process name/PID
- Check ESTABLISHED connections

**Strings extraction:**
- Last resort for missing data
- Useful for commands/URLs
- Grep for known keywords

### Scheduled Task Persistence

**Command breakdown:**
```cmd
schtasks /Create /F /SC DAILY /ST 09:00 /TN Updater
```

**/Create:** Create new task

**/F:** Force overwrite

**/SC DAILY:** Daily schedule

**/ST 09:00:** Start time

**/TN Updater:** Task name

**Payload:**
- PowerShell hidden window
- Reads registry key
- Base64 decode and execute

**Registry location:**
```
HKCU:\Software\Microsoft\Windows\CurrentVersion debug
```

### Detection Points

**Email:**
- External sender
- Unsolicited resume
- .doc attachment

**Document:**
- AutoOpen macro
- Downloads external file
- .png extension (suspicious)

**Execution:**
- wscript.exe spawned by WINWORD.EXE
- Downloads .exe from same domain
- Runs from Windows\Tasks

**Network:**
- Connection to unknown IP
- Port 8080
- Persistent beacon

**Persistence:**
- Scheduled task created
- PowerShell hidden execution
- Registry-based payload

---

## Key Takeaways

- **Memory dumps** preserve evidence
- **Volatility3** analyzes Windows memory
- **AutoOpen macro** executes automatically
- **Multi-stage payloads** evade detection
- **wscript.exe** runs JavaScript
- **Process relationships** reveal attack chain
- **windows.pslist** lists processes
- **windows.netscan** shows connections
- **strings + grep** finds hidden data
- **Scheduled tasks** common persistence
- **Registry** stores malicious payloads
- **Base64** obfuscates commands
- **C2 beacons** maintain access

Happy hunting!
