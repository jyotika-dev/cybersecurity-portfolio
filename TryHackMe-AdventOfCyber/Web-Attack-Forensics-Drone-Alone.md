# Advent of Cyber 2025 Day 15 - TryHackMe

> Web Attack Forensics - Splunk Investigation

**Day 15 Link:** https://tryhackme.com/room/webattackforensics-aoc2025-b4t7c1d5f8

---

## Story

**Scenario:** TBFC's drone scheduler under attack

**Problem:** Suspicious HTTP requests with Base64 payloads

**Alert:** "Apache spawned unusual process"

**Goal:** Investigate command injection attack using Splunk

---

## Command Injection Basics

**What is it:**
- Web vulnerability
- Execute OS commands through web app
- Exploits unsanitized input

**How it works:**
```
Normal: /cgi-bin/hello.bat?name=John
Attack: /cgi-bin/hello.bat?name=John;whoami
                                    ^^^^^^^^ injected
```

**Common injection characters:**
```
;   # Command separator
&   # Background execution
|   # Pipe
&&  # AND operator
||  # OR operator
``  # Command substitution
$() # Command substitution
```

---

## Splunk Setup

### Access Details

**URL:** `http://MACHINE_IP:8000`

**Credentials:**
- Username: `Blue`
- Password: `Pass1234`

**Important:** Set time range to "Last 7 days" or "All time"!

---

## Investigation Queries

### Query 1: Find Suspicious Commands

**Goal:** Detect command execution in web requests

```spl
index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression") 
| table _time host clientip uri_path uri_query status
```

**Look for:**
- Base64 strings in URLs
- cmd.exe or powershell in parameters
- /cgi-bin/ paths
- Status 200 (successful)

---

### Query 2: Check Error Logs

**Goal:** Find server-side execution errors

```spl
index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")
```

**Important:** Select "View: Raw" above Event display

**Look for:**
- 500 Internal Server Error
- Command execution attempts
- CGI script errors

---

### Query 3: Trace Process Creation

**Goal:** Find processes spawned by Apache

```spl
index=windows_sysmon ParentImage="*httpd.exe"
```

**Important:** Select "View: Table" above Event display

**Look for:**
- ParentImage: httpd.exe (Apache)
- Image: cmd.exe or powershell.exe
- **Critical:** Apache should NEVER spawn system commands!

---

### Query 4: Confirm Reconnaissance

**Goal:** Find whoami command execution

```spl
index=windows_sysmon *cmd.exe* *whoami*
```

**Why whoami:**
- Attackers check privileges
- Confirms successful execution
- Post-exploitation recon

---

### Query 5: Find Encoded PowerShell

**Goal:** Detect Base64-encoded commands

```spl
index=windows_sysmon Image="*powershell.exe" (CommandLine="*enc*" OR CommandLine="*-EncodedCommand*" OR CommandLine="*Base64*")
```

**Look for:**
- `-enc` flag
- Base64 strings
- Encoded payloads

---

## Decoding Base64

### Example Payload

**Encoded:**
```
VABoAGkAcwAgAGkAcwAgAG4AbwB3ACAATQBpAG4AZQAhACAATQBVAEEASABBAEEASABBAEEA
```

**Steps:**
1. Copy Base64 string
2. Visit https://www.base64decode.org/
3. Paste and click "DECODE"

**Decoded:**
```
This is now Mine! MUAHAHAHA
```

---

## Questions & Answers

### Q1: Reconnaissance executable name?

**Query used:**
```spl
index=windows_sysmon *cmd.exe* *whoami*
```

**Answer:** `whoami.exe`

---

### Q2: Executable attacker ran via injection?

**Query used:**
```spl
index=windows_apache_access (cmd.exe OR powershell)
```

Or:
```spl
index=windows_sysmon ParentImage="*httpd.exe"
```

**Answer:** `PowerShell.exe`

---

## Attack Chain Reconstruction

### Phase 1: Web Attack
```
Attacker crafts malicious URL
    ↓
/cgi-bin/hello.bat?cmd=powershell -enc [BASE64]
    ↓
Apache processes request
    ↓
Command injection successful
```

