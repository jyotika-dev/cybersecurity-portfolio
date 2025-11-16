# The Greenholt Phish - TryHackMe

> Investigate a suspicious email received by a sales executive

**Room Link:** https://tryhackme.com/room/phishingemails5fgjlzxc
---

## Scenario

Sales executive received unexpected email from customer claiming:
- Money transferred to his account
- Included unrequested attachment
- Suspicious circumstances

**Task:** Investigate the email using `challenge.eml` file.

---

## Questions

### Q1: What is the Transfer Reference Number listed in the email's Subject?

**Analysis:**

Open email with Thunderbird:
```bash
thunderbird challenge.eml
```
Check Transfer Reference Number in the mail

- Answer: `06/10/2020 05:58`

---

### Q2: Who is the email from?

Check "From" header in email.

Displays sender's name.

- Answer: `Mr. James Jackson`

---

### Q3: What is his email address?

Listed next to sender's name in "From" field.

- Answer: `info@mutawamarine.com`

---

### Q4: What email address will receive a reply to this email?

Check "Reply-To" header.

**Red flag:** Different from "From" address!

- Answer: `info.mutawamarine@mail.com`

---

### Q5: What is the Originating IP?

Open `challenge.eml` file in text editor.

Search email headers for "Received:" or "X-Originating-IP:"

- Answer: `192.119.71.157`

---

### Q6: Who is the owner of the Originating IP?

**Method 1:** WHOIS lookup on IP

**Method 2:** Domain reputation check

**Note:** NameCheap, Inc. is registrar, but actual owner needed.

Use multiple sources for verification.

- Answer: `Hostwinds LLC`

---

### Q7: What is the SPF record for the Return-Path domain?

**Tool:** MXToolbox

**Steps:**
1. Copy full email headers
2. Paste into MXToolbox header analyzer
3. Find SPF section

**SPF record format:** `v=spf1 [mechanisms] [policy]`

- Answer: `v=spf1 include:spf.protection.outlook.com -all`

---

### Q8: What is the DMARC record for the Return-Path domain?

Use same MXToolbox analysis.

Find DMARC section.

**Policy:** `p=quarantine` means suspicious emails go to spam.

- Answer: `v=DMARC1; p=quarantine; fo=1`

---

### Q9: What is the name of the attachment?

Open `challenge.eml` in text editor.

Search for "Content-Disposition: attachment"

Look for filename parameter.

**Red flags in filename:**
- Multiple extensions (.PDF__.CAB)
- Unusual characters
- Trying to appear as PDF

- Answer: `SWT_#09674321____PDF__.CAB`

---

### Q10: What is the SHA256 hash of the file attachment?

**Steps:**
1. Open email in Thunderbird
2. Save attachment to desktop
3. Calculate hash:
```bash
sha256sum SWT_#09674321____PDF__.CAB
```
- Answer: `2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f`

---

### Q11: What is the attachments file size? (format: NUM KB)

**Method:** VirusTotal lookup

1. Copy SHA256 hash
2. Search on virustotal.com
3. Check file details section

- Answer: `400.26 KB`

---

### Q12: What is the actual file extension of the attachment?

Check VirusTotal "Details" tab.

Look for "File type" or "Type description"

**Finding:** Disguised as .CAB but actually .RAR

**This is malicious behavior** - file extension spoofing!

- Answer: `RAR`

---

## Quick Reference

### Investigation Tools Used

| Tool | Purpose |
|------|---------|
| **Thunderbird** | View email visually |
| **Text Editor** | Read raw .eml file |
| **WHOIS** | IP ownership lookup |
| **MXToolbox** | Email header analysis |
| **sha256sum** | Calculate file hash |
| **VirusTotal** | File analysis |

### Email Headers Checklist

**Sender Information:**
- [ ] From address
- [ ] Reply-To address (compare to From)
- [ ] Return-Path

**Routing Information:**
- [ ] Originating IP
- [ ] Received headers (read bottom-up)
- [ ] X-Originating-IP

**Authentication:**
- [ ] SPF result
- [ ] DKIM result
- [ ] DMARC result

**Content:**
- [ ] Subject line
- [ ] Attachments
- [ ] Links/URLs

### Red Flags Found

**1. Reply-To Mismatch**
```
From: info@mutawamarine.com
Reply-To: info.mutawamarine@mail.com
```
Replies go to different domain!

**2. Suspicious Attachment Name**
```
SWT_#09674321____PDF__.CAB
```
- Double extensions
- Trying to appear as PDF
- Random numbers/characters

**3. File Extension Spoofing**
```
Filename: *.CAB
Actual type: RAR
```
Disguising true file type

**4. Unexpected Email**
- Not expecting communication
- Unrequested attachment
- Claims of money transfer

### SPF Record Breakdown

```
v=spf1 include:spf.protection.outlook.com -all

v=spf1        - SPF version 1
include:      - Authorized domain
-all          - Fail non-authorized (hard fail)
```

### DMARC Record Breakdown

```
v=DMARC1; p=quarantine; fo=1

v=DMARC1      - DMARC version 1
p=quarantine  - Policy: send to spam if fail
fo=1          - Forensic reporting options
```

### File Analysis Workflow

**1. Extract attachment safely**
- Use email client
- Save to isolated location
- Don't execute!

**2. Calculate hash**
```bash
sha256sum filename
md5sum filename
```

**3. Check reputation**
- VirusTotal
- Hybrid Analysis
- Any.Run

**4. Analyze file properties**
- True file type
- File size
- Creation date
- Digital signature

### VirusTotal Analysis

**Basic Details:**
- File name
- File size
- File type (actual)
- Hash values (MD5, SHA1, SHA256)

**Detection:**
- AV detection ratio
- Community score
- Behavior sandbox

**Relations:**
- Similar files
- C2 infrastructure
- Campaign indicators

### Investigation Steps Summary

1. **Open email visually** (Thunderbird)
   - Get timestamp, sender, subject

2. **Check headers** (text editor)
   - Originating IP
   - Routing path
   - Authentication results

3. **Verify sender** (WHOIS, reputation)
   - IP owner
   - Domain age
   - Reputation scores

4. **Analyze authentication** (MXToolbox)
   - SPF records
   - DMARC policy
   - DKIM signatures

5. **Inspect attachment** (carefully!)
   - Extract safely
   - Calculate hash
   - Check VirusTotal

6. **Document findings**
   - All IOCs
   - Red flags
   - Verdict

---

## Key Takeaways

- **Thunderbird** opens .eml files visually
- **Reply-To mismatch** is major red flag
- **info@mutawamarine.com** sender address
- **Different Reply-To domain** = suspicious
- **192.119.71.157** originating IP
- **Hostwinds LLC** IP owner
- **SPF includes** outlook.com for authentication
- **DMARC quarantine** policy for failures
- **Double extensions** hide true file type
- **.CAB disguising .RAR** = malicious
- **sha256sum** calculates file hashes
- **VirusTotal** analyzes file reputation
- **MXToolbox** decodes email headers
- **400.26 KB** file size
- **Always verify sender** before opening attachments

Never trust unexpected emails with attachments
Happy investigating!
