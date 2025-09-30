# Splunk Day 4 - Security Monitoring Dashboard

This document summarizes my hands-on experience from Day 4 of Splunk training, focusing on creating a comprehensive security monitoring dashboard with multiple visualization panels.

**Lab Focus:** Building an interactive dashboard to monitor web traffic, detect anomalies, and identify potential security threats.

---

## Dashboard Overview

Created a security monitoring dashboard with 5 key panels to analyze web server logs and identify suspicious activities:

1. Success vs Failure Pie Chart
2. Error Trends Over Time
3. Top 10 Offending IPs
4. Error Code Distribution
5. Error-Heavy User-Agents

<img width="1866" height="910" alt="image" src="https://github.com/user-attachments/assets/d0d19a3b-2f67-4666-9d19-621d3753b999" />
<img width="1845" height="901" alt="image" src="https://github.com/user-attachments/assets/d0ec90b9-b935-4f66-87ba-95574a303e7a" />
<img width="1868" height="481" alt="image" src="https://github.com/user-attachments/assets/4f83db8d-6ba6-4db2-8ad9-627f99a63d1e" />



---

## SPL Queries and Analysis

### 1. Multi-Stage Attack Detection

**Objective:** Detect brute force attacks by identifying IPs that fail multiple times then succeed within a short timeframe.

```spl
index=main (status=200 OR status=401 OR status=403 OR status=404)
| transaction clientip maxspan=5m
| search eventcount>10 AND status=200
```

**Analysis:**
- Groups events by `clientip` within 5-minute windows
- Identifies IPs with more than 10 attempts ending in success (status=200)
- Classic indicator of brute force attack success

<img width="1892" height="832" alt="image" src="https://github.com/user-attachments/assets/bdf195f9-4f01-4bfb-a8a7-5f97b95a5c72" />


---

### 2. Success vs Failure Analysis (Dashboard Panel 1)

**Objective:** Visualize the ratio of successful requests vs failed requests.

```spl
index=main 
| eval outcome=if(status=200 OR status=206, "Success", "Failure") 
| stats count by outcome
```

**Visualization:** Pie Chart

<img width="1876" height="572" alt="image" src="https://github.com/user-attachments/assets/c235f444-369d-48b9-b167-27bf889d1a0f" />


**Key Insights:**
- Quick overview of overall traffic health
- High failure rate indicates potential issues or attacks
- Success codes: 200 (OK), 206 (Partial Content)

---

### 3. Top Offending IPs (Dashboard Panel 3)

**Objective:** Identify client IPs generating the highest number of errors.

```spl
index=main status>=400 
| stats count by clientip 
| sort -count 
| head 10
```

<img width="1865" height="718" alt="image" src="https://github.com/user-attachments/assets/c97c0818-49e5-4a32-8c53-07b7b4a2c6b2" />


**Key Insights:**
- Highlights potentially malicious IPs
- Useful for IP blocking decisions
- May indicate scanning or attack attempts

---

### 4. Error Trends Over Time (Dashboard Panel 2)

**Objective:** Plot error occurrences over time to identify attack patterns and spikes.

```spl
index=main status>=400 
| timechart count by status
```

**Visualization:** Line Chart

<img width="1858" height="763" alt="image" src="https://github.com/user-attachments/assets/8a6940d6-667f-46cf-848e-a25fc09ca8f7" />


**Key Insights:**
- Temporal patterns reveal attack timing
- Sudden spikes indicate potential incidents
- Different error codes shown as separate lines
- Helps correlate errors with specific events

---

### 5. Suspicious IPs (Only Errors, No Successes)

**Objective:** Find IPs that only generate errors without any successful requests.

```spl
index=main (status=200 OR status=206 OR status=301 OR status=304 OR status=403 OR status=404 OR status=500 OR status=416) 
| stats count(eval(status IN (200, 206, 304, 301))) as success 
       count(eval(status IN (403, 404, 500, 416))) as failures by clientip 
| where failures>10 AND success=0 
| sort -failures
```

<img width="1877" height="522" alt="image" src="https://github.com/user-attachments/assets/c2f3f2f3-a3e6-4579-ab5c-e2f002da5e89" />


**Key Insights:**
- IPs with only failures are highly suspicious
- Likely automated scanners or malicious bots
- Prime candidates for blocking
- Threshold set at 10+ failures for relevance

---

### 6. Most Frequent Errors (Dashboard Panel 4)

**Objective:** Identify which HTTP error codes appear most frequently.

```spl
index=main status>=400 
| stats count by status 
| sort -count
```

**Visualization:** Bar Chart

<img width="1856" height="822" alt="image" src="https://github.com/user-attachments/assets/693def23-3984-4633-b073-9859a6c42881" />


