# Nmap: The Basics - TryHackMe

> Learn how to use Nmap to discover live hosts, find open ports, and detect service versions

**Room Link:** https://tryhackme.com/room/nmap

<img width="1556" height="926" alt="image" src="https://github.com/user-attachments/assets/7574fbd0-6ed5-4dbf-a603-9513da81fa40" />


---

## Introduction

Nmap is an open-source network scanner first released in 1997. It's powerful and flexible for discovering:
- Live hosts on a network
- Open ports on those hosts
- Service versions running

**Better than:** Manual scanning with ping, telnet, or scripts - way too slow!

---

## Task 2: Host Discovery - Finding Who is Online

### Target Specification

**IP range:**
```bash
nmap 192.168.0.1-10
```

**IP subnet (CIDR):**
```bash
nmap 192.168.0.1/24
```

**Hostname:**
```bash
nmap example.com
```

### Ping Scan (-sn)

Discovers online hosts without port scanning.

**Local network scan:**
```bash
nmap -sn 192.168.66.0/24
```
- Uses ARP requests for local networks
- Fast and reliable

**Remote network scan:**
```bash
nmap -sn 192.168.11.0/24
```
- Uses ICMP echo (ping), SYN, and ACK packets
- Goes through routers

### List Scan (-sL)

Lists targets without actually scanning:
```bash
nmap -sL 192.168.0.1/24
```
- Useful to verify targets before scanning

### Questions

**What is the last IP address scanned when your target is 192.168.0.1/27?**
```bash
nmap -sL 192.168.0.1/27
```
- Answer: `192.168.0.31`

---

## Task 3: Port Scanning - Finding Open Services

### TCP Connect Scan (-sT)

Complete three-way handshake with target:
```bash
nmap -sT 192.168.1.1
```
- More detectable
- Works without sudo

### SYN Scan (-sS) - Stealth Scan

Only sends SYN packet, doesn't complete handshake:
```bash
nmap -sS 192.168.1.1
```
- Faster and stealthier
- Less likely to be logged
- **Requires sudo**

### UDP Scan (-sU)

Scans UDP services (like DNS):
```bash
nmap -sU 192.168.1.1
```
- Slower than TCP
- Important for services like DNS (port 53)

### Port Range Options

**Fast mode (100 most common ports):**
```bash
nmap -F 192.168.1.1
```

**Specific port range:**
```bash
nmap -p 10-1024 192.168.1.1
nmap -p-25 192.168.1.1          # Ports 1-25
nmap -p- 192.168.1.1            # ALL ports (1-65535)
```

### Questions

**How many TCP ports are open on 10.10.100.85?**
```bash
nmap -sT 10.10.100.85
```
- Answer: `6`

**Find the web server on 10.10.100.85 and access it. What is the flag?**
```
Port 8008 is open
Browse to: http://10.10.100.85:8008/
```
- Answer: `THM{SECRET_PAGE_38B9P6}`

---

## Task 4: Version Detection - Extract More Information

### OS Detection (-O)

Guess the operating system:
```bash
nmap -O 192.168.1.1
```
- Compares responses to known fingerprints
- Not 100% accurate but close

### Service Version Detection (-sV)

Detect service and version:
```bash
nmap -sV 192.168.1.1
```
- Shows exact software version
- Example: "OpenSSH 8.9p1"

### Aggressive Scan (-A)

Combines multiple scans:
```bash
nmap -A 192.168.1.1
```
- OS detection
- Version detection
- Traceroute
- More features

### Force Scan (-Pn)

Scan hosts even if they appear down:
```bash
nmap -Pn 192.168.1.1
```
- Skips host discovery
- Scans all targets

### Questions

**What is the name and version of the web server on 10.10.100.85?**
```bash
nmap -sV 10.10.100.85
```
- Answer: `lighttpd 1.4.74`

---

## Task 5: Timing - How Fast is Fast

### Timing Templates (T0-T5)

Control scan speed:

| Template | Name | Speed | Use Case |
|----------|------|-------|----------|
| `-T0` | Paranoid | Very slow | Evade IDS |
| `-T1` | Sneaky | Slow | Evade IDS |
| `-T2` | Polite | Slow | Less bandwidth |
| `-T3` | Normal | Default | Balanced |
| `-T4` | Aggressive | Fast | Fast & reliable network |
| `-T5` | Insane | Fastest | Very fast network |

**Example:**
```bash
nmap -sS -T4 192.168.1.1
```

