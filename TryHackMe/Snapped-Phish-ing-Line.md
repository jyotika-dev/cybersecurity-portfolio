# Snapped Phish-ing Line - TryHackMe

> Investigate a real-world phishing campaign targeting SwiftSpend Financial

**Room Link:** https://tryhackme.com/room/snappedphishingline

<img width="1304" height="926" alt="image" src="https://github.com/user-attachments/assets/eeaafaae-89c9-4e6e-814a-f369f2923f5f" />


---

## Scenario

**Company:** SwiftSpend Financial

**Problem:** Multiple employees received suspicious emails. Some submitted credentials and can't log in.

**Your task:** Investigate phishing campaign by:
1. Analyzing email samples
2. Examining phishing URLs
3. Retrieving phishing kit
4. Using CTI tools
5. Analyzing adversary infrastructure

**Location:** Email samples in `phish-emails` directory on Desktop

---

## Questions

### Q1: Who is the individual who received an email attachment containing a PDF?

**Analysis:**

Open email samples with Thunderbird (.eml format).

Look for email with PDF attachment.

**Subject:** "Quote for Services Rendered processed on June 29 2020 100132 AM"

Check "To" field for recipient name.

- Answer: `William McClean`

---

### Q2: What email address was used by the adversary to send the phishing emails?

Check "From" field in email headers.

Look for sender address.

- Answer: `Accounts.Payable@groupmarketingonline.icu`

---

### Q3: What is the redirection URL to the phishing page for Zoe Duncan? (defanged format)

**Find email to Zoe Duncan.**

Open HTML attachment.

Extract full URL with parameters.

Defang the URL

- Answer: `hxxp[://]kennaroads[.]buzz/data/Update365/office365/40e7baa2f826a57fcf04e5202526f8bd/?email=zoe[.]duncan@swiftspend[.]finance&error`

---

### Q4: What is the URL to the .zip archive of the phishing kit? (defanged format)

**Enumerate the phishing domain.**

Browse to kennaroads.buzz using Firefox.

Look for directory listing or exposed files.

Find ZIP archive location.

Defang the URL

- Answer: `hxxp[://]kennaroads[.]buzz/data/Update365[.]zip`

---

### Q5: What is the SHA256 hash of the phishing kit archive?

**Download the ZIP file.**

**Calculate hash using CyberChef:**
1. Open CyberChef (in "tools" folder)
2. Load ZIP file into input
3. Use SHA2 function
4. Set hash size: 256

- Answer: `ba3c15267393419eb08c7b2652b8b6b39b406ef300ae8a18fee4d16b19ac9686`

---

### Q6: When was the phishing kit archive first submitted? (format: YYYY-MM-DD HH:MM:SS UTC)

**Use VirusTotal (on local machine):**
1. Search for file hash
2. Check "Details" tab
3. Find "First Submission" timestamp

- Answer: `2020-04-08 21:55:50 UTC`

---

### Q7: When was the SSL certificate the phishing domain used to host the phishing kit archive first logged? (format: YYYY-MM-DD)

**In ThreatBook:**

Search for domain: `kennaroads.buzz`

Check "Whois" tab.

- Answer: `2020-06-25`

---

### Q8: What was the email address of the user who submitted their password twice?

**Enumerate phishing domain further.**

Look for `log.txt` file.

**Location:** Usually in data directory or logs folder

**Contents:** Captured credentials from victims

Find entry with duplicate password submission.

- Answer: `michael.ascot@swiftspend.finance`

---

### Q9: What was the email address used by the adversary to collect compromised credentials?

**Download and extract phishing kit.**

**Search for email addresses in code:**
```bash
grep -r "@" * | grep -E "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b"
```

Look for email receiving stolen data.

**Common locations:**
- Mailer scripts
- Configuration files
- POST request handlers

- Answer: `m3npat@yandex.com`

---

### Q10: What is the email address that ends in "@gmail.com"?

Continue searching phishing kit files.

Look for Gmail address in code.

**Common locations:**
- Backup email addresses
- Notification recipients
- Test accounts

- Answer: `jamestanner2299@gmail.com`

---

### Q11: What is the hidden flag?

**Hint:** File called `flag.txt`

**Enumerate domain directories:**
```
hxxp://kennaroads.buzz/flag.txt
hxxp://kennaroads.buzz/data/flag.txt
hxxp://kennaroads.buzz/data/Update365/flag.txt
```

**Once found:**

