# Alert Triage with Elastic - TryHackMe

> Investigate a multi-stage cyber attack using Elastic Stack and Kibana

**Room Link:** https://tryhackme.com/room/alerttriagewithelastic

<img width="1568" height="875" alt="image" src="https://github.com/user-attachments/assets/ba56c96a-2fb1-4f87-bbaa-f154e7fe5e5f" />

---

## Task 2: Environment Setup

### Initial Reconnaissance

**Data View:** Alert Triage With Elastic

**Time Range:** Select "Entire data range"

### Questions

**Q1: Total logs available for analysis?**

Check hit count above log entries.

- Answer: `1467`

---

**Q2: client.ip field value in weblogs index?**

**Query:**
```kql
_index:weblogs
```

Check client.ip field in Available Fields panel.

- Answer: `203.0.113.55`

---

## Task 3: Web Attacks

### ProxyLogon Exploitation

**Vulnerability:** CVE-2021-26855 (Microsoft Exchange)

**Attacker IP:** 203.0.113.55

### Questions

**Q1: POST requests to proxyLogon.ecp from 203.0.113.55?**

**Query:**
```kql
_index:weblogs and client.ip:203.0.113.55 and http.request.method:POST
```

Build table with:
- client.ip
- user.agent
- http.request.method
- url.path
- http.response.status_code

Count POST entries.

- Answer: `3`

---

**Q2: user.agent paired with IP 203.0.113.55?**

Check user.agent column from same query.

- Answer: `python-requests/2.25.1`

---

**Q3: Logs containing cmd= parameter in url.path?**

**Query:**
```kql
_index:weblogs and client.ip:203.0.113.55 and http.request.method:GET and errorEE.aspx
```

Sort Old → New, count cmd= entries.

- Answer: `20`

---

**Q4: Command run via errorEE.aspx on Jul 20, 2025 @ 04:45:50.000?**

Check url.path at timestamp 04:45:50.000.

Parse: errorEE.aspx?cmd=hostname

- Answer: `hostname`

---

## Task 4: Account Activity

### Windows Security Events

**Focus:** Authentication and user account manipulation

### Questions

**Q1: winlog.record_id of Administrator 4624 logon event?**

**Query:**
```kql
@timestamp >= "2025-07-20T05:11:22" and winlog.event_id:4624 and host.name:winserv2019.some.corp and winlog.event_data.TargetUserName:Administrator
```

Build table with:
- winlog.event_id
- host.name
- winlog.event_data.TargetUserName
- winlog.logon.type
- winlog.event_data.IpAddress

Expand log, find winlog.record_id.

- Answer: `17166`

---

**Q2: process.pid of Sysmon 1 event at Jul 20, 2025 @ 05:11:27.996?**

**Query:**
```kql
@timestamp >= "2025-07-20T05:11:27.996" and winlog.event_id:1 and user.name:Administrator
```

Table columns:
- user.name
- process.parent.name
- process.command_line
- process.pid

- Answer: `964`

---

**Q3: winlog.event_id for new user account creation?**

**Query:**
```kql
@timestamp >= "2025-07-20T05:13:10.000" and winlog.channel:Security and winlog.task:User Account Management
```

Sort Old → New, find user creation event.

- Answer: `4720`

---

**Q4: Name of new user account?**

Check message field in 4720 event.

Look for Account Name or TargetUserName.

- Answer: `svc_backup`

---

## Task 5: Command Execution

### Privilege Escalation

**Focus:** Group modifications and reconnaissance

### Questions

**Q1: Command to add account to Remote Desktop Users?**

**Query:**
```kql
@timestamp >= "2025-07-20T05:13:15" and process.parent.name:cmd.exe and user.name:Administrator
```

Table columns:
- process.command_line
- process.name
- process.parent.name

Find net localgroup command.

- Answer: `net localgroup "Remote Desktop Users" svc_backup /add`

---

**Q2: winlog.record_id of 4732 event adding user to Administrators?**

**Query:**
```kql
@timestamp >= "2025-07-20T05:13:15" and (winlog.event_id:4732 or process.parent.name:cmd.exe)
```

Table:
- winlog.event_id
- winlog.record_id
- process.command_line
- message

Find 4732 for Administrators group addition.

- Answer: `17254`

