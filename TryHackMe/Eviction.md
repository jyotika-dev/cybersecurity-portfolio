# Eviction - TryHackMe

> Use MITRE ATT&CK Navigator to track and stop APT28

**Room Link:** https://tryhackme.com/room/eviction

<img width="1321" height="925" alt="image" src="https://github.com/user-attachments/assets/356ecd0f-9fbf-491b-8176-d2fbb9ccb159" />


---

## Scenario

Sunny is a SOC analyst at E-corp (rare earth metals manufacturer). Intelligence reports suggest APT28 might be targeting similar organizations. Use MITRE ATT&CK Navigator to identify APT28's TTPs and stop the attack.

**APT28 Background:**
- Attributed to Russia's GRU
- Military unit 26165
- Active since at least 2004

**Navigator Layer:** https://static-labs.tryhackme.cloud/sites/eviction

---

## Questions

### Q1: What is a technique used by the APT to both perform recon and gain initial access?

**Analysis:**

Look at two tactics in ATT&CK Navigator:
- Reconnaissance column
- Initial Access column
- Answer: `Spearphishing link`

---

### Q2: Which accounts might the APT compromise while developing resources?

**Analysis:**

Check the **Resource Development** tactic.

**Relevant technique:** Email Accounts

- Answer: `Email accounts`

---

### Q3: What two techniques of user execution should Sunny look out for?

**Analysis:**

Check the **Execution** tactic for social engineering techniques.

**Two techniques:**
1. **Malicious File** - User opens infected document
2. **Malicious Link** - User clicks malicious URL
3. 
- Answer: `Malicious File and Malicious Link`

---

### Q4: Which scripting interpreters should Sunny search for to identify successful execution?

**Analysis:**

Still in **Execution** tactic.

Look for scripting language techniques (highlighted in blue).

**Two interpreters:**
1. **PowerShell** - Common Windows scripting
2. **Windows Command Shell** - cmd.exe execution

- Answer: `PowerShell and Windows Command Shell`

---

### Q5: Which registry keys should Sunny observe to track persistence changes?

**Analysis:**

Question mentions "maintaining persistence" - check **Persistence** tactic.

**Relevant technique:** Registry Run Keys

- Answer: `Registry run keys`

---

### Q6: Which system binary's execution should Sunny scrutinize for proxy execution?

**Analysis:**

"Evading defenses" - check **Defense Evasion** tactic.

Look for system binary technique.

**Answer:** Rundll32

- Answer: `Rundll32`

---

### Q7: Which technique might the APT be using tcpdump for?

**Analysis:**

Check **Discovery** tactic.

**What is tcpdump?**
- Network packet analyzer
- Captures network traffic
- Runs on command line
- Discovers network devices

**Relevant technique:** Network Sniffing

- Answer: `Network sniffing`

---

### Q8: Which remote services should Sunny observe for lateral movement?

**Analysis:**

Check **Lateral Movement** tactic.

Only 3 relevant techniques here.

**Answer:** SMB/Windows Admin Shares

- Answer: `SMB/Windows Admin Shares`

---

### Q9: Which information repository can be the likely target?

**Analysis:**

Check **Collection** tactic.

APT28's goal: Steal intellectual property.

**Target:** SharePoint

- Answer: `Sharepoint`

---

### Q10: What types of proxy might the APT use for C2 communication?

**Analysis:**

Check **Command and Control** tactic.

Look for proxy techniques (bottom of list).

**Two techniques:**
1. **External Proxy** - Route through external servers
2. **Multi-hop Proxy** - Chain multiple proxies

- Answer: `External Proxy and Multi-hop Proxy`

---

## Quick Reference

### APT28 Attack Chain

```
Reconnaissance → Resource Development → Initial Access → Execution → 
Persistence → Defense Evasion → Discovery → Lateral Movement → 
Collection → Command & Control → (Exfiltration blocked!)
```

### Techniques by Tactic

