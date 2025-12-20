# Advent of Cyber 2025 Day 9 - TryHackMe

> Passwords - A Cracking Christmas

**Day 9 Link:** https://tryhackme.com/room/attacks-on-ecrypted-files-aoc2025-asdfghj123
---

## Story

**Scenario:** Sir Carrotbane found encrypted files labeled "North Pole Asset List"

**Files found:** Locked PDF and ZIP files

**Goal:** Crack passwords to reveal hidden contents

---

## Password Cracking Basics

**How it works:**
- Don't break encryption (too hard)
- Guess the password instead
- Test passwords until one works

**Attack types:**

**Dictionary attack:**
- Use wordlist of common passwords
- Fast and effective
- Common list: rockyou.txt

**Brute-force attack:**
- Try every possible combination
- Guaranteed to work but slow
- Time grows with password length

**Mask attack:**
- Try specific patterns only
- Example: `?l?l?l?d?d` (3 letters + 2 digits)
- Faster than full brute-force

---

## Tools Needed

**For PDF:**
- `pdfcrack` - cracks PDF passwords
- `pdf2john` + `john` - alternative method

**For ZIP:**
- `zip2john` - extracts hash
- `john` - cracks the hash
- `fcrackzip` - alternative

**Wordlist:**
- `/usr/share/wordlists/rockyou.txt`

---

## Walkthrough

### Setup

Navigate to files:
```bash
cd Desktop
```

Check file types:
```bash
file flag.pdf
file flag.zip
```

### Part 1: Crack PDF Password

**Check if protected:**
```bash
# Try opening - it's password protected!
```

**Crack with pdfcrack:**
```bash
pdfcrack -f flag.pdf -w /usr/share/wordlists/rockyou.txt
```

**Output:**
```
PDF version 1.7
Security Handler: Standard
Average Speed: 30000 w/s
Current Word: 'testing123'
...
found user-password: 'XXXXXXXXXX'
```

**Open PDF:**
- Use cracked password
- Find first flag inside!

**Q1: First flag?**
- Answer: `THM{...}` (found in PDF)

### Part 2: Crack ZIP Password

**Extract hash:**
```bash
zip2john flag.zip > zhash.txt
```

**Crack hash:**
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt zhash.txt
```

**Output:**
```
Loaded 1 password hash (ZIP)
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort
XXXXXXXXXXX      (flag.zip/flag.txt)     
1g 0:00:02:12 DONE
Session completed.
```

**Extract ZIP:**
```bash
unzip flag.zip
# Enter cracked password
```

**Read flag:**
```bash
cat flag.txt
```

**Q2: Second flag?**
- Answer: `THM{...}` (found in flag.txt)

---

## Quick Reference

### Password Cracking Commands

**PDF cracking:**
```bash
# Method 1: pdfcrack
pdfcrack -f file.pdf -w wordlist.txt

# Method 2: john
pdf2john file.pdf > hash.txt
john --wordlist=wordlist.txt hash.txt
```

**ZIP cracking:**
```bash
# Extract hash
zip2john file.zip > zhash.txt

# Crack hash
john --wordlist=wordlist.txt zhash.txt

# Show cracked passwords
john --show zhash.txt
```

**Alternative for ZIP:**
```bash
fcrackzip -u -D -p wordlist.txt file.zip
```

### Tool Flags

**pdfcrack:**
- `-f` = file to crack
- `-w` = wordlist path

**john:**
- `--wordlist` = dictionary file
- `--show` = display cracked passwords
- `--rules` = apply word mangling rules

**zip2john/pdf2john:**
- Converts encrypted file to hash format
- Output redirected to text file

---

## Password Security

### Weak Passwords

**Common mistakes:**
- Short passwords (< 8 chars)
- Dictionary words
- Common patterns (123456, password)
- Personal info (names, dates)
- No special characters

### Strong Passwords

**Best practices:**
- Long (12+ characters)
- Mix of upper/lower/numbers/symbols
- Random combinations
- Unique for each account
- Use password manager

### Encryption Strength

**Important notes:**
- Encryption is only as strong as password
- Weak password = easy to crack
- Strong encryption + weak password = vulnerable
- Offline attacks have no rate limits

---

## Detection & Defense

### Detecting Password Cracking

**Process indicators:**
- `john`, `hashcat`, `pdfcrack` running
- High CPU/GPU usage
- Command lines with `--wordlist`
- References to `rockyou.txt`

**File indicators:**
- `.john/john.pot` files
- `.hashcat/hashcat.potfile`
- Large wordlist downloads

**Network indicators:**
- Downloads of rockyou.txt
- Git clones of SecLists
- Tool installations

### Defense Strategies

**Preventive:**
- Enforce strong password policies
- Use multi-factor authentication
- Limit file encryption tools
- Monitor GPU usage spikes

**Detective:**
- Log process creation
- Monitor file access patterns
- Alert on cracking tool execution
- Track wordlist downloads

**Response:**
- Isolate compromised hosts
- Rotate affected credentials
- Review decrypted file access
- Enforce MFA on accounts

---

## Attack Flow

```
1. Find Encrypted Files
    ↓
2. Check File Type (PDF/ZIP)
    ↓
3. Choose Tool
    ├─ PDF → pdfcrack
    └─ ZIP → zip2john + john
    ↓
4. Run Dictionary Attack
    ↓
5. Password Found!
    ↓
6. Extract Contents
    ↓
7. Retrieve Flags
```

---

## Key Takeaways

- **Encryption** is only as strong as password
- **Dictionary attacks** are fast and effective
- **Weak passwords** can be cracked in minutes
- **Rockyou.txt** contains millions of leaked passwords
- **Offline cracking** has no rate limits
- **GPU acceleration** speeds up cracking
- **Always use strong** random passwords
- **Password managers** are essential
- **MFA** adds extra security layer
- **Monitor for** cracking tool usage

---

Happy hunting
