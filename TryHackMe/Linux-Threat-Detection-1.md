# Linux Threat Detection 1 - TryHackMe

> Detect initial access attacks on Linux systems

**Room Link:** https://tryhackme.com/room/linuxthreatdetection1

<img width="1555" height="881" alt="image" src="https://github.com/user-attachments/assets/26a065aa-6057-478c-b6bc-8836b9113031" />

---

## Task 1: Introduction

**Topics:**
- SSH attack detection
- Service breach analysis
- Process tree forensics
- Initial access techniques

**No answer needed**

---

## Task 2: Initial Access via SSH

### SSH Exposure Risk

**SSH** = Secure Shell (like RDP for Windows)

**MITRE:** External Remote Services

### Attack Methods

**Stolen Keys:**
- From GitHub repos
- Ansible servers
- Infected admin laptops

**Breached Passwords:**
- Weak test passwords
- Contractor access ("12345678")
- Old exposed servers

### Questions

**Q1: When did Ubuntu user first login via SSH?**

**Command:**
```bash
cat /var/log/auth.log | grep "sshd"
```

Look for first successful login entry.

- Answer: `2024-10-22`

---

**Q2: Did Ubuntu use SSH keys instead of password? (Yea/Nay)**

Check for "accepted publickey" in logs.

SHA256 hash at end = key-based auth.

- Answer: `Yea`

---

## Task 3: Detecting SSH Attacks

### Malicious Login Indicators

1. Multiple failed logins → success
2. Logins from external/untrusted IP
3. Password-based authentication

### Investigation Fields

- **Username:** Expected login?
- **Source IP:** TI lookup, trusted?
- **Login history:** Preceding brute force?
- **Next steps:** Analyze post-login activity

### Questions

**Q1: When did SSH brute force start?**

**Command:**
```bash
cat /var/log/auth.log | grep "password"
```

Look for multiple "Failed password" entries.

- Answer: `2025-08-21`

---

**Q2: Which four users did botnet attempt to breach?**

Check failed password attempts.

List targeted usernames alphabetically.

- Answer: `root, roy, sol, user`

---

**Q3: Which IP breached the root user?**

**Command:**
```bash
cat /var/log/auth.log | grep "Accepted"
```

Find "Accepted password" for root.

- Answer: `91.224.92.79`

---

## Task 4: Initial Access via Services

### Web Service Vulnerabilities

**Common issues:**
- Command injection
- File inclusion
- Insecure APIs

### Questions

**Q1: Path to Python file attacker tried to open?**

Examine web access logs.

Look for `cat` command = reading file.

**Command injection** in ping service.

- Answer: `/opt/trypingme/main.py`

---

**Q2: Flag inside the opened file?**

```bash
cat /opt/trypingme/main.py
```

- Answer: `THM{i_am_vulnerable!}`

---

## Task 5: Detecting Service Breach

### Process Tree Analysis

**Why use it?**
- Trace suspicious commands to origin
- Find parent process
- Identify attack entry point

### Building Process Tree

**Start:** Suspicious command (whoami)

**Goal:** Find which app/user initiated it

### Questions

**Q1: PPID of suspicious whoami command?**

**Command:**
```bash
ausearch -i -x whoami
```

PPID = Parent Process ID

- Answer: `1018`

---

**Q2: PID of TryPingMe app?**

**Command:**
```bash
ausearch -i --pid 1018
```

Move up the tree, find parent.

- Answer: `577`

---

**Q3: Program used for reverse shell?**

**Command:**
```bash
ausearch -i --pid 577
```

Check what spawned the malicious process.

- Answer: `Python`

---

## Task 6: Advanced Initial Access

### Attack Types

**Phishing:** User-targeted attacks

**USB Attacks:** Physical access

**Supply Chain:** Compromised software updates

### Detection Method

**Process Tree Analysis:**
1. Alert on suspicious command/connection
2. Build process tree
3. Trace to origin (web server, SSH, app)
4. Determine legitimate vs malicious

### Questions

**Q1: Technique when trusted app runs malicious commands?**

Software compromised at source.

- Answer: `Supply Chain Compromise`

---

**Q2: Detection method for various Initial Access techniques?**

- Answer: `Process Tree Analysis`

---

## Quick Reference

### SSH Log Analysis

**Location:** `/var/log/auth.log`

**Key patterns:**
```bash
# All SSH activity
grep "sshd" /var/log/auth.log

# Failed logins
grep "Failed password" /var/log/auth.log

# Successful logins
grep "Accepted" /var/log/auth.log

# Key-based auth
grep "publickey" /var/log/auth.log
```

### Brute Force Indicators

- Same IP, multiple failures
- Short time intervals
- Multiple usernames tried
- Eventually succeeds

### Process Tree Commands

**Find specific command:**
```bash
ausearch -i -x whoami
```

**Trace by PID:**
```bash
ausearch -i --pid 1018
```

**Full audit search:**
```bash
ausearch -i | grep "keyword"
```

### Process Tree Flow

```
whoami (suspicious)
    ↑
PPID 1018 (parent)
    ↑
PID 577 (TryPingMe app)
    ↑
Python (reverse shell)
    ↑
Web request (command injection)
```

### Initial Access Techniques

| Technique | Vector | Detection |
|-----------|--------|-----------|
| **SSH Brute Force** | Exposed SSH | auth.log failures |
| **Stolen Keys** | Leaked credentials | Unexpected key auth |
| **Service Exploit** | Web vulnerability | Access logs, auditd |
| **Supply Chain** | Compromised update | Process tree |
| **Phishing** | User interaction | Process tree |

### Detection Workflow

1. **Alert received** (suspicious command)
2. **Check auth.log** (SSH activity)
3. **Check access.log** (web activity)
4. **Build process tree** (auditd)
5. **Trace to origin** (PID → PPID)
6. **Determine verdict** (legitimate)

---

## Key Takeaways

- **SSH** most common Linux initial access
- **External Remote Services** MITRE technique
- **auth.log** contains SSH activity
- **Multiple failures** then success = brute force
- **Key-based auth** shows SHA256 hash
- **91.224.92.79** breached root account
- **Command injection** in web apps
- **Process tree** traces attack origin
- **PPID** = Parent Process ID
- **ausearch** queries audit logs
- **Python** common for reverse shells
- **Supply chain** = trusted app compromised
- **Process Tree Analysis** detects various attacks
- **Always trace** suspicious commands to source

Trace the attack back to its origin
Happy hunting!
