# Introduction to EDR - TryHackMe

> Learn the fundamentals of EDR and explore its features and working

**Room Link:** https://tryhackme.com/room/introductiontoedrs


<img width="1432" height="858" alt="image" src="https://github.com/user-attachments/assets/cc20ea09-0aa6-491d-b89d-aba2a70ebd64" />


---

## Task 1: Introduction

**No answer needed**

---

## Task 2: EDR Features

### What is EDR?

**Endpoint Detection and Response (EDR):**
- Security solution for endpoints
- Monitors workstations and servers
- Detects and responds to threats

### Key Features

**Visibility**
- Complete context for detections
- Full timeline of events
- Parent-child process relationships

**Detection**
- Identifies malicious activity
- Uses behavioral analysis
- Threat intelligence integration

**Response**
- Isolates compromised endpoints
- Kills malicious processes
- Quarantines files

### Questions

**Which feature of EDR provides a complete context for all the detections?**

Seeing the full picture of what happened.

- Answer: `Visibility`

---

**Which process spawned sc.exe?**

Check the process tree to see the parent process.

Look for what launched sc.exe.

- Answer: `cmd.exe`

---

## Task 3: EDR vs Antivirus

### The Immigration Analogy

**Airport Security Layers:**

**Immigration Check (Antivirus):**
- Checks passports at entry
- Static verification
- Signature-based detection

**CCTV Cameras (EDR):**
- Monitors behavior inside airport
- Tracks movements
- Detects suspicious activity

### Real Attack Scenario

**Process Injection Attack:**

Attacker hijacks legitimate Windows process:
- Uses svchost.exe (trusted system process)
- Injects malicious code
- AV sees clean file signature
- EDR sees suspicious behavior

### Why EDR is Needed

**Antivirus limitations:**
- Only checks file signatures
- Misses fileless attacks
- Can't detect process injection
- No behavioral monitoring

**EDR advantages:**
- Monitors process behavior
- Detects living-off-the-land attacks
- Tracks process relationships
- Provides full context

### Questions

**In the given analogy, what presents an AV?**

Static check at entry point.

- Answer: `immigration check`

---

**Which legitimate process was hijacked by the attacker in the scenario?**

Trusted Windows system process used for injection.

- Answer: `svchost.exe`

---

**Which security solution might mark this activity as clean?**

Signature-based solution seeing legitimate file.

- Answer: `Antivirus`

---

## Task 4: EDR Components

### Architecture Overview

**Three Main Components:**

**1. Agent (Sensor)**
- Installed on endpoints
- Collects telemetry data
- Sends to management server
- Lightweight process

**2. Management Server**
- Central collection point
- Stores telemetry data
- Runs detection logic
- Manages agents

**3. Dashboard/Console**
- Analyst interface
- View detections
- Investigate alerts
- Take response actions

### Data Flow

```
Endpoint → Agent collects data → Management Server processes → Analyst views in Dashboard
```

### Questions

**Which component of the EDR is responsible for collecting telemetry from the endpoints?**

Software installed on each endpoint.

- Answer: `Agent`

---

**An EDR agent is also known as a?**

Alternative term for monitoring agent.

- Answer: `sensor`

---

## Task 5: Telemetry Types

### What EDR Collects

**Process Telemetry:**
- Process creation/termination
- Parent-child relationships
- Command-line arguments
- Execution paths

**Network Connections:**
- Outbound connections
- C2 communications
- Data exfiltration attempts
- Suspicious domains

**File Operations:**
- File creation/modification
- File deletions
- Downloaded files
- Suspicious locations

**Registry Changes:**
- Configuration modifications
- Persistence mechanisms
- Startup changes
- Policy alterations

**User Activity:**
- Login events
- Privilege changes
- Account modifications
- Authentication attempts

### Why Each Matters

| Telemetry Type | Detects |
|----------------|---------|
| Process | Malware execution, suspicious commands |
| Network | C2 beaconing, data exfiltration |
| File | Malware drops, ransomware |
| Registry | Persistence, privilege escalation |
| User | Compromised accounts, lateral movement |

### Questions

**Which telemetry data helps in detecting C2 communications?**

Outbound connections to attacker infrastructure.

- Answer: `Network Connections`

---

**Where are the configuration settings of a Windows system primarily stored?**

Windows configuration database.

- Answer: `registry`

---

## Task 6: Detection Methods

### How EDR Detects Threats

**IOC Matching:**
- Known malicious indicators
- File hashes
- IP addresses
- Domain names
- Based on threat intelligence

**Behavioral Analysis:**
- Unusual process activity
- Abnormal network patterns
- Suspicious file operations
- Deviation from baseline

**Machine Learning:**
- Identifies unknown threats
- Detects zero-day attacks
- Pattern recognition
- Adaptive learning

### Detection Flow

```
Telemetry collected → Detection engine analyzes → IOC match or behavioral anomaly → Alert generated
```

### Questions

**Which feature of the EDR helps you identify threats based on known malicious behaviours?**

