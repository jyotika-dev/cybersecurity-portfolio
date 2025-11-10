# Incident Response Fundamentals - TryHackMe

> Learn how to perform Incident Response in cyber security

**Room Link:** https://tryhackme.com/room/incidentresponsefundamentals

<img width="1485" height="863" alt="image" src="https://github.com/user-attachments/assets/55e71b77-383e-4710-9df3-af0fa8327eed" />

---

## Task 1: Introduction to Incident Response

### What is Incident Response?

Incident Response (IR) is the process of handling security incidents when they occur. It's like being a cyber firefighter - you need to respond quickly, contain the damage, and prevent it from spreading!

### Why is IR Important?

**Minimize Damage**
- Stop attacks quickly
- Reduce financial loss
- Protect sensitive data

**Learn and Improve**
- Understand attack methods
- Strengthen defenses
- Prevent future incidents

**Meet Compliance**
- Legal requirements
- Industry standards
- Data protection laws

**No answer needed**

---

## Task 2: What are Incidents?

### Understanding Security Events

**Event** - Any observable occurrence in a system
- User login
- File access
- Network connection

**Alert** - Notification that suspicious activity was detected
- Triggered by security tools
- Requires investigation

**Incident** - Confirmed harmful or malicious activity
- Security breach
- Data theft
- System compromise

### Types of Alerts

**True Positive (TP)**
- Alert correctly identifies real threat
- Example: Detecting actual malware

**False Positive (FP)**
- Alert incorrectly flags benign activity
- Example: Smoke alarm triggered by cooking (not fire!)

**True Negative (TN)**
- No alert when there's no threat
- System working correctly

**False Negative (FN)**
- No alert when there IS a threat
- Most dangerous! Threat goes undetected

### Security Tools

**SIEM (Security Information and Event Management)**
- Collects logs from multiple sources
- Correlates events
- Generates alerts
- Examples: Splunk, QRadar, ArcSight

**EDR (Endpoint Detection and Response)**
- Monitors endpoints (computers, servers)
- Detects suspicious behavior
- Responds to threats automatically
- Examples: CrowdStrike, Carbon Black

### Questions

**What is triggered after an event or group of events point at a harmful activity?**
- Answer: `Alert`

---

**If a security solution correctly identifies a harmful activity from a set of events, what type of alert is it?**

This is detecting real threat = True Positive

- Answer: `true positive`

---

**If a fire alarm is triggered by smoke after cooking, is it a true positive or a false positive?**

The alarm triggered, but there's no actual fire - it's a false alarm!

- Answer: `false positive`

---

## Task 3: Types of Incidents

### Common Incident Types

### 1. Malware Infection

**What it is:**
- Malicious software on systems
- Viruses, trojans, ransomware

**How it happens:**
- Email attachments
- Malicious downloads
- Infected USB drives

**Example:** User downloads file from phishing email

### 2. Phishing Attack

**What it is:**
- Fake emails/messages
- Tricks users into giving credentials
- Impersonates legitimate organizations

**How it happens:**
- Spoofed emails
- Fake login pages
- Urgent requests

### 3. Denial of Service (DoS/DDoS)

**What it is:**
- Overwhelms system with traffic
- Makes service unavailable
- Disrupts business operations

**How it happens:**
- Flood of requests
- Resource exhaustion
- Network saturation

### 4. Unauthorized Access

**What it is:**
- Accessing systems without permission
- Stolen credentials
- Privilege escalation

**How it happens:**
- Weak passwords
- Exploiting vulnerabilities
- Social engineering

### 5. Data Breach

**What it is:**
- Unauthorized data access/theft
- Sensitive information exposed
- Customer data stolen

**How it happens:**
- Compromised databases
- Insider threats
- Misconfigured systems

### 6. Insider Threat

**What it is:**
- Threat from within organization
- Employees, contractors, partners
- Intentional or unintentional

**How it happens:**
- Disgruntled employees
- Negligence
- Compromised accounts

### Questions

**A user's system got compromised after downloading a file attachment from an email. What type of incident is this?**

