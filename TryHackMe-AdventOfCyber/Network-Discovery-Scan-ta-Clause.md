# Advent of Cyber 2025 Day 7 - TryHackMe

> Network Discovery - Scan-ta Clause

**Day 7 Link:** https://tryhackme.com/room/networkservices-aoc2025-jnsoqbxgky

---

## Story

**Scenario:** HopSec breached TBFC's QA environment and locked everyone out

**Target:** 10.67.173.8 (tbfc-devqa01 server)

**Goal:** Find open ports, discover hidden keys, regain access

---

## Network Discovery Basics

**Port Scanning:** Finding which ports are open and what services are running

**Why it matters:**
- Identify attack surface
- Find vulnerabilities
- Map the network

**Tools needed:**
- nmap (port scanner)
- ftp (file transfer)
- nc (netcat)
- dig (DNS queries)

---

## Phase 1: Basic Scan

### Scan Common Ports

```bash
nmap 10.67.173.8
```

**Results:**
```
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
```

### Check Website

Visit http://10.67.173.8 in browser

**Q1: What message is displayed on defaced website?**
- Answer: `Pwned by HopSec`

---

## Phase 2: Full Port Scan

### Scan All Ports

```bash
nmap -p- --script=banner 10.67.173.8
```

**Flags:**
- `-p-` = scan all 65535 ports
- `--script=banner` = grab service info

**New discoveries:**
```
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
21212/tcp open  trinket-agent (FTP)
25251/tcp open  unknown (TBFC app)
```

---

## Phase 3: FTP Enumeration (Key 1)

### Connect to FTP

```bash
ftp 10.67.173.8 21212
```

**Login:**
- Name: `anonymous`
- Password: [just press Enter]

### Get First Key

```bash
ftp> ls
ftp> get tbfc_qa_key1 -
```

**Q2: First key found?**
- Answer: `3aster_`

Exit:
```bash
ftp> !
```

---

## Phase 4: Custom Service (Key 2)

### Connect with Netcat

```bash
nc -v 10.67.173.8 25251
```

### Enumerate Commands

```
HELP
```

**Available commands:**
- HELP
- STATUS
- GET KEY
- QUIT

### Get Second Key

```
GET KEY
```

**Q3: Second key found?**
- Answer: `15_th3_`

Exit: Press `CTRL+C`

---

## Phase 5: UDP Scan (Key 3)

### Scan UDP Ports

```bash
nmap -sU 10.67.173.8
```

**Flag:**
- `-sU` = UDP scan

**Result:**
```
PORT   STATE SERVICE
53/udp open  domain
```

### Query DNS

```bash
dig @10.67.173.8 TXT key3.tbfc.local +short
```

**Q4: Third key found?**
- Answer: `n3w_xm45`

---

## Phase 6: Access Admin Panel

### Combine Keys

**Full key:** `3aster_15_th3_n3w_xm45`

**Access:**
1. Visit http://10.67.173.8
2. Find admin login
3. Enter combined key
4. Access granted!

---

## Phase 7: Internal Port Discovery

### List Open Ports

```bash
ss -tunlp
```

**Flags:**
- `-t` = TCP
- `-u` = UDP
- `-n` = numeric
- `-l` = listening only
- `-p` = show process

**Found:**
```
127.0.0.1:3306    # MySQL (local only)
127.0.0.1:8000    # Unknown
127.0.0.1:7681    # Unknown
```

**Q5: Which port is MySQL running on?**
- Answer: `3306`

---

## Phase 8: Database Access

### List Tables

```bash
mysql -D tbfcqa01 -e "show tables;"
```

**Result:**
```
+--------------------+
| Tables_in_tbfcqa01 |
+--------------------+
| flags              |
+--------------------+
```

### Get Final Flag

```bash
mysql -D tbfcqa01 -e "select * from flags;"
```

**Q6: Final flag?**
- Answer: `THM{4ll_s3rvice5_d1sc0vered}`

---

## Quick Reference

### Common Ports

| Port | Service | Purpose |
|------|---------|---------|
| 22   | SSH     | Remote access |
| 53   | DNS     | Domain lookup |
| 80   | HTTP    | Web traffic |
| 443  | HTTPS   | Secure web |
| 21   | FTP     | File transfer |
| 3306 | MySQL   | Database |

### Nmap Cheat Sheet

**Basic scans:**
```bash
nmap 10.67.173.8              # Quick scan
nmap -p- 10.67.173.8          # All TCP ports
nmap -sU 10.67.173.8          # UDP scan
nmap -p 22,80,443 10.67.173.8 # Specific ports
```

**Enhanced scans:**
```bash
nmap -sV 10.67.173.8          # Version detection
nmap --script=banner 10.67.173.8  # Banner grabbing
nmap -A 10.67.173.8           # Aggressive scan
```

### Tool Usage

**FTP:**
```bash
ftp IP PORT
# Login: anonymous / [blank]
ls                  # list files
get filename -      # download & display
!                   # exit
```

**Netcat:**
```bash
nc -v IP PORT       # connect to service
# -v = verbose
```

**DNS:**
```bash
dig @IP TXT domain.local +short
# @IP = specific DNS server
# TXT = text records
# +short = brief output
```

**MySQL:**
```bash
mysql -D database -e "query"
# -D = database name
# -e = execute command
```

---

## Security Lessons

### Vulnerabilities Found

**Anonymous FTP:**
- No password required
- Anyone can access files

**Unauthenticated service:**
- Custom app had no login
- Exposed sensitive data

**DNS info leak:**
- TXT records contained keys
- Public information

**Local MySQL:**
- No password for localhost
- Trusted by default

### Defense Tips

**Port security:**
- Close unnecessary ports
- Use firewall rules
- Bind to localhost when possible

**Service hardening:**
- Require authentication
- Use encryption
- Regular updates

**Best practices:**
- Disable anonymous access
- Remove sensitive DNS records
- Password-protect all services
- Monitor for unusual activity

---

## Attack Flow

```
1. Basic Scan (nmap)
    ↓
2. Find HTTP (port 80) → Defaced site
    ↓
3. Full Scan (nmap -p-)
    ↓
4. FTP (21212) → Key 1
    ↓
5. Custom App (25251) → Key 2
    ↓
6. DNS (53 UDP) → Key 3
    ↓
7. Combine Keys → Admin Access
    ↓
8. Internal Scan (ss -tunlp)
    ↓
9. MySQL (3306) → Final Flag
```

---

## Key Takeaways

- **Scan all ports** (not just top 1000)
- **Check both TCP and UDP**
- **Try anonymous login** on FTP
- **Use netcat** for custom services
- **DNS can leak** information
- **Internal services** often trust localhost
- **Misconfigurations** = easy access
- **Document everything** you find
- **Security through obscurity** doesn't work
- **Always audit** your exposed services

---

Happy hunting