1. Flag is encoded (Base64)
2. Also reversed

**Decode using CyberChef:**
1. Use "Magic" function (auto-detects Base64)
2. Add "Reverse" function after Base64

**Or manually:**
1. From Base64 recipe
2. Reverse recipe
3. Get plain text flag

- Answer: `THM{pL4y_w1Th_tH3_URL}`

---

## Quick Reference

### Investigation Tools

| Tool | Purpose |
|------|---------|
| **Thunderbird** | Open .eml files |
| **Firefox** | Browse phishing site |
| **CyberChef** | Hash calculation, decoding |
| **VirusTotal** | File/domain reputation |
| **grep** | Search for patterns in files |

### Phishing Kit Components

**Common files:**
- `index.html` or `index.php` - Login page
- `mailer.php` - Sends stolen credentials
- `log.txt` - Captured credentials
- `config.php` - Email settings
- Assets folder - Images, CSS, JS

**Key information to extract:**
- Adversary email addresses
- Victim data
- C2 infrastructure
- Creation dates
- Campaign targets

### Email Analysis Checklist

**Headers:**
- [ ] From address
- [ ] To address
- [ ] Subject line
- [ ] Date/timestamp
- [ ] Reply-To

**Body:**
- [ ] Links (extract and defang)
- [ ] Attachments
- [ ] Urgency indicators
- [ ] Spoofed branding

**Attachments:**
- [ ] File type (.html, .pdf, .zip)
- [ ] Contains links
- [ ] Redirects to phishing page

### File Hash Calculation

**Using CyberChef:**
1. Open CyberChef
2. Load file in Input pane
3. Search for "SHA2" in Operations
4. Drag to Recipe
5. Set Size: 256
6. Copy hash from Output

**Command line:**
```bash
sha256sum filename.zip
md5sum filename.zip
```

### VirusTotal Investigation

**File lookup:**
1. Search by hash
2. Check detection ratio
3. Find first submission date
4. Review behavior reports

**Domain lookup:**
1. Search domain name
2. Check registration date
3. Review DNS records
4. Find related URLs
5. Check reputation scores

### Common Phishing Kit Structures

```
phishing-kit/
├── index.html          # Fake login page
├── mailer.php          # Sends credentials
├── config.php          # Settings
├── log.txt             # Captured data
├── assets/
│   ├── images/
│   ├── css/
│   └── js/
└── data/
    └── logs/
```

### CyberChef Recipes

**Base64 Decode + Reverse:**
1. From Base64
2. Reverse ('Line feed')

**Magic Function:**
- Auto-detects encoding
- Tries multiple methods
- Shows confidence scores

**Common Operations:**
- From/To Base64
- Reverse
- ROT13
- URL Decode
- Hex operations

### Investigation Workflow

**1. Analyze emails:**
- Open with Thunderbird
- Extract sender info
- Note recipients
- Check attachments

**2. Examine URLs:**
- Extract from HTML attachments
- Defang before sharing
- Browse safely (VM only)

**3. Enumerate infrastructure:**
- Find phishing kit
- Download safely
- Calculate hash

**4. Use CTI tools:**
- VirusTotal for reputation
- WHOIS for registration
- Check submission dates

**5. Analyze phishing kit:**
- Extract ZIP archive
- Search for IOCs
- Find adversary emails
- Document findings

### IOCs to Collect

**From emails:**
- Sender addresses
- Phishing domains
- File attachments
- URLs

**From phishing kit:**
- Adversary emails
- Victim data
- File hashes
- Creation dates

**From infrastructure:**
- Domain registration
- Hosting provider
- Related domains
- IP addresses

---

## Key Takeaways

- **William McClean** received PDF attachment
- **Accounts.Payable@groupmarketingonline.icu** sent phishing emails
- **HTML attachments** contained phishing links
- **kennaroads.buzz** hosted phishing kit
- **Update365.zip** is the phishing kit archive
- **VirusTotal** shows first submission April 8, 2020
- **Domain registered** June 25, 2020 (recently created)
- **log.txt** captures victim credentials
- **michael.ascot** submitted password twice
- **Yandex email** collected stolen credentials
- **Gmail backup** email also present
- **grep** quickly finds patterns in code
- **CyberChef** decodes and reverses encoded data
- **Always defang** URLs before sharing
- **Phishing kits** leave forensic artifacts

Real phishing campaigns leave digital breadcrumbs
Happy hunting!
