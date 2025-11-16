# Network Traffic Basics - TryHackMe

> Learn network traffic analysis fundamentals and capture techniques

**Room Link:** https://tryhackme.com/room/networktrafficbasics

<img width="1558" height="795" alt="image" src="https://github.com/user-attachments/assets/5a4e739a-bb96-4335-ad3f-4bd507d32fb7" />

---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Purpose of Network Traffic Analysis

### What is NTA?

**Network Traffic Analysis (NTA)** monitors network data to detect threats, performance issues, and anomalies.

### Key Use Cases

**Security:**
- Detect malware C2 communication
- Identify data exfiltration
- Spot covert channels
- Find unauthorized access

**Performance:**
- Monitor bandwidth usage
- Identify bottlenecks
- Analyze application behavior

**Compliance:**
- Log network activity
- Audit access patterns
- Meet regulatory requirements

### Question

**Answer: DNS tunneling**

**What it is:** Covert channel using DNS queries/responses to hide C2 commands or exfiltrate data.

**How it works:**
- Encode data in DNS labels
- Use TXT records for commands
- Bypass firewalls (DNS usually allowed)
- Unusual query patterns

**Detection methods:**
- High DNS query volume
- Large TXT records
- Base64/Base32-encoded labels
- Unusual domain names

- Answer: `DNS tunneling`

---

## Task 3: What Network Traffic Can We Observe?

### Traffic Types

**HTTP/HTTPS:**
- Web traffic
- REST APIs
- File downloads

**DNS:**
- Domain resolution
- Potential tunneling

**TCP/UDP:**
- Connection-oriented (TCP)
- Connectionless (UDP)

**SMB:**
- File sharing
- Windows authentication

### Questions

**Q1: Look at the HTTP example: what is the size of the ZIP attachment included in the HTTP response?**

Check HTTP response headers.

Look for Content-Length field.

**Size:** 10485760 bytes = 10 MB

**Why it matters:** Large downloads to unexpected hosts = suspicious.

- Answer: `10485760 bytes`

---

**Q2: Which evasion technique do attackers use to try to evade an IDS?**

Attackers split malicious payloads across small packets.

IDS signatures miss fragmented patterns.

**Countermeasure:** IDS must reassemble fragments.

- Answer: `fragmentation`

---

**Q3: What field in the TCP header can we use to detect session hijacking?**

Session hijacking involves forged TCP packets.

Monitor expected progression of this field.

**Detection:** Sudden jumps or out-of-order values.

- Answer: `sequence number`

---

## Task 4: Network Traffic Sources and Flows

### Traffic Sources

**Endpoint Devices:**
- Servers, desktops, laptops
- Mobile devices, IoT
- Generate most traffic

**Intermediary Devices:**
- Routers, switches
- Firewalls, proxies
- Mainly forward traffic

### Traffic Flows

**North-South:**
- Traffic leaving/entering network
- Internet-bound traffic
- External connections

**East-West:**
- Traffic within network
- Server-to-server
- Lateral movement

### Questions

**Q1: Which category of devices generates the most traffic in a network?**

Desktops, servers, phones = originate data.

Intermediaries just forward packets.

- Answer: `endpoint`

---

**Q2: Before an SMB session can be established, which service needs to be contacted first for authentication?**

**Windows domain environment:**

Client requests service ticket from Domain Controller.

Obtains authentication before SMB access.

**Protocol:** Authentication service in Active Directory.

- Answer: `Kerberos`

---

**Q3: What does TLS stand for?**

Protocol that encrypts application-layer traffic.

Used in HTTPS, secure email, etc.

**Impact on NTA:** Can't see encrypted content, must use metadata.

- Answer: `Transport Layer Security`

---

## Task 5: How Can We Observe Network Traffic?

### Capture Methods

**1. Logs**
- Connection records
- Metadata only
- Efficient storage

**2. Full Packet Capture**
- Complete packet contents
- High storage needs
- Deep inspection possible

**3. Network Statistics**
- Flow data (NetFlow)
- Aggregated metrics
- Efficient for large networks

### Scenario 1: HTTP Traffic

**Task:** Find flag in HTTP traffic from WP1 device.

**Steps:**
1. Open static lab page
2. Click Start
3. Load topology file
4. Locate WP1 device
5. Click through packet table pages
6. Find last page
7. Look for HTTP GET with 200 OK response
8. File size: 10485760 bytes (application/zip)
9. Expand packet row
10. View Body Preview
11. Flag in HTTP response body

