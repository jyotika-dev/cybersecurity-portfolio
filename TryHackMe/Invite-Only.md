# Invite Only - TryHackMe

> Investigate suspicious indicators using threat intelligence to map the attack chain

**Room Link:** https://tryhackme.com/room/inviteonly

<img width="1309" height="924" alt="image" src="https://github.com/user-attachments/assets/df3262a9-4a77-489d-a9ba-8d788d55405f" />

---

## Scenario

SOC analyst at TrySecureMe receives escalated indicators:
- Suspicious IP address
- SHA256 hash

**Mission:** Investigate using TryDetectThis2.0 tool

---

## Task 1: File Identification

### Question

**Q: Name of file identified with flagged SHA256 hash?**

Check file properties in TryDetectThis2.0.

- Answer: `syshelpers.exe`

---

## Task 2: File Type

### Question

**Q: File type associated with flagged SHA256 hash?**

Check metadata for file format.

- Answer: `Win32 EXE`

---

## Task 3: Execution Parents

### Question

**Q: Execution parents of flagged hash? (chronological order)**

Trace infection chain backwards.

Format: parent1,parent2

- Answer: `361GJX7J,installer.exe`

---

## Task 4: Dropped File

### Question

**Q: Name of file being dropped?**

Check what installer.exe drops.

Note the hash for later use.

- Answer: `Aclient.exe`

---

## Task 5: Malicious Dropped Files

### Question

**Q: Research second hash from Q3. List four malicious dropped files (top to bottom)?**

Investigate second parent hash (installer.exe).

Multiple files dropped: EXEs and VBS scripts.

- Answer: `searchhost.exe,syshelpers.exe,nat.vbs,runsys.vbs`

---

## Task 6: Malware Family

### Question

**Q: Malware family linking these files?**

Analyze files related to flagged IP.

Remote access trojan (RAT).

- Answer: `asyncrat`

---

## Task 7: Original Report

### Question

**Q: Title of original report mentioning these indicators?**

OSINT research for public documentation.

- Answer: `From Trust to Threat: Hijacked Discord Invites Used for Multi-Stage Malware Delivery`

---

## Task 8: Cookie Stealing Tool

### Question

**Q: Tool used to steal Chrome browser cookies?**

Check report for credential theft tools.

- Answer: `ChromeKatz`

---

## Task 9: Phishing Technique

### Question

**Q: Phishing technique used by attackers?**

Social engineering method for malicious links.

- Answer: `ClickFix`

---

## Task 10: Platform Used for Redirection

### Question

**Q: Platform used to redirect users to malicious servers?**

Legitimate platform abused for malware delivery.

- Answer: `Discord`

---

## Quick Reference

### Attack Chain

```
Phishing (ClickFix)
    ↓
Discord Invite (hijacked)
    ↓
361GJX7J
    ↓
installer.exe
    ↓
Multiple Drops:
├─ searchhost.exe
├─ syshelpers.exe
├─ nat.vbs
├─ runsys.vbs
└─ Aclient.exe
    ↓
AsyncRAT Infection
    ↓
ChromeKatz (cookie theft)
```

### Investigation Steps

**1. Hash Analysis**
- Identify file name
- Check file type
- Extract metadata

**2. Execution Chain**
- Find parent processes
- Trace infection origin
- Map lineage

**3. Dropped Files**
- Identify payloads
- Note persistence mechanisms
- Collect hashes

**4. Attribution**
- Identify malware family
- Search OSINT reports
- Correlate indicators

**5. TTPs**
- Phishing technique
- Delivery platform
- Post-exploitation tools

### Key Artifacts

**Files:**
- syshelpers.exe (initial)
- 361GJX7J (parent 1)
- installer.exe (parent 2)
- Aclient.exe (dropped)
- searchhost.exe (dropped)
- nat.vbs (persistence)
- runsys.vbs (persistence)

**Malware:**
- AsyncRAT (main family)
- ChromeKatz (cookie stealer)

**Techniques:**
- ClickFix (phishing)
- Discord hijacking

### AsyncRAT Characteristics

**Type:** Remote Access Trojan

**Capabilities:**
- Remote control
- Data theft
- Persistence
- Keylogging
- File manipulation

**Delivery:**
- Multi-stage droppers
- Obfuscated scripts
- Legitimate platform abuse

### OSINT Resources

**Report research:**
- Security vendor blogs
- Threat intelligence feeds
- Public malware databases
- GitHub IOC repositories

**Tools mentioned:**
- TryDetectThis2.0 (in-house TI tool)
- ChromeKatz (attacker tool)

### Indicators of Compromise (IOCs)

**Hashes:**
- SHA256 values of malicious files

**Files:**
- *.exe executables
- *.vbs scripts

**Behavior:**
- Multiple file drops
- Execution chain
- Cookie theft
- Remote access

---

## Key Takeaways

- **Pivot across** multiple IOCs
- **Execution parents** reveal infection chain
- **Dropped files** indicate persistence
- **AsyncRAT** is the malware family
- **Multi-stage delivery** common in campaigns
- **Discord** abused for redirection
- **ClickFix** phishing technique
- **ChromeKatz** steals browser cookies
- **OSINT research** confirms attribution
- **File analysis + TI** = complete picture
- **Trace backwards** from detection
- **Map the full** attack chain

Follow the breadcrumbs
Happy hunting
