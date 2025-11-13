# SOC Workbooks and Lookups - TryHackMe

> Learn how to use SOC workbooks and lookup methods for efficient alert triage

**Room Link:** https://tryhackme.com/room/socworkbookslookups

<img width="1561" height="857" alt="image" src="https://github.com/user-attachments/assets/020f9a00-79d6-4d86-a13a-7398b6b3b537" />


---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Identity and Asset Inventory

### The Scenario

Alert: G.Baker logged into HQ-FINFS-02, downloaded "Financial Report US 2024.xlsx" and shared with R.Lund.

**Questions to answer:**
- Who is G.Baker? Role? Working hours?
- What is HQ-FINFS-02? Purpose? Location?
- Why would R.Lund need financial records?

### Key Concepts

**Identity Inventory:**
- Catalogue of employees (user accounts)
- Service accounts (machine accounts)
- Details: privileges, contacts, roles

**Asset Inventory:**
- List of computing resources
- Servers and workstations
- Hardware in IT environment

### Questions

**Looking at the identity inventory, what is the role of R.Lund at the company?**

Check identity inventory for R.Lund's role.

- Answer: `US Financial Adviser`

---

**Checking the asset inventory, what data does the HQ-FINFS-02 server store?**

Look up server purpose in asset inventory.

- Answer: `Financial records`

---

**Finally, does the file sharing from the scenario look legitimate and expected? (Yea/Nay)**

R.Lund is a Financial Adviser accessing financial records = legitimate!

- Answer: `Yea`

---

## Task 3: Network Diagrams

### Network Attack Scenario

Timeline:
- **08:00** - IP 103.61.240.174 connects to firewall port TCP/10443
- **08:23** - IP translated to internal 10.10.0.53
- **08:25** - 10.10.0.53 scans 172.16.15.0/24 (blocked)
- **08:32** - Scans 172.16.23.0/24 (ongoing)

### Network Diagrams Purpose

Visual schema showing:
- Existing locations
- Subnets
- Network connections

### Attack Reconstruction

Using network diagram:
1. Attacker (103.61.240.174) brute-forced VPN at vpn.tryhackme.thm
2. Successful login → assigned VPN subnet IP
3. Attempted Database Subnet scan (blocked by firewall)
4. Switched to Office Subnet scan

### Questions

**According to the network diagram, which service is exposed on the TCP/10443 port?**

Port 10443 = VPN service

- Answer: `VPN`

---

**Now, which subnet would the server behind 172.16.15.99 IP belong to?**

172.16.15.0/24 range = Database subnet

- Answer: `Database Subnet`

---

**Finally, does the scenario look like a True Positive (TP) or False Positive (FP)?**

VPN brute force + network scanning = Real attack!

- Answer: `TP`

---

## Task 4: SOC Workbooks

### What are SOC Workbooks?

**Also called:**
- Playbooks
- Runbooks  
- Workflows

**Purpose:**
Structured document defining investigation steps for specific threats.

### Why Use Workbooks?

**For L1 Analysts:**
- Junior specialists
- Need guidance
- Follow standardized steps
- Avoid mistakes

**Benefits:**
- Consistent analysis
- No missed steps
- Quality assurance
- Streamlined process

### Workbook Structure

**Three Logical Groups:**

**1. Enrichment:**
- Use Threat Intelligence
- Check identity inventory
- Gather user context

**2. Investigation:**
- Use gathered data
- Check SIEM logs
- Make verdict

**3. Escalation:**
- Escalate to L2 if needed
- Communicate with user
- Take action

### Questions

**Which SOC role would use workbooks the most?**

Junior analysts need guidance most.

- Answer: `SOC L1 Analyst`

---

**What is the process of gathering user, host, or IP context using TI and lookups?**

First step in workbooks = Enrichment

- Answer: `Enrichment`

---

**Looking at the workbook example, what platform is used as an identity inventory source?**

Check the workbook diagram for HR platform.

- Answer: `BambooHR`

---

## Task 5: Workbooks Practice

### Building Workbooks

Different teams have different approaches:

**Complex Approach:**
- Hundreds of detailed workbooks
- Like SOAR automation
- Every detection rule covered

**Simple Approach:**
- Few high-level workbooks
- Common attack vectors only
- Rely on analyst experience

### What L1 Analysts Need

**Skills required:**
- Divide investigation into modular blocks
- Build simple workbooks
- Organize logical flow

### Practice Exercise

**Steps:**
1. Click "View Site" button
2. Complete three workbook exercises
3. Drag and drop steps to correct positions
4. Steps will stick if placed correctly
5. Get flag after completion

### Questions

**What flag did you receive after completing the first workbook?**

Complete the first drag-and-drop workbook exercise.

Place all steps in correct order.

- Answer: `THM{workbook_1_completed}`

---

**What flag did you receive after completing the second workbook?**

Complete the second drag-and-drop workbook exercise.

Organize investigation steps properly.

- Answer: `THM{workbook_2_completed}`

---

**What flag did you receive after completing the third workbook?**

Complete the third drag-and-drop workbook exercise.

Build the final workbook correctly.

- Answer: `THM{workbook_3_completed}`

---

## Quick Reference

### Key Inventories

| Type | Contains |
|------|----------|
| Identity Inventory | Users, roles, privileges, contacts |
| Asset Inventory | Servers, workstations, hardware |

### Network Diagram Uses

- Understand network topology
- Identify subnet purposes
- Track attack paths
- Correlate IP addresses

### Workbook Components

| Phase | Purpose |
|-------|---------|
| Enrichment | Gather context (TI, lookups) |
| Investigation | Analyze and make verdict |
| Escalation | Take action or escalate |

### Investigation Questions

**For Users:**
- Role and responsibilities?
- Working hours?
- Access permissions?

**For Assets:**
- Server purpose?
- Data stored?
- Access restrictions?

**For Network:**
- Which subnet?
- What service?
- Expected traffic?

### Workbook Approaches

| Approach | Characteristics |
|----------|-----------------|
| Complex | Hundreds of workbooks, SOAR-like |
| Simple | Few high-level guides, analyst-driven |

---

## Common Scenarios

### Legitimate Activity

- User accessing appropriate data
- Within role permissions
- During work hours
- Expected behavior

### Suspicious Activity

- Off-hours access
- Unusual data access
- Network scanning
- VPN brute force

---

## Key Takeaways

- **Identity inventory** = user/account catalogue
- **Asset inventory** = server/workstation list
- **Network diagrams** show topology
- **R.Lund** = US Financial Adviser
- **Financial records** on HQ-FINFS-02
- **File sharing** was legitimate (Yea)
- **VPN** on TCP/10443
- **Database Subnet** = 172.16.15.x
- **Network scanning** = True Positive
- **SOC L1 Analysts** use workbooks most
- **Enrichment** = gathering context
- **BambooHR** for identity inventory
- **Three workbook phases** - Enrichment, Investigation, Escalation
- **Workbooks ensure consistency**
- **Modular investigation blocks** are essential
- **SOAR** = complex automation playbooks
- **Practice builds** workbook skills

SOC workbooks and proper lookups help analysts make accurate, efficient decisions during alert triage
Happy investigating
