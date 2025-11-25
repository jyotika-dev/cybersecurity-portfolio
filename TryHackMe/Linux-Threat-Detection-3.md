# Linux Threat Detection 3 - TryHackMe

> Cover the last stages of attacks on Linux and learn how they look in system logs

**Room Link:** https://tryhackme.com/room/linuxthreatdetection3

<img width="1592" height="872" alt="image" src="https://github.com/user-attachments/assets/7b4ccb6a-8f4f-42a1-825b-41fe23fc25c3" />


---

## Task 1: Introduction

**Topics:**
- Reverse shells
- Privilege escalation
- Persistence mechanisms
- Post-exploitation activities

**No answer needed**

---

## Task 2: Reverse Shells

### What is a Reverse Shell?

**Reverse shell** = victim connects back to attacker's machine

**Why attackers use it:**
- Bypass firewalls (outbound allowed)
- Remote control of system
- Execute commands

### Command Injection Example

**TryPingMe app** has command injection vulnerability.

Attacker injects: `127.0.0.1 && whoami`

### Questions

**Q1: Run `127.0.0.1 && whoami` in TryPingMe. What output after ping?**

Execute command in web app.

Check output after ping results.

- Answer: `svctrypingme`

---

**Q2: What flag in TryPingMe response?**

- Answer: `THM{revshells_practitioner!}`

---

**Q3: Which IP spawned similar reverse shell via TryPingMe?**

Check auditd logs at `/home/ubuntu/scenario`.

Look for reverse shell activity.

- Answer: `10.14.105.255`

---

## Task 3: Privilege Escalation

### Why Escalate?

**Goal:** Gain root/admin access

**Methods:**
- Exploit vulnerabilities
- Weak credentials
- Misconfigured permissions

### Detection

Check audit logs for privilege changes.

### Questions

**Q1: Command used to search for "pass" keyword?**

Look for grep commands in logs.

- Answer: `grep -iR pass .`

---

**Q2: Command used to escalate to root?**

Check for user switching commands.

- Answer: `su root`

---

**Q3: Root password found in .env file?**

Check discovered credentials.

- Answer: `nGql1pQkGa`

---

## Task 4: Startup Persistence

### Persistence Methods

**Service persistence:**
- Systemd services
- Auto-start on boot

**Cron jobs:**
- Scheduled tasks
- Survive reboots

### Questions

**Q1: Flag from malware persisting as service?**

Run the persistent service malware.

- Answer: `THM{hidden_penguin!}`

---

**Q2: Flag from malware persisting as cron job?**

Execute the cron-based malware.

- Answer: `THM{ressurect_on_reboot!}`

---

## Task 5: Account Persistence

### Attacker Techniques

**Create backdoor user:**
- New admin account
- Add to sudo group
- SSH key installation

### Questions

**Q1: Which user was created and added to sudo?**

Check user creation logs.

- Answer: `koichi`

---

**Q2: File changed for SSH key persistence?**

Look for authorized_keys modifications.

- Answer: `/root/.ssh/authorized_keys`

---

## Task 6: Targeted Attacks and Recap

### Linux Ransomware

**Reality:** Linux systems targeted too

**Examples:**
- Server encryption
- Cloud infrastructure
- Critical systems

### Questions

**Q1: Does Linux ransomware exist and impact orgs? (Yea/Nay)**

- Answer: `Yea`

---

**Q2: Should you learn Linux threats even with Windows? (Yea/Nay)**

- Answer: `Yea`

---

## Quick Reference

### Reverse Shell Detection

**Look for:**
- Unusual network connections
- Spawned shells (bash, sh)
- Parent process analysis

**Commands:**
```bash
# Check network connections
netstat -antp

# Audit logs
ausearch -i
```

### Privilege Escalation Signs

**Indicators:**
- `su` or `sudo` commands
- Password searches
- Credential files accessed
- SUID binary execution

**Common searches:**
```bash
grep -iR pass .
find / -perm -4000
```


### Attack Chain

```
Initial Access (SSH/Web)
    ↓
Command Injection
    ↓
Reverse Shell
    ↓
Privilege Escalation
    ↓
Persistence Established
```

---

## Key Takeaways

- **Reverse shells** bypass firewalls via outbound
- **Command injection** common entry point
- **grep -iR pass** searches for credentials
- **su root** escalates to administrator
- **Systemd services** provide boot persistence
- **Cron jobs** run on schedule
- **Backdoor accounts** maintain access
- **authorized_keys** enables SSH persistence
- **Linux ransomware** is real threat
- **Cross-platform knowledge** essential
- **Audit logs** reveal attack stages
- **Always check** persistence mechanisms

Stay vigilant, check your logs
