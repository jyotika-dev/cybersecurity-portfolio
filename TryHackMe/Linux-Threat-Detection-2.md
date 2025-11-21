# Linux Threat Detection 2 - TryHackMe

> Detect post-exploitation and lateral movement on Linux

**Room Link:** https://tryhackme.com/room/linuxthreatdetection2

<img width="1551" height="868" alt="image" src="https://github.com/user-attachments/assets/023a5b0b-d312-4a05-9de8-e1ead304f66f" />

---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Environment Discovery

### Attacker Goals

**Discover:**
- Cloud environment
- Security tools installed
- System configuration

### Commands

**Detect virtualization:**
```bash
systemd-detect-virt
```

**List processes:**
```bash
ps aux
```

### Questions

**Q1: Output of systemd-detect-virt?**

Identifies cloud provider.

- Answer: `amazon`

---

**Q2: Full path to antimalware binary?**

Check `ps aux` for EDR/AV processes.

- Answer: `/var/lib/ultrasec/malscan`

---

## Task 3: Discovery Commands

### Script-Based Discovery

Attackers often use scripts for enumeration.

**Common discovery commands:**
- hostname
- whoami
- ps
- cat /etc/passwd

### Questions

**Q1: Path of script that ran "hostname"?**

Trace back through process tree.

- Answer: `/home/itsupport/debug.sh`

---

**Q2: Last discovery command by script?**

Check script execution order.

- Answer: `ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu`

---

**Q3: Email of script author?**

Read script contents.

- Answer: `greg@tryhackme.thm`

---

## Task 4: Downloads and Staging

### Attacker Downloads

**Common tools:**
- curl
- wget

**Staging directories:**
- /tmp
- /var/tmp
- /dev/shm

### Questions

**Q1: Domain where Elastic agent downloaded from?**

Check download logs for legitimate software.

- Answer: `artifacts.elastic.co`

---

**Q2: Full path to helper.sh script?**

Look in staging directories.

- Answer: `/var/tmp/helper.sh`

---

**Q3: Which downloaded file more likely malicious - curl or wget?**

Compare download sources and destinations.

- Answer: `curl`

---

## Task 5: SSH Brute Force & Post-Access

### Attack Sequence

1. Brute force SSH
2. Gain access
3. Run discovery commands
4. Check for security tools

### Questions

**Q1: IP that brute-forced SSH?**

Check auth.log for failures then success.

- Answer: `45.9.148.125`

---

**Q2: Command to list last logged-in users?**

Common post-exploitation discovery.

- Answer: `last`

---

**Q3: Three EDR processes searched with egrep?**

Attacker checking for security tools.

Alphabetical order.

- Answer: `ds_agent,falcon,sentinel`

---

## Task 6: Lateral Movement & Mining

### Post-Exploitation

**Actions:**
- Transfer tools via SCP
- Deploy cryptominer
- Scan for more targets

### Questions

**Q1: Malicious archive transferred via SCP?**

Check SCP/file transfer logs.

- Answer: `kernupd.tar.gz`

---

**Q2: Full cryptominer launch command?**

Look for `nohup` (background execution).

- Answer: `nohup /tmp/.apt/kernupd/kernupd`

---

**Q3: IP range scanned for exposed SSH?**

Internal network reconnaissance.

- Answer: `10.10.12.1-10.10.12.10`

---

## Quick Reference

### Environment Discovery Commands

```bash
systemd-detect-virt    # Cloud/VM detection
hostnamectl            # System info
uname -a               # Kernel info
cat /etc/os-release    # OS version
ps aux                 # Running processes
```

### Common Staging Paths

```
/tmp/
/var/tmp/
/dev/shm/
/home/user/.cache/
/tmp/.hidden/
```

### EDR Process Names

- **falcon** - CrowdStrike
- **sentinel** - SentinelOne
- **ds_agent** - Trend Micro

### Detection Patterns

**Discovery phase:**
- Multiple recon commands
- Script-based enumeration
- EDR/AV checks

**Staging phase:**
- Downloads to /tmp or /var/tmp
- Hidden directories (.apt, .cache)
- Archive extraction

**Lateral movement:**
- Internal IP scanning
- SCP file transfers
- SSH to other hosts

### Key Indicators

| Activity | Indicator |
|----------|-----------|
| Cloud detection | systemd-detect-virt |
| AV evasion | egrep for EDR names |
| Persistence | nohup command |
| Cryptomining | Hidden binary execution |
| Lateral movement | Internal IP scan |

---

## Key Takeaways

- **amazon** cloud detected via systemd
- **/var/lib/ultrasec/malscan** is AV binary
- **debug.sh** ran discovery commands
- **artifacts.elastic.co** legitimate download
- **/var/tmp/helper.sh** staging location
- **curl download** more suspicious
- **45.9.148.125** brute-forced SSH
- **last** command shows login history
- **falcon,sentinel,ds_agent** EDR processes
- **kernupd.tar.gz** malicious archive via SCP
- **nohup** runs cryptominer in background
- **/tmp/.apt/** hidden staging directory
- **10.10.12.1-10** internal scan range
- **Process tree** reveals attack chain

Trace the full attack lifecycle
Happy hunting!
