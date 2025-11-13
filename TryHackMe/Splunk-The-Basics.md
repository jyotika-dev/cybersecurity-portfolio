# Splunk: Basics - TryHackMe

> Learn the basics of Splunk and how to create simple search queries

**Room Link:** https://tryhackme.com/room/splunk101


<img width="1774" height="889" alt="image" src="https://github.com/user-attachments/assets/a68e69f1-72ea-49c2-bd6a-4cfa702175e8" />


---

## Task 1: Introduction

Splunk is a leading SIEM solution that collects, analyzes, and correlates network and machine logs in real-time.

**Learning objectives:**
- Splunk overview
- Splunk components
- Log ingestion methods
- Log normalization

**No answer needed**

---

## Task 2: Connect with the Lab

Deploy the machine and access it at `http://MACHINE_IP`

Wait 3-5 minutes for startup.

**No answer needed**

---

## Task 3: Splunk Components

### Three Main Components

**1. Forwarder**
- Lightweight agent on endpoints
- Collects data from monitored systems
- Sends to Splunk instance
- Minimal resource usage

**Common data sources:**
- Web server logs
- Windows Event Logs
- Linux host logs
- Database logs

---

**2. Indexer**
- Processes data from forwarders
- Normalizes into field-value pairs
- Determines data types
- Stores as events

---

**3. Search Head**
- User interface for searching
- Uses Splunk Search Processing Language (SPL)
- Transforms results into visualizations
- Creates reports and dashboards

### Data Flow

```
Endpoint → Forwarder (collects) → Indexer (processes) → Search Head (searches)
```

### Questions

**Which component is used to collect and send data over the Splunk instance?**

Agent installed on endpoints.

- Answer: `forwarder`

---

## Task 4: Navigating Splunk

### Interface Overview

**Splunk Bar (Top Panel)**
- Messages - System notifications
- Settings - Configuration
- Activity - Job progress
- Help - Tutorials
- Find - Search feature

**Apps Panel**
- Installed Splunk apps
- Default: Search & Reporting
- Switch between apps

**Explore Splunk**
- Add data
- Add apps
- Access documentation

**Home Dashboard**
- View dashboards
- Create custom dashboards
- Organize visualizations

### Questions

**In the Add Data tab, which option is used to collect data from files and ports?**

Check the Add Data options.

- Answer: `Monitor`

---

## Task 5: Adding Data

### Data Ingestion Process

**5 Steps to Upload Data:**

1. **Select Source** - Choose log source
2. **Select Source Type** - Define log type
3. **Input Settings** - Set index and hostname
4. **Review** - Verify configuration
5. **Done** - Data ready to analyze

### Upload VPN Logs

**Steps:**
1. Download VPN_logs file
2. Click "Add Data" → "Upload"
3. Select file
4. Choose source type
5. Create index "VPN_Logs"
6. Complete upload

### Questions

**Upload the data attached to this task and create an index "VPN_Logs". How many events are present in the log file?**

After uploading, check the event count displayed.

Total events ingested.

- Answer: `2862`

---

**How many log events by the user Maleena are captured?**

Click "UserName" field on the left panel.

Find Maleena's count.

- Answer: `60`

---

**What is the name associated with IP 107.14.182.38?**

Search query:
```
Source_ip="107.14.182.38"
```

Check the Name field in results.

- Answer: `Smith`

---

**What is the number of events that originated from all countries except France?**

Use "Not Equal To" operator:
```
Source_Country!="France"
```

Check total events count.

- Answer: `2814`

---

**How many VPN Events were observed by the IP 107.3.206.58?**

Search query:
```
Source_ip="107.3.206.58"
```

Count the events.

- Answer: `14`

---

## Task 6: Conclusion

**No answer needed**

---

## Quick Reference

### Splunk Architecture

```
[Endpoints with Forwarders]
         ↓
    [Indexer]
         ↓
   [Search Head]
         ↓
   [Analyst Dashboard]
```

### Basic Search Operators

| Operator | Usage | Example |
|----------|-------|---------|
| `=` | Equal to | `UserName="Maleena"` |
| `!=` | Not equal to | `Source_Country!="France"` |
| `AND` | Both conditions | `UserName="Smith" AND Source_Country="USA"` |
| `OR` | Either condition | `Source_Country="USA" OR Source_Country="UK"` |

### Common Field Names

- `Source_ip` - Source IP address
- `UserName` - User account name
- `Source_Country` - Origin country
- `timestamp` - Event time
- `action` - Action taken
- `status` - Connection status

### Navigation Quick Tips

**To search logs:**
1. Go to Search & Reporting app
2. Enter search query
3. Set time range
4. Click Search button

**To filter results:**
- Click field names in left panel
- Select values to filter
- Use search operators

**To create visualization:**
1. Run search query
2. Click "Visualization" tab
3. Choose chart type
4. Configure settings

### Data Upload Checklist

- [ ] Download log file
- [ ] Click Add Data → Upload
- [ ] Select source file
- [ ] Choose correct source type
- [ ] Create/select index
- [ ] Set hostname
- [ ] Review settings
- [ ] Complete upload
- [ ] Verify event count

### Common Source Types

- `access_combined` - Web server logs
- `WinEventLog` - Windows events
- `syslog` - Linux system logs
- `cisco:asa` - Cisco firewall
- `aws:cloudtrail` - AWS logs
- `json` - JSON formatted logs

---

## Key Takeaways

- **Forwarder** collects data from endpoints
- **Indexer** processes and stores events
- **Search Head** is where analysts search and analyze
- **Field-value pairs** make data searchable
- **SPL** (Splunk Processing Language) is the query language
- **Index** organizes and stores log data
- **Source type** defines how logs are parsed
- **2862 events** in VPN logs file
- **Use `=`** for exact match searches
- **Use `!=`** for exclusion searches
- **Click fields** in left panel for quick filtering
- **Visualizations** transform search results into charts
- **Dashboards** organize multiple visualizations
- **Upload process** has 5 steps

Splunk makes log analysis powerful and fast
Happy searching
