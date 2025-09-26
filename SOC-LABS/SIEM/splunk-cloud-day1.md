# Splunk Cloud - Day 1

This document captures my first day learning Splunk Cloud for log analysis and SIEM operations.

---

## Data Setup

**Data Source**: Apache web server logs
- Downloaded from: https://github.com/elastic/examples/tree/master/Common%20Data%20Formats/apache_logs
- Data ingestion: Used "Add Data" → "Start Searching" workflow
- Index: `main` (default index for the imported data)

---

## Search Queries Practiced

### 1. Basic Log Count
**Query**: `index=main | stats count`

**Purpose**: Get total number of log entries in the dataset
- Fundamental Splunk command to understand data volume
- Useful for baseline metrics and data validation
- ![Basic Log Count](https://github.com/user-attachments/assets/105e26f2-5643-462f-9fa2-93b2f7ebad98)


### 2. Top Client IPs
**Query**: `index=main | top limit=10 clientip`

**Purpose**: Identify the top 10 IP addresses accessing the server
- **Security relevance**: Detect potential botnet activity, DDoS sources, or suspicious traffic patterns
- Shows both count and percentage of total requests per IP
- 

### 3. HTTP Error Analysis
**Query**: `index=main status>=400 | stats count by status`

**Purpose**: Analyze failed and suspicious HTTP requests
- **Status codes 400+**: Client errors (400-499) and server errors (500-599)
- **Security relevance**: 
  - 401/403: Unauthorized access attempts
  - 404: Potential directory enumeration
  - 500: Server errors that might indicate attacks

### 4. Request Method Analysis
**Query**: `index=main | stats count by method`

**Purpose**: Break down HTTP requests by method (GET, POST, PUT, DELETE, etc.)
- **Security relevance**: 
  - Unusual method distribution might indicate attacks
  - POST requests could indicate form submissions or API abuse
  - Helps understand normal vs abnormal traffic patterns

### 5. Time-Series Analysis
**Query**: `index=main | timechart count by status`

**Purpose**: Visualize request patterns over time, segmented by HTTP status codes
- **Security relevance**: 
  - Identify attack time windows
  - Spot traffic spikes or unusual patterns
  - Correlate events with specific timeframes
- Creates a timeline chart showing trends and anomalies

---

## Key Takeaways

**Splunk Search Language (SPL) Patterns**:
- `index=main` - specifies the data source
- `|` (pipe) - chains commands together
- `stats count` - aggregation function
- `top limit=N field` - shows most frequent values
- `timechart` - creates time-based visualizations

**Security Analysis Applications**:
- **Baseline establishment**: Understanding normal traffic patterns
- **Anomaly detection**: Identifying unusual IP activity or error spikes
- **Attack timeline reconstruction**: Using time-series analysis
- **Threat hunting**: Leveraging IP and error code analysis

**Next Steps**:
- Explore more advanced filtering and correlation
- Learn about Splunk alerts and dashboards
- Practice with different log types (Windows, firewall, etc.)
- Study common attack patterns in web logs

---

## Screenshots
*Screenshots of query results will be added here*