| Tactic | Key Techniques |
|--------|---------------|
| **Reconnaissance** | Spearphishing Link |
| **Resource Development** | Email Accounts |
| **Initial Access** | Spearphishing Link |
| **Execution** | Malicious File/Link, PowerShell, cmd.exe |
| **Persistence** | Registry Run Keys |
| **Defense Evasion** | Rundll32 |
| **Discovery** | Network Sniffing (tcpdump) |
| **Lateral Movement** | SMB/Windows Admin Shares |
| **Collection** | SharePoint |
| **Command & Control** | External/Multi-hop Proxy |

### Technique IDs Reference

- **T1598/003** - Spearphishing Link (Recon)
- **T1586/002** - Email Accounts
- **T1204/001** - Malicious Link
- **T1204/002** - Malicious File
- **T1059/001** - PowerShell
- **T1059/003** - Windows Command Shell
- **T1547/001** - Registry Run Keys
- **T1218/011** - Rundll32
- **T1040** - Network Sniffing
- **T1021/002** - SMB/Windows Admin Shares
- **T1213/002** - SharePoint
- **T1090/001** - External Proxy
- **T1090/003** - Multi-hop Proxy

### Detection Points

**Reconnaissance:**
- Monitor for phishing emails
- Suspicious links in emails
- User clicking patterns

**Execution:**
- PowerShell script execution
- cmd.exe spawning unusual processes
- Office documents with macros

**Persistence:**
- Registry Run Keys modifications
- Startup folder changes
- Scheduled tasks

**Defense Evasion:**
- Rundll32.exe execution
- Unusual DLL loads
- Process injection

**Discovery:**
- tcpdump installation
- Network scanning tools
- Enumeration commands

**Lateral Movement:**
- SMB connections
- Admin share access
- Pass-the-hash attempts

**Collection:**
- SharePoint access logs
- Large data transfers
- Unusual file access

**C2 Communication:**
- Proxy usage
- Tor connections
- VPN tunnels

### MITRE Navigator Tips

**Color coding:**
- Blue highlights = Techniques used
- Darker = More frequently used
- Click technique for details

**Navigation:**
- Columns = Tactics
- Rows = Techniques
- Expand for sub-techniques

**Analysis:**
- Compare multiple groups
- Export as JSON/Excel
- Create custom layers

### Common APT28 Tools

- **Malware:** X-Agent, Sofacy, Chopstick
- **Exploits:** CVE-based exploits
- **Scripts:** PowerShell, VBScript
- **Utilities:** Mimikatz, tcpdump

### Defense Strategies

**Prevent Initial Access:**
- Email filtering
- User awareness training
- Link scanning

**Detect Execution:**
- PowerShell logging
- Command-line auditing
- Script block logging

**Stop Persistence:**
- Registry monitoring
- Startup item auditing
- Scheduled task reviews

**Block Lateral Movement:**
- Segment networks
- Disable SMB v1
- Monitor admin share access

**Protect Data:**
- SharePoint access controls
- Data loss prevention
- Activity monitoring

**Disrupt C2:**
- Block known proxies
- Monitor Tor traffic
- Inspect encrypted traffic

---

## Key Takeaways

- **APT28** is Russian GRU-linked threat group
- **Spearphishing links** used for both recon and initial access
- **Email accounts** compromised during resource development
- **Malicious files/links** trick users into execution
- **PowerShell and cmd.exe** are execution interpreters
- **Registry Run Keys** maintain persistence
- **Rundll32** evades detection via proxy execution
- **tcpdump** used for network discovery
- **SMB/Admin Shares** enable lateral movement
- **SharePoint** is target for IP theft
- **Proxy chains** obscure C2 communication
- **MITRE ATT&CK Navigator** maps threat group TTPs
- **Understanding tactics** helps predict attacker moves
- **Layered detection** catches attacks at multiple stages

Track adversary TTPs to stay one step ahead
Happy hunting
