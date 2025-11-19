# Detecting Web Attacks - TryHackMe

> Detect and analyze web-based attacks in logs and traffic

**Room Link:** https://tryhackme.com/room/detectingwebattacks

<img width="1443" height="815" alt="image" src="https://github.com/user-attachments/assets/0b8efc21-e552-4fda-8d6a-12e1dd33562d" />

---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Client-Side Attacks

### What Are They?

Attacks exploiting user's browser or behavior.

**Common types:**
- XSS (Cross-Site Scripting)
- Phishing
- Drive-by downloads
- Malicious redirects

### Questions

**Q1: What class relies on exploiting user's behavior/device?**

- Answer: `Client-Side`

---

**Q2: Most common client-side attack?**

Cross-Site Scripting exploits browser.

- Answer: `XSS`

---

## Task 3: Server-Side Attacks

**File:** `access.log` on Desktop

### Questions

**Q1: Attacker's User-Agent during directory fuzz?**

Look for fuzzing tool signatures in logs.

- Answer: `FFUF v2.1.0`

---

**Q2: Page where brute-force attack performed?**

Look for repeated POST requests.

Multiple attempts to same endpoint.

- Answer: `/login.php`

---

**Q3: Complete decoded SQLi payload on `/changeusername.php`?**

**Steps:**
1. Find SQLi payload in logs
2. Copy URL-encoded string
3. Use CyberChef to decode

- Answer: `%' OR '1'='1`

---

## Task 4: Host-Based Detection

**No questions in walkthrough**

---

## Task 5: Network-Based Detection

**File:** `traffic.pcap`

### Questions

**Q1: Password identified in brute-force?**

- Answer: `astrongpassword123`

---

**Q2: Flag found in database using SQLi?**

**Steps:**
1. Add User-Agent as column
2. Filter for sqlmap (common SQLi tool)
3. Follow HTTP stream
4. Find database dump results

**Filter:**
```
http.user_agent contains "sqlmap"
```

- Answer: `THM{dumped_the_db}`

---

## Task 6: Web Application Firewall

### What is WAF?

Inspects and filters web traffic based on rules.

**Functions:**
- Block malicious requests
- Filter by patterns
- Protect web applications

### Questions

**Q1: What do WAFs inspect and filter?**

- Answer: `Web Requests`

---

**Q2: Create custom rule to block User-Agent "BotTHM"**

- Answer: `IF User-Agent CONTAINS "BotTHM" THEN block`

---

## Quick Reference

### Attack Types

**Client-Side:**
- XSS (browser exploitation)
- Phishing (social engineering)
- Drive-by downloads
- Malicious scripts

**Server-Side:**
- SQLi (database attacks)
- Directory fuzzing
- Brute-force
- Command injection

### Common Tools in Logs

| Tool | Purpose | Signature |
|------|---------|-----------|
| **FFUF** | Directory fuzzing | FFUF v2.1.0 |
| **sqlmap** | SQL injection | sqlmap/1.x |
| **Hydra** | Brute-force | THC-Hydra |
| **Burp** | Web testing | Burp Suite |

### HTTP Response Codes

- **200** - OK (success)
- **302** - Redirect (often successful login)
- **401** - Unauthorized
- **403** - Forbidden
- **404** - Not Found
- **500** - Server Error

### Wireshark Filters

**Successful logins:**
```
http.response.code == 302
```

**Specific User-Agent:**
```
http.user_agent contains "sqlmap"
```

**POST requests:**
```
http.request.method == "POST"
```

**Specific URL:**
```
http.request.uri contains "/login"
```

### Detection Patterns

**Directory Fuzzing:**
- Rapid requests
- Sequential URLs
- 404 responses
- Fuzzing tool User-Agent

**Brute-Force:**
- Repeated POST to /login
- Multiple failed attempts
- Eventually successful (302)
- Same source IP

**SQL Injection:**
- SQL keywords in parameters
- Special characters (', ", =)
- OR statements
- Database commands

### WAF Rule Components

**Condition:**
- Field (User-Agent, URI, IP)
- Operator (CONTAINS, EQUALS, MATCHES)
- Value (pattern to match)

**Action:**
- block
- allow
- log
- redirect

**Example rules:**
```
IF User-Agent CONTAINS "bot" THEN block
IF IP EQUALS "malicious_ip" THEN block
IF URI CONTAINS "../" THEN block
```

---

## Key Takeaways

- **Client-side** attacks exploit user's browser
- **XSS** most common client-side attack
- **FFUF** used for directory fuzzing
- **Brute-force** targets `/login.php`
- **SQLi payload:** `%' OR '1'='1`
- **302 response** = successful login
- **Password found:** astrongpassword123
- **sqlmap** common SQLi tool
- **Flag extracted** from database dump
- **WAFs** inspect web requests
- **User-Agent** reveals attack tools
- **Follow HTTP stream** shows full request/response
- **URL decode** SQLi payloads for analysis

Detect attacks by analyzing patterns in logs and traffic
Happy defending!
