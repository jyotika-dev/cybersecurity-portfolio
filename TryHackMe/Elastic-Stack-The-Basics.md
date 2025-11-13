# Investigating with ELK 101 - TryHackMe

> Learn to use Kibana for searching, filtering, and visualizing logs

**Room Link:** https://tryhackme.com/room/investigatingwithelk101


<img width="1488" height="892" alt="image" src="https://github.com/user-attachments/assets/a87a51db-91c2-441a-9578-4aae52cbef13" />


---

## Task 1: Introduction

Learn how to use the Kibana interface to investigate VPN logs for anomalies.

**Learning objectives:**
- Search and filter logs
- Create visualizations
- Investigate VPN logs
- Build dashboards

**No answer needed**

---

## Task 2: Incident Handling Scenario

### The Scenario

CyberT company detected VPN log anomalies. Investigate VPN logs for January 2022.

**Key points:**
- Index name: `vpn_connections`
- User Johny Brown terminated on January 1, 2022
- Failed connection attempts detected
- Suspicious activity needs investigation

**No answer needed**

---

## Task 3: ElasticStack Overview

### The Four Components

**Elasticsearch**
- Full-text search engine
- Stores JSON documents
- Supports RESTful API
- Database for logs

**Logstash**
- Data processing engine
- Three parts: Input, Filter, Output
- Normalizes logs
- Sends to Elasticsearch

**Beats**
- Lightweight data shippers
- Installed on endpoints
- Collects specific data types
- Examples: Winlogbeat, Filebeat, Packetbeat

**Kibana**
- Web-based visualization
- Analyzes data from Elasticsearch
- Creates dashboards
- Interactive search interface

### How They Work Together

```
Endpoints → Beats (collect) → Logstash (process) → Elasticsearch (store) → Kibana (visualize)
```

### Questions

**Logstash is used to visualize the data. (yay / nay)**

Logstash processes data, Kibana visualizes it.

- Answer: `nay`

---

**Elasticstash supports all data formats apart from JSON. (yay / nay)**

Elasticsearch stores JSON-formatted documents.

- Answer: `nay`

---

## Task 4: Kibana Overview

### Main Tabs

**Discover**
- Search logs
- Apply filters
- View documents

**Visualization**
- Create charts
- Build graphs
- Visual analysis

**Dashboard**
- Combine visualizations
- Custom views
- Overview displays

### Lab Access

**Credentials:**
- Username: `Analyst`
- Password: `analyst123`

**No answer needed**

---

## Task 5: Discover Tab

### Key Interface Elements

**Logs (Documents)**
- Individual log entries
- Field-value pairs
- Event information

**Fields Pane**
- Left panel
- Parsed fields list
- Click to filter

**Search Bar**
- Query input
- Apply filters
- Narrow results

**Time Filter**
- Filter by time range
- Multiple options
- Quick select available

**Timeline**
- Event count over time
- Identify spikes
- Visual overview

### Creating Tables

**Steps:**
1. Click on document
2. Select fields to display
3. Toggle column in table
4. Save table format

### Questions

**Select the index vpn_connections and filter from 31st December 2021 to 2nd Feb 2022. How many hits are returned?**

Set time filter to the specified range.

Check total hit count.

- Answer: `2861`

---

**Which IP address has the max number of connections?**

Click `Source_ip` field in left panel.

Top value has highest count.

- Answer: `238.163.231.224`

---

**Which user is responsible for max traffic?**

Click `UserName` field in left panel.

First username has most connections.

- Answer: `James`

---

**Create a table with the fields IP, UserName, Source_Country and save.**

**Steps:**
1. Expand an event
2. Hover over field name
3. Click "Toggle column in table"
4. Repeat for all three fields
5. Save the table

**No answer needed**

---

**Apply Filter on UserName Emanda; which SourceIP has max hits?**

Add filter: `UserName: Emanda`

Check `Source_ip` field - first value.

- Answer: `107.14.1.247`

---

**On 11th Jan, which IP caused the spike observed in the time chart?**

Filter to January 11, 2022 only.

Click `Source_ip` field.

First IP caused the spike.

- Answer: `172.201.60.191`

---

**How many connections were observed from IP 238.163.231.224, excluding the New York state?**

Add two filters:
1. `Source_ip: 238.163.231.224`
2. `NOT Source_State: New York`

Check hit count.

- Answer: `48`

---

## Task 6: KQL Overview

### Kibana Query Language (KQL)

**Two search types:**
- Free text search
- Field-based search

### Free Text Search

**Simple search:**
```
United States
```
Returns all documents with this exact phrase.

**Wildcard search:**
```
United*
```
Returns: United States, United Kingdom, etc.

### Logical Operators

