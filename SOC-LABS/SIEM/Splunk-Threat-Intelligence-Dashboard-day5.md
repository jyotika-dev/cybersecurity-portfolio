# Splunk Threat Intelligence Dashboard - SIEM Mini Project

This document summarizes my SIEM mini project where I created a Threat Intelligence Dashboard in Splunk using FireHOL IP blacklist data to identify and analyze malicious network communications.

**Project Focus:** Threat intelligence correlation and malicious IP detection using Splunk lookups and visualization.

<img width="1864" height="911" alt="image" src="https://github.com/user-attachments/assets/35db1bab-78fd-4ef3-a21d-e4300103e163" />
<img width="1851" height="883" alt="image" src="https://github.com/user-attachments/assets/1e573fc2-5258-4e15-aad6-99057b2c1a1e" />


---

## Project Overview

### Objectives
- Integrate threat intelligence feeds into Splunk
- Correlate network traffic with known malicious IPs
- Create comprehensive threat intelligence dashboard
- Identify compromised or suspicious client IPs
- Analyze threat patterns and trends over time

### Data Sources
- **Primary Index:** `main`
- **Threat Intel Feed:** FireHOL Level 1 IP blacklist
- **Lookup Table:** `firehol_level1`

---

## Dashboard Components

### Panel 1: Correlation Search - Malicious IP Communications

**Purpose:** Identify which client IPs communicated with known malicious IPs.

**SPL Query:**
```spl
index=main 
| lookup firehol_level1 ip as clientip OUTPUT category 
| where isnotnull(category) 
| stats count by clientip category 
| sort -count
```

**Query Breakdown:**

| Component | Function |
|-----------|----------|
| `index=main` | Search primary data index |
| `lookup firehol_level1 ip as clientip OUTPUT category` | Match client IPs against threat intel feed |
| `where isnotnull(category)` | Filter only IPs with threat category matches |
| `stats count by clientip category` | Aggregate by client IP and threat category |
| `sort -count` | Sort by highest count (most active) |

**Visualization:** Table  
**Use Case:** Incident response, compromised host identification

<img width="1877" height="617" alt="image" src="https://github.com/user-attachments/assets/5b614ce0-6aae-48a9-b5a3-3b89b1cbdd52" />


---

### Panel 2: Category-wise Breakdown

**Purpose:** Understand which threat categories dominate in the environment.

**SPL Query:**
```spl
index=main 
| lookup firehol_level1 ip AS clientip OUTPUT category 
| where isnotnull(category) 
| stats count by category 
| sort -count
```

**Query Breakdown:**

| Component | Function |
|-----------|----------|
| `lookup firehol_level1 ip AS clientip OUTPUT category` | Enrich data with threat categories |
| `where isnotnull(category)` | Include only matched threats |
| `stats count by category` | Count occurrences per category |
| `sort -count` | Prioritize most common threats |

**Visualization:** Bar Chart or Pie Chart  
**Use Case:** Threat landscape overview, priority assessment

<img width="1853" height="543" alt="image" src="https://github.com/user-attachments/assets/6c1cf21b-3a91-4f7c-9bfe-3ece8bbc3a72" />


**Common Threat Categories:**
- Malware distribution
- Command & Control (C2) servers
- Phishing infrastructure
- Spam sources
- Compromised hosts
- Botnet nodes

---

### Panel 3: Top 10 Malicious IPs

**Purpose:** Spot repeat offenders and frequently contacted malicious IPs.

**SPL Query:**
```spl
index=main 
| lookup firehol_level1 ip AS clientip OUTPUT category 
| where isnotnull(category) 
| top limit=10 clientip
```

**Query Breakdown:**

| Component | Function |
|-----------|----------|
| `lookup firehol_level1 ip AS clientip OUTPUT category` | Match against threat intelligence |
| `where isnotnull(category)` | Filter matched IPs only |
| `top limit=10 clientip` | Return top 10 most active IPs |

**Visualization:** Table with count and percentage  
**Use Case:** IP reputation analysis, blocking priority

<img width="1848" height="851" alt="image" src="https://github.com/user-attachments/assets/2c728e3f-0bc0-47a7-83a3-42408179b356" />


**Key Metrics:**
- IP address
- Communication count
- Percentage of total malicious traffic
- First/last seen timestamps

---

### Panel 4: Trend Over Time

**Purpose:** Identify time-based spikes in malicious IP communications.

**SPL Query:**
```spl
index=main 
| lookup firehol_level1 ip AS clientip OUTPUT category 
| where isnotnull(category) 
| timechart count by category span=1h
```

**Query Breakdown:**

| Component | Function |
|-----------|----------|
| `lookup firehol_level1 ip AS clientip OUTPUT category` | Enrich with threat data |
| `where isnotnull(category)` | Include only threats |
| `timechart count by category span=1h` | Plot counts over time in 1-hour intervals |

**Visualization:** Line Chart or Area Chart  
**Use Case:** Temporal attack pattern analysis, incident timeline

