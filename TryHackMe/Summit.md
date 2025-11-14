# Summit - TryHackMe

> Chase a simulated adversary up the Pyramid of Pain through detection engineering

**Room Link:** https://tryhackme.com/room/summit

<img width="1301" height="919" alt="image" src="https://github.com/user-attachments/assets/31b47c1c-f50d-47ec-87c8-6a9a4aca003a" />


---

## Overview

Mock threat simulation and detection engineering engagement. Configure security tools to detect and prevent malware execution while climbing the Pyramid of Pain.

### Key Concepts

**Pyramid of Pain:**
- Framework for IoC difficulty levels
- Bottom = Easy to detect (hashes)
- Top = Hard to detect (TTPs)
- Higher levels cause more "pain" to attackers

**MITRE ATT&CK:**
- Knowledge base of attack techniques
- Documents adversary TTPs
- Used globally for threat detection

---

## Sample 1: Hash-Based Detection

### Analysis

Scan malware using sandbox environment.

**Findings:**
- File size
- Hash values (MD5, SHA1, SHA256)
- Basic behavior analysis

### Detection Method

**Block by hash:**
1. Go to "Manage Hashes" tab
2. Set configuration to SHA1
3. Paste hash value
4. Submit rule

**Why this works:** Hash values are unique file identifiers.

**Limitation:** Easy for attackers to change (recompile file).

### Flag

- Answer: `THM{f3cbf08151a11a6a331db9c6cf5f4fe4}`

---

## Sample 2: Network-Based Detection

### Analysis

Scan reveals network activity.

**Key findings:**
- GET request to IP: 154.35.10.113
- Outbound connection (egress traffic)

### Detection Method

**Block by IP address:**
1. Go to Firewall Rule Manager
2. Create rule to deny connections
3. Target IP: 154.35.10.113
4. Set action: Deny/Block

**Egress vs Ingress:**
- Egress = Data leaving network
- Ingress = Data entering network

**Why this works:** Blocks C2 communication.

**Limitation:** Attacker can use different IP.

### Flag

- Answer: `THM{2ff48a3421a938b388418be273f4806d}`

---

## Sample 3: Domain-Based Detection

### Analysis

**Findings:**
- DNS requests
- HTTP connections
- Specific domains contacted

**Malicious domains:**
- services.microsoft.com (spoofed)
- emudyn.bresonicz.info

### Detection Method

**Block by domain name:**
1. Go to DNS Rules
2. Create custom rule
3. Add both domains
4. Set action: Deny/Block

**Why this works:** Stops DNS resolution to malicious sites.

**Limitation:** Attacker can use different domains.

### Flag

- Answer: `THM{4eca9e2f61a19ecd5df34c788e7dce16}`

---

## Sample 4: Registry-Based Detection

### Analysis

**Three registry events observed:**

1. **sample4.exe (PID 3806)**
   - Disabled Windows Defender
   - Set DisableRealtimeMonitoring = 1

2. **explorer.exe (PID 1928)**
   - Enabled balloon tips
   - Set EnableBalloonTips = 1

3. **notepad.exe (PID 9876)**
   - Read .txt file association

### Detection Method

**Create Sigma rule:**
- Target: Registry modifications
- Focus: DisableRealtimeMonitoring key
- Attack technique: Defense evasion

**Sigma rule components:**
- Registry key path
- Registry value name
- Registry value data
- Attack ID (MITRE ATT&CK)

**Why this works:** Detects defense evasion techniques.

**Limitation:** Requires understanding of attack patterns.

### Flag

- Answer: `THM{c956f455fc076aea829799c0876ee399}`

---

## Sample 5: Network Pattern Detection

### Analysis

Connection logs instead of malware sample.

**Patterns identified:**
- Consistent byte sizes
- Specific ports
- Timed connections
- Regular beaconing behavior

**Indication:** C2 (Command and Control) communication

### Detection Method

**Create custom rule based on:**
- Connection frequency
- Byte size consistency
- Port numbers
- Timing patterns

**Why this works:** Detects C2 beaconing patterns.

