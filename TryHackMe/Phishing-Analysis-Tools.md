# Phishing Analysis Tools - TryHackMe

> Learn tools to investigate suspicious emails

**Room Link:** https://tryhackme.com/room/phishingemails3tryoe

<img width="1294" height="918" alt="image" src="https://github.com/user-attachments/assets/26d7643a-e332-42a4-9846-130bebf9c140" />


---

## Task 1: Introduction

**No answer needed**

---

## Task 2: What Information Should We Collect?

**No answer needed**

---

## Task 3: Email Header Analysis

### Question

**What is the official site name of the bank that capitai-one.com tried to resemble?**

**Analysis:**

Look at the suspicious domain: `capitai-one.com`

Notice the typo: "capitai" vs "capital"

Google the legitimate bank name.

**Real website:** capitalone.com

**This is typosquatting** - registering domains with slight misspellings.

- Answer: `capitalone.com`

---

## Task 4: Email Body Analysis

### Question

**How can you manually get the location of a hyperlink?**

Right-click on a hyperlink in email body.

Select the option to view URL destination.

- Answer: `Copy Link Location`

---

## Task 5: Malware Sandbox

**No answer needed**

---

## Task 6: PhishTool

### Question

**Look at the Strings output. What is the name of the EXE file?**

Open PhishTool analysis.

Navigate to Strings section.

Look for executable file name.

**Found:** Disguised as PDF but actually EXE.

- Answer: `#454326_PDF.exe`

---

## Task 7: Phishing Case 1

### Netflix Phishing Campaign

**Brand impersonation case using PhishTool for analysis.**

### Questions

**What brand was this email tailored to impersonate?**

Check email content and branding.

Look for company logos and styling.

- Answer: `Netflix`

---

**What is the From email address?**

Check email headers in PhishTool.

Look at "From" field.

**Red flag:** Long random string in suspicious domain.

- Answer: `JGQ47wazXe1xYVBrkeDg-JOg7ODDQwWdR@JOg7ODDQwWdR-yVkCaBkTNp.gogolecloud.com`

---

**What is the originating IP? Defang the IP address.**

Find originating IP in headers.

Use CyberChef to defang.

**Defanging:** Replace dots with [.] to prevent accidental clicks.

- Answer: `209[.]85[.]167[.]226`

---

**From what you can gather, what do you think will be a domain of interest? Defang the domain.**

Check Return-Path header.

Look for suspicious domain.

Use CyberChef to defang.

- Answer: `etekno[.]xyz`

---

**What is the shortened URL? Defang the URL.**

Check Message URLs section.

Find shortened URL (t.co link).

Defang using CyberChef.

**Defanging URLs:**
- `https://` → `hxxps[://]`
- `.` → `[.]`

- Answer: `hxxps[://]t[.]co/yuxfzm8kpg?amp=1`

---

## Task 8: Phishing Case 2

### ANY.RUN Sandbox Analysis

**PDF-based phishing campaign.**

### Questions

**What does AnyRun classify this email as?**

Check classification banner on right side.

- Answer: `Suspicious activity`

---

**What is the name of the PDF file?**

Open text report in ANY.RUN.

Find file details section.

Look for PDF filename.

- Answer: `Payment-Advice.pdf`

---

**What is the SHA 256 hash for the file?**

Continue in text report.

Find hash values section.

Copy SHA256 hash.

- Answer: `CC6F1A04B10BCB168AEEC8D870B97BD7C20FC161E8310B5BCE1AF8ED420E2C24`

---

**What two IP addresses are classified as malicious? Defang the IP addresses.**

**Format:** IP_ADDR,IP_ADDR

Find Connections section in text report.

Look for IPs with "malicious" reputation.

Defang both IPs with CyberChef.

- Answer: `2[.]16[.]107[.]24,2[.]16[.]107[.]83`

---

**What Windows process was flagged as Potentially Bad Traffic?**

Check process list in ANY.RUN.

Look for flagged processes.

**Common target:** Windows system process.

- Answer: `svchost.exe`

---

## Task 9: Phishing Case 3

### Excel-based Malware Campaign

**Exploits known CVE vulnerability.**

### Questions

**What is this analysis classified as?**

Check ANY.RUN classification.

Higher severity than previous case.

- Answer: `Malicious activity`

---

**What is the name of the Excel file?**

Check file details in ANY.RUN.

Look for XLSX filename.

- Answer: `CBJ200620039539.xlsx`

---

**What is the SHA 256 hash for the file?**

Open Details section.

Find SHA256 hash value.

- Answer: `5F94A66E0CE78D17AFC2DD27FC17B44B3FFC13AC5F42D3AD6A5DCFB36715F3EB`

---

**What domains are listed as malicious? Defang the URLs & submit answers in alphabetical order.**

**Format:** URL1,URL2,URL3

Find malicious domains in report.

Defang each domain.

Sort alphabetically.

- Answer: `biz9holdings[.]com,findresults[.]site,ww38[.]findresults[.]site`

---

