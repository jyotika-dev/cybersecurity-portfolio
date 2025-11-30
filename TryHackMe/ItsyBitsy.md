# ItsyBitsy - TryHackMe

> Investigate a potential C2 communication alert using Kibana

**Room Link:** https://tryhackme.com/room/itsybitsy

<img width="1470" height="892" alt="image" src="https://github.com/user-attachments/assets/3f2cf08a-b947-4a85-b90c-6426c005958d" />

---

## Task 1: Introduction

**Scenario:** IDS alert about potential C2 communication

**User:** Browne (HR department)

**Suspicious pattern:** THM:{________}

**Data:** Week-long HTTP connection logs in Kibana

**Credentials:**
- Username: Admin
- Password: elastic123

**No answer needed**

---

## Task 2: Investigate C2 Communication

### Investigation

**Index:** connection_logs

**Goal:** Find the malicious file and C2 server

### Questions

**Q1: Events returned for March 2022?**

Set date range in Discover tab:
- Start: March 1, 2022
- End: March 31, 2022

Check hit count.

- Answer: `1482`

---

**Q2: IP associated with suspected user?**

Filter by source_ip field.

Two IPs found:
- One with 99.6% traffic (normal)
- One with 0.4% traffic (suspicious)

Check user_agent for suspicious IP.

**Key finding:** bitsadmin user agent

**What is bitsadmin?**
- Windows binary for updates
- Also abused by attackers for downloads

Check destination for bitsadmin traffic.

**Red flag:** Connects to pastebin.com, not Microsoft!

- Answer: `192.166.65.54`

---

**Q3: Windows binary used to download file?**

Check user_agent field for suspicious IP.

- Answer: `bitsadmin`

---

**Q4: Filesharing site acting as C2 server?**

Check destination domain for bitsadmin traffic.

- Answer: `pastebin.com`

---

**Q5: Full URL of C2 connection?**

Combine domain + URI path.

Check uri field in logs.

- Answer: `pastebin.com/yTg0Ah6a`

---

**Q6: Name of file accessed?**

Check the URI or file referenced in logs.

- Answer: `secret.txt`

---

**Q7: Secret code in file format THM{_____}?**

Access the pastebin URL or check log content.

- Answer: `THM{SECRET__CODE}`

---

## Quick Reference

### Investigation Steps

```
Alert Received
    ↓
Check All Events (date range)
    ↓
Identify Source IPs
    ↓
Analyze User Agents
    ↓
Check Destinations
    ↓
Identify Anomalies
    ↓
Extract IOCs
```

### Key Indicators

**Suspicious user_agent:**
- bitsadmin (legitimate tool abused)

**Suspicious destination:**
- pastebin.com (not Microsoft update servers)

**C2 characteristics:**
- Low traffic volume (0.4%)
- Filesharing site
- Non-standard binary usage

### Kibana Filtering

**By field:**
- Click field name in left panel
- View top values
- Click to filter

**Date range:**
- Click time picker
- Set custom range
- Apply to search

**Field analysis:**
- source_ip
- user_agent
- destination
- uri

### Bitsadmin Abuse

**Legitimate use:**
- Windows update downloads
- Background Intelligent Transfer Service
- Microsoft servers

**Malicious use:**
- Download payloads
- C2 communication
- Evade detection (LOLBin)

**Detection:**
- Check destination domains
- Verify against Microsoft IPs
- Unusual URIs

### C2 Communication Patterns

**Indicators:**
- Legitimate tools misused
- Filesharing sites
- Low-volume traffic
- Non-standard destinations
- Encoded payloads

**Common C2 channels:**
- Pastebin
- GitHub
- Discord
- Legitimate cloud services

### Analysis Tips

**Don't assume:**
- Low traffic % doesn't mean normal
- High traffic % doesn't mean malicious
- Need context

**Verify:**
- User agent purpose
- Destination legitimacy
- Expected behavior

**Pivot:**
- IP → User Agent
- User Agent → Destination
- Destination → URI
- URI → Content

### IOCs Collected

**Network:**
- IP: 192.166.65.54
- Domain: pastebin.com
- URL: pastebin.com/yTg0Ah6a

**Host:**
- Binary: bitsadmin
- File: secret.txt

**Artifact:**
- Pattern: THM{SECRET__CODE}

---

## Key Takeaways

- **Kibana** powerful for log analysis
- **Date ranges** filter events
- **Field analysis** reveals patterns
- **bitsadmin** LOLBin (Living Off Land Binary)
- **Pastebin** commonly abused for C2
- **Low traffic** doesn't mean benign
- **User agents** reveal tools used
- **Destination matters** more than volume
- **Context is key** for detection
- **Verify expected** vs actual behavior
- **Legitimate tools** can be abused
- **Filesharing sites** used by attackers
- **Always investigate** anomalies

Happy hunting
