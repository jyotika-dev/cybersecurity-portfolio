# IDS Fundamentals - TryHackMe

> Learn about Intrusion Detection Systems and how they protect networks

**Room Link:** https://tryhackme.com/room/idsfundamentals

<img width="1778" height="852" alt="image" src="https://github.com/user-attachments/assets/d631e089-2479-4401-9290-08f3816fd790" />


---

## Task 1: What is an IDS?

### Understanding IDS

**The Problem:**
- Firewalls block traffic based on rules
- But what if malicious traffic looks legitimate?
- What about threats that already got past the firewall?

**The Solution: IDS**

An **Intrusion Detection System (IDS)** monitors network traffic and detects malicious activity that has already entered the network.

### IDS vs Firewall

**Firewall:**
- Blocks traffic before it enters
- Prevention tool
- Works at network perimeter

**IDS:**
- Monitors traffic inside network
- Detection tool
- Alerts on suspicious activity
- Does NOT block traffic

### Key Point

IDS **detects** threats but **doesn't prevent** them!

Think of it as a security camera - it sees the threat and alerts you, but doesn't stop it.

### Questions

**Can an intrusion detection system (IDS) prevent the threat after it detects it? Yea/Nay**

IDS only detects and alerts - it doesn't prevent!

- Answer: `Nay`

---

## Task 2: Types of IDS

### Three Detection Methods

### 1. Signature-Based IDS

**How it works:**
- Keeps database of known attack patterns (signatures)
- Compares traffic to signatures
- Alerts if match found

**Pros:**
- Very accurate for known attacks
- Low false positives
- Fast detection

**Cons:**
- Can't detect new attacks (zero-day)
- Needs regular signature updates
- Database must be maintained

**Example:** Like virus scanner with signature database

### 2. Anomaly-Based IDS

**How it works:**
- Learns normal network behavior (baseline)
- Detects deviations from baseline
- Flags unusual activity as suspicious

**Pros:**
- Can detect zero-day attacks
- Finds unknown threats
- Adaptive

**Cons:**
- High false positives
- Needs training period
- Legitimate unusual activity flagged

**Example:** Like noticing someone acting strangely

### 3. Hybrid IDS

**How it works:**
- Combines both signature-based and anomaly-based
- Uses signatures for known threats
- Uses anomaly detection for new threats

**Pros:**
- Best of both worlds
- Detects known and unknown threats
- More comprehensive

**Cons:**
- More complex
- Resource intensive

### IDS Deployment Types

**Network IDS (NIDS):**
- Monitors entire network
- Placed at strategic points
- Sees all network traffic

**Host IDS (HIDS):**
- Monitors single host/device
- Checks system logs and files
- Device-specific

### Questions

**Which type of IDS is deployed to detect threats throughout the network?**
- Answer: `Network Intrusion Detection System`

---

**Which IDS leverages both signature-based and anomaly-based detection techniques?**
- Answer: `Hybrid IDS`

---

## Task 3: IDS Example - Snort

### What is Snort?

**Snort** is one of the most popular open-source IDS tools, created in 1998.

**Features:**
- Signature-based detection
- Anomaly-based detection
- Rule-based system
- Packet logging
- Real-time traffic analysis

### Snort Operating Modes

**1. Sniffer Mode:**
- Reads packets
- Displays on screen
- Basic packet capture

**2. Packet Logger Mode:**
- Logs packets to disk
- Saves as PCAP files
- For later analysis

**3. Network IDS Mode:**
- Primary mode
- Analyzes packets against rules
- Generates alerts
- Real-time detection

### Snort Rules

**Built-in Rules:**
- Pre-installed rule files
- Cover many known attacks
- Regularly updated

**Custom Rules:**
- Create your own rules
- Detect specific threats
- Tailored to your needs

### Questions

**Which mode of Snort helps us to log the network traffic in a PCAP file?**
- Answer: `Packet Logging Mode`

---

**What is the primary mode of Snort called?**
- Answer: `Network Intrusion Detection System Mode`

---

## Task 4: Snort Usage

### Snort Directory Structure

