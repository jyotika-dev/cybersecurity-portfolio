# Network Security Essentials - TryHackMe

> Learn network security fundamentals and perimeter defense

**Room Link:** https://tryhackme.com/room/networksecurityessentials

<img width="1462" height="926" alt="image" src="https://github.com/user-attachments/assets/1e3b8b80-d031-4950-b265-2ad2d8859497" />


---

## Task 1: Introduction

Learn network security from defensive perspective.

**Topics:**
- Network components
- Network perimeter
- Perimeter threats
- Firewall log analysis

**No answer needed**

---

## Task 2: Lab Connection

**Files location:** `Perimeter_logs` folder on Desktop

**Splunk:** Access at `MACHINE_IP:8000`

**No answer needed**

---

## Task 3: Network Components

### Key Components

**User Workstations:**
- Common entry point for attacks
- Phishing targets
- Endpoint logs reveal malicious processes

**File & Database Servers:**
- Store business data
- Ransomware targets
- Monitor for exfiltration

**Application Servers:**
- Web, Email, VPN
- Externally facing = high-value targets
- Monitor for exploit attempts

**Active Directory:**
- Identity backbone
- Controls all user accounts
- Target for privilege escalation

**Routers & Switches:**
- Network infrastructure
- Intercept traffic if compromised
- Create backdoors

**Firewalls:**
- Primary security gateway
- Filter traffic
- Log connection attempts

**No answer needed**

---

## Task 4: Network Visibility

### Two Log Types

**Host-Centric Logs:**
- OS logs (Windows Event, syslog)
- Application logs
- EDR/antivirus logs
- What happens ON a device

**Network-Centric Logs:**
- Firewall logs
- IDS/IPS logs
- Router flow data
- Web proxy logs
- VPN logs
- What happens BETWEEN devices

**Key principle:** Combine both for complete picture.

**No answer needed**

---

## Task 5: Network Perimeter

### What is Perimeter?

Boundary between trusted internal network and untrusted Internet.

**Components:**
- Firewalls
- Routers/Gateways
- DMZ (public-facing servers)
- VPN gateways

### Why It Matters

**First line of defense:**
- Attackers probe from outside
- Entry point for threats
- Early detection opportunity

**Weaknesses lead to:**
- Exposed services exploited
- Network reconnaissance
- Brute-force attacks
- Data exfiltration

**No answer needed**

---

## Task 6: Monitoring Perimeter

**Files:** `/Perimeter_logs/task6/`

### Scenario 1: Port Scanning

**Firewall log pattern:**
- Same external IP
- Multiple ports
- Rapid succession

**Detection:** Repeated connections to different ports.

**Questions:**

**Q1: Which IP is performing port scan?**

Look for multiple BLOCK entries from same IP.

- Answer: `203.0.113.10`

---

### Scenario 2: Web Attacks

**WAF log indicators:**
- XSS attempts
- SQL injection
- Directory traversal

**Filter:** `action=BLOCK`

**Q2: Which IP responsible for blocked web attacks?**

Check WAF logs for attack_type fields.

- Answer: `198.51.100.12`

---

### Scenario 3: VPN Brute-Force

**VPN log pattern:**
- Multiple FAILED_AUTH
- Same source IP
- Different usernames

**Q3: How many failed brute-force attempts?**

Count FAILED_AUTH from suspicious IP.

- Answer: `90`

---

**Q4: Which IP attempted VPN brute-force?**

Group by source IP, find highest failures.

- Answer: `45.137.22.13`

---

## Task 7: Breach Investigation

**Scenario:** Initech Corp incident - one month of logs

**Files:** `Perimeter_logs/challenge/`
- firewall.log
- ids_alerts.log
- vpn_auth.log

### Investigation Steps

**1. Reconnaissance:**
```bash
cat firewall.log | grep "BLOCK" | cut -d' ' -f5 | cut -d: -f1 | sort | uniq -c
```

**Q1: External IP with most reconnaissance?**

Count BLOCK entries per IP.

- Answer: `203.0.113.45`

---

**Q2: Which internal host targeted by scans?**

Check BLOCK entries for destination IPs.

Look for ports 21, 22, 23, 445, 3389.

- Answer: `10.0.0.20`

---

**2. Credential Access:**
```bash
cat vpn_auth.log | grep FAIL | cut -d' ' -f3 | sort | uniq -c
```

**Q3: Username targeted in VPN logs?**

Look for multiple failed attempts against same user.

- Answer: `svc_backup`

---

**Q4: Internal IP assigned after successful login?**

Find SUCCESS entry after failed attempts.

Check assigned_ip field.

- Answer: `10.8.0.23`

---

**3. Lateral Movement:**
```bash
cat ids_alerts.log | grep "SMB"
```

**Q5: Which port for lateral SMB attempts?**

Check IDS alerts for SMB exploitation.

- Answer: `445`

---

**4. C2 Communication:**
```bash
cat ids_alerts.log | grep "C2"
```

**Q6: Which host beaconed to C2?**

Look for "Possible C2 Beaconing" alerts.

Check source IP in alerts.

- Answer: `10.0.0.60`

---

**Q7: Which IP associated with C2?**

Check destination IP in C2 beaconing alerts.

External IP receiving beacons.

- Answer: `198.51.100.77`

---

**5. Data Exfiltration:**
```bash
cat ids_alerts.log | grep "HTTP POST Large"
```

**Q8: Which host showed exfiltration attempts?**

Look for "Possible HTTP POST Large Upload" alerts.

Check source IP.

- Answer: `10.0.0.51`

---

## Task 8: Conclusion

**No answer needed**

---

## Quick Reference

### Attack Timeline

```
1. Reconnaissance → Port scanning blocked IPs
2. Initial Access → VPN brute-force success
3. Lateral Movement → SMB exploitation
4. C2 Communication → Regular beaconing
5. Data Exfiltration → Large POST uploads
```

### Attack Patterns

**Port Scanning:**
- Same source IP
- Multiple destination ports
- Rapid succession

**Brute-Force:**
- Multiple failures
- Same destination
- Different credentials

**C2 Beaconing:**
- Regular intervals
- Same destination IP/port
- Persistent connections

**Data Exfiltration:**
- Large outbound transfers
- HTTP POST uploads
- Unusual destinations

### Common Ports

- 21 - FTP
- 22 - SSH
- 23 - Telnet
- 25 - SMTP
- 53 - DNS
- 80 - HTTP
- 443 - HTTPS
- 445 - SMB
- 3389 - RDP
- 4444 - Common C2 port
- 8080 - Alternative HTTP

### Detection Keywords

**IDS Alert Types:**
- ET SCAN - Scanning activity
- ET EXPLOIT - Exploitation attempts
- ET TROJAN - Trojan/malware
- ET POLICY - Policy violations
- ET INFO - Informational
  
---

## Key Takeaways

- **Network perimeter** = boundary between trusted/untrusted
- **Host-centric logs** = what happens ON devices
- **Network-centric logs** = what happens BETWEEN devices
- **Combine both** for complete picture
- **Port scanning** = same IP, multiple ports
- **VPN brute-force** = multiple failed logins
- **Lateral movement** via SMB (port 445)
- **C2 beaconing** = regular external connections
- **Data exfiltration** = large POST uploads
- **203.0.113.45** performed most reconnaissance
- **10.0.0.20** targeted by scans
- **svc_backup** account brute-forced
- **10.8.0.23** assigned after successful VPN login
- **10.0.0.60** beaconed to C2
- **198.51.100.77** is C2 server
- **10.0.0.51** exfiltrated data

Monitor the perimeter to catch attacks early!
Happy defending