User downloaded malicious file = Malware!

- Answer: `malware infection`

---

**What type of incident aims to disrupt the availability of an application?**

Disrupting availability = Denial of Service

- Answer: `Denial of service`

---

## Task 4: Incident Response Process

### SANS Incident Response Lifecycle

**6 Phases:**

**1. Preparation**
- Set up IR team
- Create policies and procedures
- Train staff
- Deploy security tools

**2. Identification**
- Detect incidents
- Analyze alerts
- Determine if real threat
- Document findings

**3. Containment**
- Stop threat from spreading
- Short-term: Isolate systems
- Long-term: Apply patches

**4. Eradication**
- Remove threat completely
- Delete malware
- Close vulnerabilities
- Patch systems

**5. Recovery**
- Restore systems to normal
- Monitor for signs of threat
- Verify systems are clean
- Resume operations

**6. Lessons Learned**
- Review what happened
- Document improvements
- Update procedures
- Train team

### NIST Incident Response Lifecycle

**4 Phases:**

**1. Preparation**
- Similar to SANS preparation
- Tools, policies, training

**2. Detection and Analysis**
- Combines SANS Identification
- Analyze and confirm incidents

**3. Containment, Eradication, and Recovery**
- Combines 3 SANS phases
- Stop, remove, restore

**4. Post-Incident Activity**
- Same as SANS Lessons Learned
- Review and improve

### Comparison

| SANS | NIST |
|------|------|
| 6 Phases | 4 Phases |
| More detailed | More condensed |
| Both widely used | Both effective |

### Questions

**The Security team disables a machine's internet connection after an incident. Which phase of the SANS IR lifecycle is followed here?**

Isolating the system = Containment phase

- Answer: `containment`

---

**Which phase of NIST corresponds with the lessons learned phase of the SANS IR lifecycle?**

Lessons learned = Post-Incident Activity

- Answer: `Post Incident Activity`

---

## Task 5: Incident Response Techniques

### What are Playbooks?

**Playbooks** are step-by-step guides for responding to specific incidents.

**Think of them as:**
- Recipe for handling incidents
- Checklist for responders
- Standard operating procedures

### Why Use Playbooks?

**Consistency**
- Everyone follows same steps
- Reduces errors
- Standardized response

**Speed**
- No need to think about what to do
- Pre-planned actions
- Faster response time

**Efficiency**
- Proven procedures
- Best practices
- Resource optimization

**Training**
- Easy to teach new team members
- Clear documentation
- Knowledge retention

### Playbook Example: Phishing Email

**Step 1: Identification**
- User reports suspicious email
- Security team reviews

**Step 2: Analysis**
- Check email headers
- Analyze links/attachments
- Determine if malicious

**Step 3: Containment**
- Block sender
- Delete emails from all mailboxes
- Isolate affected systems

**Step 4: Eradication**
- Remove malware if present
- Reset compromised credentials
- Update email filters

**Step 5: Recovery**
- Restore clean backups
- Monitor for reinfection
- Resume normal operations

**Step 6: Documentation**
- Record timeline
- Document actions taken
- Update playbook

### Components of a Good Playbook

**Clear Steps**
- Numbered actions
- Easy to follow
- No ambiguity

**Assigned Roles**
- Who does what
- Clear responsibilities
- Contact information

**Tools and Resources**
- Required software
- Access credentials
- Documentation links

**Decision Points**
- If/then scenarios
- Escalation criteria
- Alternative paths

### Questions

**Step-by-step comprehensive guidelines for incident response are known as?**
- Answer: `Playbooks`

---

## Task 6: Lab Work Incident Response

### The Scenario

You're a Security Analyst investigating a potential security incident. Let's walk through the investigation!

### Investigation Steps

**Step 1: Initial Alert**
- Review the alert dashboard
- Check what triggered the alert
- Gather basic information

**Step 2: Identify the Source**
- Find who sent the malicious email
- Check email headers
- Identify sender information

**Step 3: Determine Attack Vector**
- How did the attack happen?
- What method was used?
- Email attachment, link, etc.