Matching against known bad indicators.

- Answer: `IOC Matching`

---

## Task 7: Practical Investigation

### Investigation Scenarios

**Scenario 1: Malware Download on DESKTOP-HR01**

**Attack chain:**
1. CMD.exe executed
2. Download tool launched
3. Payload downloaded to disk

**Key questions:**
- What tool was used?
- Where was file saved?

**Scenario 2: Data Exfiltration on WIN-ENG-LAPTOP03**

**Attack indicators:**
- Suspicious executable running
- Outbound connection to file-sharing site
- Memory dump being uploaded

**Scenario 3: False Positive on DESKTOP-DEV01**

**Investigation reveals:**
- Known internal IT tool
- Legitimate business use
- Threat intel classification helps

### Questions

**Which tool was launched by CMD.exe to download the payload on DESKTOP-HR01?**

Check process tree under CMD.exe.

Look for download utility.

- Answer: `CURL.exe`

---

**What is the absolute path to the downloaded malware on the DESKTOP-HR01 machine?**

Check file operations after CURL.exe execution.

Look in Public folder.

- Answer: `C:\Users\Public\install.exe`

---

**What is the absolute path to the suspicious syncsvc.exe on the WIN-ENG-LAPTOP03 machine?**

Check process details for syncsvc.exe.

Look in Temp folder.

- Answer: `C:\Users\haris.khan\AppData\Local\Temp\syncsvc.exe`

---

**On which URL was the exfiltration attempt being made on WIN-ENG-LAPTOP03?**

Check network connections from syncsvc.exe.

Look for file-sharing upload URL.

- Answer: `https://files-wetransfer.com/upload/session/ab12cd34ef56/dump_2025.dmp`

---

**What was UpdateAgent.exe labelled by Threat Intel on DESKTOP-DEV01?**

Check threat intelligence classification.

Internal tool, not malware.

- Answer: `Known internal IT utility tool`

---

## Task 8: Conclusion

**No answer needed**

---

## Quick Reference

### EDR vs Antivirus

| Feature | Antivirus | EDR |
|---------|-----------|-----|
| Detection | Signature-based | Behavioral + Signatures |
| Scope | File scanning | Full endpoint monitoring |
| Coverage | Known threats | Known + unknown threats |
| Response | Block/quarantine | Investigate + remediate |
| Visibility | Limited | Complete timeline |

### EDR Components

```
[Endpoint with Agent] 
        ↓ (collects telemetry)
[Management Server] 
        ↓ (processes & stores)
[Dashboard/Console] 
        ↓ (analyst investigates)
[Response Actions]
```

### Telemetry Checklist

- [ ] Process creation/execution
- [ ] Network connections
- [ ] File operations
- [ ] Registry modifications
- [ ] User authentication
- [ ] Command-line arguments
- [ ] DLL loads
- [ ] Driver loads

### Common Attack Indicators

**Suspicious Process Activity:**
- CMD.exe spawning download tools
- PowerShell with encoded commands
- Processes from Temp folders
- Unusual parent-child relationships

**Suspicious Network Activity:**
- Connections to file-sharing sites
- C2 beaconing patterns
- Large data transfers
- Connections to suspicious domains

**Suspicious File Operations:**
- Files in Public/Temp folders
- Executable file downloads
- Registry persistence changes
- Files with random names

### Investigation Tips

**Process Analysis:**
1. Check parent process
2. Review command-line arguments
3. Verify file location
4. Check digital signature

**Network Analysis:**
1. Identify destination IP/domain
2. Check reputation
3. Review transferred data
4. Correlate with threat intel

**File Analysis:**
1. Check file path
2. Review file hash
3. Check threat intel
4. Analyze file metadata

### Common Malware Locations

```
C:\Users\Public\
C:\Users\[username]\AppData\Local\Temp\
C:\Windows\Temp\
C:\ProgramData\
```

### EDR Response Actions

**Containment:**
- Isolate endpoint
- Kill malicious process
- Block network connections
- Quarantine files

**Investigation:**
- Collect forensic data
- Review full timeline
- Check lateral movement
- Identify patient zero

**Remediation:**
- Remove malware
- Restore from backup
- Reset credentials
- Patch vulnerabilities

---

## Key Takeaways

- **EDR provides visibility** into endpoint activity
- **Agents/sensors** collect telemetry data
- **Behavioral detection** catches unknown threats
- **Process trees** show parent-child relationships
- **cmd.exe spawning tools** is suspicious
- **Temp folder executables** are red flags
- **Network telemetry** detects C2 and exfiltration
- **Registry monitoring** catches persistence
- **IOC matching** uses threat intelligence
- **CURL.exe from CMD** = likely malware download
- **svchost.exe** is commonly hijacked by attackers
- **Antivirus** misses behavioral/fileless attacks
- **EDR** monitors what happens, not just what files look like
- **Threat intel** helps classify false positives

EDR gives you the full story, not just a snapshot
Happy hunting
