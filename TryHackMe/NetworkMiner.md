# NetworkMiner - TryHackMe

> Network forensics tool for analyzing PCAP files and extracting artifacts

**Room Link:** https://tryhackme.com/room/networkminer

<img width="1563" height="925" alt="image" src="https://github.com/user-attachments/assets/6bf42ae4-1801-4baa-97b2-cb776ad6b1d1" />


---

## Task 1: Introduction

### What is NetworkMiner?

**Network Forensic Analysis Tool (NFAT):**
- Passive network sniffer
- PCAP analyzer
- Extracts artifacts from captured traffic

**Capabilities:**
- OS fingerprinting
- Extract files and certificates
- Grab credentials
- Identify hosts and sessions

### NetworkMiner vs Snort

| Feature | NetworkMiner | Snort |
|---------|-------------|-------|
| **Analysis** | Passive (post-capture) | Active (real-time) |
| **Purpose** | Forensics investigation | Intrusion detection |
| **Data** | PCAP files | Live traffic |
| **Use case** | Past traffic analysis | Ongoing monitoring |

**No answer needed**

---

## Task 2: Network Forensics

### What is Network Forensics?

Investigating network traffic to detect breaches, ensure compliance, and analyze behavior.

**Key Questions:**
- **Who** - Source IP
- **What** - Data transferred
- **Where** - Destination IP
- **When** - Timestamp
- **Why** - Cause of event

### Use Cases

- Network discovery
- Packet reassembly
- Data leakage detection
- Threat detection
- Compliance monitoring

**No answer needed**

---

## Task 3: What is NetworkMiner?

### Key Features

**Traffic Analysis:**
- Parse PCAP files
- OS fingerprinting (Satori, p0f)
- File extraction
- Credential grabbing
- Cleartext keyword parsing

### NetworkMiner vs Wireshark

| Feature | NetworkMiner | Wireshark |
|---------|-------------|-----------|
| **Purpose** | Quick overview | Deep analysis |
| **OS Fingerprinting** | ✅ | ❌ |
| **File Extraction** | ✅ (Easy) | ✅ |
| **Credential Discovery** | ✅ (Auto) | ✅ (Manual) |
| **Protocol Analysis** | Limited | ✅ |
| **Filtering** | Basic | Advanced |

### When to Use

**NetworkMiner:** Quick PCAP overview, extract artifacts

**Wireshark:** Deep packet analysis, protocol inspection

**No answer needed**

---

## Task 4: Tool Overview 1

### Interface Menus

**File Menu:**
- Load PCAP files
- Receive PCAP over IP

**Tools Menu:**
- Clear data
- Reset dashboard

**Case Panel:**
- View loaded PCAPs
- Access metadata

**Hosts Menu:**
- IP/MAC addresses
- OS types
- Open ports
- Packet counts

**Sessions Menu:**
- Client/Server connections
- Protocol details
- Timestamps

**DNS Menu:**
- DNS queries and answers
- TTL values

**Credentials Menu:**
- Kerberos hashes
- NTLM hashes
- HTTP cookies
- FTP/SMTP credentials

### Questions

**Use mx-3.pcap. What is the total number of frames?**

File > Open > mx-3.pcap

Right-click case > Show Metadata

Check total frames.

- Answer: `460`

---

**How many IP addresses use the same MAC address with host 145.253.2.203?**

Hosts menu > Find 145.253.2.203

Check MAC address: FEFF20000100

Expand dropdown to see IPs with same MAC.

- Answer: `2`

---

**How many packets were sent from host 65.208.228.223?**

Click on IP (one of the two from previous answer)

Check "Packets sent" field.

- Answer: `72`

---

**What is the name of the webserver banner under host 65.208.228.223?**

Expand Host Details dropdown.

Check webserver banner.

- Answer: `Apache`

---

**Use mx-4.pcap. What is the extracted username?**

File > Open > mx-4.pcap

Go to Credentials menu.

Check fourth column.

- Answer: `#B\Administrator`

---

**What is the extracted password?**

Same Credentials menu.

Look for NTLM hash (second row).

Use $NETNTLMv2$ format for cracking.

- Answer: `$NETNTLMv2$#B$136B077D942D9A63$FBFF3C253926907AAAAD670A9037F2A5$01010000000000000094D71AE38CD60170A8D571127AE49E00000000020004003300420001001E003000310035003600360053002D00570049004E00310036002D004900520004001E0074006800720065006500620065006500730063006F002E0063006F006D0003003E003000310035003600360073002D00770069006E00310036002D00690072002E0074006800720065006500620065006500730063006F002E0063006F006D0005001E0074006800720065006500620065006500730063006F002E0063006F006D00070008000094D71AE38CD601060004000200000008003000300000000000000000000000003000009050B30CECBEBD73F501D6A2B88286851A6E84DDFAE1211D512A6A5A72594D340A001000000000000000000000000000000000000900220063006900660073002F003100370032002E00310036002E00360036002E0033003600000000000000000000000000`

---

## Task 5: Tool Overview 2

### Additional Menus

**Files Menu:**
- Extracted files from PCAPs
- File details and paths

**Images Menu:**
- Extracted images
- Hover for details

