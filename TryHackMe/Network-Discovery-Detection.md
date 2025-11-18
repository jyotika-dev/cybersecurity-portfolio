# Network Discovery Detection - TryHackMe

> Detect network scanning and discovery activities in logs

**Room Link:** https://tryhackme.com/room/networkdiscoverydetection

<img width="1503" height="923" alt="image" src="https://github.com/user-attachments/assets/bdf9cc2b-af5d-4319-b677-197a2a050ec6" />


---

## Task 1: Introduction

**No answer needed**

---

## Task 2: What Attackers Scan

**Question:** What do attackers scan, other than IP addresses, ports, and OS versions, to identify vulnerabilities?

**Answer:** Services

**Why it matters:**
- Knowing port 80 is open isn't enough
- Need to know what service and version
- Example: Apache 2.2.15 vs nginx 1.14
- Service + version = attack surface

- Answer: `Services`

---

## Task 3: External vs Internal Scanning

**Files:** log-session-0.csv, log-session-1.csv, log-session-2.csv

### Questions

**Q1: Which file contains internal scanning activity?**

Check which file shows internal IP (192.168.x.x) as source.

- Answer: `log-session-2.csv`

---

**Q2: How many log entries for internal IP performing scanning?**

Same command output shows count.

**Internal scanner:** 192.168.230.127

- Answer: `2276`

---

**Q3: External IP performing scanning?**

**External IP:** 203.0.113.25 (appears ~2014 times)

- Answer: `203.0.113.25`

---

## Task 4: Horizontal vs Vertical Scanning

### Scan Types

**Horizontal Scan:**
- One source → Many destination IPs
- Same port(s)
- Looking for specific service

**Vertical Scan:**
- One source → One destination IP
- Many different ports
- Enumerating services

### Questions

**Q1: Which IP range was scanned (horizontal)?**

**Pattern found:** Many 203.0.113.* addresses
- 203.0.113.2, 203.0.113.3, ... 203.0.113.254
- Same /24 network

- Answer: `203.0.113.0/24`

---

**Q2: Which IP is target of vertical scan?**

**Output:**
- 192.168.230.127 → 192.168.230.145 (1001 ports)
- 192.168.230.127 → 192.168.230.1 (5 ports)

Most ports scanned = 192.168.230.145

- Answer: `192.168.230.145`

---

**Q3: Which ports scanned on host with common services?**

Host 192.168.230.1 scanned on few ports.

**Ports found:**
- 80 (HTTP)
- 445 (SMB)
- 3389 (RDP)

- Answer: `80, 445, 3389`

---

## Task 5: Scan Mechanics

**Access Kibana:**
- URL: http://localhost:5601
- Username: elastic
- Password: changeme

**Navigate:** Hamburger menu → Discover → All logs

### Questions

**Q1: Which source IP performs ping sweep?**

**Kibana query:**
```
network.protocol:"icmp"
```

**Ping sweep pattern:**
- Many ICMP echo requests
- One source → Many destinations
- Across whole subnet

**Scanner:** 192.168.230.127

- Answer: `192.168.230.127`

---

**Q2: What type of scan is 203.0.113.25 performing against 192.168.230.145?**

Check Zeek conn_state values.

**S0 state:**
- SYN sent
- No SYN/ACK reply
- Half-open scan

**Many S0 entries = TCP SYN scan**

- Answer: `TCP SYN scan`

---

**Q3: Is there UDP scanning attempt? (Y/N)**

**UDP scanning indicators:**
- Many UDP ports probed
- Multiple destinations
- ICMP port unreachable replies

**Evidence in logs:**
- Only multicast/discovery UDP (239.255.255.250:3702)
- No systematic UDP scanning pattern
- No multi-port UDP probes

- Answer: `N`

---

## Quick Reference

### Scan Types

| Type | Pattern | Example |
|------|---------|---------|
| **Horizontal** | 1 source → Many IPs, same port | Web server scan across /24 |
| **Vertical** | 1 source → 1 IP, many ports | Port sweep on single host |
| **Ping Sweep** | ICMP echo to many IPs | Find live hosts |


### Zeek conn_state Values

- **S0** - SYN sent, no reply (SYN scan)
- **SF** - Normal established connection
- **RSTO** - Connection reset by originator
- **RSTR** - Connection reset by responder

### Common Service Ports

- 80 - HTTP
- 443 - HTTPS
- 445 - SMB (file sharing)
- 3389 - RDP (remote desktop)
- 22 - SSH
- 3702 - WS-Discovery (SSDP)

### Attack Patterns

**Reconnaissance Phase:**
1. Ping sweep (find live hosts)
2. Horizontal scan (find services)
3. Vertical scan (enumerate ports)
4. Service detection (identify versions)

**Log Indicators:**

**Horizontal scan:**
- Same source IP
- Many destination IPs
- Same destination port(s)

**Vertical scan:**
- Same source IP
- Same destination IP
- Many destination ports

**Ping sweep:**
- ICMP type 8 (echo request)
- Many destination IPs
- Short time window

---

## Key Takeaways

- **Services** are scanned to find vulnerabilities
- **log-session-2.csv** contains internal scanning
- **192.168.230.127** is internal scanner (2276 entries)
- **203.0.113.25** is external scanner (2014 entries)
- **203.0.113.0/24** horizontally scanned
- **192.168.230.145** vertically scanned (1001 ports)
- **80, 445, 3389** common service ports scanned
- **Ping sweep** from 192.168.230.127
- **TCP SYN scan** uses S0 conn_state
- **No UDP scanning** detected in logs
- **Zeek logs** reveal scan patterns
- **conn_state S0** = SYN scan indicator
- **Cut and awk** powerful for log analysis

Detect scans early to stop reconnaissance
Happy hunting! 
