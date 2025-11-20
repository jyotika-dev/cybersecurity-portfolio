# Linux Logging for SOC - TryHackMe

> Master Linux logs for threat detection and forensics

**Room Link:** https://tryhackme.com/room/linuxloggingforsoc

<img width="1567" height="882" alt="image" src="https://github.com/user-attachments/assets/069be1a8-defb-4a4e-8a6b-fc9eec427d3a" />

---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Working with Text Logs

### Common Log Files

- **/var/log/syslog** - System events
- **/var/log/auth.log** - Authentication
- **/var/log/dpkg.log** - Package management
- **/var/log/kern.log** - Kernel messages

### Questions

**Q1: Which time server domain did VM contact?**

**Command:**
```bash
grep -i ntp /var/log/syslog
```

**Why NTP matters:** Time sync critical for log correlation.

- Answer: `ntp.ubuntu.com`

---

**Q2: Kernel message from Yama in syslog?**

**Command:**
```bash
grep -i Yama /var/log/syslog
```

**Yama:** Linux Security Module for stricter debugging policies.

- Answer: `Yama: becoming mindful.`

---

## Task 3: Authentication Logs

**File:** `/var/log/auth.log`

**Contains:**
- Login attempts
- sudo usage
- User management

### Questions

**Q1: Which IP failed SSH login on multiple users?**

**Command:**
```bash
cat /var/log/auth.log | grep "sshd" | grep "Failed"
```

**Pattern:** Multiple failures = brute force.

- Answer: `10.14.94.82`

---

**Q2: User created and added to sudo group?**

**Command:**
```bash
cat /var/log/auth.log | grep -E "(useradd|usermod)"
```

**Red flag:** New privileged account = persistence.

- Answer: `xerxes`

---

## Task 4: Common Linux Logs

### Package Manager Logs

**File:** `/var/log/dpkg.log`

**Tracks:** Package installs, updates, removals

### Bash History

**Location:** `~/.bash_history` (per user)

**Also check:** `/root/.bash_history`

### Questions

**Q1: Which unzip version installed?**

**Command:**
```bash
cat /var/log/dpkg.log | grep unzip
```

- Answer: `6.0-28ubuntu4.1`

---

**Q2: Flag in user's bash history?**

**Commands:**
```bash
grep -i "THM{" /home/*/.bash_history
sudo grep -i "THM{" /root/.bash_history
```

**Note:** Always check root's history!

- Answer: `THM{note_to_remember}`

---

## Task 5: Runtime Monitoring

### System Calls

**What they are:** Kernel interface for user programs.

**Key syscall:** `execve` - Execute programs

**All file/process operations go through syscalls.**

### Questions

**Q1: Linux syscall to execute program?**

- Answer: `execve`

---

**Q2: Can programs bypass syscalls? (Yea/Nay)**

No legitimate way to bypass kernel syscalls.

- Answer: `Nay`

---

## Task 6: Using auditd

### What is auditd?

Linux audit system for detailed system monitoring.

**Features:**
- Tamper-resistant logging
- File access tracking
- Process execution monitoring
- Command argument capture

**Location:** `/var/log/audit/audit.log`

### Commands

**Search audit logs:**
```bash
ausearch -i | grep "keyword"
```

**View specific event:**
```bash
ausearch -i -k rule_name
```

### Questions

**Q1: When was secret.thm opened first time?**

**Command:**
```bash
ausearch -i | grep "secret.thm"
```

Check timestamp in audit record.

- Answer: `08/13/25 18:36:54`

---

**Q2: Original filename downloaded from GitHub via wget?**

**Command:**
```bash
ausearch -i | grep "github"
```

Check proctitle field for full URL.

- Answer: `naabu_2.3.5_linux_amd64.zip`

---

**Q3: Network range scanned using downloaded tool?**

**Command:**
```bash
ausearch -i | grep naabu
```

Check proctitle for command arguments.

**Pattern:** `./naabu -host 192.168.50.0/24`

- Answer: `192.168.50.0/24`

---

## Quick Reference

### Essential Log Locations

```
/var/log/syslog       - System events, NTP
/var/log/auth.log     - SSH, sudo, users
/var/log/dpkg.log     - Package management
/var/log/kern.log     - Kernel messages
/var/log/audit/audit.log - Detailed syscall logs
~/.bash_history       - User commands
/root/.bash_history   - Root commands
```

### Common grep Patterns

**Find SSH failures:**
```bash
grep "Failed password" /var/log/auth.log
```

**New users:**
```bash
grep "useradd" /var/log/auth.log
```

**Package installs:**
```bash
grep "install" /var/log/dpkg.log
```

**Audit search:**
```bash
ausearch -i | grep "pattern"
```

### Investigation Workflow

**1. Start with syslog:**
- Get timeline overview
- Check NTP sync
- Find kernel events

**2. Check auth.log:**
- Failed logins
- New users
- sudo usage

**3. Review package logs:**
- Installed tools
- Version tracking

**4. Examine history files:**
- User commands
- Root activity
- Check ALL users

**5. Use auditd:**
- File access times
- Process execution
- Command arguments
- Network activity

### Detection Patterns

**Brute Force:**
```
Multiple "Failed password" from same IP
Different usernames tried
Short time intervals
```

**Privilege Escalation:**
```
New user created
Added to sudo group
Unexpected privilege changes
```

**Reconnaissance:**
```
Network scanning tools installed (nmap, naabu)
Large IP ranges scanned
Port enumeration
```

**Persistence:**
```
Backdoor accounts
Unexpected sudo access
Modified startup scripts
```

### Useful Commands

**Count failures per IP:**
```bash
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn
```

**List all users created:**
```bash
grep "new user" /var/log/auth.log
```

**Find commands by user:**
```bash
sudo grep "COMMAND=" /var/log/auth.log | grep "username"
```

**Check all bash histories:**
```bash
find /home -name ".bash_history" -exec cat {} \;
```

---

## Key Takeaways

- **NTP sync** essential for timeline correlation
- **/var/log/auth.log** most critical for security
- **Failed SSH** attempts indicate brute force
- **New privileged users** = high-severity finding
- **Package logs** reveal installed tools
- **Check root's history** if user history empty
- **execve** syscall executes programs
- **Syscalls cannot** be legitimately bypassed
- **auditd** provides forensic-level detail
- **proctitle** shows full command with arguments
- **Timestamps** in auditd are authoritative
- **Network scanning** tools = reconnaissance
- **Layer logs** for complete picture

Linux logs tell the complete story
Happy hunting!