**Limitation:** Requires behavioral analysis skills.

### Flag

- Answer: `THM{46b21c4410e47dc5729ceadef0fc722e}`

---

## Sample 6: Command-Based Detection

### Analysis

Command logs provided, not malware.

**Commands observed:**
- System enumeration
- Data collection
- File operations
- Log creation (exfiltr8.log)

**Purpose:** Collecting system information for exfiltration

### Detection Method

**Create rule targeting:**
- File path
- File name (exfiltr8.log)
- Attack ID (MITRE technique)
- Command patterns

**Why this works:** Detects TTPs (highest Pyramid level).

**Limitation:** Most difficult but causes most pain to attacker.

### Flag

- Answer: `THM{c8951b2ad24bbcbac60c16cf2c83d92c}`

---

## Quick Reference

### Pyramid of Pain Levels

```
        [TTPs] ← Hardest to change (Most Pain)
          ↑
      [Tools]
          ↑
    [Network/Host Artifacts]
          ↑
    [Domain Names]
          ↑
    [IP Addresses]
          ↑
    [Hash Values] ← Easiest to change (Least Pain)
```

### Detection Methods Used

| Sample | Detection Type | Pain Level | Tool |
|--------|---------------|------------|------|
| Sample 1 | Hash | Low | Hash Manager |
| Sample 2 | IP Address | Low-Medium | Firewall |
| Sample 3 | Domain | Medium | DNS Rules |
| Sample 4 | Registry | Medium-High | Sigma Rules |
| Sample 5 | Network Pattern | High | Custom Rules |
| Sample 6 | TTPs/Commands | Very High | Behavioral Rules |

### Why Each Level Matters

**Hash Values (Trivial):**
- Easy detection
- Easy for attacker to bypass (recompile)
- Quick wins

**IP Addresses (Easy):**
- Block malicious infrastructure
- Attacker changes servers easily
- Better than nothing

**Domain Names (Simple):**
- Harder to change than IPs
- Requires registering new domains
- More disruptive to attacker

**Network/Host Artifacts (Annoying):**
- Registry modifications
- File paths
- Harder for attacker to change

**Tools (Challenging):**
- Detection of specific malware tools
- Forces attacker to use different tools
- Significant effort to bypass

**TTPs (Tough):**
- Behavioral detection
- Attack techniques and procedures
- Hardest for attacker to change
- Requires changing entire approach

### Detection Strategy Tips

**Layer your defenses:**
- Don't rely on one detection type
- Combine multiple methods
- Defense in depth

**Move up the pyramid:**
- Start with easy detections (hashes)
- Progress to harder ones (TTPs)
- Force attacker to adapt

### Common Tools Used

**Sandbox Analysis:**
- Safe environment for malware
- Captures behavior
- Generates IoCs

**Firewall Rules:**
- Block IP addresses
- Control traffic flow
- Network segmentation

**DNS Rules:**
- Block malicious domains
- Prevent DNS resolution
- Sinkhole traffic

**Sigma Rules:**
- Generic detection rules
- Platform-agnostic
- Behavioral detection

### Command Analysis

**Enumeration Commands:**
- `whoami` - User info
- `ipconfig` - Network info
- `net user` - Account info
- `systeminfo` - System details

**Data Collection:**
- Output redirection (>)
- Log file creation
- Batch operations
- Scheduled tasks

---

## Key Takeaways

- **Pyramid of Pain** guides detection strategy
- **Hash detection** is easiest but least effective long-term
- **IP blocking** stops known infrastructure
- **Domain blocking** is harder for attackers to bypass
- **Registry detection** catches defense evasion
- **Pattern detection** identifies C2 behavior
- **TTP detection** causes most pain to attackers
- **Multi-layered defense** is essential
- **Behavioral analysis** beats signature-based detection
- **Sigma rules** provide flexible detection logic
- **Understanding attacker TTPs** is crucial
- **MITRE ATT&CK** maps attack techniques
- **Defense in depth** combines multiple strategies
- **Context matters** for effective detection

Climb the Pyramid of Pain to make attackers' lives harder
Happy detecting!
