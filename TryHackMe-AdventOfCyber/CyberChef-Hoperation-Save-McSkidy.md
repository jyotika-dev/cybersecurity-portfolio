# Advent of Cyber 2025 Day 17 - TryHackMe

> CyberChef - Encoding/Decoding Challenge

**Day 17 Link:** https://tryhackme.com/room/encoding-decoding-aoc2025-s1a4z7x0c3

---

## Story

**Scenario:** McSkidy imprisoned in Quantum Warren

**Problem:** Five locks to break for escape

**Clues:** Guards communicate in Base64

**Goal:** Decode messages to get passwords for all locks

---

## CyberChef Basics

**What is it:**
- Browser-based data manipulation tool
- "Swiss Army Knife" for encoding/decoding
- Drag-and-drop operations
- Chain multiple operations

**Common operations:**
- Base64 encode/decode
- Hex conversion
- XOR cipher
- MD5 hash
- ROT13/Caesar cipher

**Access:** https://gchq.github.io/CyberChef/

---

## Key Concepts

### Encoding Types

| Type | Use Case |
|------|----------|
| Base64 | Binary to text |
| Hex | Binary to hexadecimal |
| XOR | Simple encryption |
| MD5 | One-way hash |
| ROT13 | Simple substitution |

### Important Info

**Chat encoding:** All guard chat is Base64

**Username:** Guard's name (Base64 encoded)

**Headers:** Check Network tab for clues

**Login logic:** Check Debugger tab (app.js)

---

## Challenge Setup

**Access:** `http://MACHINE_IP:8080`

**Five locks:**
1. Outer Gate
2. Outer Wall
3. Guard House
4. Inner Castle
5. Prison Tower

---

## Lock 1: Outer Gate

### Step 1: Find Guard Name

**Inspect page → Find name**

**Guard:** CottonTail

**Base64 encode:** `Q290dG9uVGFpbA==`

### Step 2: Check Headers

**Network tab → Refresh → First response**

**Magic Question found:** What is the password for this level?

**Base64 encode question:** `V2hhdCBpcyB0aGUgcGFzc3dvcmQgZm9yIHRoaXMgbGV2ZWw/`

### Step 3: Ask Guard

**Send encoded question to guard**

**Response:** `SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IFNXRnRjMjltYkhWbVpuaz0=`

**Decode (Base64):** Here is the password: SWFtc29mbHVmZnk=

**Decode again:** `Iamsofluffy`

### Answer

**Password:** `Iamsofluffy`

---

## Lock 2: Outer Wall

### Step 1: Guard Name

**Guard:** CarrotHelm

**Base64:** `Q2Fycm90SGVsbQ==`

### Step 2: Magic Question

**From headers:** Did you change the password?

**Base64:** `RGlkIHlvdSBjaGFuZ2UgdGhlIHBhc3N3b3JkPw==`

### Step 3: Decode Response

**Response:** `SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IFUxaFNkbUpIVWpWaU0xWXdZakpPYjFsWE5XNWFWMnd3U1ZFOVBRPT0=`

**First decode:** Here is the password: U1hSdmJHUjViM1YwYjJOb1lXNW5aV2wwSVE9PQ==

**Second decode:** SXRvbGR5b3V0b2NoYW5nZWl0IQ==

**Third decode:** `Itoldyoutochangeit!`

### Answer

**Password:** `Itoldyoutochangeit!`

---

## Lock 3: Guard House

### Step 1: Guard Name

**Guard:** LongEars

**Base64:** `TG9uZ0VhcnM=`

### Step 2: Check Logic

**Debugger → app.js**

**Recipe:** Decode Base64 then XOR

**Key from headers:** cyberchef

### Step 3: Ask & Decode

**Question:** Password Please?

**Base64:** `UGFzc3dvcmQgUGxlYXNlPw==`

**Response:** `SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IElRd0ZGakFXQmdzZg==`

**First decode:** Here is the password: IQwFFjAWBgsf

**XOR decode (key: cyberchef):** `BugsBunny`

### Answer

**Password:** `BugsBunny`

---

## Lock 4: Inner Castle

### Step 1: Guard Name

**Guard:** Lenny

**Base64:** `TGVubnk=`

### Step 2: Check Logic

