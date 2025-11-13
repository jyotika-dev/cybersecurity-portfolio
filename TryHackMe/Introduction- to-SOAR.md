# SOAR - TryHackMe

> Learn about Security Orchestration, Automation and Response

**Room Link:** https://tryhackme.com/room/soar

<img width="1489" height="759" alt="image" src="https://github.com/user-attachments/assets/5382d0b9-df4a-4b86-a86f-92cb89b801c7" />


---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Security Operations Centres

### SOC Evolution

**First Generation:**
- Manual log review
- Basic monitoring
- Limited tools

**Second Generation:**
- SIEM tools introduced
- Centralized logging
- Better correlation

**Third Generation:**
- SOAR platforms
- Automation workflows
- Orchestration capabilities

### Alert Fatigue

**Problem:**
- Too many alerts
- Overwhelming for analysts
- Real threats get missed
- Analyst burnout

**Solution:** SOAR automation

### Questions

**Under which SOC generation did SIEM tools emerge?**

SIEM introduced in the second phase.

- Answer: `Second`

---

**How would you describe the experience of having an overload of security events being triggered within a SOC?**

Too many alerts overwhelming analysts.

- Answer: `Alert fatigue`

---

## Task 3: Security Orchestration, Automation and Response

### What is SOAR?

**Three Key Components:**

**Security Orchestration**
- Connects security tools
- Integrates systems
- Creates seamless workflows
- Tools work together

**Automation**
- Executes repetitive tasks
- Reduces manual work
- Speeds up response
- Consistent actions

**Response**
- Handles incidents
- Takes remediation actions
- Follows playbooks
- Closes incidents faster

### Playbooks

**Definition:** Predefined list of actions to handle incidents

**Examples:**
- Phishing email response
- Malware infection handling
- Data breach investigation
- Account compromise workflow

### Benefits

- Faster response times
- Consistent processes
- Reduced human error
- Better documentation
- Analyst focus on complex tasks

### Questions

**The act of connecting and integrating security tools and systems into seamless workflows is known as?**

Making tools work together.

- Answer: `Security Orchestration`

---

**What do we call a predefined list of actions to handle an incident?**

Step-by-step incident handling guide.

- Answer: `Playbook`

---

## Task 4: SOAR Workflows

### Workflow Components

**Automated Actions:**
- Extract IOCs from emails
- Query threat intelligence
- Generate file hashes
- Check reputation scores
- Update tickets

**Manual Actions:**
- Review analysis results
- Make final decisions
- Handle edge cases
- Investigate anomalies

### Why Manual Analysis Matters

**Not everything can be automated:**
- Complex decision-making
- Unknown threats
- False positive validation
- Business context needed

**Analysts remain critical** for:
- Final verdict
- Strategic decisions
- Complex investigations
- Unusual scenarios

### Questions

**Are manual analyses vital within a SOAR workflow? yay or nay?**

Humans still needed for complex decisions.

- Answer: `yay`

---

## Task 5: Threat Intel Workflow Practical

### The Scenario

You're a SOC Lead setting up SOAR automation. Build a threat intelligence workflow by activating workflow elements.

### Workflow Steps

**1. Case Ticket**
- Suspected emails received
- Isolated in sandbox
- Trigger creates ticket (TheHive)
- Document incident

**Click:** Computer with "Case Ticket"

---

**2. Data Extraction**
- Parse email for IOCs
- Extract URLs
- Extract attachments
- Identify indicators

**Click:** Computer with "Data Extraction"

---

**3. Threat Intel**
- Generate file hashes
- Prepare for analysis
- Collect IOC data

**Click:** Computer with "Threat Intel"

---

**4. Reputation Checks**
- VirusTotal analysis
- Check URLs
- Verify file hashes
- Automated scanning

**Click:** Computer with "Reputation Checks"

---

**5. Course of Action**
- Review results
- If no results → manual analysis
- If malicious → delete email
- Notify organization
- Update ticket
- Generate report

**Click:** Computer with "Course of Action"

---

**6. Run Workflow**
- All steps configured
- Test workflow
- Get smooth transition
- Receive flag

### Workflow Flow

```
Email Received → Sandbox → Create Ticket → Extract IOCs → Generate Hashes → 
Reputation Check → Manual Review (if needed) → Take Action → Update Ticket → Close
```

### Questions

**What is the flag received?**

Complete all workflow steps in order.

Click "Run" to test workflow.

- Answer: `THM{SOAR_AUTOMATION_ROCKS}`

---

## Task 6: Conclusion

**No answer needed**

---

## Quick Reference

### SOAR vs SIEM

| Feature | SIEM | SOAR |
|---------|------|------|
| Purpose | Log collection & correlation | Automation & orchestration |
| Function | Detect threats | Respond to threats |
| Focus | Visibility | Action |
| Generation | Second | Third |

### SOAR Benefits

**Speed:**
- Automated response
- Faster investigation
- Quick remediation

**Consistency:**
- Standard processes
- Playbook-driven
- Reduced errors

**Efficiency:**
- Less manual work
- Better resource use
- Analyst focus on hard problems

**Documentation:**
- Automatic logging
- Complete audit trail
- Better reporting

### Common Playbook Scenarios

**Phishing Response:**
1. Isolate email
2. Extract IOCs
3. Check reputation
4. Delete if malicious
5. Notify users
6. Update ticket

**Malware Infection:**
1. Isolate host
2. Collect samples
3. Analyze file hashes
4. Check threat intel
5. Remediate system
6. Document findings

**Account Compromise:**
1. Disable account
2. Reset credentials
3. Check login history
4. Identify lateral movement
5. Contain breach
6. Notify user

### Threat Intel Workflow Components

**Input:**
- Suspicious emails
- Malware samples
- URLs
- File attachments

**Processing:**
- IOC extraction
- Hash generation
- Reputation checks
- VirusTotal queries

**Analysis:**
- Automated scanning
- Manual review
- Verdict determination

**Output:**
- Ticket updates
- Email deletion
- User notifications
- Report generation

### Automation vs Manual Tasks

**Good for Automation:**
- IOC extraction
- Hash generation
- Reputation checks
- Ticket creation
- Log collection
- Standard responses

**Need Manual Review:**
- Final verdict
- Complex decisions
- Unknown threats
- Business impact
- Strategic choices
- Edge cases

### Tool Integration Examples

**Case Management:**
- TheHive
- ServiceNow
- Jira

**Threat Intelligence:**
- VirusTotal
- AlienVault OTX
- MISP

**Email Security:**
- Email gateways
- Sandbox solutions
- Spam filters

**Communication:**
- Slack
- Email notifications
- SMS alerts

---

## Key Takeaways

- **SIEM** emerged in second SOC generation
- **Alert fatigue** = too many alerts overwhelming analysts
- **Security Orchestration** connects tools together
- **Playbooks** are predefined action lists
- **Manual analysis** still vital in SOAR
- **SOAR** speeds up response times
- **Automation** handles repetitive tasks
- **Humans** make complex decisions
- **Workflows** standardize processes
- **Integration** makes tools work seamlessly
- **TheHive** is case management tool
- **VirusTotal** checks reputation
- **IOC extraction** can be automated
- **Sandbox** isolates suspicious content
- **Documentation** improves with automation

SOAR makes SOC teams faster and more efficient
