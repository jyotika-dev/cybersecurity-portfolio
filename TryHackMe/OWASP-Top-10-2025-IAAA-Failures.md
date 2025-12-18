# OWASP Top 10 2025: IAAA Failures - TryHackMe

> Learn about A01, A07, and A09 related to IAAA model failures

**Room Link:** https://tryhackme.com/room/owasptopten2025one

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/580a3a7f-10f3-4fb0-bc16-09991dceb543" />

---

## Task 1: Introduction

**No answer needed**

---

## Task 2: What is IAAA?

### IAAA Model

**Four critical components:**

**I - Identity:**
- Unique account (user ID/email)
- Represents person or service

**A - Authentication:**
- Proving identity
- Passwords, OTP, passkeys

**A - Authorisation:**
- What identity is allowed to do
- Permissions and roles

**A - Accountability:**
- Recording who did what
- When and from where
- Logging and alerting

**Important:** Cannot skip a level - each builds on previous!

**No answer needed**

---

## Task 3: A01 - Broken Access Control

### What is Broken Access Control?

**Problem:** Server doesn't enforce access properly

**Common issue:** IDOR (Insecure Direct Object Reference)

**Example:**
```
?id=7 → ?id=6
```
Change ID to see another user's data!

### Privilege Escalation Types

**Horizontal:**
- Same role level
- Access other user's data
- Example: View another customer's orders

**Vertical:**
- Jump to higher role
- Admin-only actions
- Example: Regular user → Admin

**Root cause:** Application trusts client too much

### Questions

**Q1: If you view another user's data at same role level, what privilege escalation type?**

- Answer: `Horizontal`

---

**Q2: Note found when viewing user with $1 million+?**

Find user with high balance and access their account.

- Answer: `THM{Found.the.Millionare!}`

---

## Task 4: A07 - Authentication Failures

### What are Authentication Failures?

**Problem:** Can't reliably verify user identity

### Common Issues

**Username Enumeration:**
- Discover valid usernames
- Different responses for valid/invalid users

**Weak Passwords:**
- No lockout mechanism
- No rate limiting
- Easily guessable

**Logic Flaws:**
- Login/registration bypass
- Session confusion
- Account binding issues

**Insecure Session Handling:**
- Predictable session IDs
- Session fixation
- No proper expiration

**Impact:** Attacker can log in as someone else!

### Questions

**Q: Flag on admin user's dashboard?**

Exploit authentication weakness to access admin account.

- Answer: `THM{Account.confusion.FTW!}`

---

## Task 5: A09 - Logging & Alerting Failures

### What are Logging Failures?

**Problem:** Don't record security-relevant events

**Impact:** Can't detect or investigate attacks

### Accountability Requirements

**What to log:**
- Who did what
- When it happened
- From where (IP, location)
- What was the result

### Common Failures

**Missing Events:**
- No authentication logs
- Missing privilege changes
- No failed login tracking

**Poor Quality:**
- Vague error messages
- Insufficient context
- No correlation IDs

**No Alerting:**
- Brute force undetected
- No privilege change alerts
- No anomaly detection

**Short Retention:**
- Logs deleted too quickly
- Can't investigate historical attacks

**Tampering:**
- Logs stored insecurely
- Attackers can delete evidence

### Questions

**Q1: IP of attacker performing brute-force?**

Check authentication logs for multiple failed attempts.

- Answer: `203.0.113.45`

---

**Q2: Username attacker gained access to?**

Find successful login after brute force.

- Answer: `admin`

---

**Q3: Endpoint attacker accessed?**

Check access logs for suspicious admin actions.

- Answer: `/supersecretadminstuff`

---

## Quick Reference

### IAAA Flow

```
Identity → Authentication → Authorisation → Accountability
```

**Cannot skip levels!**

### OWASP Top 10 2025 IAAA Failures

**A01:** Broken Access Control

**A07:** Authentication Failures

**A09:** Logging & Alerting Failures

### A01: Broken Access Control

**IDOR Example:**
```
/user/profile?id=123
/user/profile?id=124  ← Unauthorized access
```

**Detection:**
- Try changing IDs
- Test different user contexts
- Check URL parameters

**Prevention:**
- Server-side validation
- Session-based access checks
- Indirect references

### A07: Authentication Failures

**Attack Vectors:**
- Credential stuffing
- Brute force
- Session hijacking
- Account enumeration

**Detection:**
- Multiple failed logins
- Different error messages
- Session anomalies

**Prevention:**
- Rate limiting
- Account lockout
- MFA/2FA
- Strong session management

### A09: Logging & Alerting

**What to Log:**
```
[timestamp] [user] [action] [resource] [result] [IP]
```

**Critical Events:**
- Authentication (success/fail)
- Authorization changes
- Sensitive data access
- Administrative actions
- Configuration changes

**Alerting Triggers:**
- Multiple failed logins
- Privilege escalation
- After-hours access
- Suspicious patterns

### Privilege Escalation

**Horizontal:**
| From | To | Risk |
|------|-----|------|
| User A | User B | Medium |
| Customer 1 | Customer 2 | Medium |

**Vertical:**
| From | To | Risk |
|------|-----|------|
| User | Admin | High |
| Employee | Root | Critical |

### Testing Checklist

**Access Control:**
- [ ] Change user IDs
- [ ] Modify role parameters
- [ ] Test direct object references
- [ ] Check authorization on every request

**Authentication:**
- [ ] Test username enumeration
- [ ] Try weak passwords
- [ ] Test rate limiting
- [ ] Check session handling
- [ ] Test account recovery

**Logging:**
- [ ] Verify authentication logs
- [ ] Check authorization logs
- [ ] Test alerting mechanism
- [ ] Verify log retention
- [ ] Check log integrity

### Common Mistakes

**Access Control:**
- Client-side only checks
- Trusting user input
- No re-authorization
- Missing parameter validation

**Authentication:**
- Weak password policy
- No failed login tracking
- Predictable credentials
- Session never expires

**Logging:**
- Logging disabled
- No alerts configured
- Insufficient detail
- Logs easily tampered

---

## Key Takeaways

- **IAAA** cannot skip levels
- **Identity** unique account
- **Authentication** proves identity
- **Authorisation** defines permissions
- **Accountability** logs actions
- **A01** broken access control
- **IDOR** common vulnerability
- **Horizontal** same role escalation
- **Vertical** role elevation
- **A07** authentication failures
- **Rate limiting** prevents brute force
- **A09** logging failures
- **Logs** enable detection
- **Alerting** notifies defenders

Test every layer!

Happy hunting!
