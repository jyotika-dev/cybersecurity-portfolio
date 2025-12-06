# Splunk Security Monitoring Dashboard

This project focuses on building an 8-panel Security Monitoring Dashboard in Splunk using Apache web access logs to help SOC analysts detect anomalies, suspicious activity, and early signs of attacks such as scanning, enumeration, bot behavior, and misconfigurations.

## Project Objective

Build a dashboard that helps SOC analysts monitor web traffic, detect irregular patterns, and identify potential threats such as scanning, enumeration, bot activity, and misconfigurations.

## Dashboard Overview — 8 Panels Implemented

The dashboard analyzes web server logs using visualizations that highlight traffic health, errors, suspicious IPs, and attacker-like behavior.

### Panels Included

1. Success vs Failure Overview
2. Error Trends Over Time
3. Top Offending IPs
4. Error Code Distribution
5. Error-Heavy User-Agents
6. Successful Requests After Repeated Failures
7. Suspicious IPs (Only Errors, No Success)
8. Redirect Analysis (301/304)

These panels collectively provide a complete picture of web traffic activity and anomalies.

---

## Detailed SPL Queries & Security Analysis

### 1. Success vs Failure Overview

**Objective:** Understand overall server health and traffic pattern.

```spl
index=main
| eval outcome=if(status=200 OR status=206, "Success", "Failure")
| stats count by outcome
```

**Key Insights:**
- High error ratio may indicate scanning, bugs, or misconfigurations.
- Success codes include 200 and 206.

**Visualization:** Pie Chart

<img width="1853" height="450" alt="image" src="https://github.com/user-attachments/assets/2bd1dc50-5c2b-460a-bda3-4f4b7b4bbc1a" />


---

### 2. Error Trends Over Time

**Objective:** Identify spikes or unusual error patterns.

```spl
index=main status>=400
| timechart count by status
```

**Key Insights:**
- 404 spikes → scanning or probing
- 403 spikes → unauthorized access attempts
- 500 spikes → potential exploitation attempts

**Visualization:** Line Chart

<img width="1814" height="434" alt="image" src="https://github.com/user-attachments/assets/cc8358a9-249a-46d4-aecb-8a48d3dd6364" />


---

### 3. Top Offending IPs

**Objective:** Identify IPs generating the highest number of errors.

```spl
index=main status>=400
| stats count by clientip
| sort -count
| head 10
```

**Key Insight:**  
High-error IPs often belong to bots, crawlers, scanners, or misconfigured clients.

**Visualization:** Bar Chart

<img width="1848" height="433" alt="image" src="https://github.com/user-attachments/assets/de38c019-c4cd-41fe-b475-5c5dcc2199de" />


---

### 4. Error Code Distribution

**Objective:** Determine which error types dominate.

```spl
index=main status>=400
| stats count by status
| sort -count
```

**Common Error Code Meanings:**
- **404** → path enumeration / scanning
- **403** → forbidden access attempts
- **500** → possible exploitation
- **416** → malformed/malicious ranges

**Visualization:** Bar Chart

<img width="1836" height="426" alt="image" src="https://github.com/user-attachments/assets/e1b31216-713b-4146-8b88-1f242b16bae1" />


---

### 5. Error-Heavy User-Agents

**Objective:** Identify suspicious tools or bots causing errors.

```spl
index=main status>=400
| stats count by useragent
| sort -count
| head 10
```

**Key Insights:**
- Suspicious user agents: `curl`, `python-requests`, unknown/blank values.
- Helps refine WAF rules.

**Visualization:** Bar Chart

<img width="1859" height="458" alt="image" src="https://github.com/user-attachments/assets/f3ee4309-ee61-479b-938e-d467c4daf2ad" />


---

### 6. Successful Requests After Multiple Failures

> **Note:**  
> This is not brute force since dataset lacks 401 events.  
> It represents scanning (404/403 errors) followed by a successful page load.

**Objective:** Spot IPs showing repeated errors before a successful request.

```spl
index=main (status=200 OR status=403 OR status=404 OR status=500)
| transaction clientip maxspan=5m
| search eventcount>5 AND status=200
```

**Key Insight:**  
Attackers commonly probe multiple invalid paths before reaching a valid one.

**Visualization:** Table

<img width="1832" height="879" alt="image" src="https://github.com/user-attachments/assets/af5ddb26-6e86-4fbe-abe2-3a057bbf9a2c" />


---

### 7. Suspicious IPs (Only Errors, No Success)

**Objective:** Identify pure malicious traffic.

```spl
index=main (status=200 OR status=206 OR status=301 OR status=304 OR status=403 OR status=404 OR status=500 OR status=416)
| stats count(eval(status IN (200,206,301,304))) as success 
        count(eval(status IN (403,404,500,416))) as failures 
        by clientip
| where failures>10 AND success=0
| sort -failures
```

**Key Insights:**
- Normal users show mixed success + failure.
- Only-failures indicate bots or scanners.

**Visualization:** Table

<img width="1860" height="175" alt="image" src="https://github.com/user-attachments/assets/46dda38e-68f9-45b5-b7a1-af0591a0c472" />


---

### 8. Redirect Analysis (301/304)

**Objective:** Identify aggressive crawling or redirect loops.

```spl
index=main status=301 OR status=304
| stats count by clientip
| sort -count
```

**Key Insights:**  
Excessive redirects from an IP may indicate:
- Cache poisoning
- Loop detection attempts
- Automated crawling

**Visualization:** Bar Chart

<img width="1845" height="418" alt="image" src="https://github.com/user-attachments/assets/bf3b5a98-c718-46da-9148-8a19851e4302" />


---

## 🛡 Security Monitoring Workflow

### 1. High-Level Health Check
- Review Success vs Failure
- Identify abnormal ratios

### 2. Error Spike Analysis
- Time-series monitoring
- Spot attack windows or anomalies

### 3. IP Reputation Investigation
- Analyze top offenders
- Validate against threat intel

### 4. Error Behavior Analysis
- **404** → scanning
- **500** → exploitation attempts
- **403** → unauthorized attempts

### 5. Bot & Automation Detection
- Investigate unusual or blank user-agents

### 6. Pattern-Based Detection
- Repeated failures before success
- IPs generating only errors

---

## 🧩 Key Takeaways

### SPL Skills Developed
- `stats`, `timechart`, `sort`, `where`, `top`
- `transaction` for correlation
- Field extraction and data enrichment
- Condition-based grouping using `eval`

### Security Skills Strengthened
- Identifying web attack patterns
- Enumeration detection
- Bot/scanner identification
- HTTP behavior analysis
- Dashboard design for SOC operations

### Operational Best Practices
- Thresholds for alerting (>10 errors)
- Multi-panel correlation
- Documentation of query logic
- Time-window-based pattern detection

---

## 🎯 Practical SOC Applications

### SOC Analysts Can Use This Dashboard For:
- Real-time monitoring
- Incident triage
- Reconnaissance detection
- Anomaly spotting
- Suspicious IP investigation

### Threat Hunters Can Use It For:
- Historical hunting
- Bot activity analysis
- Path enumeration patterns
- Error-based attack detection

---

## 🏁 Project Completion Summary

I successfully built an **8-panel Splunk Security Monitoring Dashboard** that detects:

- Traffic anomalies
- Scanning and enumeration behavior
- Suspicious IP patterns
- Unusual user-agent activity
- Redirect anomalies

This project demonstrates my ability to:

- Write effective SPL queries
- Build actionable SOC dashboards
- Analyze logs like a security analyst
- Translate raw data into meaningful security insights

---
