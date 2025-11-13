# SOC Metrics and Objectives - TryHackMe

> Learn key metrics that drive SOC effectiveness and how to improve them

**Room Link:** https://tryhackme.com/room/socmetricsobjectives


<img width="1574" height="862" alt="image" src="https://github.com/user-attachments/assets/099a5238-c238-43e2-b89f-b4318cff8b7a" />


---

## Task 1: Introduction

SOC efficiency can be measured using different indicators and metrics. This room explores common evaluation approaches like MTTD and MTTR, and how to improve them.

**No answer needed**

---

## Task 2: Core Metrics

### SOC Main Goal

Protect the **confidentiality, integrity, and availability** of the organization's digital assets.

### The Four Core Metrics

**1. Alerts Count**

**Too many alerts (80+ per day):**
- Overwhelming for analysts
- Real threats get missed
- Analysts suffer burnout

**Too few alerts (0 per week):**
- SIEM might be broken
- Lack of visibility
- Undetected breaches

**Ideal range:** 5-30 alerts per day per L1 analyst

---

**2. False Positive Rate**

**High FP rate (80%+):**
- Analysts become less vigilant
- Threats treated as "just spam"
- Alert fatigue sets in

**Example:**
- 75 False Positives out of 80 alerts = 94% FP rate (BAD!)

**Target:** Keep below 80%

**Solution:** False Positive Remediation
- Tune detection rules
- Filter out system noise
- Exclude normal IT activity

---

**3. Alert Escalation Rate**

**What it measures:**
- How often L1 escalates to L2
- L1 independence level
- L1 experience

**Too high escalation:**
- L1 lacks confidence
- Needs more training
- Overloading L2

**Target:** Below 50%, ideally below 20%

---

**4. Threat Detection Rate (TDR)**

**Most critical metric!**

**Example scenario:**
- 6 attacks in 2025
- Detected 4 attacks
- Missed 1 (broken rule)
- Missed 1 (analyst error)
- TDR = 4/6 = 67% (VERY BAD!)

**Target:** Always aim for 100%

**Why:** Every missed threat = potential ransomware, data theft, breach

### Questions

**Is zero alerts for one month a good sign for your SOC team? (Yea/Nay)**

No alerts could mean SIEM is broken or no visibility.

- Answer: `Nay`

---

**What is the False Positive Rate if only 10 out of 50 alerts appear to be real threats?**

40 False Positives out of 50 total alerts.

FP Rate = (40/50) × 100 = 80%

- Answer: `80%`

---

## Task 3: Triage Metrics

### Service Level Agreement (SLA)

**What is it?**

Document signed between:
- Internal SOC team ↔ Company management
- Managed SOC (MSSP) ↔ Customers

**What it defines:**
- Required response times
- Detection speed requirements
- Remediation timelines

### The Three Triage Metrics

**MTTD - Mean Time To Detect**
- Time from attack start to alert generation
- Measures detection speed
- Depends on SIEM rule frequency

**MTTA - Mean Time To Acknowledge**
- Time from alert generation to analyst action
- Measures analyst response speed
- Shows if team is overwhelmed

**MTTR - Mean Time To Respond**
- Total time from alert to threat containment
- Includes: MTTA + investigation + remediation
- Most important for customers

### Metric Formulas

| Metric | Calculation |
|--------|-------------|
| **MTTD** | Attack start → Alert generated |
| **MTTA** | Alert generated → Analyst starts work |
| **MTTR** | Alert generated → Threat contained |

**Note:** MTTR includes MTTA time!

### Questions

**Imagine a scenario where the SOC team receives a critical alert on Saturday. If the team works 8/5, on which day of the week will they acknowledge the alert?**

8/5 schedule = Monday-Friday, 8 hours per day.

Weekend alerts wait until next business day.

- Answer: `Monday`

---

**Imagine a scenario where an employee was lured into running data stealer malware:**

**Timeline:**
1. Alert received after 12 minutes (MTTD)
2. L1 moved to "In Progress" 10 minutes later (MTTA)
3. Escalated after 6 minutes
4. L2 cleaned malware in 35 minutes

**Provide the MTTD, MTTA, and MTTR via comma (e.g. 10,20,30)**

**Breakdown:**
- **MTTD** = 12 minutes (attack to alert)
- **MTTA** = 10 minutes (alert to acknowledgment)
- **MTTR** = 10 + 6 + 35 = 51 minutes (alert to complete containment)

- Answer: `12,10,51`

---

## Task 4: Improving Metrics

### Why Metrics Matter to L1 Analysts

**Two main reasons:**

1. **Better security** - More efficient SOC = fewer successful attacks
2. **Career growth** - Good metrics = promotions and raises

### How to Improve Metrics

**Reduce Alerts Count:**
- Implement False Positive remediation
- Tune detection rules with SOC engineers
- Filter out known benign activity
- Use threat intelligence feeds

**Lower False Positive Rate:**
- Review and tune SIEM rules regularly
- Exclude system noise and IT maintenance
- Improve alert logic and thresholds
- Document known False Positive patterns

