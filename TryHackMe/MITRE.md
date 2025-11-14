# MITRE - TryHackMe

> Explore MITRE's cyber security frameworks and resources

**Room Link:** https://tryhackme.com/room/mitre

<img width="1556" height="925" alt="image" src="https://github.com/user-attachments/assets/49083caa-0bb2-4e83-82b7-495c428dd554" />


---

## Task 1: Introduction

MITRE is a not-for-profit organization conducting research in cyber security, AI, healthcare, and space systems.

**Learning objectives:**
- Understand MITRE ATT&CK Framework
- Apply ATT&CK in security work
- Use CTI with ATT&CK Matrix
- Discover other MITRE frameworks

**No answer needed**

---

## Task 2: ATT&CK® Framework

### What is ATT&CK?

**ATT&CK** = Adversarial Tactics, Techniques, and Common Knowledge

Global knowledge base of adversary tactics and techniques based on real-world observations.

### Understanding TTPs

**Tactic**
- Adversary's goal
- The "why" of an attack

**Technique**
- How goal is achieved
- The "how" of an attack

**Procedure**
- Implementation details
- How technique is executed

### ATT&CK Matrix

Visual representation of all tactics and techniques.

**Structure:**
- Tactics across the top
- Techniques nested below
- Sub-techniques under each technique

### Example Flow

**1. Tactic:** Reconnaissance (goal)

**2. Technique:** Active Scanning (method)

**3. Sub-technique:** 
- Scanning IP Blocks
- Vulnerability Scanning
- Wordlist Scanning

### Technique Pages Include

- Description
- Technique ID
- Procedure examples
- Mitigations
- Detections
- References

### Questions

**What Tactic does the Hide Artifacts technique belong to in the ATT&CK Matrix?**
- Answer: `Defense Evasion`

---

**Which ID is associated with the Create Account technique?**
- Answer: `T1136`

---

## Task 3: ATT&CK in Operation

### Why ATT&CK Matters

**Standard Language:**
- Consistent terminology
- Unique IDs for techniques
- Better communication
- Easier data comparison

**Bridges Gap:**
- Links threat intelligence to defense
- Translates intel into detections
- Creates actionable queries
- Builds playbooks

### Who Uses ATT&CK?

| Team | Purpose |
|------|---------|
| CTI Teams | Map threat actor behavior to TTPs |
| SOC Analysts | Link activity to techniques, prioritize alerts |
| Detection Engineers | Map SIEM/EDR rules to ATT&CK |
| Incident Responders | Map incident timelines to techniques |
| Red/Purple Teams | Build emulation plans, test defenses |

### Mustang Panda Example

**Group:** G0129

**Targets:** Government, non-profits, NGOs

**TTPs:**
- Phishing for initial access
- Scheduled tasks for persistence
- File obfuscation for evasion
- Ingress tool transfer for C2

### Questions

**In which country is Mustang Panda based?**
- Answer: `China`

---

**Which ATT&CK technique ID maps to Mustang Panda's Reconnaissance tactics?**
- Answer: `T1598`

---

**Which software is Mustang Panda known to use for Access Token Manipulation?**
- Answer: `Cobalt Strike`

---

## Task 4: ATT&CK for Threat Intelligence

### The Scenario

You're a security analyst in the aviation sector migrating to cloud.

**Task:** Research APT groups targeting aviation sector.

### Questions

**Which APT group has targeted the aviation sector and has been active since at least 2013?**
- Answer: `APT33`

---

**Which ATT&CK sub-technique used by this group is a key area of concern for companies using Office 365?**
- Answer: `Cloud Accounts`

---

**According to ATT&CK, what tool is linked to the APT group and the sub-technique you identified?**
- Answer: `Ruler`

---

**Which mitigation strategy advises removing inactive or unused accounts to reduce exposure to this sub-technique?**
- Answer: `User Account Management`

---

**What Detection Strategy ID would you implement to detect abused or compromised cloud accounts?**
- Answer: `DET0546`

---

## Task 5: Cyber Analytics Repository (CAR)

### What is CAR?

Knowledge base of detection analytics based on ATT&CK.

**Purpose:**
- Ready-made detection analytics
- Maps to ATT&CK techniques
- Provides example queries
- Validates detections

### CAR Structure

**Each Analytic Includes:**
- Description
- ATT&CK tactics/techniques
- Implementations (Pseudocode, Splunk, LogPoint)
- Unit Tests (sometimes)

### Example: CAR-2020-09-001

**Analytic:** Scheduled Task - File Access

**Provides:**
- Pseudocode explanation
- Splunk query example
- LogPoint search example

### Questions

**Which ATT&CK Tactic is associated with CAR-2019-07-001?**
- Answer: `Defense Evasion`

---

**What is the Analytic Type for Access Permission Modification?**
- Answer: `Situational Awareness`

---

## Task 6: MITRE D3FEND Framework

### What is D3FEND?

**D3FEND** = Detection, Denial, and Disruption Framework Empowering Network Defense

