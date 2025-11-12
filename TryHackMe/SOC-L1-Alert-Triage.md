# SOC L1 Alert Triage - TryHackMe

> Learn how to handle SOC alerts from detection to resolution

**Room Link:** https://tryhackme.com/room/socl1alerttriage

<img width="1557" height="836" alt="image" src="https://github.com/user-attachments/assets/cc7ddf46-cedd-4786-b680-4defcc0c6a7f" />


---

## Task 1: Introduction

### What You'll Learn

- SOC alert concepts
- Alert fields and statuses
- Alert classification
- L1 analyst triage process
- Real SOC workflows

### Prerequisites

- Common network attacks
- Windows/Linux attacks
- SOC roles and duties

**No answer needed**

---

## Task 2: Events and Alerts

### How Alerts Work

**Events** → **Logs** → **Analysis** → **Alerts**

**Event examples:**
- User login
- Process launch
- File download

**Alert Tools:**
- SIEM (Splunk, Elastic)
- EDR/NDR (Defender, CrowdStrike)
- SOAR (Automation platforms)
- ITSM (Jira, TheHive)

### SOC Roles

**L1 Analysts:**
- First line of defense
- Review and validate alerts
- Escalate confirmed threats

**L2 Analysts:**
- Deep investigation
- Handle escalated alerts

**SOC Engineers:**
- Fine-tune alerts
- Reduce false positives

### Questions

**What is the number of alerts you see in the SOC dashboard?**

Count the alerts on the dashboard.

- Answer: `5`

---

**What is the name of the most recent alert you see?**

Top alert in the list.

- Answer: `Double-Extension File Creation`

---

## Task 3: Alert Properties

### Core Alert Fields

**Alert Time:**
- When alert was created
- Usually minutes after event

**Alert Name:**
- Description of activity
- Example: "Unusual Login Location"

**Severity:**
- 🟢 Low
- 🟡 Medium  
- 🟠 High
- 🔴 Critical

**Status:**
- New
- In Progress
- Closed

**Verdict:**
- 🔴 True Positive (real attack)
- 🟢 False Positive (harmless)

**Assignee:**
- Analyst responsible

**Description:**
- Details about the alert

**Fields:**
- Technical data (IPs, commands, etc.)

### Questions

**What was the verdict for the "Unusual VPN Login Location" alert?**

Check alert #4, look at verdict column.

- Answer: `False Positive`

---

**What user was mentioned in the "Unusual VPN Login Location" alert?**

Open alert details, check source user.

- Answer: `M.Clark`

---

## Task 4: Alert Prioritisation

### Priority Rules

**1. Filter First:**
- Skip assigned alerts
- Focus on new/unresolved

**2. Sort by Severity:**
- Critical → High → Medium → Low

**3. Sort by Time:**
- Oldest first
- Older breach = more damage

### Questions

**Should you first prioritise medium over low severity alerts? (Yea/Nay)**

Yes! Severity comes first.

- Answer: `Yea`

---

**Should you first take the newest alerts and then the older ones? (Yea/Nay)**

No! Oldest first - likely causing more damage.

- Answer: `Nay`

---

**Assign yourself to the first-priority alert and change its status to In Progress. The name of your selected alert will be the answer.**

**Steps:**
1. Find highest priority alert (Critical + oldest)
2. Assignee → You
3. Status → In Progress

- Answer: `Potential Data Exfiltration`

---

## Task 5: Alert Triage

### Three Triage Stages

**1. Initial Actions:**
- Assign to yourself
- Set Status: In Progress
- Review details

**2. Investigation:**
- Check who/what is affected
- Understand suspicious action
- Review related events
- Use threat intelligence

**3. Final Actions:**
- Decide verdict (TP or FP)
- Write comment explaining reasoning
- Set Status: Closed

---

### Question 1: First Priority Alert

**Which flag did you receive after you correctly triaged the first-priority alert?**

**Alert:** Potential Data Exfiltration

**Investigation:**
- Large data transfer to external destination
- Check if legitimate (Zoom meetings)
- Looks like normal business activity

**Verdict:** False Positive

**Steps:**
1. Status → Closed
2. Verdict → False Positive
3. Add comment about Zoom usage

- Answer: `THM{looks_like_lots_of_zoom_meetings}`

---

### Question 2: Second Priority Alert

**Which flag did you receive after you correctly triaged the second-priority alert?**

**Alert:** Double-Extension File Creation

**Investigation:**
- File: cats_video.mp4.exe
- Double extension = suspicious!
- Check MD5 hash on VirusTotal
- Result: Trojan detected!

**Verdict:** True Positive

**Steps:**
1. Assign to yourself
2. Status → Closed
3. Verdict → True Positive
4. Add comment about malware

- Answer: `THM{how_could_this_user_fall_for_it?}`

---

### Question 3: Third Priority Alert

**Which flag did you receive after you correctly triaged the third-priority alert?**

**Alert:** Download from GitHub Repository

**Investigation:**
- Downloaded from github.com/facebook/react
- React = popular JavaScript framework
- Official repository = legitimate
- Normal developer activity

**Verdict:** False Positive

**Steps:**
1. Assign to yourself
2. Status → Closed
3. Verdict → False Positive
4. Add comment about legitimate repo

- Answer: `THM{should_we_allow_github_for_devs?}`

---

## Task 6: Conclusion

**No answer needed**

---

## Quick Reference

### Alert Severity Levels

| Severity | Priority | Color |
|----------|----------|-------|
| Critical | Highest | 🔴 |
| High | High | 🟠 |
| Medium | Medium | 🟡 |
| Low | Lowest | 🟢 |

### Alert Statuses

| Status | Meaning |
|--------|---------|
| New | Not yet reviewed |
| In Progress | Being investigated |
| Closed | Investigation complete |

### Alert Verdicts

| Verdict | Meaning |
|---------|---------|
| True Positive | Real threat |
| False Positive | Benign/harmless |

### Triage Workflow

```
1. Filter → New/Unresolved alerts
2. Sort → Severity (Critical first)
3. Sort → Time (Oldest first)
4. Assign → Take ownership
5. Status → In Progress
6. Investigate → Analyze the alert
7. Decide → True/False Positive
8. Comment → Explain reasoning
9. Status → Closed
```

### Investigation Steps

1. **Check affected entity** (user, host, IP)
2. **Review suspicious action** (login, download, etc.)
3. **Examine related events** (before/after)
4. **Verify with threat intel** (VirusTotal, etc.)
5. **Make decision** (TP or FP)

---

## Real Alert Examples

### False Positive Examples

- Legitimate business tools (Zoom, VPN)
- Developer downloads (GitHub repos)
- Normal user behavior
- Approved software

### True Positive Examples

- Double-extension files (.mp4.exe)
- Malware detected on VirusTotal
- Unauthorized access
- Suspicious command execution

---

## Key Takeaways

- **5 alerts** in dashboard
- **Double-Extension File Creation** most recent
- **M.Clark** in VPN alert
- **Prioritize by severity** then time
- **Oldest alerts first** - likely causing damage
- **Three triage stages** - Initial, Investigation, Final
- **Assign to yourself** before investigating
- **Mark In Progress** during investigation
- **Close with verdict** after investigation
- **True Positive** = real threat
- **False Positive** = harmless activity
- **Comment always** - explain your reasoning
- **VirusTotal** checks file hashes
- **Double extensions** often malicious
- **Official repos** usually legitimate

Alert triage is the foundation of SOC work. Proper investigation and documentation ensure real threats are caught and false alarms don't waste time
Happy triaging!