---

**Q3: PowerShell command on Jul 20, 2025 @ 05:16:14.628?**

**Query:**
```kql
@timestamp >= "2025-07-20T05:13:15" and event.module:powershell and event.code:4104
```

Add powershell.file.script_block_text column.

Navigate to timestamp 05:16:14.628.

- Answer: `net group "Domain Admins" /domain`

---

**Q4: Archive name created using Rar.exe?**

**Query:**
```kql
process.name: "Rar.exe"
```

Check process.command_line for output filename.

- Answer: `finance_it_archive.rar`

---

## Task 6: Conclusion

**No answer needed**

---

## Quick Reference

### Attack Timeline

```
04:38:00 - ProxyLogon exploit
04:40:30 - Web shell deployed (errorEE.aspx)
04:45:50 - Command execution
05:11:22 - RDP logon as Administrator
05:13:10 - Backdoor account created (svc_backup)
05:13:52 - Added to Administrators
05:16:14 - Domain recon
05:18:20 - Data compression (RAR)
```

### KQL Query Patterns

**Index filtering:**
```kql
_index:weblogs
_index:windows
```

**Field matching:**
```kql
field:value and field2:value2
```

**Time filtering:**
```kql
@timestamp >= "2025-07-20T05:00:00"
```

**Wildcards:**
```kql
url.path:*cmd=*
```

### Key Event IDs

**Windows Security:**
- 4624: Successful logon
- 4720: User account created
- 4732: Member added to group

**Sysmon:**
- 1: Process creation

**PowerShell:**
- 4104: Script block logging

### IOCs Identified

**Network:**
- IP: 203.0.113.55
- UserAgent: python-requests/2.25.1

**Host:**
- Web shell: errorEE.aspx
- Backdoor: svc_backup
- Archive: finance_it_archive.rar

**Behavioral:**
- Off-hours activity (5 AM)
- Rapid group modifications
- Domain reconnaissance
- Data staging

### Attack Phases

**1. Initial Access:**
- ProxyLogon exploitation
- Web shell deployment
- Command execution

**2. Privilege Escalation:**
- RDP logon
- Account creation
- Group membership

**3. Persistence:**
- Remote Desktop Users
- Server Operators
- Administrators

**4. Discovery:**
- whoami /priv
- hostname
- net group "Domain Admins" /domain

**5. Collection:**
- RAR compression
- Documents archived
- Scripts archived

### Kibana Tips

**Build tables:**
- Click "+" on fields
- Adds column to view
- Easy data visualization

**Sort results:**
- Click column header
- "Sort Old → New" for timeline
- "Sort New → Old" for recent first

**Expand logs:**
- Click ">" arrow
- View full JSON
- Find specific fields

**Time picker:**
- Top-right calendar icon
- Quick selects available
- Custom range option

### Investigation Techniques

**Multi-source correlation:**
- IIS logs (web attacks)
- Security logs (authentication)
- Sysmon logs (processes)
- PowerShell logs (commands)

**Timeline construction:**
- Sort chronologically
- Track attacker progression
- Identify attack phases

**Context analysis:**
- Normal vs abnormal
- Legitimate tools abused
- Time of day matters

### Red Flags

**Web:**
- Python UserAgent on web server
- POST to proxyLogon.ecp
- cmd= parameter in URLs
- errorEE.aspx web shell

**Authentication:**
- Off-hours logons
- Source IP matches attack
- Logon Type 10 (RDP)

**Account Activity:**
- New service accounts
- Rapid group additions
- Multiple privilege escalations
- Domain enumeration

**Data Access:**
- RAR compression
- IT admin folders
- Scripts directory
- Unusual archive names

---

## Key Takeaways

- **ProxyLogon** critical Exchange flaw
- **python-requests** = automated exploit
- **Web shells** execute commands
- **Event ID 4624** successful logon
- **Sysmon Event ID 1** process creation
- **Event ID 4720** user account created
- **Event ID 4732** group membership
- **svc_backup** backdoor account
- **net localgroup** adds to groups
- **PowerShell 4104** script blocks
- **Domain recon** signals escalation
- **RAR.exe** legitimate tool abused
- **Multi-source logs** complete picture
- **Timeline matters** for correlation

Correlate across sources!

Happy hunting!
