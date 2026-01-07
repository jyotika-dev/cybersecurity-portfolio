# TryHackMe Identity and Access Management - Walkthrough

> Understanding IAAA and Access Control

**Room Link:** https://tryhackme.com/room/iaaaidm

<img width="1305" height="911" alt="image" src="https://github.com/user-attachments/assets/c96f384e-06fd-40e2-bdb4-f8a5a401cb7f" />

---

## Overview

**Topics covered:**
- IAAA Model (Identification, Authentication, Authorization, Accountability)
- Access Control Models
- Single Sign-On (SSO)
- Identity Management

---

## IAAA Model

**Four pillars of security:**

| Process | What it does | Example |
|---------|--------------|---------|
| Identification | Who you claim to be | Username, email |
| Authentication | Prove who you are | Password, fingerprint |
| Authorization | What you can access | Read/write permissions |
| Accountability | Track your actions | Logging, audit trails |

### Q&A

**Q: You are granted access to read and send email. What is this?**
- Answer: `Authorisation`

**Q: Which process requires you to enter username?**
- Answer: `Identification`

**Q: Process required to enforce policy?**
- Answer: `Accountability`

---

## Identification

**What it is:**
- Claiming an identity
- Username, email, ID number
- No verification yet

**Can be used:**
- Email address ✓
- Mobile number ✓
- Passport number ✓
- Student ID ✓

**Cannot be used:**
- Year of birth ✗ (not unique)
- Street number ✗ (not unique)

---

## Authentication

**Methods:**

**Something you know:**
- Password
- PIN
- Security questions

**Something you have:**
- Smart card
- Phone (for SMS)
- Security token

**Something you are:**
- Fingerprint
- Face recognition
- Voice recognition

**Less common:**
- Somewhere you are (location)
- Something you do (behavior)

### Multi-Factor Authentication (MFA)

**Using 2 or more methods:**
- Password + SMS code
- Card + PIN
- More secure than single method

### Q&A

**Q: Email with username and password uses what authentication?**
- Answer: `1` (Something you know)

**Q: Banking app with password then SMS code?**
- Answer: `4` (2FA)

**Q: Phone system with secret number to hear messages?**
- Answer: `1` (Something you know)

**Q: Card swipe + 4-digit PIN for elevator?**
- Answer: `4` (2FA)

---

## Authorization & Access Control

**Authorization:**
- Defines what you can access
- Sets permissions and privileges
- Policy decision

**Access Control:**
- Enforces the policy
- Blocks unauthorized access
- Technical implementation

### Q&A

**Q: Secretary can send email on manager's behalf?**
- Answer: `1` (Authorization - policy)

**Q: View-only document permissions?**
- Answer: `2` (Access Control - enforcement)

**Q: Cleaning staff needs access to all rooms?**
- Answer: `1` (Authorization - policy)

---

## Accountability & Logging

**Why logging:**
- Track user actions
- Compliance requirements
- Incident investigation
- Security monitoring

**Best practices:**
- Centralized logging
- Tamper-proof logs
- Separate log server
- Log forwarding

**SIEM (Security Information and Event Management):**
- Aggregates logs from multiple sources
- Analyzes for threats
- Alerts on anomalies
- Compliance reporting

---

## Identity Management

**IdM (Identity Management):**
- Manages user identities
- Focuses on authentication
- User/group management
- Centralized database

**IAM (Identity and Access Management):**
- Broader than IdM
- Manages identities AND access
- Role-based control
- Monitors and controls access

**Benefits:**
- Protect sensitive data
- Compliance
- Simplified access
- Better user experience
- Cost reduction

### Q&A

**Q: What does IdM stand for?**
- Answer: `Identity Management`

**Q: What does IAM stand for?**
- Answer: `Identity and Access Management`

---

## Authentication Attacks

**Replay Attack:**
- Attacker captures encrypted password
- Replays it to authenticate
- Works if no time/session validation

**Prevention:**
- Challenge-response
- Timestamps
- Session tokens
- One-time passwords

### Q&A

**Q: Attack using user's encrypted password response?**
- Answer: `Replay Attack`

---

## Access Control Models

### DAC (Discretionary Access Control)

**How it works:**
- Data owner controls access
- Explicit permissions
- File sharing example

**When to use:**
- Small scale
- Individual control needed

### RBAC (Role-Based Access Control)

**How it works:**
- Users assigned roles
- Roles have permissions
- Access based on role

**Benefits:**
- Easy maintenance
- Scalable
- Group-based

### MAC (Mandatory Access Control)

**How it works:**
- System enforces rules
- Users have limited control
- Security-focused

**Examples:**
- SELinux
- AppArmor

### Q&A

**Q: Sharing document with accounting department only?**
- Answer: `2` (RBAC - role-based)

**Q: Social media post visible to 3 friends?**
- Answer: `1` (DAC - owner controls)

---

## Single Sign-On (SSO)

**What it is:**
- One login for multiple services
- Centralized authentication
- Single password

**Advantages:**
- One strong password
- MFA configured once
- Simpler support
- More efficient

**Disadvantages:**
- All services compromised if hacked
- Single point of failure
- Complex implementation

### Q&A

**Q: What does SSO stand for?**
- Answer: `Single Sign-On`

**Q: Does SSO simplify MFA (set up once)?**
- Answer: `Yea`

**Q: Does SSO require different passwords for services?**
- Answer: `Nay`

**Q: Access various services after signing in once?**
- Answer: `Yea`

**Q: Need to remember single password with SSO?**
- Answer: `Yea`

---

## Scenarios (Interactive)

**Click "View Site" and answer:**

| Item | Type |
|------|------|
| Fingerprint/Pattern/Code | Authentication |
| ATM random code | Identification |
| Mail | Identification |
| Policy | Authorization |
| Name | Identification |
| Unix logging attempts | Logging |
| shadow file | Access Control |
| Pattern | Authentication |

**Flag:** `THM_ACCESS_CONTROL`

---

## Quick Reference

### IAAA Summary

```
Identification → Who you claim to be
    ↓
Authentication → Prove you are who you claim
    ↓
Authorization → What you're allowed to access
    ↓
Accountability → Track what you did
```

### Authentication Factors

```
Factor 1: Something you know (password)
Factor 2: Something you have (phone)
Factor 3: Something you are (fingerprint)
```

### Access Control Comparison

| Model | Control | Best For |
|-------|---------|----------|
| DAC | Owner decides | Small scale, file sharing |
| RBAC | Role-based | Large organizations |
| MAC | System enforces | High security needs |

---

## Key Concepts

**Identification:**
- Claims identity (username)
- Not verified yet
- Must be unique

**Authentication:**
- Proves identity
- Requires credentials
- MFA is stronger

**Authorization:**
- Grants permissions
- Based on role/policy
- What you can do

**Accountability:**
- Tracks actions
- Requires logging
- Who did what

**Access Control:**
- Enforces policies
- Technical implementation
- Blocks unauthorized

---

## Best Practices

**For Users:**
- Strong passwords
- Enable MFA
- Don't share credentials
- Log out when done

**For Organizations:**
- Implement IAAA
- Centralized logging
- Regular audits
- Principle of least privilege

**For Security:**
- Role-based access
- Monitor logs
- Implement SSO carefully
- Regular reviews

---

## Key Takeaways

- **IAAA** is foundation of security
- **Identification** claims identity
- **Authentication** proves identity
- **Authorization** grants access
- **Accountability** tracks actions
- **MFA** adds security layer
- **RBAC** scales better than DAC
- **SSO** simplifies but centralizes risk
- **Logging** essential for accountability
- **Access control** enforces policies

---

Happy learning!
