# Log Analysis with SIEM - TryHackMe

> Master SIEM log analysis to detect and investigate malicious activity

**Room Link:** https://tryhackme.com/room/loganalysiswithsiem

<img width="1336" height="927" alt="image" src="https://github.com/user-attachments/assets/6d74e1c7-a0b8-43d5-b29d-24109348318b" />


---

## Task 1: Introduction

**Topics:**
- SIEM data sources
- Log correlation
- Windows/Linux/Web logs
- Malicious behavior analysis

**Prerequisites:**
- Introduction to SIEM
- Windows Logging Capabilities
- Cyber Kill Chain
- Sysmon

**Lab Access:** https://LAB_WEB_URL.p.thmlabs.com

Wait 4-5 minutes for Splunk to launch.

**No answer needed**

---

## Task 2: Benefits of SIEM for Analysts

### Key Benefits

**Centralisation:**
- All logs in one place
- No switching between systems
- Faster investigations

**Correlation:**
- Link separate events
- Build complete picture
- Enrich alerts with context

**Historical Events:**
- Review past activity
- Identify patterns
- Track behavior over time

### Questions

**Q1: Process of linking data from multiple sources to identify relationships?**

- Answer: `Correlation`

---

**Q2: Process of collecting logs from multiple systems into single location?**

- Answer: `Centralisation`

---

## Task 3: Log Sources Overview

### Host-Based Logs

**Sources:**
- Workstations
- Servers (web, SQL, DNS)
- Endpoints

**Value:** Nearly every attack involves hosts

### Network-Based Logs

**Sources:**
- Firewalls
- Routers
- IDS/IPS

**Value:** Visibility into device communication

### Web-Based Logs

**Sources:**
- Web applications
- HTTP servers

**Value:** Common attack entry point

### Important Concepts

**Time Pitfalls:**
- Logs from different time zones
- SIEM time vs local time
- Always check time zone settings

**Log Normalisation:**
- Convert various formats (JSON, XML, text)
- Single consistent structure
- Easier searching and filtering

### Questions

**Q1: Process of converting logs from different formats into single format?**

- Answer: `Normalisation`

---

**Q2: Log source type to detect malicious script execution?**

- Answer: `Host-Based`

---

## Task 4: Windows Logs

### Scenario

Alert: Suspicious network connection on port 5678 from host WIN-105

### Investigation

**Initial query:**
```spl
index=task4 ComputerName=WIN-105 DestinationPort=5678
| table Time host User EventType EventCode Image SourceIp SourcePort DestinationIp DestinationPort Message
```

### Questions

**Q1: Which IP was connection established with?**

- Answer: `10.10.114.80`

---

**Q2: Which process initiated suspicious connection?**

- Answer: `SharePoint.exe`

---

**Q3: MD5 hash of malicious process?**

Filter for SharePoint.exe:
```spl
index=task4 *SharePoint*
```

- Answer: `770D14FFA142F09730B415506249E7D1`

---

**Q4: Name of scheduled task created on system?**

- Answer: `Office365 Install`

---

## Task 5: Linux Logs

### Scenario

Alert: Possible persistence via new remote-ssh user on Ubuntu server

### Investigation

**Query for account creation:**
```spl
index=task5
| search "Account Created" OR "new user"
| table _time _raw
```

### Questions

**Q1: Timestamp of remote-ssh account creation?**

- Answer: `2025-08-12 09:52:57`

---

**Q2: User who escalated to root prior to account creation?**

Check for privilege escalation:
```spl
index=task5 (su OR sudo)
| table _time _raw
```

- Answer: `jack-brown`

---

**Q3: IP address user logged in from?**

Filter SSH logs:
```spl
index=task5 "jack-brown" "ssh"
| table _time _raw
```

- Answer: `10.14.94.82`

---

**Q4: Failed login attempts before successful login?**

Check auth.log:
```spl
index=task5 source="auth.log"
| search "Failed password for jack-brown"
| table _time _raw
```

- Answer: `4`

---

**Q5: Port for persistence mechanism?**

