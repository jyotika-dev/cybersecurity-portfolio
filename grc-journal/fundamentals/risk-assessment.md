# Risk = Threat x Vulnerability x Impact

## Core Components

* **THREAT:** A potential event or action that could harm the organization. Example: Cyberattacks
* **VULNERABILITY:** A weakness in the system that could be exploited by a threat. Example: Weak passwords
* **IMPACT:** The extent of damage or loss if the threat exploits the vulnerability. Example: Financial losses

The Risk Formula helps organization quantify the risks to prioritize mitigation and decision-making.

Using this formula organizations assesses the risks and its severity prioritizing them for mitigation on that basis 

---

## Real-World Example

An IT company has setup new hardware equipment

* Vulnerability: The hardware equipment still has the manufacturer's default username and password.
* Threat: The malicious attacker is actively scanning the internet for devices using those default credentials
* Impact: The attackers implemented the ransomware attack by capturing their client data. Resulting in financial losses, reputational damage and regulatory fines.

---

# Risk Likelihood, Impact and Scoring

## Key Purpose

* **Risk Likelihood:** Measures how likely a risk is going to occur
    * Rated on a scale (Example: 1-5)
    * Based on past incidents, controls and threat environments
    * Example scale:
        
        | Score | Description | Probability |
        | --- | --- | --- |
        | 1 | Rare | 0-5% |
        | 2 | Possibly | 6-20% |
        | 3 | Likely | 21-50% |
        | 4 | Very Likely | 51-80% |
        | 5 | Almost certain | >80% |
      
* **Risk Impact (Severity):** Indicates how damaging the event would be if it happened.
    * Rated on a similar scale 1-5 (example: low to catastrophic)
    * Example scale
        
        | Score | Description  | Example Consequences |
        | --- | --- | --- |
        | 1 | Insignificant | Temporary service glitch |
        | 2 | Minor | Slight delay in operations |
        | 3 | Moderate | Data exposure of few users |
        | 4 | Major | Service disruption |
        | 5 | Catastrophic | Major data breach |
      
* **Risk Score:**
    * Risk Score = Likelihood x Impact
    * Results are typically categorized as:
        
        | Score Range | Risk Level |
        | --- | --- |
        | 1-5 | Low |
        | 6-12 | Medium |
        | 12-20 | High |
    * The higher the combined score, the greater the priority for mitigation.

---
 
## Real-World Example

A Fintech company is assessing the risk of a phishing attack on its employees.

  * **Likelihood (Probability):** Based on the recent incidents and staff awareness, it is rated as 4/5 (very likely).
  * **Impact (Severity):** If successul, it may leak financial details, rated as 5/5 (severe impact - catastrophic).
  * Thus **Risk Score** = 4 x 5 = 20, classified as **High Risk**, requiring immediate action.
