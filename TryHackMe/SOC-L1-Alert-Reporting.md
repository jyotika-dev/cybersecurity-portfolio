# SOC L1 Alert Reporting & Escalation - TryHackMe

> Learn how to properly report, escalate, and communicate SOC alerts

**Room Link:** https://tryhackme.com/room/socreportingescalation

<img width="1574" height="861" alt="image" src="https://github.com/user-attachments/assets/392c914e-137e-4634-a05e-28c66f8dc153" />


---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Alert Funnel

### The Three Essential Steps

After triaging an alert, your work continues through:

**1. Reporting**
- Document investigation steps
- Include collected evidence
- Write clear conclusions

**2. Escalation**
- Pass real threats to L2 analysts
- Save time with detailed reports
- Enable faster response

**3. Communication**
- Coordinate with IT, HR, management
- Keep organization aligned
- Verify critical information

### Questions

**What is the process of passing suspicious alerts to an L2 analyst for review?**

When real threats need deeper analysis.

- Answer: `alert escalation`

---

**What is the process of formally describing alert details and findings?**

Documenting what you found.

- Answer: `alert reporting`

---

## Task 3: Reporting Guide

### Why Write Reports?

**Benefits:**
- Saves L2 time
- Preserves evidence
- Improves your skills
- Prevents data loss

### The Five Ws Framework

**Who**
- User involved
- Service account
- Process name

**What**
- Exact actions taken
- Commands run
- Files downloaded

**When**
- Start timestamp
- End timestamp
- Duration

**Where**
- Host name
- IP address
- URL or cloud instance

**Why**
- Your reasoning
- Classification logic
- Supporting evidence

### Report Writing Tips

- Keep it short and factual
- Include key log snippets
- Back up with evidence
- Make it easy to read

### Questions

**According to the SOC dashboard, which user email leaked the sensitive document?**

Check the "Sensitive Document Share to External" alert.

Look under "source user email".

- Answer: `m.boslan@tryhackme.thm`

---

**Looking at the new alerts, who is the "sender" of the suspicious, likely phishing email?**

Open "Email marked as Phishing after Delivery" alert.

Check the Sender field.

- Answer: `support@microsoft.com`

---

**Open the phishing alert, read its details, and try to understand the activity. Using the Five Ws template, what flag did you receive after writing a good report?**

Analyze the phishing email alert:

**Key indicators:**
- SPF and DKIM tests failed (spoofing sign)
- Urgency triggers ("urgent notice", "price increase")
- Archive attachment (red flag)
- Fake Microsoft support address

**Example report structure:**

**Who:** support@microsoft.com (spoofed sender) targeting internal users

**What:** Phishing email with failed authentication checks, urgency-based social engineering, and suspicious archive attachment

**When:** [Use timestamp from alert]

**Where:** Email delivered to user inbox despite authentication failures

**Why:** True Positive - Multiple phishing indicators (SPF/DKIM failures, social engineering tactics, suspicious attachment)

- Answer: `THM{nice_attempt_faking_microsoft_support}`

---

## Task 4: Escalation Guide

### When to Escalate

Escalate to L2 if:

- Major cyberattack indicator
- Remediation needed (malware removal, isolation)
- External communication required
- You need senior help

### Escalation Process

**Steps:**
1. Write your alert report
2. Set alert to "In Progress"
3. Reassign to L2 on shift
4. Ping them in chat

### What L2 Does Next

- Reviews your report
- Validates True Positive
- Communicates with departments
- Starts Incident Response if needed

### Requesting Help

**It's okay to ask for support!**

Better to discuss unclear alerts than close them without understanding.

### Questions

**Who is your current L2 in the SOC dashboard that you can assign (escalate) the alerts to?**

Check the "In Progress" alerts to see who's assigned.

- Answer: `E.Fleming`

---

**What flag did you receive after correctly escalating the alert from the previous task to L2?**

Set phishing alert to "In Progress" and assign to E.Fleming.

Click Save.

- Answer: `THM{good_job_escalating_your_first_alert}`

---