Check syslog:
```spl
index=task5 source="syslog" *port*
| table _time _raw
```

- Answer: `7654`

---

## Task 6: Web Application Logs

### Scenario

Alert: Spike in activity on web server

### Investigation

**Query:**
```spl
index=task6 method=POST
| bin _time span=5m
| stats values(referer_domain) as referer_domain values(status) as status values(useragent) as UserAgent values(uri_path) as uri_path count by clientip _time
| table referer_domain clientip UserAgent uri_path count status
```

### Questions

**Q1: URI path with highest number of requests?**

Check uri_path column for highest count.

- Answer: `/wp-login.php`

---

**Q2: Source IP address of activity?**

- Answer: `10.10.243.134`

---

**Q3: How can this activity be classified?**

Multiple failed login attempts.

- Answer: `Brute Force`

---

**Q4: Which tool did threat actor use?**

Check UserAgent field.

- Answer: `WPScan`

---

## Task 7: Conclusion

**No answer needed**

---

## Quick Reference

### Splunk Query Basics

**Search syntax:**
```spl
index=<name> field=value
| search "keyword"
| table field1 field2 field3
| stats count by field
```

**Time binning:**
```spl
| bin _time span=5m
```

**Wildcards:**
```spl
*keyword*
```

### Investigation Workflow

```
Alert Received
    ↓
Identify Known Data (IP, hostname, port)
    ↓
Initial Broad Search
    ↓
Filter for Specifics
    ↓
Correlate Related Events
    ↓
Extract IOCs
    ↓
Classify Activity
```

### Common Splunk Commands

| Command | Purpose |
|---------|---------|
| `index=` | Specify data source |
| `search` | Filter results |
| `table` | Display specific fields |
| `stats` | Aggregate data |
| `sort` | Order results |
| `dedup` | Remove duplicates |
| `bin` | Group by time |
| `top` | Most common values |

### Windows Log Indicators

**Suspicious activity:**
- Unusual network connections
- Unknown processes
- Scheduled tasks
- Non-standard ports
- Hash mismatches

**Key fields:**
- ComputerName
- EventCode
- Image (process)
- DestinationPort
- SourceIp

### Linux Log Indicators

**Suspicious activity:**
- New user accounts
- Privilege escalation (su, sudo)
- Failed login attempts
- SSH from unknown IPs
- Unusual ports in syslog

**Key sources:**
- auth.log (authentication)
- syslog (system events)
- secure (security events)

### Web Log Indicators

**Suspicious activity:**
- High request volume
- POST to login pages
- Known attack tools in UserAgent
- 401/403 status codes
- Unusual URI paths

**Key fields:**
- clientip
- uri_path
- useragent
- status
- method

### Attack Classifications

**Brute Force:**
- Multiple failed logins
- Same target
- Short time span
- Single source IP

**Persistence:**
- New user accounts
- Scheduled tasks
- Startup items
- SSH keys added

**Network Recon:**
- Port scans
- Service enumeration
- Network discovery

**Web Attacks:**
- Login brute force
- SQL injection attempts
- Path traversal
- Known exploit tools

### SIEM Best Practices

**Time handling:**
- Check SIEM time zone
- Compare with local time
- Use UTC for consistency
- Note time differences

**Correlation strategy:**
- Start with known indicator
- Expand to related events
- Check multiple log sources
- Build timeline

**Field identification:**
- Use broad search first
- Identify available fields
- Narrow with specifics
- Create targeted queries

---

## Key Takeaways

- **SIEM centralises** all logs
- **Correlation** connects events
- **Normalisation** unifies formats
- **Host-based logs** detect execution
- **Time zones** matter in analysis
- **Always start broad** then filter
- **Table command** displays results
- **Stats command** aggregates data
- **Windows logs** show processes
- **Linux logs** track authentication
- **Web logs** reveal attacks
- **UserAgent** identifies tools
- **Failed logins** indicate brute force
- **Privilege escalation** = red flag
- **Multiple sources** = complete picture

Correlate to investigate
Happy hunting