**Parameters Menu:**
- Extracted parameters
- Values and timestamps

**Keywords Menu:**
- Filter keywords
- Context and location

**Messages Menu:**
- Emails and chats
- Sender/receiver info

**Anomalies Menu:**
- Detected anomalies
- Frame numbers

### Questions

**Use mx-7.pcap. What is the name of the Linux distro mentioned in the file associated with frame 63075?**

Files menu > Find frame 63075

Check file details.

- Answer: `CentOS`

---

**What is the header of the page associated with frame 75942?**

Files menu > Frame 75942

Check `<h1>` tag contents.

- Answer: `Password-Ned AB`

---

**What is the source address of the image "ads.bmp.2E5F0FD9.bmp"?**

Images menu > Find image (last in list)

Hover over image for details.

- Answer: `80.239.178.187`

---

**What is the frame number of the possible TLS anomaly?**

Anomalies menu > Check list

Two anomalies present.

- Answer: `36255`

---

**Use mx-9.pcap. Which platform sent a password reset email?**

Messages menu > Look for password-related email.

- Answer: `Facebook`

---

**What is the email address of Branson Matheson?**

Messages menu > Lines 5 and 6.

- Answer: `Branson@sandsite.org`

---

## Task 6: Version Differences

### NetworkMiner 1.6 vs 2.7

| Feature | Version 1.6 | Version 2.7 |
|---------|------------|------------|
| MAC conflict detection | ❌ | ✅ |
| Frame processing | ✅ | ❌ |
| Detailed packet info | ✅ | ❌ |
| Parameter extraction | Limited | Improved |

### Questions

**Which version can detect duplicate MAC addresses?**

MAC processing added in v2.0+

- Answer: `2.7`

---

**Which version can handle frames?**

Frame processing removed in v2.0

- Answer: `1.6`

---

**Which version can provide more details on packet details?**

Detailed packet info removed in v2.0

- Answer: `1.6`

---

## Task 7: Exercises

### Questions

**Use case1.pcap. What is the OS name of host 131.151.37.122?**

Hosts menu > Find IP > Check OS field.

- Answer: `Windows - Windows NT 4`

---

**How many data bytes were received from host 131.151.32.91 to host 131.151.37.122 through port 1065?**

Check receiving host (131.151.37.122) > Incoming sessions

Or check sending host (131.151.32.91) > Outgoing sessions

- Answer: `192`

---

**How many data bytes were received from host 131.151.37.122 to host 131.151.32.21 through port 143?**

Check host 131.151.32.21 > Outgoing sessions.

- Answer: `20769`

---

**What is the sequence number of frame 9?**

Switch to NetworkMiner 1.6.1

Frames menu > Find frame 9

Check TCP sequence number.

- Answer: `2AD77400`

---

**What is the number of detected "content types"?**

Switch to NetworkMiner 2.7.2

Parameters menu > Filter "Content-Type"

Count unique types: text/plain, multipart/mixed

- Answer: `2`

---

**Use case2.pcap. What is the USB product's brand name?**

Images menu > Find USB key image (row 4)

Check reconstructed domain.

- Answer: `ASIX`

---

**What is the name of the phone model?**

Images menu > Look for phone image.

- Answer: `Lumia 535`

---

**What is the source IP of the fish image?**

Files menu > Search "fish"

Check source IP (5th column).

- Answer: `50.22.95.9`

---

**What is the password of "homer.pwned.se@gmx.com"?**

Credentials menu > Find entry with password.

- Answer: `spring2015`

---

**What is the DNS Query of frame 62001?**

DNS menu > Filter frame 62001.

- Answer: `pop.gmx.com`

---

## Task 8: Conclusion

**No answer needed**

---

## Quick Reference

### Best Practices

**Use NetworkMiner for:**
- Quick PCAP overview
- Extract files/credentials
- OS fingerprinting
- Initial investigation

**Use Wireshark for:**
- Deep packet analysis
- Protocol inspection
- Advanced filtering
- Manual investigation

### Common Workflows

**1. Load PCAP:**
```
File > Open > Select PCAP
```

**2. Quick Overview:**
- Hosts menu for devices
- Sessions for connections
- Files for extracted content

**3. Extract Artifacts:**
- Credentials menu for passwords
- Images menu for pictures
- Files menu for documents

**4. Deep Dive:**
- Export interesting items
- Switch to Wireshark
- Analyze specific packets

### Key Menus

- **Hosts** - Device inventory
- **Sessions** - Connections
- **DNS** - Queries
- **Credentials** - Passwords
- **Files** - Extracted files
- **Images** - Pictures
- **Messages** - Emails
- **Anomalies** - Issues

---

## Key Takeaways

- **NetworkMiner** = passive PCAP analysis
- **Snort** = active real-time IDS
- **OS fingerprinting** via Satori and p0f
- **Extracts credentials** automatically
- **Version 2.7** detects MAC conflicts
- **Version 1.6** handles frames better
- **Metadata view** shows frame counts
- **Hover over images** for details
- **NTLM hashes** can be cracked
- **Use with Wireshark** for complete analysis
- **Not for live sniffing** - forensics only
- **Quick overview** then deep dive

NetworkMiner simplifies PCAP analysis
Happy investigating!
