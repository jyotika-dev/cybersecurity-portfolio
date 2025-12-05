# Advent of Cyber 2025 Day 3 - TryHackMe

> Log Analysis - Ransomware Investigation with Splunk

**Day 3 Link:** https://tryhackme.com/room/splunkforloganalysis-aoc2025-x8fj2k4rqp

---

## Story

**Crisis:** Ransomware attack by King Malhare's Bandit Bunnies

**Target:** TBFC web server (10.10.1.5)

**Goal:** EAST-mas takeover

**Mission:** Investigate with Splunk, find attack vector

---

## Investigation Setup

**Access Splunk:** Click Start Machine, visit provided URL

**Wait:** 2-3 minutes for Splunk to boot

**Navigate:** Search & Reporting tab

### Data Sources

**web_traffic:** Web connections to/from server

**firewall_logs:** Network traffic (allowed/blocked)

**Server IP:** 10.10.1.5

---

## Investigation Steps

### 1. Initial Search

```spl
index=main sourcetype=web_traffic
```

**Results:** 17,172 events

**Key fields:**
- user_agent
- path
- client_ip
- status

### 2. Timeline Analysis

**Query:**
```spl
index=main sourcetype=web_traffic 
| timechart span=1d count
```

Click Visualization tab for graph.

**Sort by count:**
```spl
index=main sourcetype=web_traffic 
| timechart span=1d count 
| sort by count 
| reverse
```

Peak traffic day identified!

### 3. Anomaly Detection

**Check suspicious fields:**
- user_agent (curl, wget, sqlmap)
- client_ip (single IP with high volume)
- path (traversal attempts, shell.php)

### 4. Filter Legitimate Traffic

```spl
index=main sourcetype=web_traffic 
user_agent!=*Mozilla* 
user_agent!=*Chrome* 
user_agent!=*Safari* 
user_agent!=*Firefox*
```

Single attacker IP identified!

### 5. Top Malicious IPs

```spl
sourcetype=web_traffic 
user_agent!=*Mozilla* 
user_agent!=*Chrome* 
user_agent!=*Safari* 
user_agent!=*Firefox* 
| stats count by client_ip 
| sort -count 
| head 5
```

**Note:** Replace <REDACTED> in queries below with attacker IP

### 6. Reconnaissance

```spl
sourcetype=web_traffic 
client_ip="<REDACTED>" 
AND path IN ("/.env", "/*phpinfo*", "/.git*") 
| table _time, path, user_agent, status
```

Tools: curl, wget

Status: 404/403/401

### 7. Path Traversal

```spl
sourcetype=web_traffic 
client_ip="<REDACTED>" 
AND path="*..\/..\/*" OR path="*redirect*" 
| stats count by path
```

Attempts to read system files!

### 8. SQL Injection

```spl
sourcetype=web_traffic 
client_ip="<REDACTED>" 
AND user_agent IN ("*sqlmap*", "*Havij*") 
| table _time, path, status
```

Tools: sqlmap, Havij

Payload: SLEEP(5)

Status: 504 (successful injection)

### 9. Exfiltration Attempts

```spl
sourcetype=web_traffic 
client_ip="<REDACTED>" 
AND path IN ("*backup.zip*", "*logs.tar.gz*") 
| table _time, path, user_agent
```

Large file downloads detected!

### 10. Ransomware Execution

```spl
sourcetype=web_traffic 
client_ip="<REDACTED>" 
AND path IN ("*bunnylock.bin*", "*shell.php?cmd=*") 
| table _time, path, user_agent, status
```

**RCE confirmed:** shell.php execution

**Ransomware:** bunnylock.bin

### 11. C2 Communication

```spl
sourcetype=firewall_logs 
src_ip="10.10.1.5" 
AND dest_ip="<REDACTED>" 
AND action="ALLOWED" 
| table _time, action, protocol, src_ip, dest_ip, dest_port, reason
```

Outbound C2 connection established!

### 12. Data Exfiltration Volume

```spl
sourcetype=firewall_logs 
src_ip="10.10.1.5" 
AND dest_ip="<REDACTED>" 
AND action="ALLOWED" 
| stats sum(bytes_transferred) by src_ip
```

Calculate total bytes transferred!

---

## Questions

**Q1: Attacker IP compromising web server?**

From filtered queries showing single malicious IP.

- Answer: `198.51.100.55`

---

**Q2: Day of peak traffic? (YYYY-MM-DD)**

From timechart sorted results.

- Answer: `2025-10-12`

---

**Q3: Count of Havij user_agent events?**

Check Havij query results.

- Answer: `993`

---

**Q4: Path traversal attempts to access sensitive files?**

From path traversal query count.

- Answer: `658`

---

**Q5: Bytes transferred to C2 server?**

From sum(bytes_transferred) query.

- Answer: `126167`

---

## Quick Reference

### Attack Chain

```
1. Reconnaissance (curl, wget)
2. Enumeration (path traversal)
3. SQL Injection (sqlmap, Havij)
4. Exfiltration (backup files)
5. RCE (shell.php)
6. Ransomware (bunnylock.bin)
7. C2 Communication
8. Data Exfiltration
```

### Splunk SPL Commands

**timechart:**
```spl
| timechart span=1d count
```

**stats:**
```spl
| stats count by field
| stats sum(field) by field
```

**sort:**
```spl
| sort -count     # Descending
| reverse         # Reverse order
```

**table:**
```spl
| table _time, field1, field2
```

**head:**
```spl
| head 5          # Top 5 results
```

### Key Fields

**web_traffic:**
- client_ip
- user_agent
- path
- status
- _time

**firewall_logs:**
- src_ip
- dest_ip
- action
- bytes_transferred
- reason

### Attack Indicators

**Reconnaissance:**
- /.env, /.git
- phpinfo
- 404/403 responses

**Path Traversal:**
- ../../
- *redirect*
- System file access

**SQL Injection:**
- sqlmap user agent
- Havij user agent
- SLEEP(5) payload
- 504 status code

**RCE:**
- shell.php
- cmd= parameter
- Binary execution

**Ransomware:**
- bunnylock.bin
- C2 communication
- Data exfiltration

### Filtering Techniques

**Exclude:**
```spl
user_agent!=*Mozilla*
```

**Include:**
```spl
path IN ("*.php*", "*.bin*")
```

**AND:**
```spl
field1="value" AND field2="value"
```

**OR:**
```spl
path="*value1*" OR path="*value2*"
```

### Timeline Analysis

**Purpose:**
- Identify peak activity
- Find attack window
- Correlate events

**Visualization:**
- Graph view
- Event distribution
- Anomaly detection

### C2 Communication

**Indicators:**
- Outbound connections
- Unusual ports
- High data transfer
- ALLOWED action
- C2_CONTACT reason

**Confirmation:**
- Firewall logs
- Source = compromised server
- Destination = attacker IP

---

## Key Takeaways

- **Splunk** powerful for log analysis
- **timechart** visualizes patterns
- **Filter** legitimate traffic first
- **user_agent** reveals tools
- **Path traversal** enumeration technique
- **SQL injection** exploit method
- **RCE** via web shells
- **Ransomware** final payload
- **C2** post-exploitation
- **Firewall logs** show exfiltration
- **Correlate** multiple sources
- **Attack chains** follow patterns

Logs tell the story!

Happy hunting!
