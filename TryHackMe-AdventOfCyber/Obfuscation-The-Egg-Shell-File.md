# Advent of Cyber 2025 Day 18 - TryHackMe

> Obfuscation - The Egg Shell File

**Day 18 Link:** https://tryhackme.com/room/obfuscation-aoc2025-e5r8t2y6u9

---

## Story

**Scenario:** Suspicious email from "northpole-hr" (fake!)

**File:** PowerShell script with obfuscated code

**Problem:** Code contains random gibberish strings

**Goal:** Deobfuscate malicious script and extract flags

---

## Obfuscation Basics

**What is it:**
- Making code hard to read
- Hiding malicious intent
- Evading detection
- Delaying analysis

**Why attackers use it:**
- Bypass security tools
- Hide malicious strings
- Avoid signature detection
- Make analysis harder

---

## Common Techniques

### ROT1

**How it works:**
- Shift each letter by 1
- `a` → `b`, `b` → `c`

**Example:**
```
Original: carrot coins
ROT1:     dbsspu dpjot
```

### ROT13

**How it works:**
- Shift each letter by 13
- Common 3-letter words change

**Example:**
```
the → gur
and → naq
```

### Base64

**Pattern:**
- Long alphanumeric strings
- Contains `A-Z`, `a-z`, `0-9`
- May have `+` or `/`
- Often ends with `=` or `==`

**Example:**
```
Hello World → SGVsbG8gV29ybGQ=
```

### XOR

**Pattern:**
- Random-looking symbols
- Same length as original
- Needs key to decode

**Example:**
```
Original: carrot supremacy
XOR (key: a): ikxxe~*yzxgkis!
```

---

## CyberChef Usage

**Access:** https://gchq.github.io/CyberChef/

### Basic Steps

1. **Input:** Paste obfuscated text
2. **Operation:** Drag decode operation to Recipe
3. **Settings:** Configure key/options
4. **Output:** Click "BAKE!" to see result

### Magic Operation

**Auto-detect encoding:**
1. Add "Magic" operation
2. Enable "Intensive mode"
3. Check multiple results
4. Find readable output

---

## Challenge Walkthrough

### Setup

**Location:** Desktop → `SantaStealer.ps1`

**Open with:** Visual Studio Code (double-click)

**PowerShell commands:**
```powershell
cd .\Desktop\
.\SantaStealer.ps1
```

---

## Part 1: Deobfuscate C2 URL

### Step 1: Find Obfuscated String

**Open:** `SantaStealer.ps1`

**Navigate to:** "Start here" section

**Find variable:** Look for C2 URL variable (name hints at encoding)

### Step 2: Deobfuscate in CyberChef

**Obfuscated string:** (from script)

**Encoding type:** Variable name is a hint!

**CyberChef steps:**
1. Paste obfuscated string
2. Use appropriate decode operation
3. Get readable C2 URL

### Step 3: Update Script

**Replace obfuscated string with decoded URL**

**Save file:** Ctrl+S

### Step 4: Run Script

```powershell
cd .\Desktop\
.\SantaStealer.ps1
```

### Q1: First flag after deobfuscating C2 URL?

**Answer:** `THM{C2_De0bfuscation_29838}`

---

## Part 2: Obfuscate API Key

### Step 1: Navigate to Part 2

**In script:** Find "Part 2: Obfuscation" section

**Task:** Obfuscate API key using XOR

### Step 2: Get Key Value

**XOR Key:** `0x37` (hexadecimal)

**Convert to decimal:** `55`

### Step 3: Obfuscate in CyberChef

**API Key:** (plain text from script)

**CyberChef steps:**
1. Input: Plain API key
2. Operation: XOR
3. Key: `55` (decimal)
4. Key type: Decimal
5. Bake!

### Step 4: Update Script

**Variable:** `$ObfAPIKEY`

**Replace with:** CyberChef output

**Save file**

### Step 5: Run Script Again

```powershell
.\SantaStealer.ps1
```

### Q2: Second flag after obfuscating API key?

**Answer:** `THM{API_Obfusc4tion_ftw_0283}`

---

## Quick Reference

### Detecting Obfuscation

| Type | Visual Clue |
|------|-------------|
| ROT1 | Words look "one letter off" |
| ROT13 | Three-letter words: the→gur, and→naq |
| Base64 | Long alphanumeric, ends with = |
| XOR | Random symbols, same length |