**Step 4: Assess Impact**
- How many devices affected?
- What actions were taken?
- Who downloaded/executed?

**Step 5: Containment**
- Isolate affected systems
- Block malicious indicators
- Prevent spread

---

### Question 1: Malicious Email Sender

Look at the email details in the investigation dashboard.

Check the "From" field to find the sender's name.

- Answer: `Jeff Johnson`

---

### Question 2: Threat Vector

**Threat Vector** = How the attack was delivered

Look at how the malware was introduced:
- Email attachment? ✓
- Malicious link?
- USB drive?
- Network exploit?

- Answer: `Email Attachment`

---

### Question 3: Devices that Downloaded

Check the logs for download events.

Count how many different devices/users downloaded the attachment.

- Answer: `3`

---

### Question 4: Devices that Executed

**Important distinction:**
- Downloaded = File saved to computer
- Executed = File opened/run

Check which devices actually ran the malicious file.

- Answer: `1`

---

### Question 5: The Flag

After completing the investigation and following proper IR procedures, you'll find the flag!

Navigate through the lab environment and complete all steps.

- Answer: `THM{My_First_Incident_Response}`

---

## Task 7: Conclusion

Congratulations! You've learned the fundamentals of Incident Response!

### What You've Learned

✅ What incidents are and how they're detected
✅ Types of security incidents
✅ SANS and NIST IR frameworks
✅ Incident response phases
✅ Using playbooks for response
✅ Practical IR investigation

### Next Steps

- Practice with more IR scenarios
- Learn IR tools (SIEM, EDR)
- Create your own playbooks
- Study real-world incidents
- Join IR communities

**No answer needed**

---

## Quick Reference

### SANS IR Lifecycle

| Phase | Actions |
|-------|---------|
| 1. Preparation | Set up team, tools, procedures |
| 2. Identification | Detect and analyze incidents |
| 3. Containment | Stop threat from spreading |
| 4. Eradication | Remove threat completely |
| 5. Recovery | Restore normal operations |
| 6. Lessons Learned | Review and improve |

### NIST IR Lifecycle

| Phase | SANS Equivalent |
|-------|-----------------|
| 1. Preparation | Preparation |
| 2. Detection and Analysis | Identification |
| 3. Containment, Eradication, Recovery | Containment, Eradication, Recovery |
| 4. Post-Incident Activity | Lessons Learned |

### Types of Incidents

| Type | Description |
|------|-------------|
| Malware | Malicious software infection |
| Phishing | Fake emails to steal credentials |
| DoS/DDoS | Overwhelm systems with traffic |
| Unauthorized Access | Accessing systems without permission |
| Data Breach | Unauthorized data theft |
| Insider Threat | Threats from within organization |

### Alert Types

| Type | Meaning | Example |
|------|---------|---------|
| True Positive | Real threat detected | Actual malware found |
| False Positive | False alarm | Cooking smoke triggers fire alarm |
| True Negative | No alert, no threat | Normal operation |
| False Negative | No alert, but threat exists | Malware not detected |

### Essential IR Tools

| Tool | Purpose |
|------|---------|
| SIEM | Log collection and correlation |
| EDR | Endpoint monitoring and response |
| Network Monitor | Traffic analysis |
| Forensics Tools | Evidence collection |
| Ticketing System | Incident tracking |

---

## Key Takeaways

- **Incidents are confirmed threats**, not just events
- **Alerts notify** about suspicious activity
- **True positives** = Real threats detected
- **False positives** = False alarms (like cooking smoke)
- **Malware infection** from email attachments
- **DoS attacks** disrupt service availability
- **SANS has 6 phases**, NIST has 4
- **Containment** = Isolating the threat
- **Playbooks** = Step-by-step IR guides
- **Documentation** is critical throughout
- **Preparation** prevents panic during incidents
- **Lessons learned** improve future response
- **Every incident** is a learning opportunity
- **IR is a team effort** requiring coordination

Incident Response is crucial for protecting organizations from cyber threats. Understanding the process, using playbooks, and following established frameworks helps respond effectively to security incidents