**What it shows:** Full capture reveals hidden payloads like malware downloads.

- Answer: `THM{FoundTheMalware}`

---

### Scenario 2: DNS Traffic

**Task:** Find flag in DNS traffic.

**Steps:**
1. On static lab page
2. Load topology
3. Click link between sw01 and SRV-DNS
4. DNS traffic displayed
5. Page through packet table
6. Find UDP DNS response
7. Look for: "Standard query response 0x41eb TXT c2.tryhackme.thm"
8. Seventh row in table
9. Expand packet
10. Check TXT record data
11. Flag in DNS TXT response

**What it shows:** DNS TXT records commonly abused for C2 commands and tunneling.

- Answer: `THM{C2CommandFound}`

---

## Quick Reference

### DNS Tunneling Indicators

**Volume:**
- High query rate
- Many subdomains
- Unusual patterns

**Content:**
- Long domain labels
- Base64/Base32 encoding
- Random-looking strings
- TXT record abuse

**Behavior:**
- Regular intervals
- Consistent sizes
- Non-existent domains

### TCP Session Hijacking Detection

**Monitor:**
- Sequence numbers
- Acknowledgment numbers
- Window sizes
- Timing

**Red flags:**
- Sudden sequence jumps
- Duplicate packets
- Out-of-order packets
- Reset storms

### Fragmentation Evasion

**How it works:**
- Split payload across fragments
- IDS can't match signatures
- Reassembly required

**Defense:**
- Enable fragment reassembly
- Monitor fragment rates
- Set fragment limits
- Block excessive fragmentation

### Common Traffic Analysis Tools

| Tool | Purpose |
|------|---------|
| **Wireshark** | Packet capture and analysis |
| **tcpdump** | Command-line capture |
| **Zeek** | Network security monitor |
| **Suricata** | IDS/IPS with NTA |
| **NetFlow** | Flow data collection |

### Protocol Analysis Tips

**HTTP/HTTPS:**
- Check User-Agent strings
- Monitor Content-Length
- Track download patterns
- Watch for unusual URLs

**DNS:**
- Query types (A, TXT, CNAME)
- Query frequency
- Domain entropy
- Response sizes

**SMB:**
- Authentication flows
- File access patterns
- Named pipes
- Kerberos tickets

**TCP:**
- Three-way handshake
- Sequence numbers
- Window sizes
- Connection states

### Traffic Flow Patterns

**North-South (External):**
- Web browsing
- Cloud services
- Email (external)
- Software updates

**East-West (Internal):**
- File sharing (SMB)
- Database queries
- Internal APIs
- Backup traffic

### Capture Best Practices

**What to capture:**
- Critical network segments
- Internet gateways
- DMZ zones
- Server VLANs

**Storage considerations:**
- Full capture = high volume
- Rotate logs regularly
- Compress old data
- Archive important cases

**Performance:**
- Use capture filters
- Limit to relevant ports
- Sample high-volume links
- Aggregate when possible

### Analysis Workflow

**1. Define scope:**
- What to monitor
- Time period
- Network segments

**2. Capture traffic:**
- Choose method (logs/full/stats)
- Set filters
- Start collection

**3. Analyze patterns:**
- Baseline normal traffic
- Identify anomalies
- Focus on outliers

**4. Investigate alerts:**
- Expand suspicious packets
- Check payloads
- Correlate events

**5. Document findings:**
- IOCs identified
- Attack patterns
- Remediation steps

### Common Ports Reference

| Port | Protocol | Purpose |
|------|----------|---------|
| 53 | DNS | Domain resolution |
| 80 | HTTP | Web traffic |
| 443 | HTTPS | Encrypted web |
| 445 | SMB | File sharing |
| 88 | Kerberos | Authentication |
| 389 | LDAP | Directory services |

---

## Key Takeaways

- **DNS tunneling** hides C2 in DNS queries
- **10485760 bytes** = 10 MB ZIP attachment
- **Fragmentation** evades IDS detection
- **Sequence numbers** detect session hijacking
- **Endpoint devices** generate most traffic
- **Kerberos** authenticates before SMB
- **TLS** encrypts application traffic
- **Full packet capture** reveals payloads
- **DNS TXT records** commonly abused
- **HTTP responses** can contain malware
- **Base64 encoding** hides data in DNS
- **North-South** = external traffic
- **East-West** = internal traffic
- **Reassembly required** to detect fragmentation

Network traffic analysis exposes hidden threats!
Happy analyzing