### Phase 2: OS Execution
```
Apache spawns cmd.exe
    ↓
cmd.exe runs whoami (recon)
    ↓
cmd.exe runs powershell -enc
    ↓
Base64 payload executes
```

### Phase 3: Compromise
```
Payload: "This is now Mine!"
    ↓
System compromised
```

---

## Quick Reference

### Splunk SPL Commands

```spl
# Specify data source
index=windows_sysmon

# Display as table
| table field1 field2

# Count events
| stats count by field

# Filter results
| where status=500

# Sort results
| sort -_time

# Remove duplicates
| dedup clientip
```

### Important Indexes

```
windows_apache_access  # Web access logs
windows_apache_error   # Web error logs
windows_sysmon         # Process execution
windows_security       # Windows events
```

---

## Evidence Found

| Step | Source | Finding |
|------|--------|---------|
| 1 | Access logs | Suspicious URI with cmd.exe |
| 2 | Error logs | 500 errors, execution attempts |
| 3 | Sysmon | Apache spawned cmd.exe |
| 4 | Sysmon | whoami.exe executed |
| 5 | Sysmon | PowerShell with Base64 |

---

## Detection Patterns

### Suspicious Web Traffic

**Red flags:**
- cmd.exe in URL parameters
- powershell in query strings
- Base64-encoded data
- /cgi-bin/ paths
- Multiple encoding layers

### Abnormal Processes

**Web server should NOT spawn:**
- cmd.exe
- powershell.exe
- whoami.exe
- net.exe
- Any system commands

**Normal Apache processes:**
- httpd.exe
- Worker threads only
- No child processes

---

## Defense Strategies

### Input Validation

```python
# Whitelist approach
import re
allowed = re.compile(r'^[a-zA-Z0-9_]+$')
if not allowed.match(user_input):
    return "Invalid input"
```

### WAF Rules

```
# Block injection patterns
Deny cmd.exe
Deny powershell
Deny whoami
Deny encoded commands
```

### Apache Hardening

```apache
# Disable CGI if not needed
# LoadModule cgi_module modules/mod_cgi.so

# Run as non-privileged user
User apache
Group apache
```

### Monitoring

```spl
# Alert on Apache spawning commands
index=windows_sysmon ParentImage="*httpd.exe" 
(Image="*cmd.exe" OR Image="*powershell.exe")
| alert
```

---

## Key Indicators

### Web Layer

- cmd.exe or powershell in URLs
- Base64-encoded strings
- 200 OK with suspicious params
- Unusual query lengths

### OS Layer

- Web server spawning system processes
- whoami execution
- Encoded PowerShell commands
- Reconnaissance activity

### Post-Exploitation

- Account enumeration
- System information gathering
- Network scanning
- Persistence mechanisms

---

## Investigation Workflow

```
1. Check Web Access Logs
    ↓
2. Look for Injection Patterns
    ↓
3. Review Error Logs
    ↓
4. Check Process Creation (Sysmon)
    ↓
5. Identify Parent-Child Relationships
    ↓
6. Find Reconnaissance Commands
    ↓
7. Decode Base64 Payloads
    ↓
8. Reconstruct Attack Chain
```

---

## Useful Splunk Searches

### Failed Logins
```spl
index=windows_security EventCode=4625 
| stats count by Account_Name
```

### Process Timeline
```spl
index=windows_sysmon EventCode=1 
| table _time ParentImage Image CommandLine
| sort _time
```

### Network Connections
```spl
index=windows_sysmon EventCode=3 
| stats count by SourceIp DestinationIp
```

### PowerShell Downloads
```spl
index=windows_sysmon Image="*powershell.exe" 
(CommandLine="*downloadstring*" OR CommandLine="*invoke-webrequest*")
```

---

## Key Takeaways

- **Command injection** executes OS commands via web
- **Splunk** correlates web and OS logs
- **Base64** is not encryption
- **whoami** indicates successful exploitation
- **Apache spawning commands** = compromise
- **Multiple log sources** show full picture
- **Parent-child relationships** reveal exploitation
- **Quick detection** prevents damage
- **Always decode** suspicious strings
- **Defense in depth** is essential

---

Happy hunting!