**Main directory:** `/etc/snort`

Contains:
- Configuration files
- Rule files
- Log files

### Snort Rule Format

```
alert icmp any any -> any any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)
```

**Rule components:**
- `alert` - Action (alert, log, pass)
- `icmp` - Protocol
- `any any` - Source IP and port
- `->` - Direction
- `any any` - Destination IP and port
- `msg` - Alert message
- `sid` - Signature ID (unique identifier)
- `rev` - Revision number

### Important Rule Fields

| Field | Purpose |
|-------|---------|
| `sid` | Signature ID (unique) |
| `rev` | Revision number |
| `msg` | Alert message |
| `classtype` | Attack classification |
| `priority` | Alert priority |

### Custom Rules File

**File:** `local.rules`

This is where you add your custom Snort rules!

### Questions

**Where is the main directory of Snort that stores its files?**
- Answer: `/etc/snort`

---

**Which field in the Snort rule indicates the revision number of the rule?**
- Answer: `rev`

---

**Which protocol is defined in the sample rule created in the task?**

Looking at the example rule, it uses ICMP protocol.

- Answer: `icmp`

---

**What is the file name that contains custom rules for Snort?**
- Answer: `local.rules`

---

## Task 5: Practical Lab

### The Scenario

You're a forensic investigator. A company was attacked and gave you a PCAP file to analyze.

**File:** `Intro_to_IDS.pcap`
**Location:** `/etc/snort/`

### Running Snort on PCAP

**Command:**
```bash
sudo snort -r /etc/snort/Intro_to_IDS.pcap -A console -c /etc/snort/snort.conf
```

**Flag breakdown:**
- `-r` - Read PCAP file
- `-A console` - Alert mode (display in console)
- `-c` - Configuration file

### Analyzing Results

Snort will process the PCAP and show alerts based on rules.

---

### Question 1: SSH Connection IP

**What is the IP address of the machine that tried to connect to the subject machine using SSH?**

**Steps:**
1. Run Snort command
2. Look for SSH alerts
3. Find source IP address

Look for alerts containing "SSH" and note the source IP.

- Answer: `10.11.90.211`

---

### Question 2: Other Detected Rule

**What other rule message besides the SSH message is detected in the PCAP file?**

**Steps:**
1. Look at all alerts generated
2. Find messages other than SSH
3. Note the alert message

Besides SSH, you'll see another detection for ping traffic.

- Answer: `Ping Detected`

---

### Question 3: SSH Rule SID

**What is the sid of the rule that detects SSH?**

**Steps:**
1. Look at SSH alert details
2. Find the `sid` field
3. Note the number

Each rule has a unique SID (Signature ID).

- Answer: `1000002`

---

## Quick Reference

### IDS Types Comparison

| Type | Detection Method | Pros | Cons |
|------|------------------|------|------|
| Signature-Based | Known patterns | Accurate, low false positives | Can't detect zero-day |
| Anomaly-Based | Baseline deviation | Detects new threats | High false positives |
| Hybrid | Both methods | Comprehensive | Complex |

### Snort Modes

| Mode | Purpose |
|------|---------|
| Sniffer | Display packets |
| Packet Logger | Save packets to file |
| NIDS | Detect threats with rules |

### Important Snort Locations

| Path | Contents |
|------|----------|
| `/etc/snort/` | Main directory |
| `/etc/snort/snort.conf` | Main config file |
| `/etc/snort/rules/local.rules` | Custom rules |

---

## Key Takeaways

- **IDS detects** threats, doesn't prevent them
- **Firewalls prevent**, IDS detects
- **Signature-based** = known attack patterns
- **Anomaly-based** = detects unusual behavior
- **Hybrid** = combines both methods
- **NIDS** monitors entire network
- **Snort** is popular open-source IDS
- **PCAP files** contain captured network traffic
- **sid** = unique rule identifier
- **rev** = rule revision number
- **local.rules** = custom rules file
- **IDS complements firewall**, doesn't replace it

Understanding IDS is crucial for network security monitoring and threat detection
Happy detecting!