<img width="1882" height="816" alt="image" src="https://github.com/user-attachments/assets/367deb27-4e61-4515-a9a2-872df4e2f841" />


**Time Analysis Benefits:**
- Detect attack campaigns
- Identify business hour vs off-hour threats
- Correlate with security events
- Establish baseline threat levels
- Spot anomalous spikes

---

## Threat Intelligence Integration

### FireHOL Level 1 Lookup

**What is FireHOL?**
- Community-maintained IP blacklist
- Aggregates threat intelligence from multiple sources
- Regularly updated with known malicious IPs
- Categorizes threats by type and severity

**Lookup Table Structure:**

| Field | Description |
|-------|-------------|
| `ip` | Malicious IP address |
| `category` | Threat classification |
| `source` | Intelligence feed source |
| `confidence` | Threat confidence level |

### Lookup Configuration

**Creating the Lookup:**
```bash
# Upload CSV file to Splunk
# Define lookup in transforms.conf
[firehol_level1]
filename = firehol_level1.csv
```

---

## Security Use Cases

### 1. Compromised Host Detection

**Scenario:** Internal system communicating with known malicious IPs

**Investigation Workflow:**
```spl
# Identify suspicious client
index=main | lookup firehol_level1 ip as clientip OUTPUT category 
| where isnotnull(category) 
| stats count by clientip category 
| where count > 10

# Deep dive on specific IP
index=main clientip="192.168.1.100" 
| lookup firehol_level1 ip as clientip OUTPUT category
| stats count by category, dest_ip, dest_port
```

**Actions:**
- Isolate affected system
- Analyze malware indicators
- Review authentication logs
- Check data exfiltration

### 2. Command & Control Detection

**Scenario:** Identifying C2 communications

**Detection Query:**
```spl
index=main 
| lookup firehol_level1 ip as clientip OUTPUT category 
| where category="c2" OR category="malware"
| stats count by clientip dest_ip 
| where count > 5
```

**Indicators:**
- Periodic beaconing patterns
- Unusual outbound connections
- Non-standard ports
- Encrypted traffic to suspicious IPs

### 3. Botnet Activity

**Scenario:** Multiple hosts contacting same malicious IP

**Detection Query:**
```spl
index=main 
| lookup firehol_level1 ip as dest_ip OUTPUT category 
| where isnotnull(category)
| stats dc(clientip) as unique_clients by dest_ip category
| where unique_clients > 5
| sort -unique_clients
```

**Indicators:**
- Multiple internal hosts
- Same destination IP
- Similar timing patterns
- Coordinated activity

### 4. Data Exfiltration

**Scenario:** Large data transfers to malicious IPs

**Detection Query:**
```spl
index=main 
| lookup firehol_level1 ip as dest_ip OUTPUT category 
| where isnotnull(category)
| stats sum(bytes_out) as total_bytes by clientip dest_ip category
| where total_bytes > 10000000
| sort -total_bytes
```

**Indicators:**
- High bandwidth usage
- Known malicious destinations
- Off-hours activity
- Unusual protocols

---

## Best Practices

### Threat Intelligence Management

**Feed Maintenance:**
- Update lookup tables daily
- Verify feed reliability
- Document false positives
- Maintain historical data

**Data Enrichment:**
- Add confidence scores
- Include threat actor attribution
- Tag with MITRE ATT&CK framework
- Add business context

### Performance Optimization

**Query Efficiency:**
- Use `tstats` for faster searches
- Implement data model acceleration
- Schedule summary indexing
- Set appropriate time ranges

**Lookup Optimization:**
```spl
# Use indexed lookups for large threat feeds
[firehol_level1]
filename = firehol_level1.csv
max_matches = 1
min_matches = 1
default_match = false
```

### Dashboard Maintenance

**Regular Reviews:**
- Weekly dashboard validation
- Monthly threat feed updates
- Quarterly query optimization
- Annual design refresh

---

## Project Completion

### Deliverables

✅ **Threat Intelligence Dashboard** - 4 comprehensive panels  
✅ **SPL Queries** - Correlation, categorization, trending  
✅ **Lookup Integration** - FireHOL Level 1 threat feed  
✅ **Use Case Documentation** - Detection scenarios  
✅ **Alert Configuration** - Automated threat detection  

### Skills Demonstrated

✅ SIEM configuration and management  
✅ Threat intelligence integration  
✅ SPL query development  
✅ Dashboard design and visualization  
✅ Security correlation and analysis  
✅ Incident detection and response  

### Technical Competencies

**Splunk Expertise:**
- Lookup table configuration
- SPL search optimization
- Dashboard XML development
- Data enrichment techniques

**Security Analysis:**
- Threat intelligence application
- Network traffic analysis
- Malicious IP identification
- Attack pattern recognition

**Incident Response:**
- Compromised host detection
- C2 communication identification
- Threat hunting methodologies
- Alert tuning and optimization

---

**Project successfully completed!** Created a comprehensive threat intelligence dashboard in Splunk that enables proactive threat detection, rapid incident response, and enhanced security monitoring through correlation with FireHOL IP blacklist data.