**Debugger → app.js**

**Method:** MD5 hash decode

### Step 3: Crack Hash

**Question:** Password Please?

**Response:** `SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IGI0YzBiZTdkN2U5N2FiNzRjMTMwOTFiNzY4MjVjZjM5`

**First decode:** Here is the password: b4c0be7d7e97ab74c13091b76825cf39

**Crack MD5 (use crackstation.net):** `passw0rd1`

### Answer

**Password:** `passw0rd1`

---

## Lock 5: Prison Tower

### Step 1: Guard Name

**Guard:** Carl

**Base64:** `Q2FybA==`

### Step 2: Check Headers & Logic

**Headers:**
- X-Recipe-ID: R2
- X-Recipe-Key: cyberchef

**Debugger → app.js → Case R2**

**Recipe:** Base64 → Hex → Reverse

### Step 3: Decode

**Question:** Password Please?

**Response:** `SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IE56SXpNelppTmpNek1EWmpOREkyT0RZek16UXpNemN5TkRJM01qTXhNelU9`

**First decode:** Here is the password: NzIzMzZiNjMzMDZjNDI2ODYzMzQzMzcyNDI3MjMxMzU=

**CyberChef recipe:**
1. From Base64
2. From Hex
3. Reverse

**Result:** `51rBr34chBl0ck3r`

### Answer

**Password:** `51rBr34chBl0ck3r`

---

## Final Flag

**After breaking all locks:**

**Flag:** `THM{M3D13V4L_D3C0D3R_4D3P7}`

---

## Quick Reference

### CyberChef Operations

**Base64:**
```
From Base64 - Decode
To Base64 - Encode
```

**XOR:**
```
XOR - Need key
Example key: cyberchef
```

**Hex:**
```
From Hex - Hex to text
To Hex - Text to hex
```

**Reverse:**
```
Reverse - Flip string
```

**MD5:**
```
Use crackstation.net
Or MD5 databases
```

### Browser Tools

**Network tab:**
- Shows HTTP headers
- Find magic questions
- Check response headers

**Debugger tab:**
- View app.js
- Check login logic
- Find decoding recipes

---

## Chaining Operations

**Example workflow:**

```
Input: SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IElRd0ZGakFXQmdzZg==
   ↓
From Base64: Here is the password: IQwFFjAWBgsf
   ↓
XOR (key: cyberchef): BugsBunny
```

**Complex chain:**
```
Base64 → Hex → Reverse
```

---

## Common Patterns

### Multiple Base64 Layers

```
Encoded → Decode → Still looks encoded?
→ Decode again → Keep going until readable
```

### XOR Encoding

```
Needs key from headers/comments
Apply XOR with key to decrypt
```

### Hash Cracking

```
MD5 hash → Use crackstation
Or rainbow tables
Not reversible, needs lookup
```

---

## Investigation Checklist

**For each lock:**
- [ ] Find guard name
- [ ] Encode name to Base64
- [ ] Check Network tab (headers)
- [ ] Check Debugger tab (app.js)
- [ ] Encode question to Base64
- [ ] Send to guard
- [ ] Decode response
- [ ] Apply correct recipe
- [ ] Test password

---

## CyberChef Tips

**Magic operation:**
- Auto-detects encoding
- Suggests decode steps
- Good for unknown data

**Recipe chaining:**
- Drag operations in order
- Test step-by-step
- Save useful recipes

**Common mistakes:**
- Forgetting multiple layers
- Wrong key for XOR
- Not reversing after hex
- Skipping Base64 decode

---

## Encoding vs Encryption

**Encoding (reversible):**
- Base64
- Hex
- URL encoding
- No key needed

**Encryption (needs key):**
- XOR (simple)
- AES (strong)
- RSA (asymmetric)
- Key required

**Hashing (one-way):**
- MD5
- SHA1/SHA256
- Not reversible
- Lookup tables work

---

## Key Takeaways

- **Base64** is most common encoding
- **Multiple layers** are common
- **Headers** contain clues
- **Browser tools** reveal logic
- **CyberChef** chains operations
- **XOR** needs key from headers
- **MD5** needs cracking
- **Always check** app.js for logic
- **Encode questions** before sending
- **Decode responses** layer by layer

---

Happy hunting!
