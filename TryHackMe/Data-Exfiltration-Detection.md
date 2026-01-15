# TryHackMe Data Exfiltration Detection - Walkthrough

> Detect Data Exfiltration in Network Traffic

**Room Link:** https://tryhackme.com/room/dataexfildetection

<img width="1474" height="899" alt="image" src="https://github.com/user-attachments/assets/cd4b6858-3558-43b3-9f41-d30951406c06" />

---

## Overview

**What is data exfiltration:**
- Moving data out of network
- Unauthorized transfer
- Various methods/channels
- Need to detect and stop

---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Lab Connection

**No answer needed**

---

## Task 3: Data Exfil Overview

### Q: Data exfil type using network protocols?

**Answer:** `Network-based`

---

## Task 4: DNS Tunneling Detection

**DNS tunneling:**
- Hides data in DNS queries
- Looks like normal DNS
- Unusual patterns reveal it

### Q1: Suspicious domain receiving DNS traffic?

**Answer:** `tunnelcorp.net`

### Q2: How many DNS queries to suspicious domain?

**Answer:** `315`

### Q3: Which internal IP sending DNS queries?

**Answer:** `192.168.1.103`

---

## Task 5: FTP Exfiltration Detection

**FTP exfil:**
- File transfers to external IPs
- Check connections and files
- Monitor payload sizes

### Q1: How many connections from guest account?

**Answer:** `5`

### Q2: Customer file exfiltrated from root account?

**Answer:** `customer_data.xlsx`

### Q3: Internal IP sending largest payload to external IP?

**Answer:** `192.168.1.105`

### Q4: Flag in FTP stream transferring CSV file?

**Answer:** `THM{ftp_exfil_hidden_flag}`

---

## Task 6: HTTP Exfiltration Detection

**HTTP exfil:**
- Data sent via HTTP POST
- Look in web traffic
- Check request bodies

### Q1: Which compromised host exfiltrated data?

**Answer:** `192.168.1.103`

### Q2: Flag in exfiltrated data?

**Answer:** `THM{http_raw_3xf1ltr4t10n_succ3ss}`

---

## Task 7: ICMP Exfiltration Detection

**ICMP exfil:**
- Uses ping packets
- Data hidden in ICMP payload
- Often overlooked

### Q: Flag in ICMP exfiltrated data?

**Answer:** `THM{1cmp_3ch0_3xf1ltr4t10n_succ3ss}`

---

## Task 8: Conclusion

**No answer needed**

---

## Quick Reference

### Exfiltration Methods

| Method | Channel | Detection |
|--------|---------|-----------|
| DNS Tunneling | DNS queries | Unusual query patterns, long subdomains |
| FTP | File transfers | Large transfers, suspicious IPs |
| HTTP | Web traffic | POST requests, encoded data |
| ICMP | Ping packets | Large payload, high frequency |

---

## Detection Techniques

### DNS Tunneling

**Red flags:**
- Many DNS queries to same domain
- Long subdomain names
- Unusual DNS query patterns
- Non-standard query types

**Wireshark filter:**
```
dns
dns.qry.name contains "suspicious"
```

### FTP Exfiltration

**Red flags:**
- Connections to external IPs
- Large file transfers
- Unusual times/accounts
- Sensitive filenames

**Wireshark filter:**
```
ftp
ftp-data
```

### HTTP Exfiltration

**Red flags:**
- Large POST requests
- Encoded/encrypted payloads
- Suspicious user agents
- Unusual destinations

**Wireshark filter:**
```
http.request.method == "POST"
http contains "data"
```

### ICMP Exfiltration

**Red flags:**
- Large ICMP packets
- High ping frequency
- Data in payload
- Unusual destinations

**Wireshark filter:**
```
icmp
icmp.type == 8
data
```

---

## Wireshark Analysis

### Common Filters

```
# DNS traffic
dns

# FTP connections
ftp || ftp-data

# HTTP POST requests
http.request.method == "POST"

# ICMP packets
icmp

# Specific IP
ip.addr == 192.168.1.103

# Large packets
frame.len > 1000
```

### Follow Streams

**To see full data:**
1. Right-click packet
2. Follow → TCP/UDP/HTTP Stream
3. Check for flags/data

### Export Data

**Extract files:**
1. File → Export Objects
2. Choose protocol (HTTP, FTP)
3. Save files for analysis

---

## Investigation Steps

### 1. Identify Suspicious Traffic

**Look for:**
- Unusual protocols
- High volume
- External connections
- Odd timing

### 2. Filter and Focus

**Apply filters:**
- Protocol-specific
- IP addresses
- Time ranges
- Packet sizes

### 3. Analyze Patterns

**Check:**
- Frequency
- Payload size
- Destinations
- Encoded data

### 4. Extract Evidence

**Get:**
- Flags
- Files
- Credentials
- Commands

---

## Common Indicators

### DNS Tunneling

**Patterns:**
- 100+ queries to one domain
- Subdomains like: `abc123.xyz456.domain.com`
- TXT records with data
- Rapid fire queries

### FTP Suspicious Activity

**Signs:**
- Guest/anonymous login
- Transfers to unknown IPs
- Sensitive filenames
- Large uploads

### HTTP Data Leaks

**Indicators:**
- POST to unknown domains
- Base64 in requests
- JSON/XML data
- API calls

### ICMP Covert Channel

**Clues:**
- Payload > 64 bytes
- Regular intervals
- ASCII text in data
- No normal ping pattern

---

## Detection Tools

**Network monitoring:**
- Wireshark
- tcpdump
- Zeek/Bro
- Suricata

**SIEM:**
- Splunk
- ELK Stack
- QRadar
- ArcSight

**DNS monitoring:**
- PassiveDNS
- DNSTwist
- DNS logs

---

## Prevention

**Network level:**
- Monitor DNS queries
- Block suspicious domains
- Inspect ICMP traffic
- Deep packet inspection

**Host level:**
- Data loss prevention (DLP)
- Endpoint detection (EDR)
- Application whitelisting
- User behavior analytics

**Policy level:**
- Least privilege access
- Network segmentation
- Egress filtering
- Regular audits

---

## Key Takeaways

- **Data exfiltration** uses various protocols
- **DNS tunneling** hides in queries
- **FTP transfers** show in connections
- **HTTP POST** can leak data
- **ICMP** isn't just ping
- **Wireshark** reveals exfil attempts
- **Patterns** indicate suspicious activity
- **Multiple methods** often combined
- **Detection** requires monitoring
- **Prevention** needs multiple layers

---

Happy hunting!