**Improve Detection Rate:**
- Create and use SOC workbooks
- Continuous training and education
- Regular tabletop exercises
- Learn from missed detections

**Speed Up Response Times:**
- Build clear escalation procedures
- Create remediation playbooks
- Automate common response actions
- Practice incident scenarios

**Reduce Escalation Rate:**
- Invest in L1 training programs
- Develop comprehensive workbooks
- Provide mentorship from L2/L3
- Build confidence through practice

### Team Collaboration

**Everyone contributes:**
- **L1** - Reports issues, follows workbooks
- **L2** - Mentors L1, validates detections
- **Engineers** - Tunes rules, fixes SIEM
- **Manager** - Tracks metrics, allocates resources

### Questions

**What is the False Positive Rate limit you should aim not to reach?**

Keep FP rate below this threshold.

- Answer: `80%`

---

**Should all SOC roles work together to keep metrics improving? (Yea/Nay)**

Improving metrics requires teamwork across all roles.

- Answer: `Yea`

---

## Task 5: Practice Scenarios

### Real-World Metric Problems

As L1 analyst, you're first to notice:
- Excessive alert numbers
- High False Positive rate
- Slow response times

**Your role:** Communicate issues and propose solutions

### Practice Exercise

Click "View Site" to complete three scenarios:
1. Identify the problematic metric
2. Select appropriate improvement task
3. Assign task to correct team member

**Scenario 1: Unhappy Customer**

**Problem:** MTTR too high, slow attack containment

**Root cause:** Lack of clear remediation procedures

**Solution:**
- Create workbook for credential rotation
- Present to team for training
- Assign to L1/L2 for implementation

- Answer: `THM{mttr:quick_start_but_slow_response}`

---

**Scenario 2: Delayed Alert**

**Problem:** MTTD of 20 minutes causes delayed triage

**Root cause:** Detection rules run infrequently

**Solution:**
- Tune SIEM to run rules more often (every 5 minutes)
- Schedule engineer review
- Assign to SOC Engineer

- Answer: `THM{mttd:time_between_attack_and_alert}`

---

**Scenario 3: Tired Analysts**

**Problem:** High False Positive Rate causing analyst burnout

**Root cause:** Too much noise in alerts

**Solution:**
- Implement False Positive remediation process
- Schedule team call to discuss
- Assign SOC engineers to exclude system/IT noise
- Tune detection rules

- Answer: `THM{fpr:the_main_cause_of_l1_burnout}`

---

## Task 6: Conclusion

**No answer needed**

---

## Quick Reference

### Core Metrics Summary

| Metric | Ideal Range | Problem Signs |
|--------|-------------|---------------|
| **Alerts Count** | 5-30/day per analyst | 80+ = overload, 0 = broken SIEM |
| **False Positive Rate** | Below 80% | 80%+ = alert fatigue |
| **Escalation Rate** | Below 20-50% | Too high = training needed |
| **Threat Detection Rate** | 100% | Below 100% = missed breaches |

### Triage Metrics Breakdown

```
Attack Occurs
    ↓
[MTTD - Mean Time To Detect]
    ↓
Alert Generated
    ↓
[MTTA - Mean Time To Acknowledge]
    ↓
Analyst Starts Work
    ↓
[Investigation + Remediation]
    ↓
Threat Contained
    ↓
[MTTR = MTTA + Investigation + Remediation]
```

### Common Problems & Solutions

**Problem: Too many alerts**
- Solution: False Positive remediation, rule tuning

**Problem: Slow detection**
- Solution: Increase SIEM rule frequency

**Problem: Slow response**
- Solution: Create workbooks, automate actions

**Problem: High escalation rate**
- Solution: More training, better documentation

**Problem: Analyst burnout**
- Solution: Reduce False Positives, improve work-life balance

### Improvement Task Owners

| Task Type | Owner |
|-----------|-------|
| Rule tuning | SOC Engineer |
| Workbook creation | L1/L2 Analysts |
| Training programs | SOC Manager + L2 |
| SIEM configuration | SOC Engineer |
| Process improvement | Entire team |

### SLA Typical Requirements

**Detection:**
- Critical alerts: < 5 minutes MTTD
- High severity: < 15 minutes MTTD

**Acknowledgment:**
- Critical alerts: < 15 minutes MTTA
- High severity: < 30 minutes MTTA

**Response:**
- Critical alerts: < 1 hour MTTR
- High severity: < 4 hours MTTR

---

## Key Takeaways

- **5-30 alerts/day** is the sweet spot per analyst
- **80% FP rate** is the danger threshold
- **100% detection rate** is mandatory - no exceptions
- **MTTR includes MTTA** - total time from alert to containment
- **8/5 schedule** means weekend alerts wait until Monday
- **False Positives** are the main cause of L1 burnout
- **MTTD** depends on SIEM rule frequency
- **Teamwork** is essential for metric improvement
- **Workbooks** speed up response times
- **L1 analysts** are first to notice metric problems
- **Career growth** follows good metric performance
- **Every missed threat** can lead to major incidents

Good metrics = efficient SOC = better security!
Happy analyzing
