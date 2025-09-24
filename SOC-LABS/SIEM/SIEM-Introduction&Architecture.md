**SECURITY INFORMATION AND EVENT MANAGER**

# SIEM: INTRODUCTION

- use rules and correlations to turn into log entries, events from security systems into actionable information
- this information can help detect vulnerabilites, threats, perform incident response and forensic investigation on security incidents and prepare for audits and compliances
- SIEM platforms can aggregate both historical log data and real-time events, and establish relationships that can help security staff identify anomalies, vulnerabilities and incidents.
- NEXT GEN SIEM
    - UEBA (User Entity and Behaviour Analysis)
        - leverage ai and deep learning to look at  patterns of human behaviour to help detect insider threats, targeted attacks, and fraud.
    - SOAR (Security Orchestration, Automation and Response)
        - integrate with enterprise systems and automate incident response
- SIEM can help with
    - Data aggregation, Analytics, Compliance, Threat Intelligence feeds and threat hunting, alerting, Incident response, retention, SOC Automation, Corelation, Dashboards and visualisation
- HOW SIEM WORKS
    - Data Collection
    - Data Storage
    - Policies and Rules
    - Data consolidation and correlation

# SIEM Architecture: Technology, Process and Data

To be covered - Log management, Log Flow, SIEM log Sources, SIEM hosting models, SIEM sizing, SIEM outputs

### Log Management Process

- **Data Collection:**
    - **Purpose:** Gather logs from many systems.
    - **Methods:** Agent-based, direct connection, file access, or streaming protocols (SNMP, etc.).
    - **Next-Gen:** Built-in cloud and SaaS integrations for direct data pull.
- **Data Management:**
    - **Purpose:** Store and organize huge amounts of data.
    - **Requirements:** Storage (on-prem, cloud), optimization (indexing), and tiered storage (hot/cold).
    - **Next-Gen:** Uses data lakes (S3, Hadoop) for scalable, low-cost storage.
- **Log Retention:**
    - **Purpose:** Store logs for compliance and forensics.
    - **Strategies:** Use Syslog to normalize/compress, set deletion schedules, filter logs, and summarize data.
    - **Next-Gen:** Retains all data in low-cost distributed storage and uses UEBA to analyze historical behavior.

### The Log Flow

- **Purpose:** A process to turn millions of raw logs into a handful of actionable alerts.
- **Steps:**
    1. **Capture:** Gather all events.
    2. **Filter:** Remove noise and irrelevant data.
    3. **Index:** Organize data for efficient searching.
    4. **Analyze & Correlate:** Find relationships and patterns in the data.
    5. **Threshold:** Set rules to turn correlated events into security alerts.

### SIEM Logging Sources

- **Purpose:** Identify which systems feed logs to a SIEM.
- **Categories:**
    - **Security Events:** Intrusion detection, firewalls, DLP, honeypots.
    - **Network Logs:** Routers, switches, DNS servers, wireless access points.
    - **Applications and Devices:** Application servers, databases, web apps, laptops, mobile devices.
    - **IT Infrastructure:** Configuration, network maps, vulnerability reports, software inventory.
- **Next-Gen:** Built-in integrations with cloud infrastructure (AWS, Azure) and SaaS applications.

### SIEM Hosting Models

- **In-House (Traditional):** Self-hosted and self-managed. High complexity and cost.
- **Cloud SIEM, Self-Managed:** You handle correlation and analysis; an MSSP (Managed Security Service Provider) handles collection and aggregation.
- **Self-Hosted, Hybrid-Managed:** You buy the hardware, and an MSSP helps with deployment and management.
- **SIEM as a Service (SaaS):** An MSSP handles the entire process, and you define the program goals.

### SIEM Sizing

- **Purpose:** Determine the system resources needed to handle log data.
- **Velocity:** Measured in Events Per Second (EPS). EPS varies between normal and peak times.
- **Sizing Factors:**
    - **Storage:** Format (flat file, database, data lake), deployment (on-prem, cloud), and tiered storage (hot/cold).
    - **Log Compression:** Use technology to reduce log volume (e.g., 1:8 ratio).
    - **Redundancy:** Build with failover and backup for business continuity.
- Scalability and Data Lakes
    - **Problem:** Explosive growth of data from networks and devices.
    - **Solution:** **Data Lakes** (e.g., Hadoop, S3, Elasticsearch)
    - **Benefits:**
        - **Unlimited Storage:** Low-cost storage for all historical data.
        - **Processing:** Tools like Hive and Spark enable fast analysis of big data.
        - **Predictable Costs:** Easily add storage capacity to match data growth.
        - **Full Retention:** Retain all raw data for deep analysis.

### SIEM Outputs: Reporting, Dashboards, and Visualization

- **Purpose:** Generate actionable insights for security teams.
- **Forms of Output:**
    - **Alerts:** Prompt staff to investigate anomalies.
    - **Data Exploration:** Allow analysts to hunt for threats.
    - **Dashboards:** Visual representation of security status and metrics.
    - **APIs:** Enable external tools (BI, analytics) to access SIEM data.
- **Next-Gen:** Uses behavioral profiling and machine learning to create intelligent alerts and automatically generate incident tickets.

### SIEM Architecture: Then and Now

- **Evolution:** Shift from expensive, monolithic systems to more agile, affordable, and lightweight solutions.
- **Key Features of Next-Gen SIEM:**
    - **Data Lake Technology:** Offers scalable, low-cost storage.
    - **Managed Services:** MSSPs provide hosting and expertise.
    - **Dynamic Scalability:** Storage grows as data volumes increase, with predictable costs.
    - **UEBA (User and Entity Behavior Analytics):** Uses machine learning to find complex anomalies.
    - **SOAR (Security Orchestration, Automation and Response):** Automates incident response actions.
