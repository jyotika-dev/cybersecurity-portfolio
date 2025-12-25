# Advent of Cyber 2025 Day 13 - TryHackMe

> YARA Rules - YARA Mean One!

**Day 13 Link:** https://tryhackme.com/room/yara-aoc2025-q9w1e3y5u7

---

## Story

**Scenario:** McSkidy sent images with hidden message

**Location:** `/home/ubuntu/Downloads/easter`

**Pattern:** Strings starting with `TBFC:` followed by keywords

**Goal:** Create YARA rule to extract hidden message

---

## YARA Basics

**What is YARA:**
- Pattern-matching tool
- Detects malware signatures
- Searches for specific strings/patterns
- Custom detection rules

**Why use YARA:**
- Detect malware variants
- Hunt threats proactively
- Scan files/memory/processes
- Share rules with community

**When to use:**
- Post-incident analysis
- Threat hunting
- Intelligence-based scans
- Memory analysis

---

## YARA Rule Structure

### Three Main Parts

**1. Metadata (optional but recommended):**
```yara
meta:
    author = "Your Name"
    description = "What this rule does"
    date = "2025-12-13"
```

**2. Strings (what to search for):**
```yara
strings:
    $s1 = "malware"
    $s2 = { 4D 5A }
    $s3 = /http:\/\/evil\.com/
```

**3. Condition (when to trigger):**
```yara
condition:
    any of them
```

---

## String Types

### Text Strings

**Basic:**
```yara
$text = "Christmas"
```

**Modifiers:**
```yara
$case = "xmas" nocase         # case-insensitive
$wide = "text" wide ascii     # wide characters
$xor = "hidden" xor           # XOR encoded
$b64 = "data" base64          # base64 encoded
```

### Hex Strings

```yara
$mz = { 4D 5A }              # MZ header
$pattern = { E3 41 ?? C8 }   # ?? = wildcard byte
```

### Regex Strings

```yara
$url = /http:\/\/.*evil.*/
$cmd = /powershell.*-enc\s+[A-Za-z0-9+/=]+/
```

---

## Conditions

**Single string:**
```yara
condition:
    $text
```

**Any string:**
```yara
condition:
    any of them
```

**All strings:**
```yara
condition:
    all of them
```

**Combine with logic:**
```yara
condition:
    ($s1 or $s2) and not $benign
```

**With file size:**
```yara
condition:
    any of them and filesize < 1MB
```

---

## Challenge Walkthrough

### Step 1: Understand Pattern

**Looking for:**
- Starts with `TBFC:`
- Followed by alphanumeric characters
- Pattern: `TBFC:[A-Za-z0-9]+`

### Step 2: Create YARA Rule

**Create file:** `mckidy_message.yar`

```yara
rule TBFC_Message_Extractor
{
    meta:
        author = "Blue Team"
        description = "Extract McSkidy's message"
        date = "2025-12-13"
    
    strings:
        $message = /TBFC:[A-Za-z0-9]+/
    
    condition:
        $message
}
```

### Step 3: Run YARA

```bash
yara -r -s mckidy_message.yar /home/ubuntu/Downloads/easter
```

**Flags:**
- `-r` = recursive scan
- `-s` = show matched strings

### Step 4: Analyze Output

**Sample output:**
```
TBFC_Message_Extractor /path/image1.png
0x1a2b:$message: TBFC:Find

TBFC_Message_Extractor /path/image2.png
0x3c4d:$message: TBFC:me

TBFC_Message_Extractor /path/image3.png
0x5e6f:$message: TBFC:in

TBFC_Message_Extractor /path/image4.png
0x7g8h:$message: TBFC:HopSec

TBFC_Message_Extractor /path/image5.png
0x9i0j:$message: TBFC:Island
```

### Step 5: Extract Keywords

**From output:**
1. Find
2. me
3. in
4. HopSec
5. Island

**Combined:** Find me in HopSec Island

---

## Questions & Answers

### Q1: How many images contain TBFC?

Count matches in output.

**Answer:** `5`

### Q2: Regex for TBFC: + alphanumeric?

Pattern explained:
- `TBFC:` = literal string
- `[A-Za-z0-9]` = any letter or digit
- `+` = one or more

**Answer:** `/TBFC:[A-Za-z0-9]+/`

### Q3: What is McSkidy's message?

Combine keywords in order.

**Answer:** `Find me in HopSec Island`

---

## Quick Reference

### Common YARA Commands

```bash
# Scan single file
yara rule.yar file

# Scan directory recursively
yara -r rule.yar directory/

# Show matched strings
yara -s rule.yar file

# Disable warnings
yara -w rule.yar file

# Print metadata
yara -m rule.yar file
```

### Example Rules

**Simple text match:**
```yara
rule Simple_Text
{
    strings:
        $text = "malware"
    condition:
        $text
}
```

**Multiple strings:**
```yara
rule Multiple_Strings
{
    strings:
        $s1 = "evil"
        $s2 = "malicious"
        $s3 = "backdoor"
    condition:
        2 of them
}
```

**Hex and text:**
```yara
rule Hex_And_Text
{
    strings:
        $mz = { 4D 5A }
        $text = "payload"
    condition:
        $mz and $text
}
```

**With file size:**
```yara
rule Size_Check
{
    strings:
        $sus = "suspicious"
    condition:
        $sus and filesize < 10KB
}
```

---

## Regex Pattern Guide

**Common patterns:**
```
.          # any character
*          # zero or more
+          # one or more
?          # zero or one
[A-Za-z]   # any letter
[0-9]      # any digit
\s         # whitespace
\d         # digit
^          # start of string
$          # end of string
```

**Example patterns:**
```
/TBFC:[A-Za-z0-9]+/           # TBFC: + alphanumeric
/http:\/\/.*evil.*/            # URLs with evil
/[0-9]{1,3}\.[0-9]{1,3}/      # IP-like pattern
/powershell.*-enc/             # PowerShell encoding
```

---

## Best Practices

**Rule writing:**
- Start simple, add complexity
- Use descriptive metadata
- Test against samples
- Comment complex patterns
- Balance specificity vs coverage

**String selection:**
- Unique to malware
- Not in benign files
- Stable across variants
- Multiple indicators better

**Conditions:**
- Avoid overly broad matches
- Combine multiple strings
- Use file size checks
- Test for false positives

**Performance:**
- Simple rules are faster
- Avoid complex regex
- Limit recursive depth
- Use specific paths

---

## Use Cases

**Malware detection:**
```yara
rule Malware_MZ_Header
{
    strings:
        $mz = { 4D 5A }
        $evil = "malicious"
    condition:
        $mz at 0 and $evil
}
```

**URL extraction:**
```yara
rule Suspicious_URLs
{
    strings:
        $url = /http:\/\/[a-z0-9.-]+\/[a-z0-9]+/
    condition:
        $url
}
```

**Credential search:**
```yara
rule Credential_Keywords
{
    strings:
        $pass = "password" nocase
        $user = "username" nocase
        $cred = "credential" nocase
    condition:
        any of them
}
```

---

## Key Takeaways

- **YARA** is pattern-matching tool
- **Three parts:** meta, strings, condition
- **String types:** text, hex, regex
- **Recursive scan** with `-r` flag
- **Show matches** with `-s` flag
- **Regex** for flexible patterns
- **Conditions** control when to trigger
- **Test rules** before deployment
- **Share rules** with community
- **Custom detection** is powerful

---

Happy hunting!
