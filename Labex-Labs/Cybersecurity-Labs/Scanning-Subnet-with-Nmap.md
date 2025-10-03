# Nmap Network Scanning Challenge

This document summarizes my completion of the Nmap network scanning challenge, where I applied my knowledge to scan localhost addresses and identify active services independently.

**Challenge Type:** Independent Challenge

---

## Challenge Overview

### Objective
Perform network scans on localhost addresses using Nmap and analyze the results to identify active hosts and services.

### Tasks
- Scan first 10 localhost IPs (127.0.0.1 to 127.0.0.10)
- Identify number of active hosts
- Find hosts running SSH service
- Save results to file

---

## Solution

### Challenge Requirements

**Working Directory:**
```bash
cd ~/project
```

**Scan Command:**
```bash
nmap 127.0.0.1-10 -oN scan_results.txt
```

### Command Breakdown

| Component | Description |
|-----------|-------------|
| `nmap` | Network scanning tool |
| `127.0.0.1-10` | Scan range (10 localhost addresses) |
| `-oN scan_results.txt` | Save output to file in normal format |

---

## Results Analysis

### Active Hosts Found

**Total hosts scanned:** 10  
**Active hosts:** 2  
**Scan time:** 1.25 seconds

### Host Details

**Host 1: 127.0.0.1 (localhost)**
- Status: Up
- Latency: 0.00021s
- Open ports: 2
  - Port 22/tcp - SSH (Secure Shell)
  - Port 80/tcp - HTTP (Web Server)

**Host 2: 127.0.0.5**
- Status: Up
- Latency: 0.00025s
- Open ports: 1
  - Port 22/tcp - SSH (Secure Shell)

### SSH Service Location

**Hosts running SSH:**
- 127.0.0.1 (port 22)
- 127.0.0.5 (port 22)

---

## Key Findings

### Services Identified

| Service | Port | Hosts |
|---------|------|-------|
| SSH | 22/tcp | 127.0.0.1, 127.0.0.5 |
| HTTP | 80/tcp | 127.0.0.1 |

### Network Status

- 8 out of 10 IPs showed no response
- Both active hosts have SSH enabled
- Only localhost (127.0.0.1) runs a web server

---

## Challenge Completion

### Tasks Completed

✅ Scanned localhost range (127.0.0.1 to 127.0.0.10)  
✅ Identified 2 active hosts  
✅ Found hosts with SSH service  
✅ Saved results to `scan_results.txt`  
✅ Executed from `~/project` directory  

### Skills Demonstrated

- Independent problem-solving
- Nmap command syntax
- IP range specification
- Output file management
- Scan result interpretation
- Service identification

---

## Commands Used

### Complete Solution

```bash
# Navigate to working directory
cd ~/project

# Perform scan and save results
nmap 127.0.0.1-10 -oN scan_results.txt

# Verify results
cat scan_results.txt
```

---

## Key Takeaways

### Localhost Scanning
- Localhost (127.0.0.0/8) is used for local services
- Multiple IPs in 127.x.x.x range can be configured
- Useful for testing and development

### Nmap Basics
- Range scanning: `IP1-IP2` format
- Output saving: `-oN filename`
- Service detection happens automatically

### Security Insights
- SSH on localhost allows secure local connections
- HTTP service indicates web server running
- Closed ports mean no service listening

---

## Real-World Applications

### Local System Auditing
- Verify expected services are running
- Identify unexpected services
- Check for security misconfigurations

### Development & Testing
- Confirm application ports are open
- Test service availability
- Troubleshoot connection issues

### Security Practice
- Safe environment for learning
- No network impact
- Practice before production scanning

---

## Important Notes

### Why Localhost Scanning?

**Safe Learning Environment:**
- No risk to external networks
- No authorization needed
- Perfect for practice

**Practical Uses:**
- System administration
- Service verification
- Troubleshooting
- Security testing

### Ethical Reminder

⚠️ **Always get authorization before scanning any network other than your own localhost or systems you own.**

---

## Lab Completion Summary

**Challenge completed successfully!**

Successfully scanned localhost range, identified active hosts, located SSH services, and properly documented results. This challenge reinforced independent problem-solving skills and practical Nmap usage.
