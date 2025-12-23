# Advent of Cyber 2025 Day 12 - TryHackMe

> Phishing - Phishmas Greetings

**Day 12 Link:** https://tryhackme.com/room/spottingphishing-aoc2025-r2g4f6s8l0

---

## Story

**Scenario:** TBFC's Email Protection Platform is down

**Problem:** No automatic filtering - manual triage needed

**Suspect:** Malhare's Eggsploit Bunnies sending phishing emails

**Goal:** Identify phishing emails vs legitimate messages

---

## Phishing vs Spam

### Phishing (Targeted Attack)

**Intent:**
- Steal credentials
- Deliver malware
- Exfiltrate data
- Financial fraud

**Characteristics:**
- Targeted and crafted
- Impersonates trusted sources
- Creates urgency
- Seeks sensitive info

### Spam (Bulk Noise)

**Intent:**
- Advertising
- Clickbait
- Data harvesting
- Fake offers

**Characteristics:**
- Mass distribution
- Generic content
- Promotional
- Not targeted

---

## Spotting Phishing

### Red Flags

**Impersonation:**
- Sender email doesn't match official domain
- Uses free email (gmail, yahoo)
- Pretends to be internal staff

**Urgency:**
- Words: "urgent", "immediately", "act now"
- Deadline pressure
- Threatens consequences

**Side channel:**
- Moves conversation off email
- Uses WhatsApp, Telegram, SMS
- Non-standard contact methods

**Malicious intent:**
- Requests credentials
- Suspicious attachments
- Fake payment approvals
- Phishing links

### Advanced Techniques

**Typosquatting:**
- `glthub.com` instead of `github.com`
- Common misspellings
- Similar-looking domains

**Punycode:**
- Unicode characters mimicking Latin
- `xn--mircrosft-8bb.com` looks like `microsoft.com`
- Non-ASCII tricks

**Spoofing:**
- Fake sender address
- Check email headers
- Verify `Authentication-Results`
- Check `Return-Path`

**Malicious attachments:**
- Disguised as voice messages
- Fake documents
- Installs malware
- Steals passwords

---

## Email Analysis

### Access Platform

**URL:**
```
https://LAB_WEB_URL.p.thmlabs.com
```

**Tool:** Wareville Email Threat Inspector

---

## Email Classification

### Email 1: Invoice Fraud

**Indicators:**
- Spoofed sender address
- Creates urgency
- Fake invoice attached
- Email headers don't match

**Verdict:** Phishing

**Flag:** `THM{yougotnumber1-keep-it-going}`

---

### Email 2: Fake Voice Message

**Indicators:**
- Spoofed as McSkidy
- Claims audio file
- Attachment is NOT audio
- Potentially malicious file

**Verdict:** Phishing

**Flag:** `THM{nmumber2-was-not-tha-thard!}`

---

### Email 3: Gmail Impersonation

**Indicators:**
- Free domain (gmail.com)
- Not official TBFC domain
- Uses "urgent" and "immediately"
- Requests side-channel communication

**Verdict:** Phishing

**Flag:** `THM{Impersonation-is-areal-thing-keepIt}`

---

### Email 4: HR Salary Scam

**Indicators:**
- Impersonates TBFC HR
- External sender domain
- Social engineering trap
- Fake approval process

**Verdict:** Phishing

**Flag:** `THM{Get-back-SOC-mas!!}`

---

### Email 5: Promotional Spam

**Indicators:**
- From CandyCane Co Logistics
- Just promotional content
- No malicious intent
- Bulk marketing

**Verdict:** Spam (Not Phishing)

**Flag:** `THM{It-was-just-a-sp4m!!}`

---

### Email 6: Punycode Attack

**Indicators:**
- Punycode (non-ASCII characters)
- Mimics official domain
- Spoofs TBFC IT team
- Phishing link: `microsoftooline.co`
- Fake Microsoft login page

**Verdict:** Phishing

**Flag:** `THM{number6-is-the-last-one!-DX!}`

---

## Quick Reference

### Phishing Checklist

**Check sender:**
- [ ] Email domain matches company
- [ ] Not from free email service
- [ ] Email structure is standard
- [ ] Authentication headers pass

**Analyze content:**
- [ ] No urgency pressure
- [ ] Reasonable request
- [ ] Standard communication channel
- [ ] Professional language

**Inspect links:**
- [ ] Hover to check actual URL
- [ ] No typosquatting
- [ ] No punycode tricks
- [ ] HTTPS and legitimate domain

**Review attachments:**
- [ ] Expected file type
- [ ] Reasonable file name
- [ ] Matches email content
- [ ] No suspicious extensions

### Email Header Analysis

**Key fields to check:**

```
From: Displayed sender
Return-Path: Actual sender path
Authentication-Results: SPF, DKIM, DMARC checks
Received: Email routing path
```

**Look for:**
- Mismatched sender/return-path
- Failed authentication
- External routing
- Suspicious IP addresses

### Common Phishing Domains

**Free email services:**
- gmail.com
- yahoo.com
- outlook.com
- hotmail.com

**Typosquatting examples:**
- microsoftonline.co (not .com)
- gooogle.com
- paypa1.com (1 instead of l)

**Punycode tricks:**
- xn-- prefix indicates punycode
- Characters that look identical
- Non-Latin alphabets

---

## Analysis Workflow

```
1. Check Sender Email
    ↓
2. Verify Domain
    ↓
3. Review Email Headers
    ↓
4. Analyze Content
    ↓
5. Check for Urgency
    ↓
6. Inspect Links/Attachments
    ↓
7. Classify: Phishing/Spam/Legitimate
```

---

## Defense Tips

**For users:**
- Always verify sender
- Don't trust urgency
- Hover before clicking
- Check file extensions
- Use standard channels
- Report suspicious emails

**For organizations:**
- Implement SPF/DKIM/DMARC
- Email filtering systems
- User awareness training
- Multi-factor authentication
- Phishing simulations
- Incident response process

**For SOC analysts:**
- Review email headers
- Check authentication results
- Analyze attachment hashes
- Investigate sender reputation
- Track similar campaigns
- Document IOCs

---

## Key Takeaways

- **Phishing** targets specific users for malicious purposes
- **Spam** is bulk unwanted content
- **Urgency** is a major red flag
- **Free domains** for business = suspicious
- **Check headers** to verify sender
- **Punycode** can hide fake domains
- **Attachments** need careful inspection
- **Side channels** avoid detection
- **Always verify** through official channels
- **Train users** to spot phishing

---

Happy hunting!
