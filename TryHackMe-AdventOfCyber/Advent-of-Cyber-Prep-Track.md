# Advent of Cyber 2025 Prep Track - TryHackMe

> 10 beginner-friendly cyber security challenges to prep for Advent of Cyber 2025

**Track Link:** https://tryhackme.com/room/adventofcyberpreptrack

---

## Story

**Location:** Wareville (The Best Festival Company - TBFC)

**Event:** SOCMAS preparation

**Problem:** Systems glitching, passwords failing, drones misbehaving

**Threat:** King Malhare

**Mission:** Join SOCMAS Response Team by completing prep challenges

---

## Challenge 1: Password Pandemonium

### Scenario

Weak passwords detected on 73 TBFC accounts!

### Task

Create strong password:
- 12+ characters
- Uppercase + lowercase + numbers + symbols
- Not in leaked password list

**Q: What's the flag?**

- Answer: `THM{StrongStart}`

---

## Challenge 2: The Suspicious Chocolate.exe

### Scenario

Mysterious USB with chocolate.exe file.

### Task

Scan file in VirusTotal-style scanner.

**Results:**
- 49 clean
- 1 malicious

**Q: What's the flag?**

- Answer: `THM{NotSoSweet}`

---

## Challenge 3: Welcome to the AttackBox!

### Scenario

Enter AttackBox environment.

### Task

Find hidden welcome message.

**Commands:**
```bash
ls
cd challenges/
cat welcome.txt
```

**Q: What's the flag?**

- Answer: `THM{Ready2Hack}`

---

## Challenge 4: The CMD Conundrum

### Scenario

McSkidy's workstation tampered with.

Suspicious folder: mystery_data

### Task

Find hidden flag using Windows CMD.

**Commands:**
```cmd
dir
dir /a
type hidden_flag.txt
```

**Q: What's the flag?**

- Answer: `THM{WhereIsMcSkidy}`

---

## Challenge 5: Linux Lore

### Scenario

TBFC drones malfunctioning.

McSkidy's Linux account suspicious.

### Task

Find secret message in home directory.

**Commands:**
```bash
cd /home/mcskidy/
ls -la
cat .secret_message
```

**Q: What's the flag?**

- Answer: `THM{TrustNoBunny}`

---

## Challenge 6: The Leak in the List

### Scenario

TBFC data breach rumored.

### Task

Check if mcskidy@tbfc.com in breach database.

Use HaveIBeenPwned-style tool.

**Q: What's the flag?**

- Answer: `THM{LeakedAndFound}`

---

## Challenge 7: WiFi Woes in Wareville

### Scenario

TBFC router accessed with default credentials.

Drones malfunctioning.

### Task

Login and change password:
- Username: admin
- Password: admin

Create strong password.

**Q: What's the flag?**

- Answer: `THM{NoMoreDefault}`

---

## Challenge 8: The App Trap

### Scenario

McSkidy's social account posting bizarre messages.

Suspicious third-party app connected.

### Task

Review connected apps.

Remove malicious app with excessive permissions.

**Q: What's the flag?**

- Answer: `THM{AppTrapped}`

---

## Challenge 9: The Chatbot Confession

### Scenario

FestiveBot leaking internal secrets.

URLs and passwords exposed.

### Task

Identify messages with sensitive data.

**Q: What's the flag?**

- Answer: `THM{DontFeedTheBot}`

---

## Challenge 10: The Bunny's Browser Trail

### Scenario

Network logs show heavy traffic.

Suspicious user agent detected.

### Task

Find anomalous user agent:
```
BunnyOS/1.0 (HopSecBot)
```

Compare with legitimate browser agents.

**Q: What's the flag?**

- Answer: `THM{EastmasIsComing}`

---

## Quick Reference

### Skills Covered

**Challenge 1:** Password security

**Challenge 2:** Malware detection

**Challenge 3:** Linux basics

**Challenge 4:** Windows CMD

**Challenge 5:** Hidden files (Linux)

**Challenge 6:** Breach checking

**Challenge 7:** Router security

**Challenge 8:** App permissions

**Challenge 9:** AI data leakage

**Challenge 10:** Log analysis

### Key Commands

**Linux:**
```bash
ls          # List files
ls -la      # List all (including hidden)
cd          # Change directory
cat         # Read file
```

**Windows:**
```cmd
dir         # List files
dir /a      # Show hidden files
type        # Read file
```

### Security Concepts

**Password Security:**
- 12+ characters minimum
- Mix uppercase, lowercase, numbers, symbols
- Never use leaked passwords
- Avoid dictionary words

**Malware Detection:**
- Scan unknown files
- Trust evidence, not appearances
- Use multiple scanners

**Breach Checking:**
- Monitor email addresses
- Use HaveIBeenPwned
- Change passwords if breached

**Router Security:**
- Change default credentials
- Use strong passwords
- Disable unnecessary services

**App Permissions:**
- Review third-party apps
- Remove excessive permissions
- Regular audits

**AI Security:**
- Don't share sensitive data with chatbots
- Review AI outputs
- Understand data retention

**Log Analysis:**
- Check user agents
- Identify anomalies
- Compare with baselines

### Hidden Files

**Linux:**
- Prefixed with `.` (dot)
- Use `ls -la` to see them
- Common: .bashrc, .ssh

**Windows:**
- Use `dir /a` to reveal
- Hidden attribute set
- Often used by malware

### Attack Indicators

**Weak Passwords:**
- Short length
- Dictionary words
- No special characters

**Malicious Files:**
- Unexpected source
- Single scanner detection
- Suspicious behavior

**Default Credentials:**
- admin:admin
- root:root
- Never change these!

**Suspicious Apps:**
- Excessive permissions
- Unknown developers
- Recent installation

**Anomalous User Agents:**
- Custom/fake OS names
- Bot identifiers
- Unusual version strings

---

## Key Takeaways

- **Strong passwords** essential defense
- **Malware scanning** before execution
- **Linux commands** for investigations
- **Windows CMD** equally important
- **Hidden files** often contain clues
- **Breach checking** proactive defense
- **Default credentials** major risk
- **App permissions** require review
- **AI chatbots** can leak data
- **User agents** reveal anomalies
- **Log analysis** detects threats
- **Hands-on practice** builds skills

Ready for Advent of Cyber 2025!