### Timing Differences

- **T0:** 5 minutes between ports
- **T1:** 15 seconds between ports
- **T2:** 0.4 seconds between ports
- **T3:** As fast as possible (default)
- **T4:** Faster (aggressive)

### Other Timing Options

**Parallel probes:**
```bash
nmap --min-parallelism 10 --max-parallelism 100 192.168.1.1
```

**Packet rate:**
```bash
nmap --min-rate 50 --max-rate 100 192.168.1.1
```

**Host timeout:**
```bash
nmap --host-timeout 5m 192.168.1.1
```

### Questions

**What is the non-numeric equivalent of -T4?**
- Answer: `-T aggressive`

---

## Task 6: Output - Controlling What You See

### Verbosity

**Basic verbose:**
```bash
nmap -v 192.168.1.1
```

**More verbose:**
```bash
nmap -vv 192.168.1.1
nmap -v2 192.168.1.1        # Same as -vv
```

**Debugging:**
```bash
nmap -d 192.168.1.1
nmap -d9 192.168.1.1        # Maximum debug level
```

### Save Scan Results

**Normal output (human-readable):**
```bash
nmap -oN scan.txt 192.168.1.1
```

**XML output:**
```bash
nmap -oX scan.xml 192.168.1.1
```

**Grepable output:**
```bash
nmap -oG scan.gnmap 192.168.1.1
```

**All formats:**
```bash
nmap -oA scan 192.168.1.1
```
Creates three files:
- scan.nmap (normal)
- scan.xml (XML)
- scan.gnmap (grepable)

### Questions

**What option enables debugging?**
- Answer: `-d`

---

## Task 7: Conclusion and Summary

### Key Points

- **Always use sudo** for full Nmap features
- Without sudo: Limited to connect scan (-sT)
- With sudo: Can use SYN scan (-sS) and more

### Scan Types

| Scan | Option | Sudo? | Speed |
|------|--------|-------|-------|
| Connect | -sT | No | Slower |
| SYN | -sS | Yes | Faster |
| UDP | -sU | Yes | Slowest |

### Questions

**What scan will Nmap use if you run `nmap 10.10.100.85` with local user privileges?**
- Answer: `Connect Scan`

---

## Quick Reference

### Common Commands

| Command | Purpose |
|---------|---------|
| `nmap -sn 192.168.1.0/24` | Find live hosts |
| `nmap -sT 192.168.1.1` | TCP connect scan |
| `nmap -sS 192.168.1.1` | SYN scan (stealth) |
| `nmap -sU 192.168.1.1` | UDP scan |
| `nmap -sV 192.168.1.1` | Version detection |
| `nmap -O 192.168.1.1` | OS detection |
| `nmap -A 192.168.1.1` | Aggressive (OS + version + more) |
| `nmap -p- 192.168.1.1` | Scan all ports |
| `nmap -F 192.168.1.1` | Fast (100 common ports) |
| `nmap -T4 192.168.1.1` | Aggressive timing |

### Essential Options

| Option | Purpose |
|--------|---------|
| `-sn` | Ping scan (no port scan) |
| `-sT` | TCP connect scan |
| `-sS` | SYN scan (stealth) |
| `-sU` | UDP scan |
| `-sV` | Version detection |
| `-O` | OS detection |
| `-A` | Aggressive (everything) |
| `-Pn` | Skip host discovery |
| `-F` | Fast mode (100 ports) |
| `-p-` | All ports |
| `-T0` to `-T5` | Timing (0=slowest, 5=fastest) |
| `-v` | Verbose |
| `-d` | Debug |
| `-oA` | Save all formats |

### Pro Tips

```bash
# Basic network discovery
sudo nmap -sn 192.168.1.0/24

# Quick TCP scan
sudo nmap -sS -F 192.168.1.1

# Thorough scan with version detection
sudo nmap -sS -sV -p- 192.168.1.1

# Aggressive scan with all features
sudo nmap -A -T4 192.168.1.1

# Save results in all formats
sudo nmap -A -oA myscan 192.168.1.1
```

---

## Key Takeaways

- **Nmap** is the go-to tool for network scanning
- **-sn** finds live hosts without port scanning
- **-sS** is faster and stealthier than -sT
- **-sV** detects service versions
- **-A** combines multiple detection methods
- **Timing** matters - use T4 for fast scans
- **Always use sudo** for full functionality
- **Save your scans** with -oA for later analysis

Nmap is essential for network discovery and security testing! 🔍