**Common Error Codes:**
- **403 Forbidden** - Access denied, possible unauthorized access attempts
- **404 Not Found** - Path enumeration or broken links
- **500 Internal Server Error** - Application issues or exploitation attempts
- **416 Requested Range Not Satisfiable** - Malformed requests

---

### 7. HTTP Redirects Analysis (301/304)

**Objective:** Analyze redirect patterns to detect potential redirect loops or suspicious behavior.

```spl
index=main (status=301 OR status=304) 
| stats count by clientip 
| sort -count
```

<img width="1849" height="838" alt="image" src="https://github.com/user-attachments/assets/c8ec36ad-efb1-4f3a-8bc9-17d210d693cf" />

**Key Insights:**
- **301 Moved Permanently** - Resource relocation
- **304 Not Modified** - Cached content validation
- Excessive redirects from single IP may indicate:
  - Redirect loop exploitation attempts
  - Aggressive crawling behavior
  - Cache poisoning attempts

---

### 8. Error Hotspots by User Agent (Dashboard Panel 5)

**Objective:** Identify which browsers, tools, or bots are generating the most errors.

```spl
index=main status>=400 
| stats count by useragent 
| sort -count 
| head 10
```

**Visualization:** Bar Chart

<img width="1864" height="843" alt="image" src="https://github.com/user-attachments/assets/b955313c-6ced-4901-b2a2-1b16bc6d36bb" />


**Key Insights:**
- Legitimate browsers vs automated tools
- Identifies misconfigured scripts or bots
- Uncommon user agents may indicate:
  - Security scanners (Nmap, Nikto, SQLMap)
  - Custom exploit tools
  - Malicious automated traffic

---

## Dashboard Panels Implemented

### Panel 1: Success vs Failure Pie Chart
- **Query:** Success/Failure evaluation using status codes
- **Purpose:** High-level traffic health overview
- **Actionable Metric:** Failure percentage threshold alerts

### Panel 2: Error Trends Over Time
- **Query:** Timechart of errors by status code
- **Purpose:** Temporal attack pattern identification
- **Actionable Metric:** Spike detection for incident response

### Panel 3: Top 10 Offending IPs
- **Query:** Top error-generating client IPs
- **Purpose:** Identify potential attackers
- **Actionable Metric:** IP reputation check and blocking

### Panel 4: Error Code Distribution
- **Query:** Count of each error status code
- **Purpose:** Understand error types and patterns
- **Actionable Metric:** Application security improvements

### Panel 5: Error-Heavy User-Agents
- **Query:** User agents with highest error counts
- **Purpose:** Identify malicious tools and bots
- **Actionable Metric:** User-agent based filtering rules

---

## Security Monitoring Workflow

### 1. Initial Assessment
- Check Success vs Failure ratio
- Look for abnormal failure percentages

### 2. Temporal Analysis
- Review error trends for spikes
- Correlate with known incidents or maintenance

### 3. IP Investigation
- Examine top offending IPs
- Cross-reference with threat intelligence
- Determine if blocking is warranted

### 4. Error Pattern Analysis
- Review error code distribution
- Identify application vulnerabilities
- Prioritize fixes based on frequency

### 5. User-Agent Analysis
- Distinguish legitimate vs malicious traffic
- Update WAF rules accordingly
- Block known malicious tools

---

## Key Takeaways

### SPL Skills Developed:
- `eval` for conditional field creation
- `stats` for aggregation and counting
- `timechart` for temporal visualization
- `transaction` for event correlation
- `where` clause for filtering results
- `sort` for result ordering

### Security Analysis Skills:
- Multi-stage attack pattern recognition
- Brute force detection techniques
- Anomaly identification in web traffic
- Correlation between multiple data points
- Dashboard design for security operations

### Best Practices Learned:
- Use appropriate time windows for attack detection
- Set meaningful thresholds (e.g., >10 failures)
- Combine multiple panels for comprehensive view
- Regularly review and update detection queries
- Document query logic for team knowledge sharing

---

## Practical Applications

### SOC Operations:
- Real-time threat monitoring
- Incident detection and response
- Pattern recognition and analysis

### Threat Hunting:
- Proactive identification of IOCs (Indicators of Compromise)
- Historical attack pattern analysis
- Anomaly detection

### Compliance and Reporting:
- Security posture documentation
- Incident metrics and trending
- Audit trail creation

---

## Lab Completion Summary

Successfully created a comprehensive security monitoring dashboard with 5 visualization panels, demonstrating proficiency in:
- SPL query development
- Data visualization techniques
- Security threat detection
- Dashboard design and implementation
- Log analysis and correlation