**OR Operator:**
```
"United States" OR "England"
```

**AND Operator:**
```
"United States" AND "Virginia"
```

**NOT Operator:**
```
"United States" AND NOT "Florida"
```

### Field-Based Search

**Syntax:** `FIELD:VALUE`

**Examples:**
```
Source_ip:238.163.231.224
UserName:James
Source_Country:"United States" AND UserName:"James"
```

### Questions

**Create a search query to filter out the logs from Source_Country as the United States and show logs from User James or Albert. How many records were returned?**

Search query:
```
Source_Country:"United States" AND (UserName:"James" OR UserName:"Albert")
```

- Answer: `161`

---

**As User Johny Brown was terminated on 1st January 2022, create a search query to determine how many times a VPN connection was observed after his termination.**

Filter: January 1, 2022 and after

Search: `UserName:"Johny Brown"`

- Answer: `1`

---

## Task 7: Creating Visualizations

### Visualization Types

- Tables
- Pie charts
- Bar charts
- Line graphs
- Heat maps

### Creating Visualizations

**Method 1:**
1. Click field in Discover tab
2. Click "Visualize"
3. Select chart type

**Method 2:**
1. Go to Visualize Library
2. Click "Create visualization"
3. Select Lens
4. Add filters and fields

### Correlation Option

Drag fields to create correlations:
- Source_IP with Source_Country
- UserName with Action
- Multiple field combinations

### Saving Visualizations

**Steps:**
1. Click Save button
2. Add title and description
3. Choose dashboard (existing or new)
4. Save and add to library

### Questions

**Which user was observed with the greatest number of failed attempts?**

**Create table visualization:**
1. Go to Visualize Library → Lens
2. Add filter: `Action: failed`
3. Select table visualization
4. Drag fields: UserName, Source_ip
5. Sort by count

Top user in results.

- Answer: `Simon`

---

**How many wrong VPN connection attempts were observed in January?**

Add time filter: January 2022

Count failed attempts.

- Answer: `274`

---

## Task 8: Creating Dashboards

### Dashboard Purpose

Combine multiple visualizations for comprehensive visibility.

### Creating Custom Dashboard

**Steps:**
1. Go to Dashboard tab
2. Click "Create dashboard"
3. Click "Add from Library"
4. Select saved visualizations
5. Arrange on dashboard
6. Save dashboard

### Dashboard Best Practices

- Group related visualizations
- Adjust sizes for clarity
- Use consistent time filters
- Add descriptive titles
- Save regularly

**No answer needed**

---

## Task 9: Conclusion

**No answer needed**

---

## Quick Reference

### ELK Stack Flow

```
Data Sources → Beats → Logstash → Elasticsearch → Kibana
```

### KQL Syntax Cheat Sheet

| Query Type | Example |
|------------|---------|
| Exact match | `UserName:"James"` |
| Wildcard | `United*` |
| OR | `"USA" OR "UK"` |
| AND | `"USA" AND "Virginia"` |
| NOT | `"USA" AND NOT "Florida"` |
| Field search | `Source_ip:192.168.1.1` |

### Common Field Names

- `UserName` - User account
- `Source_ip` - Source IP address
- `Source_Country` - Origin country
- `Source_State` - Origin state
- `Action` - Connection status
- `timestamp` - Event time

### Time Filter Options

- Last 15 minutes
- Last 30 minutes
- Last 1 hour
- Last 24 hours
- Last 7 days
- Custom range

### Visualization Types

| Type | Best For |
|------|----------|
| Table | Detailed records |
| Bar chart | Comparisons |
| Pie chart | Proportions |
| Line graph | Trends over time |
| Heat map | Correlations |

### Investigation Workflow

1. **Set time range** - Filter relevant period
2. **Search & filter** - Narrow down events
3. **Create table** - Display key fields
4. **Identify patterns** - Look for anomalies
5. **Create visualizations** - Visual analysis
6. **Build dashboard** - Combine views

---

## Key Takeaways

- **Elasticsearch** stores JSON documents
- **Logstash** processes and normalizes logs
- **Beats** collects data from endpoints
- **Kibana** is the visualization interface
- **Discover tab** is where most investigation happens
- **KQL** uses field:value syntax
- **Wildcard (*)** matches partial terms
- **Fields pane** shows top values quickly
- **Timeline spike** on Jan 11 = anomaly
- **238.163.231.224** had most connections
- **James** generated most traffic
- **Johny Brown** accessed VPN after termination
- **Simon** had most failed attempts
- **274 failed attempts** in January
- **Tables** reduce noise, improve clarity
- **Save visualizations** for reuse in dashboards
- **Dashboards** combine multiple views

ELK makes log investigation visual and efficient
Happy investigating
