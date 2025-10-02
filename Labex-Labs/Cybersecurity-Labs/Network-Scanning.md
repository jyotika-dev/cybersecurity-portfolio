# Nmap Network Scanning Lab

This document summarizes my hands-on learning from the network scanning lab using Nmap, focusing on network reconnaissance and security assessment fundamentals.

**Lab Focus:** Learning to identify active hosts, open ports, and running services using Nmap for security assessments.

---

## Lab Overview

### Objectives
- Install and configure Nmap
- Perform basic network scans
- Interpret scan results
- Understand different scanning techniques
- Practice ethical scanning on authorized networks

---

## Part 1: Installing Nmap

### Installation Commands (Pro Users)

```bash
sudo apt-get update
sudo apt-get install nmap -y
```

**Note:** Nmap was pre-installed in the lab environment for free users.

### Verify Installation

```bash
nmap --version

```

---

## Part 2: Understanding IP Addresses

### Finding Your IP Address

```bash
ip addr show | grep inet
```

**Output:**
```
inet 127.0.0.1/8 scope host lo
inet 172.19.0.3/16 brd 172.19.255.255 scope global eth1
```

### IP Address Types

| Address | Type | Description |
|---------|------|-------------|
| `127.0.0.1` | Loopback | Always refers to your own computer |
| `172.19.0.3` | Network IP | Actual network address for communication |

### Understanding CIDR Notation

**Format:** `IP_ADDRESS/PREFIX_LENGTH`

**Examples:**
- `/8` - First 8 bits identify network (255.0.0.0)
- `/16` - First 16 bits identify network (255.255.0.0)
- `/24` - First 24 bits identify network (255.255.255.0)

**Network Diagram:**
```
Internet
    |
Router (192.168.1.1)
    |
    ├── PC (192.168.1.10)
    ├── Laptop (192.168.1.11)
    ├── Smartphone (192.168.1.12)
    └── Smart TV (192.168.1.13)
```

---

## Part 3: Performing a Basic Nmap Scan

### Basic Scan Command

```bash
nmap <YOUR_IP>
```

**Example:**
```bash
nmap 172.19.0.3
```


### Understanding the Output

| Component | Description |
|-----------|-------------|
| `Host is up (0.00017s latency)` | Target responded to requests |
| `Not shown: 998 closed ports` | 998 of 1000 ports scanned were closed |
| `22/tcp open ssh` | Port 22 running SSH service |
| `3001/tcp open nessus` | Port 3001 running Nessus |

### What This Scan Does

- Scans the most common 1,000 TCP ports
- Identifies which ports are open
- Determines what services are running
- Provides response time (latency)

---

## Part 4: OS Detection Scan

### OS Detection Command

```bash
sudo nmap -O <YOUR_IP>
```

**Example:**
```bash
sudo nmap -O 172.19.0.3
```

**Note:** Requires root privileges (`sudo`) to send raw packets.

### OS Detection Information

| Field | Description |
|-------|-------------|
| `Device type` | Type of device (general purpose, router, etc.) |
| `Running` | Operating system family and version |
| `OS CPE` | Standardized OS identifier |
| `OS details` | Specific version based on fingerprint |
| `Network Distance` | Number of hops to target (0 = same machine) |

### How OS Detection Works

1. Nmap sends specially crafted packets
2. Analyzes response patterns
3. Matches against database of OS fingerprints
4. Each OS responds differently, creating unique "fingerprint"

### Factors Affecting Accuracy

- Firewalls blocking probes
- Custom kernel configurations
- Virtual machines
- Network security devices

---

## Part 5: Scanning Network Ranges

### Scanning IP Range

**Scan consecutive IPs:**
```bash
nmap 172.19.0.1-20
```

**Scans:** 172.19.0.1 through 172.19.0.20 (20 addresses)

### Scanning Entire Subnet

```bash
nmap 172.19.0.0/24
```
### Saving Scan Results

```bash
nmap 172.19.0.0/24 -oN network_scan.txt
```

**Flags:**
- `-oN` - Output in normal (human-readable) format
- Creates file `network_scan.txt` in current directory

### Viewing Results

```bash
cat network_scan.txt
```

### Scan Results Analysis

**Summary:**
- **Total IPs scanned:** 256 (entire /24 subnet)
- **Hosts up:** 5 devices responded
- **Scan time:** 3.25 seconds
- **Common services found:** SSH (22), HTTP (80), Nessus (3001)

**Per-Host Information:**
- Hostname (if resolvable)
- IP address
- Network latency
- Open ports
- Associated services

---

## Nmap Command Reference

### Basic Commands

| Command | Description |
|---------|-------------|
| `nmap <IP>` | Basic scan of target IP |
| `nmap -O <IP>` | OS detection scan |
| `nmap <IP1-IP2>` | Scan IP range |
| `nmap <NETWORK>/24` | Scan entire subnet |
| `nmap -oN <file> <IP>` | Save results to file |
| `nmap --version` | Check Nmap version |
| `nmap -h` | Display help menu |
| `man nmap` | View manual page |

### Common Scan Types

| Flag | Purpose |
|------|---------|
| `-sS` | TCP SYN scan (stealth) |
| `-sT` | TCP connect scan |
| `-sU` | UDP scan |
| `-sV` | Version detection |
| `-A` | Aggressive scan (OS, version, scripts) |
| `-p` | Specify ports (e.g., `-p 80,443`) |
| `-v` | Verbose output |
| `-T<0-5>` | Timing template (0=slowest, 5=fastest) |

---

## Practical Applications

### Network Inventory
- Maintain accurate list of network devices
- Track authorized equipment
- Identify rogue devices

### Security Auditing
- Discover unnecessary open ports
- Identify vulnerable services
- Verify firewall configurations

### Troubleshooting
- Verify network connectivity
- Check service availability
- Diagnose connection issues

### Vulnerability Assessment
- Identify outdated services
- Detect unpatched systems
- Map attack surface

---

## Key Takeaways

### Skills Acquired

✅ Nmap installation and verification  
✅ IP address identification and CIDR notation  
✅ Basic port scanning techniques  
✅ OS detection methodology  
✅ Network range scanning  
✅ Scan result interpretation  
✅ Output management and documentation  

---

## Lab Completion Summary

**Environment:**
- Lab system with Nmap pre-installed
- Local network simulation
- Multiple hosts for scanning practice

**Skills Demonstrated:**
- Network reconnaissance
- Port scanning
- OS detection
- Range scanning
- Result documentation

**Commands Used:**
```bash
# Installation
sudo apt-get update && sudo apt-get install nmap -y

# IP Discovery
ip addr show | grep inet

# Basic Scanning
nmap 172.19.0.3

# OS Detection
sudo nmap -O 172.19.0.3

# Network Range
nmap 172.19.0.0/24 -oN network_scan.txt
```

---

**Disclaimer:** This lab was completed in an authorized environment for educational purposes only. Always obtain proper authorization before scanning any network.
