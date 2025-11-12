# Systems as Attack Vectors - TryHackMe

> Learn how attackers exploit vulnerable and misconfigured systems, and how to protect them

**Room Link:** https://tryhackme.com/room/systemsasattackvectors

<img width="1572" height="880" alt="image" src="https://github.com/user-attachments/assets/a798b5c7-230f-4f91-8600-051b59fc91f0" />


---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Definition of System

### What is a System?

A system can be:
- Computers
- Servers
- Network devices
- Applications
- Any connected technology

### Key Points

**Attacks can happen without user interaction!**
- Automatic exploitation
- Zero-click attacks
- No user action needed

**Single system breach = Big consequences**
- Lateral movement
- Network access
- Data theft

### Questions

**Can cyber attacks happen without victim intervention (Yea/Nay)?**

Yes! Attacks can be automatic.

- Answer: `Yea`

---

**Can a breach of just a single system lead to disastrous consequences (yea/nay)?**

One system can compromise entire network.

- Answer: `Yea`

---

## Task 3: Attacks on Systems

### Three Main Attack Types

**1. Human-Led Attacks**
- Social engineering
- Phishing
- Requires user interaction

**2. Vulnerabilities**
- Security flaws
- Software bugs
- Can be exploited

**3. Supply Chain**
- Compromised updates
- Malicious libraries
- Trusted source attacks

### Questions

**What is the term for a security flaw that can be exploited to breach a system?**
- Answer: `Vulnerability`

---

**What is the name of the attack when malware comes from a trusted app or library?**

Through trusted sources = Supply chain

- Answer: `Supply Chain`

---

## Task 4: Vulnerabilities

### What are Vulnerabilities?

Security flaws in systems that can be exploited.

**Examples:**
- Unpatched software
- Known CVEs
- Zero-day exploits

### CVE (Common Vulnerabilities and Exposures)

Standardized identifier for vulnerabilities.

**Format:** CVE-YEAR-NUMBER

### Fixing Vulnerabilities

**Patch** = Security update that fixes vulnerabilities

### Questions

**What is the CVE for the critical SharePoint vulnerability dubbed "ToolShell"?**

Look up the CVE number for this specific vulnerability.

- Answer: `CVE-2025-53770`

---

**How would you respond to a detected vulnerability on your system?**

Apply security update = Patch

- Answer: `Patch`

---

## Task 5: Misconfigurations

### What are Misconfigurations?

Incorrect system settings that create security risks.

**Examples:**
- Default passwords
- Open ports
- Weak settings
- Unnecessary services

### Key Difference

**Vulnerabilities:**
- Fixed by patches
- Software updates

**Misconfigurations:**
- Fixed by changing settings
- Not fixed by patches!

### Finding Misconfigurations

**Penetration Testing:**
- Authorized security testing
- Find weaknesses
- Simulate attacks

### Questions

**Can a system patch or software update fix the misconfigurations (yea/nay)?**

Patches fix vulnerabilities, NOT misconfigurations!

- Answer: `Nay`

---

**Which activity involves an authorized cyber attack to detect the misconfigurations?**

Authorized testing = Penetration Testing

- Answer: `Penetration Testing`

---

## Task 6: Practice

### Challenge: Systems at Risk

Analyze four security scenarios and choose correct responses.

**Case 1: Critical vulnerability detected**
→ Release patch immediately

**Case 2: Default admin password**
→ Change to secure password

**Case 3: Unpatched Cisco firewalls**
→ Ensure all are patched

**Case 4: Compromised library**
→ Supply chain vulnerability

### Questions

**What flag did you receive after completing "Systems at Risk" challenge?**
- Answer: `THM{patch_or_reconfigure?}`

---

### Challenge: Remediation Plan

Approve appropriate security policies.

**Policies to approve:**

1. **Antivirus Protection**
   - Protects against common threats
   - Data stealers, USB worms

2. **Security Training for IT**
   - Informed team = better security
   - Prevents misconfigurations

3. **Patch Management Policy**
   - Organized patching process
   - Reduces exploitation risk

4. **Secure Password Policy**
   - Strong passwords required
   - Prevents brute-force attacks

**What flag did you receive after completing "Remediation Plan" challenge?**
- Answer: `THM{best_systems_defender!}`

---

## Task 7: Conclusion

**No answer needed**

---

## Quick Reference

### Attack Types

| Type | Description | Example |
|------|-------------|---------|
| Human-Led | Requires interaction | Phishing |
| Vulnerability | Security flaw | Unpatched software |
| Supply Chain | Compromised trusted source | Malicious update |

### Vulnerabilities vs Misconfigurations

| Aspect | Vulnerability | Misconfiguration |
|--------|---------------|------------------|
| Cause | Software flaw | Wrong settings |
| Fix | Patch/Update | Change settings |
| Detection | Scanning | Auditing |

### Security Responses

| Issue | Response |
|-------|----------|
| Vulnerability found | Patch immediately |
| Default password | Change password |
| Unpatched system | Apply updates |
| Supply chain risk | Verify sources |

### Best Practices

| Practice | Purpose |
|----------|---------|
| Antivirus | Block malware |
| Security Training | Prevent errors |
| Patch Management | Fix vulnerabilities |
| Password Policy | Prevent brute-force |
| Penetration Testing | Find weaknesses |

---

## Key Takeaways

- **Attacks can happen** without user action
- **Single breach** can compromise everything
- **Vulnerability** = security flaw
- **Supply chain** attacks use trusted sources
- **CVE-2025-53770** = SharePoint ToolShell
- **Patch** fixes vulnerabilities
- **Misconfigurations** NOT fixed by patches
- **Penetration testing** finds security issues
- **Four key policies:** Antivirus, Training, Patching, Passwords
- **Prevention** better than cure
- **Regular updates** are critical
- **Strong passwords** prevent attacks

Understanding attack vectors helps defend systems effectively. Both vulnerabilities and misconfigurations need attention
Happy defending!
