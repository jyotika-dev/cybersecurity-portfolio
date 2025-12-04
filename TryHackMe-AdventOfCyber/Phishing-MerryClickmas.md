# Advent of Cyber 2025 Day 2 - TryHackMe

> Phishing - Merry Clickmas

**Day 2 Link:** https://tryhackme.com/room/phishing-aoc2025-h2tkye9fzU

---

## Story

**Scenario:** TBFC red team tests employee phishing awareness

**Team:** Recon McRed, Exploit McRed, Pivot McRed

**Target:** factory@wareville.thm

**Goal:** Test if cyber security training worked

---

## Social Engineering Basics

**Definition:** Manipulating users to make mistakes

**Examples:**
- Share passwords
- Open malicious files
- Approve payments

**Psychological tricks:**
- Urgency
- Curiosity
- Authority

### S.T.O.P. Mnemonic

**Before clicking, ask:**
- **S**uspicious?
- **T**elling me to click?
- **O**ffering amazing deal?
- **P**ushing me to act now?

**Remember to:**
- **S**low down
- **T**ype address yourself
- **O**pen nothing unexpected
- **P**rove the sender

---

## Setting Up Phishing Campaign

### Fake Login Page

**Location:** ~/Rooms/AoC2025/Day02

**Start server:**
```bash
cd ~/Rooms/AoC2025/Day02
./server.py
```

**Output:**
```
Starting server on http://0.0.0.0:8000
```

**Test page:**
- Firefox → http://CONNECTION_IP:8000
- Or http://127.0.0.1:8000

Server captures entered credentials!

---

## Social-Engineer Toolkit (SET)

### Start SET

```bash
setoolkit
```

### Step 1: Select Attack Type

**Menu:** Social-Engineering Attacks

```
set> 1
```

### Step 2: Select Mass Mailer

**Menu:** Mass Mailer Attack

```
set> 5
```

### Step 3: Single Email

**Menu:** E-Mail Attack Single Email Address

```
set:mailer> 1
```

### Step 4: Configure Email

**Email details:**
- **To:** factory@wareville.thm
- **Delivery:** Use your own server or open relay (option 2)
- **From:** updates@flyingdeer.thm
- **Name:** Flying Deer
- **Username:** [blank - press Enter]
- **Password:** [blank - press Enter]
- **SMTP server:** MACHINE_IP
- **Port:** 25 [default]

**Message options:**
- **High priority:** no
- **Attach file:** n
- **Inline file:** n

**Email content:**
- **Subject:** Shipping Schedule Changes
- **Format:** plain text [default]
- **Body:**
```
Dear elves,
Kindly note significant changes to shipping schedules.
Please confirm new schedule: http://CONNECTION_IP:8000
Best regards,
Flying Deer
END
```

Type **END** (capitals) on new line when finished.

### Wait for Results

Check server.py terminal for captured credentials.

Wait 1-2 minutes.

---

## Questions

**Q1: Password to access TBFC portal?**

Check server.py output for captured credentials.

- Answer: `unranked-wisdom-anthem`

---

**Q2: Total toys expected for delivery?**

Visit http://MACHINE_IP in Firefox.

Login to factory user mailbox with captured password.

Check email for toy count.

- Answer: `1984000`

---

## Quick Reference

### Phishing Types

**Email (Phishing):**
- Most common
- Fake emails

**SMS (Smishing):**
- Text messages
- Short links

**Voice (Vishing):**
- Phone calls
- Impersonation

**QR Code (Quishing):**
- Malicious QR codes
- Scan and compromise

**Social Media:**
- Direct messages
- Fake profiles

### SET Workflow

```
setoolkit
    ↓
1. Social-Engineering Attacks
    ↓
5. Mass Mailer Attack
    ↓
1. Single Email Address
    ↓
Configure Email Details
    ↓
Write Phishing Message
    ↓
Send & Wait for Credentials
```

### Email Configuration

**Critical fields:**
- To: Target email
- From: Trusted source (spoofed)
- Subject: Convincing reason
- Body: Include phishing link

**Delivery:**
- Direct SMTP (MACHINE_IP)
- Port 25 (default)
- No authentication needed

### Fake Login Page

**Purpose:**
- Capture credentials
- Looks legitimate
- Hosted locally

**Setup:**
```bash
./server.py
```

**Access:**
```
http://CONNECTION_IP:8000
```

**Credentials captured automatically**

### Red vs Blue Team

**Red Team (attackers):**
- Test defenses
- Authorized attacks
- Phishing campaigns
- Identify weaknesses

**Blue Team (defenders):**
- Protect systems
- Awareness training
- Detect attacks
- Respond to threats

### Anti-Phishing Tips

**Check sender:**
- Real email address
- Not just display name
- Look for typosquatting

**Verify links:**
- Hover before clicking
- Check actual URL
- Type address manually

**Be suspicious:**
- Urgent requests
- Amazing offers
- Unexpected attachments
- Grammar mistakes

**When in doubt:**
- Contact sender directly
- Use known contact info
- Don't use message links
- Report suspicious emails

### Common Phishing Indicators

**Email red flags:**
- Generic greetings
- Spelling errors
- Suspicious sender
- Urgency tactics
- Unexpected attachments
- Too good to be true

**Link red flags:**
- Shortened URLs
- Misspelled domains
- Unusual TLDs
- HTTP (not HTTPS)
- Random subdomains

### Credential Harvesting

**How it works:**
1. Fake login page created
2. Hosted on attacker server
3. Link sent via phishing
4. User enters credentials
5. Captured by server
6. Attacker gains access

**Prevention:**
- Check URL before login
- Use password managers
- Enable 2FA
- Report phishing

---

## Key Takeaways

- **Social engineering** targets humans
- **Phishing** uses messages
- **S.T.O.P.** before clicking
- **SET** automates phishing
- **Fake login pages** steal credentials
- **Email spoofing** easy to do
- **Always verify** sender
- **Type URLs** manually
- **Red teaming** tests defenses
- **Awareness training** crucial
- **One credential** = full access
- **Password reuse** dangerous


Happy hunting
