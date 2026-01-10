# TryHackMe Risk Management - Walkthrough

> Understanding Risk Assessment and Response

**Room Link:** https://tryhackme.com/room/seriskmanagement

<img width="1315" height="908" alt="image" src="https://github.com/user-attachments/assets/b39237b7-72e7-4006-a691-e1f64874db8f" />

---

## Overview

**Topics covered:**
- Risk terminology
- Risk assessment methods
- Risk response strategies
- Risk monitoring
- Cost-benefit analysis

---

## Introduction

### Laptop Scenario Questions

**Q1: You bring extra laptop in case main one fails. What is this?**
- Answer: `Risk Reduction`

**Q2: You don't take extra actions because laptop never failed. What is this?**
- Answer: `Risk Acceptance`

---

## Basic Terminology

**Key terms:**

| Term | Definition |
|------|------------|
| Risk | Potential for loss or harm to assets |
| Vulnerability | Weakness that can be exploited |
| Threat | Anything that can harm assets |
| Asset | Valuable resource to protect |

### Questions

**Q1: Potential for loss or incident harming CIA?**
- Answer: `Risk`

**Q2: Weakness attacker could exploit?**
- Answer: `Vulnerability`

**Q3: What do you consider a business laptop?**
- Answer: `Asset`

**Q4: How to classify ransomware groups from business perspective?**
- Answer: `Threat`

---

## Risk Assessment

**NIST Methodology:**
- Prepare
- Conduct assessment
- Communicate results
- Maintain assessment

### Question

**Q: Risk assessment methodology developed by NIST?**
- Answer: `NIST SP 800-30`

---

## Risk Response Strategies

**Four main responses:**

1. **Risk Acceptance** - Do nothing
2. **Risk Avoidance** - Stop the activity
3. **Risk Reduction** - Implement controls
4. **Risk Transfer** - Insurance, outsource

---

## Calculating Safeguard Value

**Formula:**
```
ALE = (Asset Value × EF) × ARO

Safeguard Value = ALE before - ALE after - Cost

If positive → Implement
If negative → Don't implement
```

**Where:**
- **ALE** = Annual Loss Expectancy
- **EF** = Exposure Factor (% of loss)
- **ARO** = Annual Rate of Occurrence

---

## Respond to Risk (Interactive)

**Click "View Site" and calculate each safeguard**

### Safeguard 1

**Given:**
- Asset Value: $2,000
- EF before: 0.5, after: 0.1
- ARO: 2
- Cost: $20

**Calculate:**
```
ALE before = (2000 × 0.5) × 2 = 2000
ALE after = (2000 × 0.1) × 2 = 400
Value = 2000 - 400 - 20 = 1580
```

**Result:** Positive → **Justified** 

---

### Safeguard 2

**Given:**
- Asset Value: $10,000
- EF before: 0.25, after: 0
- ARO: 0.35
- Cost: $400

**Calculate:**
```
ALE before = (10000 × 0.25) × 0.35 = 875
ALE after = (10000 × 0) × 0.35 = 0
Value = 875 - 0 - 400 = 475
```

**Result:** Positive → **Justified** 

---

### Safeguard 3

**Given:**
- Asset Value: $2,000
- EF before: 0.5, after: 0.1
- ARO before: 0.25, after: 0.5
- Cost: $1,500

**Calculate:**
```
ALE before = (2000 × 0.5) × 0.25 = 250
ALE after = (2000 × 0.1) × 0.5 = 100
Value = 250 - 100 - 1500 = -1350
```

**Result:** Negative → **Not Justified** 

**Flag:** `THM{Excellent_Risk_Management}`

---

## Monitor Risk

### Questions

**Q1: Confirming laptop encryption policy mitigates data breach risk?**
- Answer: `Effectiveness`

**Q2: Keeping eye on new regulations and laws?**
- Answer: `Compliance`

---

## Putting It All Together

**Click "View Site" for final challenge**

### Control 1: Laptop Protection

**Given:**
- Asset: $2,500
- EF: 1 → 0.06
- ARO: 0.05
- Cost: $45

**Calculate:**
```
ALE before = (2500 × 1) × 0.05 = 125
ALE after = (2500 × 0.06) × 0.05 = 7.5
Value = 125 - 7.5 - 45 = 72.5
```