**Now, investigate the second new alert in the queue and provide a detailed alert comment. Then, decide if you need to escalate this alert and move on according to the process.**

Analyze the Active Directory reconnaissance alert:

**Key indicators:**
- **Suspicious process:** revshell.exe (parent process)
- **Location:** C:\Users\Public\ (common drop location)
- **Commands:** whoami, net user, Get-ADUser, nltest, net group "Domain Admins"
- **User context:** NT AUTHORITY\SYSTEM (highest privilege)
- **Host:** DMZ-MSEXCHANGE-2013 (mail server, internet-facing)
- **Parent chain:** w3wp.exe → revshell.exe → cmd.exe

**Example report:**

**Who:** NT AUTHORITY\SYSTEM executing commands via revshell.exe

**What:** Active Directory enumeration commands (whoami, net user, Get-ADUser, nltest, net group "Domain Admins" /domain)

**When:** [Alert timestamp]

**Where:** DMZ-MSEXCHANGE-2013 (mail server), process: C:\Users\Public\revshell.exe

**Why:** True Positive - Classic post-exploitation activity. Attacker likely exploited Exchange/IIS, dropped reverse shell, achieved SYSTEM privileges, and is now enumerating domain for lateral movement preparation.

**Escalate to L2:** Yes - requires immediate remediation (host isolation, credential rotation, threat hunting)

- Answer: `THM{looks_like_webshell_via_old_exchange}`

---

## Task 5: SOC Communication

### Crisis Communication Cases

**Case 1: L2 unavailable for 30+ minutes**
- Call L2 directly
- If no response → contact L3
- Finally → contact manager

**Case 2: Compromised chat account**
- Don't use the breached platform
- Use alternative method (phone call)

**Case 3: Alert overload**
- Prioritize by workflow
- Immediately inform L2

**Case 4: Realized you missed an attack**
- Contact L2 immediately
- Explain your concerns
- Better late than never

**Case 5: SIEM logs unavailable/broken**
- Don't skip the alert
- Investigate what you can
- Report issue to L2/SOC engineer

### Questions

**Should you first try to contact your manager in case of a critical threat (Yea/Nay)?**

No, follow proper escalation chain: L2 → L3 → Manager

- Answer: `nay`

---

**Should you immediately contact your L2 if you think you missed the attack (Yea/Nay)?**

Yes! Attackers can stay silent for weeks. Report concerns immediately.

- Answer: `yea`

---

## Task 6: Conclusion

**No answer needed**

---

## Quick Reference

### Alert Reporting Checklist

- [ ] Answer the Five Ws
- [ ] Include key evidence
- [ ] Keep it short and factual
- [ ] Back up conclusions
- [ ] Use clear language

### Escalation Checklist

- [ ] Write complete report
- [ ] Set status to "In Progress"
- [ ] Assign to L2 on shift
- [ ] Ping them in chat
- [ ] Be available for questions

### Red Flags for Escalation

- Privilege escalation detected
- Reverse shell or webshell
- AD enumeration commands
- Credential dumping attempts
- Lateral movement indicators
- Confirmed malware execution
- Data exfiltration

### Communication Best Practices

| Situation | Action |
|-----------|--------|
| Urgent + L2 unavailable | Call → L3 → Manager |
| Account compromise | Use alternative channel |
| Alert flood | Prioritize + inform L2 |
| Missed attack | Contact L2 immediately |
| Broken SIEM | Investigate + report issue |

---

## Key Takeaways

- **Alert reporting** preserves findings for future reference
- **Five Ws** framework keeps reports structured
- **Escalate** confirmed threats and uncertain cases
- **L2 escalation** enables proper remediation
- **Communication** keeps teams coordinated
- **SPF/DKIM failures** indicate email spoofing
- **Reverse shells** + **AD recon** = immediate escalation
- **NT AUTHORITY\SYSTEM** = highest privilege level
- **Ask for help** when uncertain - it's encouraged!
- **Document everything** - logs expire but reports don't

Proper reporting and escalation turn alerts into actionable intelligence
