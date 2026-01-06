# TryHackMe Threat Modelling - Walkthrough

> Security Engineer Path - Threats and Risks

**Room Link:** https://tryhackme.com/room/threatmodelling

<img width="1455" height="898" alt="image" src="https://github.com/user-attachments/assets/d6ed4170-a8b9-45be-abc3-fc2111f48708" />


---

## Overview

**What you'll learn:**
- Threat modelling basics
- MITRE ATT&CK framework
- DREAD framework
- STRIDE framework
- PASTA framework

---

## Task 1: Introduction

**No answer needed**

Just read and click complete.

---

## Task 2: Threat Modelling Overview

### Key Concepts

**Threat:** Anything that can exploit vulnerabilities

**Vulnerability:** Weakness in system/application

**Risk:** Likelihood of being compromised

### Questions

**Q1: What is a weakness or flaw in a system that can be exploited?**

**Answer:** `Vulnerability`

**Q2: What is the process of developing diagrams to visualize architecture?**

**Answer:** `Asset Identification`

**Q3: What diagram describes and analyses potential threats?**

**Answer:** `Attack Tree`

---

## Task 3: Modelling with MITRE ATT&CK

### MITRE ATT&CK Framework

**What it is:**
- Knowledge base of adversary tactics
- Documents techniques attackers use
- Helps understand threat methods

### Questions

**Q1: What is the technique ID of "Exploit Public-Facing Application"?**

**Answer:** `T1190`

**Q2: Under what tactic does this technique belong?**

**Answer:** `Initial Access`

---

## Task 4: Mapping with ATT&CK Navigator

### ATT&CK Navigator Tool

**Purpose:**
- Visualize threat scenarios
- Map techniques to adversaries
- Filter by platform

### Questions

**Q1: How many MITRE ATT&CK techniques are attributed to APT33?**

**Steps:**
1. Search for APT33
2. Count techniques

**Answer:** `31`

**Q2: Upon applying IaaS platform filter, how many techniques are under Discovery tactic?**

**Steps:**
1. Apply IaaS filter
2. Go to Discovery tactic
3. Count techniques

**Answer:** `13`

---

## Task 5: DREAD Framework

### DREAD Components

**D** - Damage: Potential harm

**R** - Reproducibility: How easy to recreate

**E** - Exploitability: Effort needed to exploit

**A** - Affected Users: Number of users impacted

**D** - Discoverability: How easy to find vulnerability

### Questions

**Q1: What DREAD component assesses potential harm?**

**Answer:** `Damage`

**Q2: What DREAD component evaluates how easily others can find the vulnerability?**

**Answer:** `Discoverability`

**Q3: Which DREAD component considers number of impacted users?**

**Answer:** `Affected Users`

---

## Task 6: STRIDE Framework

### STRIDE Components

**S** - Spoofing: Impersonating users/systems

**T** - Tampering: Unauthorized data modification

**R** - Repudiation: Denying actions

**I** - Information Disclosure: Exposing information

**D** - Denial of Service: Disrupting availability

**E** - Elevation of Privilege: Gaining unauthorized access

### Questions

**Q1: What foundational information security concept does STRIDE build upon?**

**Answer:** `CIA Triad`

**Q2: What policy does Information Disclosure violate?**

**Answer:** `Confidentiality`

**Q3: Which STRIDE component involves unauthorized modification of data?**

**Answer:** `Tampering`

**Q4: Which STRIDE component refers to disruption of system availability?**

**Answer:** `Denial of Service`

**Q5: Flag for simulated threat modelling exercise?**

**Steps:**
1. Complete the interactive exercise
2. Match threats to STRIDE categories
3. Answer questions based on scenarios

**Answer:** `THM{m0d3ll1ng_w1th_STR1D3}`

---

## Task 7: PASTA Framework

### PASTA Steps

**7-step process:**
1. Define Objectives
2. Define Technical Scope
3. Decompose the Application
4. Analyse the Attacks
5. Vulnerability and Weakness Analysis
6. Attack Modelling
7. Risk and Impact Analysis

### Questions

**Q1: In which step do you break down the system into components?**

**Answer:** `Decompose the Application`

**Q2: During which step do you simulate potential attack scenarios?**

**Answer:** `Analyse the Attacks`

**Q3: In which step do you create an inventory of assets?**

**Answer:** `Define the Technical Scope`

**Q4: Flag for simulated threat modelling exercise?**

**Steps:**
1. Complete interactive rooms in order
2. Follow PASTA methodology
3. Answer questions for each step

**Answer:** `THM{c00k1ng_thr34ts_w_P4ST4}`

---

## Task 8: Conclusion

**No answer needed**

Read through and click complete!

---

## Quick Reference

### Framework Comparison

| Framework | Purpose | Components |
|-----------|---------|------------|
| MITRE ATT&CK | Adversary tactics | Tactics & Techniques |
| DREAD | Risk assessment | 5 risk factors |
| STRIDE | Threat categories | 6 threat types |
| PASTA | Attack simulation | 7-step process |

### CIA Triad

**C** - Confidentiality: Keep data private

**I** - Integrity: Keep data accurate

**A** - Availability: Keep systems accessible

### When to Use Each

**MITRE ATT&CK:**
- Understanding attacker techniques
- Mapping real-world threats
- Security gap analysis

**DREAD:**
- Scoring vulnerabilities
- Prioritizing fixes
- Risk assessment

**STRIDE:**
- Application threat modeling
- Identifying threat categories
- Design phase security

**PASTA:**
- Comprehensive threat analysis
- Attack simulation
- Risk-based approach

---

## Key Takeaways

- **Threat modelling** identifies security risks early
- **MITRE ATT&CK** documents adversary techniques
- **DREAD** scores vulnerability risk
- **STRIDE** categorizes threat types
- **PASTA** provides structured 7-step process
- **Frameworks** help systematic analysis
- **Early identification** reduces risk
- **Multiple approaches** for different needs
- **CIA Triad** is foundation
- **Practice** with interactive exercises

---

Happy hunting!
