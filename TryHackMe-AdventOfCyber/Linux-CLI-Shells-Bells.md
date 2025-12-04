# Advent of Cyber 2025 Day 1 - TryHackMe

> Linux CLI fundamentals - Shells & Bells

**Day 1 Link:** https://tryhackme.com/room/linuxcli-aoc2025-o1fpqkvxti

---

## Story

**Crisis:** McSkidy kidnapped!

**Location:** tbfc-web01 Linux server

**Mission:** Find clues in Christmas wishlist processing system

**Threat:** King Malhare's EASTMAS plan

**Credentials:**
- Username: mcskidy
- Password: AoC2025!

---

## Investigation

### Basic Commands

**First commands:**
```bash
echo "Hello World!"
ls
cat README.txt
```

McSkidy left a security guide before being kidnapped!

### Navigate to Guides

```bash
pwd                    # Check current directory
cd Guides              # Change to Guides directory
ls                     # List files (empty?)
```

### Find Hidden Files

**Hidden files start with `.` (dot)**

```bash
ls -la                 # Show hidden files
cat .guide.txt         # Read hidden guide
```

**Flag found:** THM{learning-linux-cli}

### Check Logs

McSkidy's guide: "Check /var/log/"

```bash
cd /var/log
ls
grep "Failed password" auth.log
```

Multiple failed logins from HopSec location!

### Find Suspicious Files

```bash
find /home/socmas -name *egg*
```

**Found:** /home/socmas/2025/eggstrike.sh

### Analyze Eggstrike Script

```bash
cd /home/socmas/2025
cat eggstrike.sh
```

**Flag found:** THM{sir-carrotbane-attacks}

**Script analysis:**
- Steals Christmas wishes
- Dumps to /tmp/dump.txt
- Deletes wishlist.txt
- Replaces with eastmas.txt

### Switch to Root

```bash
sudo su                # Switch to root
whoami                 # Verify current user
```

### Check Bash History

```bash
cd /root
cat .bash_history
```

**Flag found:** THM{until-we-meet-again}

Evidence of data exfiltration to hopsec.thm!

---

## Questions

**Q1: CLI command to list directory?**

- Answer: `ls`

---

**Q2: Flag inside McSkidy's guide?**

- Answer: `THM{learning-linux-cli}`

---

**Q3: Command to filter logs for failed logins?**

- Answer: `grep`

---

**Q4: Flag inside Eggstrike script?**

- Answer: `THM{sir-carrotbane-attacks}`

---

**Q5: Command to switch to root user?**

- Answer: `sudo su`

---

**Q6: Flag in root bash history?**

- Answer: `THM{until-we-meet-again}`

---

## Quick Reference

### Essential Linux Commands

**Navigation:**
```bash
pwd         # Print working directory
cd          # Change directory
ls          # List files
ls -la      # List all (including hidden)
```

**Reading Files:**
```bash
cat         # Display file content
grep        # Search text in files
```

**Finding Files:**
```bash
find        # Search for files
find /path -name *keyword*
```

**User Management:**
```bash
whoami      # Current user
sudo su     # Switch to root
exit        # Return to previous user
```

**History:**
```bash
history     # Show command history
cat .bash_history    # Read history file
```

### Special Symbols

**Pipe (|):**
```bash
cat file.txt | grep "keyword"
```
Send output to next command.

**Redirect (>):**
```bash
command > output.txt
```
Overwrite file with output.

**Append (>>):**
```bash
command >> output.txt
```
Append to file.

**And (&&):**
```bash
command1 && command2
```
Run second if first succeeds.

### Hidden Files

**Characteristics:**
- Start with `.` (dot)
- Not shown by default `ls`
- Use `ls -a` to reveal
- Common: .bashrc, .bash_history

**Examples:**
- .guide.txt
- .bash_history
- .secret_file

### Important Directories

**/home/user:**
- User home directory
- Personal files

**/var/log:**
- System logs
- Security events
- Authentication logs

**/etc:**
- System configuration
- /etc/shadow (passwords)

**/tmp:**
- Temporary files
- Often used by attackers

**/root:**
- Root user home
- Requires elevated privileges

### Log Analysis

**Auth logs:**
```bash
cd /var/log
grep "Failed password" auth.log
```

**Common patterns:**
- Failed logins
- Successful authentications
- User changes
- Sudo commands

### File Permissions

**Format:** drwxrwxr-x
- d = directory
- rwx = read, write, execute
- Three groups: owner, group, others

**Example:**
```
-rw-rw-r-- 1 mcskidy mcskidy 504 .guide.txt
```

### Root User

**Why root matters:**
- Ultimate system privileges
- Can read/write anything
- Main attacker target
- Use carefully!

**Switch to root:**
```bash
sudo su
```

**Return to normal user:**
```bash
exit
```

### Bash History

**Location:**
- /home/user/.bash_history
- /root/.bash_history

**View history:**
```bash
history
cat ~/.bash_history
```

**Attacker traces:**
- Commands they ran
- Files they accessed
- Data exfiltration
- Malicious actions

---

## Key Takeaways

- **ls** lists files
- **ls -la** shows hidden files
- **cat** reads file content
- **grep** searches text
- **find** locates files
- **Hidden files** start with `.`
- **/var/log** contains logs
- **grep** filters large files
- **Pipe (|)** chains commands
- **sudo su** switches to root
- **.bash_history** tracks commands
- **Always check** logs for attacks
- **Sir Carrotbane** HopSec attacker

Happy hunting
