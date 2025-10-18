# TryHackMe - Hydra

**Room**: Hydra  
**URL**: https://tryhackme.com/room/hydra  

---

## Room Overview

This TryHackMe room covers the fundamentals of using Hydra, a popular brute force password cracking tool. The room demonstrates practical password attacks against SSH and web login forms, highlighting the importance of strong password policies.

---

## Tasks Completed

### Task 1: Hydra Introduction

**What is Hydra?**
- Brute force online password cracking program
- Quick system login password "hacking" tool
- Runs through password lists to automate authentication attacks

**Supported Protocols:**
Hydra supports extensive protocols including:
- **Network Services**: SSH, FTP, Telnet, SNMP, RDP
- **Web Services**: HTTP/HTTPS forms (GET/POST), HTTP-Proxy
- **Database Services**: MySQL, MS-SQL, PostgreSQL, Oracle
- **Email Services**: SMTP, POP3, IMAP
- **Other Services**: SMB, VNC, Cisco auth, LDAP, MongoDB

**Key Security Insight:**
- Default credentials (`admin:password`) are easily compromised
- Common passwords from large wordlists pose significant risk
- Strong passwords (8+ characters, special characters) provide better protection

### Task 2: Using Hydra

**SSH Brute Force:**
```bash
hydra -l <username> -P <wordlist> <target_ip> -t 4 ssh
```

**Command Options:**
- `-l` → specify username
- `-P` → password wordlist path
- `-t` → number of parallel threads

**Web Form Brute Force:**
```bash
hydra -l <username> -P <wordlist> <target_ip> http-post-form "<path>:<credentials>:<failure_string>" -V
```

**Form Attack Components:**
- `<path>` → login page URL (e.g., `/login.php`)
- `<credentials>` → form parameters (`username=^USER^&password=^PASS^`)
- `<failure_string>` → text appearing on failed login (`F=incorrect`)
- `-V` → verbose output for monitoring attempts

**Practical Results:**
- **Flag 1** (Web password): `THM{2673a7dd116d68e85c48ec0b1f2612e}`
- **Flag 2** (SSH password): `THM{c8eeb0468febbadea859baeb33b2541b}`

---

## Technical Analysis

**Attack Methodology:**
1. **Target identification** → SSH service and web login form
2. **Wordlist preparation** → password dictionary for brute force
3. **Thread optimization** → parallel attacks for efficiency
4. **Response analysis** → identifying successful vs failed attempts

**Command Examples Used:**
```bash
# SSH brute force against user 'molly'
hydra -l molly -P /usr/share/wordlists/rockyou.txt ssh://10.201.27.27 -v

# Web form brute force
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.201.27.27 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```

---

## Key Learning Points

**Brute Force Fundamentals:**
- Dictionary attacks use common password lists
- Threading increases attack speed and efficiency
- Different protocols require specific attack syntax

**Web Form Analysis:**
- HTTP POST forms require parameter identification
- Failure strings help distinguish unsuccessful attempts
- Browser developer tools assist in form analysis

**SSH Attack Vectors:**
- Username enumeration often precedes password attacks
- SSH brute force can be detected through log analysis
- Rate limiting and fail2ban provide defensive measures

---

## Security Applications

**Offensive Security:**
- Penetration testing weak authentication systems
- Validating password policy effectiveness
- Demonstrating credential-based attack vectors

**Defensive Security:**
- **Account lockout policies** prevent brute force attempts
- **Strong password requirements** increase attack complexity
- **Multi-factor authentication** provides additional security layers
- **Log monitoring** detects suspicious login patterns

**Incident Response:**
- Authentication logs reveal brute force attempts
- IP-based blocking prevents continued attacks
- Password policy review following successful attacks

---

## Defensive Recommendations

**Password Security:**
- Minimum 12 characters with complexity requirements
- Regular password rotation policies
- Prohibition of common/default passwords

**System Hardening:**
- Implement account lockout after failed attempts
- Deploy fail2ban for automated IP blocking
- Enable comprehensive authentication logging
- Consider key-based SSH authentication

**Monitoring:**
- Real-time authentication failure alerts
- Geographic login anomaly detection
- Brute force pattern recognition in logs

---