**What IP addresses are listed as malicious? Defang the IP addresses & submit answers from lowest to highest.**

**Format:** IP1,IP2,IP3

Find malicious IPs in connections.

Defang each IP.

Sort numerically (lowest to highest).

- Answer: `204[.]11[.]56[.]48,103[.]224[.]182[.]251,75[.]2[.]11[.]242`

---

**What vulnerability does this malicious attachment attempt to exploit?**

Check vulnerability section in report.

Look for CVE number.

**CVE-2017-11882:** Microsoft Office memory corruption vulnerability.

- Answer: `CVE-2017-11882`

---

## Task 10: Conclusion

**No answer needed**

---

## Quick Reference

### Analysis Tools

| Tool | Purpose |
|------|---------|
| **PhishTool** | Automated email analysis |
| **ANY.RUN** | Interactive malware sandbox |
| **CyberChef** | Data encoding/decoding |
| **VirusTotal** | File/URL reputation check |

### Defanging Guide

**Why defang?**
- Prevents accidental clicks
- Safe to share indicators
- Standard practice in reports

**How to defang:**

**IP Addresses:**
```
192.168.1.1 → 192[.]168[.]1[.]1
```

**URLs:**
```
https://evil.com → hxxps[://]evil[.]com
http://bad.site → hxxp[://]bad[.]site
```

**Domains:**
```
malicious.com → malicious[.]com
```

**Email addresses:**
```
bad@evil.com → bad[@]evil[.]com
```

### Email Header Fields

| Field | Information |
|-------|-------------|
| From | Sender email (can be spoofed) |
| Return-Path | Actual sending server |
| Reply-To | Where replies go |
| Received | Server chain (read bottom-up) |
| X-Originating-IP | Source IP address |
| Message-ID | Unique email identifier |
| SPF/DKIM/DMARC | Email authentication |

### Red Flags in Emails

**Suspicious Senders:**
- Long random strings in email
- Misspelled domains (typosquatting)
- Free email services for business
- Mismatched From/Reply-To

**Suspicious Links:**
- URL shorteners (bit.ly, t.co)
- Misspelled domains
- IP addresses instead of domains
- Different display text vs actual URL

**Suspicious Attachments:**
- .exe disguised as .pdf
- Office files with macros
- Compressed/password-protected files
- Unusual file extensions

### PhishTool Analysis Sections

**Headers:**
- Sender information
- Routing details
- Authentication results

**Body:**
- Email content
- HTML analysis
- Links extracted

**Attachments:**
- File hashes
- File type
- Malware scan results

**Strings:**
- Embedded URLs
- Email addresses
- Executable names

### ANY.RUN Sandbox Features

**File Analysis:**
- Behavior monitoring
- Network connections
- Process tree
- Registry changes

**Classifications:**
- No threats detected
- Suspicious activity
- Malicious activity

**Reports:**
- Text report (detailed)
- JSON report (programmatic)
- Screenshots
- Network traffic

### Common Phishing Techniques

**Brand Impersonation:**
- Netflix, Microsoft, banks
- Urgent account issues
- Verify/update information

**Typosquatting:**
- capitai-one.com (capital-one)
- gogolecloud.com (googlecloud)
- micros0ft.com (microsoft)

**URL Shorteners:**
- Hides actual destination
- Bypasses email filters
- Harder to analyze

**File Disguises:**
- .pdf.exe (double extension)
- Macro-enabled documents
- Archive files (.zip, .rar)

### CVE-2017-11882

**What it is:**
- Microsoft Office vulnerability
- Memory corruption flaw
- No macros needed
- Still exploited today

**Affected versions:**
- Office 2007-2016
- Equation Editor component

**Impact:**
- Remote code execution
- No user interaction needed
- Bypass security warnings

### Investigation Workflow

1. **Extract email file** (.eml, .msg)
2. **Analyze headers** (sender, routing, auth)
3. **Check body content** (links, branding)
4. **Extract URLs** (defang and analyze)
5. **Sandbox attachments** (ANY.RUN, VirusTotal)
6. **Document findings** (IOCs, verdict)
7. **Report results** (ticket update, escalation)

### IOCs to Collect

**From emails:**
- Sender addresses
- Originating IPs
- Domains
- URLs
- File hashes

**From sandbox:**
- Network connections
- C2 servers
- Dropped files
- Registry keys
- Process names

---

## Key Takeaways

- **PhishTool** automates email analysis
- **ANY.RUN** provides interactive sandboxing
- **Defanging** prevents accidental indicator execution
- **Typosquatting** uses domain misspellings
- **capitai-one.com** impersonates Capital One
- **Email headers** reveal true sender
- **URL shorteners** hide destinations
- **Copy Link Location** reveals actual URL
- **Netflix phishing** used random domain strings
- **svchost.exe** commonly abused for C2
- **CVE-2017-11882** exploits Office vulnerability
- **Excel macros** deliver malware
- **Multiple IPs** indicate C2 infrastructure
- **Alphabetical/numerical sorting** required for answers

Always defang IOCs when sharing or documenting
Happy analyzing