**Purpose:**
- Maps defensive techniques
- Shows how controls work
- Links to ATT&CK techniques
- Common defense language

### D3FEND Matrix

**Seven Tactics:**
- Model
- Harden
- Detect
- Isolate
- Deceive
- Evict
- Restore

### Example: Credential Rotation (D3-CRO)

**Purpose:** Regular password rotation

**Benefits:**
- Prevents credential reuse
- Limits stolen credential lifespan
- Links to ATT&CK techniques

### Questions

**Which sub-technique of User Behavior Analysis would you use to analyze the geolocation data of user logon attempts?**
- Answer: `User Geolocation Logon Pattern Analysis`

---

**Which digital artifact does this sub-technique rely on analyzing?**
- Answer: `Network Traffic`

---

## Task 7: Other MITRE Projects

### Adversary Emulation Library

**Purpose:** Step-by-step emulation plans

**Provides:**
- Real-world threat group emulations
- Detailed attack procedures
- Free resource for testing

### Caldera

**What it is:** Automated adversary emulation tool

**Features:**
- Simulates real-world attacks
- Uses ATT&CK framework
- Tests defenses
- Red and blue team exercises

### Emerging Frameworks

**AADAPT**
- Digital asset payment technologies
- Blockchain, smart contracts
- Digital wallets
- New framework

**ATLAS**
- AI/ML system threats
- Machine learning vulnerabilities
- AI-specific attacks
- Emerging threat landscape

### Questions

**What technique ID is associated with Scrape Blockchain Data in the AADAPT framework?**
- Answer: `ADT3025`

---

**Which tactic does LLM Prompt Obfuscation belong to in the ATLAS framework?**
- Answer: `Defense Evasion`

---

## Task 8: Conclusion

**No answer needed**

---

## Quick Reference

### MITRE Frameworks Comparison

| Framework | Focus | Purpose |
|-----------|-------|---------|
| **ATT&CK** | Offensive TTPs | Understand attacker behavior |
| **CAR** | Detection Analytics | Build detections from TTPs |
| **D3FEND** | Defensive Techniques | Implement security controls |
| **AADAPT** | Blockchain/Crypto | Defend digital assets |
| **ATLAS** | AI/ML Security | Protect AI systems |

### ATT&CK Key Concepts

**TTP Breakdown:**
```
Tactic (Why) → Technique (How) → Procedure (Implementation)
```

**Example:**
```
Reconnaissance → Active Scanning → Vulnerability Scanning
```

### ATT&CK Matrix Structure

```
[Tactics across top]
    ↓
[Techniques below]
    ↓
[Sub-techniques nested]
```

### Common Tactics

- Initial Access
- Execution
- Persistence
- Privilege Escalation
- Defense Evasion
- Credential Access
- Discovery
- Lateral Movement
- Collection
- Command and Control
- Exfiltration
- Impact

### Technique ID Format

- **T####** - Technique (e.g., T1136)
- **T####.###** - Sub-technique (e.g., T1136.001)

### CAR Components

**Pseudocode:**
- Human-readable logic
- Describes detection

**Implementation:**
- Splunk queries
- LogPoint searches
- Tool-specific examples

**Unit Tests:**
- Validate analytics
- Test detection logic

### D3FEND Tactics

1. **Model** - Understand system
2. **Harden** - Reduce attack surface
3. **Detect** - Identify threats
4. **Isolate** - Contain threats
5. **Deceive** - Mislead attackers
6. **Evict** - Remove threats
7. **Restore** - Recover systems

### Technique ID Formats

**ATT&CK:** T####
**CAR:** CAR-YYYY-MM-###
**D3FEND:** D3-XXX

### Using ATT&CK Effectively

**For Defense:**
1. Map security controls to techniques
2. Identify coverage gaps
3. Prioritize implementations
4. Build detection rules

**For Threat Intel:**
1. Profile threat actors
2. Map observed TTPs
3. Predict future activity
4. Share intelligence

**For Incident Response:**
1. Map incident timeline
2. Identify techniques used
3. Find related campaigns
4. Improve detections

### Emulation Tools

**Caldera:**
- Automated emulation
- ATT&CK-based
- Red/blue team use
- Free tool

**Emulation Library:**
- Real threat groups
- Step-by-step plans
- Test defenses
- Community-maintained

---

## Key Takeaways

- **ATT&CK** is the industry standard for describing threats
- **TTPs** = Tactics, Techniques, Procedures
- **Each technique** has unique ID for reference
- **Mustang Panda** (G0129) targets government/NGOs
- **APT33** active since 2013, targets aviation
- **Cloud Accounts** concern for Office 365 users
- **CAR** provides ready-made detection analytics
- **D3FEND** maps defensive techniques
- **Pseudocode** describes detection logic
- **Credential Rotation** (D3-CRO) prevents reuse
- **AADAPT** for blockchain security
- **ATLAS** for AI/ML security
- **Caldera** automates adversary emulation
- **ATT&CK Navigator** visualizes technique coverage
- **Groups section** profiles threat actors

MITRE frameworks provide the language and tools for modern cyber defense
Happy defending!