### CyberChef Operations

**Decode:**
```
From Base64
From Hex
ROT13
XOR
Gunzip
```

**Encode:**
```
To Base64
To Hex
ROT13
XOR
Gzip
```

---

## Layered Obfuscation

### Example Chain

**Obfuscated:**
```
H4sIADKZ42gA/32PT2sqQRDE7/MpitGDgrPEJJcXyOGha1xwVaLwyLvI6rbuhP3HTHswm/3uzmggIQfrUD3VzI/u7iDXljepNkFth6KDmYsWnBF2R2OoZOyrPCUDXSKB1UWdEzjZ5hQI8c9oJrU4cn1kyPXbMgRW0X/nF8WLcTSJwvFX9Jr/jUP5i1NOgPeLfjxvtKQQL8RqlOk8jZgKfGJSmTDZZWqxfacdoxFAl0814Rl6j153EyxXkR1VJSe6JNNHAzmOXiHRgnJLPk+iWeiyR63+uIm6Hb5B92NG5YGzK5sm7FnfTSxfzl3rgoJ1tWKjy0NPnpxUHKs0xXT6VBSy7zjZ3A3UY4tmOPjTAs29t4dWQu2vpwyua7niJ7iyCeZJQaIVZ/xwdy/JAQAA
```

**Steps to decode:**
1. From Base64
2. XOR (key: x)
3. Gunzip

**Recipe order:** Reverse of encoding!

---

## PowerShell Basics

### Navigation

```powershell
# Change directory
cd .\Desktop\

# List files
ls

# Run script
.\script.ps1
```

### Common Issues

**Execution policy:**
```powershell
Set-ExecutionPolicy Bypass -Scope Process
```

**Run as Administrator if needed**

---

## Visual Studio Code

### Open File

1. Double-click `SantaStealer.ps1`
2. Or: Right-click → Open with → VS Code

### Edit & Save

1. Find section to modify
2. Make changes
3. Ctrl+S to save
4. Close and run in PowerShell

### Find Text

```
Ctrl+F - Search in file
Ctrl+H - Find and replace
```

---

## Step-by-Step Summary

### Part 1 (Deobfuscation)

```
1. Open SantaStealer.ps1
    ↓
2. Find C2 URL variable
    ↓
3. Identify encoding from variable name
    ↓
4. Use CyberChef to decode
    ↓
5. Update script with decoded URL
    ↓
6. Save file
    ↓
7. Run in PowerShell
    ↓
8. Get first flag
```

### Part 2 (Obfuscation)

```
1. Navigate to Part 2 section
    ↓
2. Find plain API key
    ↓
3. Convert 0x37 to decimal (55)
    ↓
4. Use CyberChef XOR with key 55
    ↓
5. Update $ObfAPIKEY variable
    ↓
6. Save file
    ↓
7. Run script again
    ↓
8. Get second flag
```

---

## Key Concepts

### Encoding vs Encryption

**Encoding:**
- Base64, Hex, ROT13
- No key needed
- Easily reversible
- NOT for security

**Encryption:**
- XOR (simple), AES (strong)
- Requires key
- Secure if key is secret
- Harder to break

**Obfuscation:**
- Makes code hard to read
- Combination of techniques
- Delays analysis
- Not true security

---

## Tips & Tricks

**Variable names:**
- Often hint at encoding type
- `$Base64String`, `$XorEncoded`
- Read comments in code

**Testing decodings:**
- Use Magic operation first
- Try common encodings
- Check if output makes sense
- May need multiple layers

**XOR keys:**
- Can be hex or decimal
- Try both if unsure
- Common keys: 0x37, 0x42

**Layered obfuscation:**
- Decode in reverse order
- Base64 usually outermost
- Compression often innermost

---

## Key Takeaways

- **Obfuscation** hides malicious code
- **CyberChef** is Swiss Army knife
- **Variable names** give hints
- **Magic operation** auto-detects
- **XOR** needs key to decode
- **Layered** means multiple techniques
- **Reverse order** to deobfuscate
- **Test in isolated** environment
- **Always analyze** suspicious scripts
- **Encoding ≠ encryption**

---

Happy hunting!
