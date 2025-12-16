# Splunk: Dashboards and Reports - TryHackMe

> Learn to organize data with dashboards, reports, and alerts

**Room Link:** https://tryhackme.com/room/splunkdashboardsandreports

<img width="1512" height="784" alt="image" src="https://github.com/user-attachments/assets/6e18323f-0bf5-4fe0-b64a-95d65c9f3673" />

---

## Introduction

**Problem:** Too much data in Splunk, hard to make sense of

**Solution:** Dashboards, Reports, and Alerts

**Goal:** Organize data for better security monitoring

---

## Task 1: Organizing Data in Splunk

### The Problem

**Raw data search:**
```spl
index=*
```

35,000+ events shown - overwhelming!

**Issues:**
- Hard to analyze manually
- No clear overview
- Difficult to spot threats

**Solution:** Aggregate and visualize data

### Question

**Q: Search term to show results from all indices?**

- Answer: `index=*`

---

## Task 2: Creating Reports

### Why Reports?

**Recurring searches:**
- Run automatically at scheduled times
- Save results for later viewing
- Reduce manual work

**Benefits:**
- No user interaction needed
- Reduce search head load
- Schedule with intervals

### Example: VPN User Logins

**Query:**
```spl
host=vpn_server | stats count by Username
```

### Create Report

1. Run search
2. Click "Save As" → "Report"
3. Fill in details:
   - Title
   - Description
   - Content type (Statistics Table)
   - Time Range Picker: Yes
   - Permissions
4. Click "Save"

**Report saved!** Now runs automatically.

### Questions

**Q1: Create report from network-server. Highest port usage count?**

**Query:**
```spl
index=* host=network-server | stats count by port | sort -count
```

- Answer: `5`

---

**Q2: Option to enable time range picker when creating reports?**

- Answer: `Time Range Picker`

---

## Task 3: Creating Dashboards

### Why Dashboards?

**Purpose:**
- Visual overview of data
- Quick insights at a glance
- Present to management
- Spot spikes and drops

**Use cases:**
- Number of incidents
- Failed login attempts
- Traffic patterns
- Security posture

### Create Dashboard

1. Go to Dashboards tab
2. Click "Create Dashboard"
3. Fill in details:
   - Title
   - Description
   - Permissions: Shared in App
4. Select Classic Dashboard
5. Click "Create"

### Add Panel

1. Click "Add Panel"
2. Select "New from Report"
3. Choose report
4. Select visualization (column chart, line chart, etc.)
5. Click "Add to Dashboard"
6. Save

**Visualizations make data easier to understand!**

### Questions

**Q1: Create dashboard from web-server logs showing status codes. Least observed status code?**

**Query:**
```spl
host=web-server | stats count by status_code | sort -count
```

Use line chart visualization.

- Answer: `400`

---

**Q2: Name of traditional Splunk dashboard builder?**

- Answer: `Classic`

---

## Task 4: Alerting on High Priority Events

### Why Alerts?

**Problem:** Can't watch dashboards 24/7

**Solution:** Automated alerts for critical events

**Example:** Brute force detection (multiple failed logins)

### Create Alert

**Example query:**
```spl
host=vpn_server Username=Sarah
```

### Configure Alert

1. Run search
2. Click "Save As" → "Alert"
3. Configure settings:
   - Title
   - Description
   - Permissions
   - Alert Type: Scheduled
   - Schedule: Every hour
4. Set trigger conditions:
   - Number of Results > 5
5. Configure trigger actions:
   - Send email
   - Email: soc@tryhackme.com
   - Priority: Highest
6. Enable Throttle: 60 minutes
7. Save

**Alert triggers automatically when conditions met!**

### Questions

**Q1: Feature to make Splunk take automated actions?**

- Answer: `trigger actions`

---

**Q2: Alert type that triggers instantly when event occurs?**

- Answer: `Real-time`

---

**Q3: Option to send only single alert in specified time?**

Prevents alert fatigue.

- Answer: `Throttle`

---

## Quick Reference

### Reports vs Dashboards vs Alerts

**Reports:**
- Recurring searches
- Scheduled execution
- Save results
- View later

**Dashboards:**
- Visual overview
- Multiple panels
- Combine reports
- Quick insights

**Alerts:**
- Real-time notification
- Automated actions
- Trigger conditions
- Reduce alert fatigue

### Common SPL Commands

**Stats:**
```spl
| stats count by field
```

**Sort:**
```spl
| sort -count        # Descending
| sort count         # Ascending
```

**Filter by host:**
```spl
host=server_name
```

### Visualization Types

**Charts:**
- Column chart
- Line chart
- Bar chart
- Pie chart

**Tables:**
- Statistics table
- Events table

**Time-based:**
- Timechart
- Timeline

### Alert Components

**Trigger Conditions:**
- Number of Results
- Custom condition
- Threshold values

**Trigger Actions:**
- Send email
- Run script
- Webhook
- Create ticket

**Throttle:**
- Limit alert frequency
- Prevent alert fatigue
- Time-based suppression

### Report Settings

**Content Types:**
- Statistics table
- Visualization
- Events

**Scheduling:**
- Cron schedule
- Interval-based
- Time range

**Permissions:**
- Private
- Shared in App
- All Apps

### Dashboard Options

**Classic Dashboard:**
- Traditional builder
- Panel-based
- Simple layout

**Dashboard Studio:**
- Modern interface
- Advanced features
- More customization

### Best Practices

**Reports:**
- Schedule during off-peak hours
- Use meaningful names
- Document queries
- Set appropriate permissions

**Dashboards:**
- Keep it simple
- Focus on key metrics
- Use appropriate visualizations
- Update regularly

**Alerts:**
- Set realistic thresholds
- Use throttle to reduce noise
- Test trigger conditions
- Document response procedures

---

## Key Takeaways

- **Reports** automate recurring searches
- **Dashboards** visualize data
- **Alerts** notify of critical events
- **Time Range Picker** allows flexible queries
- **Throttle** prevents alert fatigue
- **Trigger actions** automate responses
- **Classic Dashboard** traditional builder
- **Visualizations** make data clearer
- **Scheduled searches** reduce load
- **Permissions** control access
- **Statistics tables** show counts
- **Real-time alerts** trigger instantly

Organize to analyze!