**Decision:** Justified 

**Final Flag:** `THM{OFFICE_RISK_MANAGED}`

---

### Control 2: Workstation

**Given:**
- Asset: $3,000
- EF: 0.7 → 0
- ARO: 0.2
- Cost: $200

**Calculate:**
```
ALE before = (3000 × 0.7) × 0.2 = 420
ALE after = (3000 × 0) × 0.2 = 0
Value = 420 - 0 - 200 = 220
```

**Decision:** Justified 

---

### Control 2: Phone

**Given:**
- Asset: $1,250
- EF: 1 → 0.4
- ARO: 0.35
- Cost: $10

**Calculate:**
```
ALE before = (1250 × 1) × 0.35 = 437.5
ALE after = (1250 × 0.4) × 0.35 = 175
Value = 437.5 - 175 - 10 = 252.5
```

**Decision:** Justified

---

### Control 3: Office

**Given:**
- Asset: $20,000
- EF: 1 → 0.15
- ARO: 0.1
- Cost: $750

**Calculate:**
```
ALE before = (20000 × 1) × 0.1 = 2000
ALE after = (20000 × 0.15) × 0.1 = 300
Value = 2000 - 300 - 750 = 950
```

**Decision:** Justified 

---

### Control 3: Workstation (Protection)

**Given:**
- Asset: $3,000
- EF: 0.1 → 0
- ARO: 0.05
- Cost: $250

**Calculate:**
```
ALE before = (3000 × 0.1) × 0.05 = 15
ALE after = (3000 × 0) × 0.05 = 0
Value = 15 - 0 - 250 = -235
```

**Decision:** Not Justified 

---

### Control 3: Laptop (Fire)

**Given:**
- Asset: $2,500
- EF: 1 → 0.24
- ARO: 0.2
- Cost: $75

**Calculate:**
```
ALE before = (2500 × 1) × 0.2 = 500
ALE after = (2500 × 0.24) × 0.2 = 120
Value = 500 - 120 - 75 = 305
```

**Decision:** Justified 

---

### Control 3: Workstation (Backup)

**Given:**
- Asset: $3,000
- EF: 0.85 → 0.05
- ARO: 0.35
- Cost: $75

**Calculate:**
```
ALE before = (3000 × 0.85) × 0.35 = 892.5
ALE after = (3000 × 0.05) × 0.35 = 52.5
Value = 892.5 - 52.5 - 75 = 765
```

**Decision:** Justified 

---

## Quick Reference

### Risk Formula

```
Step 1: Calculate ALE before
ALE = (Asset Value × EF) × ARO

Step 2: Calculate ALE after  
ALE = (Asset Value × EF) × ARO

Step 3: Calculate Value
Value = ALE before - ALE after - Cost

Step 4: Decision
Positive = Implement
Negative = Don't implement
```

### Decision Matrix

| Value | Decision |
|-------|----------|
| Positive (> 0) | Justified - Implement |
| Negative (< 0) | Not Justified - Skip |
| Zero (= 0) | Break even - Optional |

---

## Key Terms

**Asset Value:**
- Dollar value of asset
- What you'd lose if destroyed

**Exposure Factor (EF):**
- Percentage of loss
- 0.5 = 50% loss
- 1.0 = total loss

**Annual Rate of Occurrence (ARO):**
- How often per year
- 0.25 = once every 4 years
- 2 = twice per year

**Annual Loss Expectancy (ALE):**
- Expected yearly loss
- Without safeguard vs with safeguard

---

## Risk Monitoring

**What to monitor:**

**Effectiveness:**
- Are controls working?
- Reducing risk as expected?

**Compliance:**
- Following regulations?
- Meeting legal requirements?

**Changes:**
- New threats?
- New vulnerabilities?

**Cost:**
- Still worth the investment?
- Budget changes?

---

## Key Takeaways

- **Risk** = potential for loss
- **ALE** = expected annual loss
- **Safeguard justified** if value positive
- **Calculate before deciding** to implement
- **Cost matters** in risk decisions
- **Monitor effectiveness** of controls
- **Consider all factors** not just cost
- **Business decision** based on math
- **Positive value** = good investment
- **Negative value** = waste of money

---

Happy learning!
