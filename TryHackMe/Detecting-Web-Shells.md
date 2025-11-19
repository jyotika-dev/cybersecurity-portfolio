# Detecting Web Shells - TryHackMe

> Learn to detect web shells through log and network analysis

**Room Link:** https://tryhackme.com/room/detectingwebshells

<img width="1545" height="868" alt="image" src="https://github.com/user-attachments/assets/26c29295-0b58-4f30-8f41-e96f4af00130" />

---

## Task 1: Introduction

**Learning objectives:**
- Understand web shells
- Detect via logs, filesystem, network
- Use detection tooling

**No answer needed**

---

## Task 2: Web Shell Overview

### What is a Web Shell?

Malicious program uploaded to web server enabling remote command execution.

**Purpose:**
- Initial access
- Persistence mechanism
- Command execution

**Example:** `awebshell.php?cmd=whoami`

### Real-World Examples

**Hafnium (ProxyLogon):**
- Uploaded .aspx shells to Exchange
- Directory: `\inetpub\wwwroot\aspnet_client\`

**Conti Ransomware:**
- Exploited same vulnerability
- Uploaded backup shells
- Mapped network quickly

### Questions

**Q1: MITRE ATT&CK Persistence sub-technique for web shells?**

- Answer: `T1505.003`

---

**Q2: File extension for Exchange web shells?**

- Answer: `.aspx`

---

## Task 3: Anatomy of Web Shell

### How They Work

**Abuse legitimate functions:**
- PHP: `shell_exec()`, `exec()`, `system()`, `passthru()`
- Takes command from URL parameter
- Executes on server
- Returns output

**Simple example:**
```php
<?php
$cmd = $_GET['cmd'];
echo shell_exec($cmd);
?>
```

**Access:** `http://server/shell.php?cmd=whoami`

### Questions

**Q1: Access shell and run whoami**

URL: `http://10.201.81.52:8080/files/awebshell.php?cmd=whoami`

- Answer: `www-data`

---

**Q2: List directory and find flag**

Commands: `ls` then `cat flag.txt`

- Answer: `THM{W3b_Sh3ll_Usag3}`

---

## Task 4: Log-Based Detection

### Web Server Logs

**Key fields:**
- Client IP
- Timestamp
- HTTP method
- Requested resource
- Response code
- User-Agent

### Indicators

**Unusual HTTP Methods:**
- Repeated GET (reconnaissance)
- POST to upload location
- PUT (direct file upload)
- Repeated requests to same file

**Suspicious User-Agents:**
- Altered: `Mozilla/4.0+(+Windows+NT+5.1)`
- Outdated: `MSIE 6.0`
- Blacklisted: `curl/1.XX.X`, `wget/1.XX.X`

**Query Strings:**
- Keywords: `cmd=`, `exec=`
- Encoded payloads (Base64)
- Abnormally long strings

**Missing Referrer:**
- Direct file access
- No linking page

### Attack Sequence Example

```
GET /uploads/ (404) → Probe
GET /files/ (200) → Found
POST /files/upload.php (200) → Upload shell
GET /files/shell.php?cmd=whoami (200) → Execute
```

### Auditd

Linux utility tracking system events.

**Usage:**
```bash
ausearch -k web_shell
```

**Correlate with web logs:**
- POST request + `creat` syscall = file written
- Suspicious request + `execve` = command executed

### Questions

**Q1: URL part associating values to parameters?**

- Answer: `query strings`

---

**Q2: Auditd syscall confirming file written?**

After POST to /upload.php

- Answer: `creat`

---

## Task 5: Beyond Logs

### File System Analysis

**Common locations:**
- Apache: `/var/www/html/`
- Nginx: `/usr/share/nginx/html/`
- Upload dirs: `/uploads/`, `/images/`, `/admin/`
- Temp: `/tmp/`

**Suspicious patterns:**
- Random filenames
- Executable extensions (.php, .jsp)
- Double extensions (image.jpg.php)

**Commands:**

**Find recent .php files:**
```bash
find /var/www -type f -name "*.php" -newerct "2025-07-01" ! -newerct "2025-08-01"
```

**Search for suspicious functions:**
```bash
grep -r "eval(" wp-content/
```

### Network Traffic Analysis

**Wireshark filters:**

**HTTP methods:**
```
http.request.method == "POST"
```

**PHP files:**
```
http.request.uri contains ".php"
```

**User-Agent:**
```
http.user_agent
```

**PUT requests:**
```
http.request.method == "PUT"
```

### Questions

**Q1: Command to locate .php files in /var/www/?**

- Answer: `find /var/www/ -type f -name "*.php"`

---

**Q2: Wireshark filter for PUT requests?**

- Answer: `http.request.method == "PUT"`

---

## Task 6: Investigation

**Scenario:** WordPress site compromise

**Logs:** `/var/log/apache2/access.log`

**SSH access:** `ssh ubuntu@10.201.81.52` (password: TryHackMe!)

### Investigation Tips

**Look for:**
- Repeated requests
- Suspicious patterns
- Unusual User-Agents
- 404 → 200 progression

**grep examples:**
```bash
cat access.log | grep "404"
cat access.log | grep "203.0.113.66"
cat access.log | grep ".php"
```

### Questions

**Q1: Attacker's IP address?**

Look for unusual activity patterns.

- Answer: `203.0.113.66`

---

**Q2: First directory successfully identified?**

Check 200 responses after 404s.

- Answer: `/wordpress`

---

**Q3: .php file used to upload web shell?**

POST request to upload script.

- Answer: `upload_form.php`

---

**Q4: First command run using web shell?**

Check query string after upload.

`?cmd=whoami`

- Answer: `whoami`

---

**Q5: Downloaded file name?**

Look for `wget` or download command.

- Answer: `linpeas.sh`

---

**Q6: Flag hidden in web shell code?**

```bash
cat /path/to/webshell.php
```

- Answer: `THM{W3b_Sh3ll_Int3rnals}`

---

## Task 7: Conclusion

**No answer needed**

---

## Quick Reference

### HTTP Request Methods

| Method | Normal Use | Abuse |
|--------|-----------|-------|
| GET | Retrieve resource | Recon, execute shell |
| POST | Submit data | Upload shell |
| PUT | Upload file | Direct shell upload |
| DELETE | Remove resource | Cleanup |
| OPTIONS | Check methods | Reconnaissance |
| HEAD | Get headers | Detect files |

### Detection Indicators

**Web Logs:**
- Unusual HTTP methods
- Suspicious User-Agents
- Query strings with `cmd=`, `exec=`
- Missing referrer
- External IPs (if internal network)

**File System:**
- Random filenames
- Double extensions
- Recent modifications
- Executable scripts in upload dirs

**Network:**
- Unusual payloads
- Encoded commands
- Unexpected ports
- Process spawning command tools
  
---

## Key Takeaways

- **Web shells** enable remote command execution
- **T1505.003** MITRE technique for web shells
- **.aspx** common for Exchange attacks
- **Legitimate functions** abused for execution
- **Query strings** contain commands
- **creat syscall** confirms file written
- **Correlate logs** for complete picture
- **Repeated requests** indicate reconnaissance
- **Missing referrer** can be suspicious
- **Double extensions** disguise malicious files
- **eval()** common in web shells
- **PUT method** directly uploads files
- **linpeas.sh** privilege escalation tool
- **203.0.113.66** attacker IP pattern

Detect web shells early to prevent persistence
Happy hunting
