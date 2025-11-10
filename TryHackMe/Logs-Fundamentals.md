# Logs Fundamentals - TryHackMe

> Learn the fundamentals of log analysis in cybersecurity

**Room Link:** https://tryhackme.com/room/logsfundamentals

<img width="1790" height="854" alt="image" src="https://github.com/user-attachments/assets/0b4f746c-626a-4708-8a39-77e9fff9a7cc" />

---

## Task 1: Introduction to Logs

### What are Logs?

Logs are records of events that happen in systems, applications, and networks. They're like a diary for digital systems!

### Why are Logs Important?

**Security Investigation**
- Track attacker activities
- Find attack traces
- Reconstruct incidents

**Troubleshooting**
- Debug issues
- Find errors
- Monitor performance

**Compliance**
- Meet regulatory requirements
- Audit trails
- Legal evidence

### Where Attack Traces are Found

When attackers compromise a system, they leave traces. These traces are primarily found in logs!

**Logs capture:**
- Login attempts
- File access
- Network connections
- System changes
- User activities

### Questions

**Where can we find the majority of attack traces in a digital system?**

Everything leaves a trail in the logs!

- Answer: `Logs`

---

## Task 2: Types of Logs

### Common Log Types

### 1. Network Logs

**What they contain:**
- Incoming traffic
- Outgoing traffic
- Connection attempts
- Firewall events

**Examples:**
- Firewall logs
- Router logs
- IDS/IPS logs

**Use cases:**
- Detect network attacks
- Monitor bandwidth
- Track connections

### 2. Security Logs

**What they contain:**
- Authentication events
- Authorization events
- Login attempts (success/failure)
- Permission changes

**Examples:**
- Windows Security logs
- Linux auth logs
- Application security events

**Use cases:**
- Track user access
- Detect brute force attacks
- Monitor privilege escalation

### 3. System Logs

**What they contain:**
- System events
- Service status
- Hardware issues
- Boot information

**Examples:**
- Windows System logs
- Linux syslog
- Application logs

**Use cases:**
- System health monitoring
- Troubleshooting
- Performance tracking

### 4. Application Logs

**What they contain:**
- Application errors
- User actions
- Performance data
- Debug information

**Examples:**
- Web server logs
- Database logs
- Custom app logs

**Use cases:**
- Debug applications
- Track user behavior
- Monitor performance

### 5. Web Server Logs

**What they contain:**
- HTTP requests
- Response codes
- IP addresses
- User agents
- Timestamps

**Examples:**
- Apache access logs
- Nginx logs
- IIS logs

**Use cases:**
- Track website visitors
- Detect web attacks
- Monitor traffic patterns

### Questions

**Which type of logs contain information regarding the incoming and outgoing traffic in the network?**

Traffic = Network activity

- Answer: `Network logs`

---

**Which type of logs contain the authentication and authorization events?**

Login attempts, permission changes = Security events

- Answer: `Security Logs`

---

## Task 3: Windows Event Logs Analysis

### The Scenario

**Critical Cyber Attack Investigation:**

A critical organization was attacked on Friday. The attacker:
- Exfiltrated critical data
- Accessed the file server
- Used a compromised system

**Your Mission:**
Find out what the attacker did on the compromised system before accessing the file server.

### Windows Event Log Basics

**Event ID Types:**

| Event ID | Description |
|----------|-------------|
| 4720 | User account created |
| 4722 | User account enabled |
| 4724 | Password reset attempted |
| 4725 | User account disabled |
| 4726 | User account deleted |
| 4738 | User account changed |
| 4723 | Password change attempted |

### Important Fields

**Subject:**
- The account that initiated an action
- Who performed the action

**Target Account:**
- The specific account affected
- Account that action was performed on

**Remember:** Subject does the action TO the Target Account!

---

### Question 1: Last User Account Created

**Steps:**
1. Open Event Viewer
2. Filter for Event ID 4720 (User account created)
3. Sort by time (newest first)
4. Look for the most recent entry
5. Check the "Target Account Name" field

**What to look for:**
- Latest timestamp
- Event ID 4720
- Account name in event details

The most recent account created was named "hacked" - suspicious name!

- Answer: `hacked`

---

### Question 2: Who Created the Account?

**Steps:**
1. Look at the same log entry (Event ID 4720)
2. Find the "Subject" section
3. Check "Subject Account Name" field

**Remember:**
- **Subject** = Who did it
- **Target** = Who it was done to

The Administrator account created the "hacked" user.

- Answer: `Administrator`

---

### Question 3: When was Account Enabled?

**Steps:**
1. Filter for Event ID 4722 (User account enabled)
2. Look for the "hacked" account
3. Check the timestamp
4. Format as M/D/YYYY

**Event ID 4722** indicates when an account was enabled (activated).

The account was enabled on June 7, 2024.

- Answer: `6/7/2024`

---

### Question 4: Was Password Reset?

**Steps:**
1. Filter for Event ID 4724 (Password reset)
2. Look for the "hacked" account as Target
3. Check if any events exist

**Event ID 4724** indicates a password reset attempt.

If you find an event with this ID for the "hacked" account, then yes, the password was reset!

- Answer: `yes`

---

## Task 4: Web Server Access Logs Analysis

### Understanding Web Server Logs

**Common Log Format:**
```
IP - - [timestamp] "METHOD /path HTTP/version" status_code size
```

**Example:**
```
10.0.0.1 - - [06/Jun/2024:13:45:23 +0000] "GET /contact HTTP/1.1" 200 1234
```

**Fields explained:**
- `10.0.0.1` - Client IP address
- `[06/Jun/2024:13:45:23 +0000]` - Timestamp
- `GET` - HTTP method
- `/contact` - URL path
- `HTTP/1.1` - Protocol version
- `200` - Status code
- `1234` - Response size

### HTTP Methods

| Method | Purpose |
|--------|---------|
| GET | Retrieve data |
| POST | Submit data |
| PUT | Update data |
| DELETE | Remove data |

---

### Question 1: Last GET Request to /contact

**Steps:**
1. Open the web server access log
2. Search for "/contact"
3. Filter for "GET" method
4. Find the last (most recent) entry
5. Note the IP address at the beginning

**Search pattern:**
```
GET /contact
```

The last GET request to /contact came from 10.0.0.1.

- Answer: `10.0.0.1`

---

### Question 2: Last POST Request from 172.16.0.1

**Steps:**
1. Search for IP: 172.16.0.1
2. Filter for "POST" method
3. Find the most recent entry
4. Note the timestamp

**Search pattern:**
```
172.16.0.1 - - [timestamp] "POST
```

The timestamp format in logs: `DD/Mon/YYYY:HH:MM:SS`

The last POST request was made on 06/Jun/2024:13:55:44.

- Answer: `06/Jun/2024:13:55:44`

---

### Question 3: URL of the POST Request

**Steps:**
1. Look at the same log entry from Question 2
2. Find the URL path between POST and HTTP
3. The format is: `POST /url HTTP/1.1`

From the same log entry, the POST was made to the /contact URL.

- Answer: `contact`

---

## Quick Reference

### Windows Event IDs

| Event ID | Event Type |
|----------|------------|
| 4720 | User account created |
| 4722 | User account enabled |
| 4723 | Password change attempt |
| 4724 | Password reset attempt |
| 4725 | User account disabled |
| 4726 | User account deleted |
| 4728 | Member added to security group |
| 4732 | Member added to local group |
| 4738 | User account changed |
| 4740 | User account locked out |

### Log Types Summary

| Log Type | Contains | Examples |
|----------|----------|----------|
| Network | Traffic data | Firewall, IDS/IPS |
| Security | Auth/Auth events | Login attempts |
| System | System events | Service status |
| Application | App events | Errors, actions |
| Web Server | HTTP requests | Access logs |

### Web Server Log Format

```
IP - - [timestamp] "METHOD /path HTTP/version" status size
```

### HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK - Success |
| 301 | Moved Permanently |
| 302 | Found (redirect) |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

---

## Investigation Best Practices

### Windows Event Log Analysis

**Filter Efficiently:**
- Use Event ID filters
- Sort by time
- Focus on security events

**Look for Patterns:**
- Multiple failed logins
- Account changes
- Unusual times
- Suspicious account names

**Correlate Events:**
- Link related events
- Build timeline
- Track user activities

### Web Log Analysis

**Search Techniques:**
- Filter by IP address
- Filter by HTTP method
- Filter by URL path
- Sort by timestamp

**Red Flags:**
- Unusual IP addresses
- High request frequency
- Error codes (4xx, 5xx)
- POST to unusual URLs

**Investigation Steps:**
1. Identify suspicious activity
2. Filter relevant logs
3. Build timeline
4. Correlate with other logs
5. Document findings

---

## Common Log Locations

### Windows

**Event Viewer:**
```
Event Viewer > Windows Logs > Security
Event Viewer > Windows Logs > System
Event Viewer > Windows Logs > Application
```

**Log Files:**
```
C:\Windows\System32\winevt\Logs\
```

### Linux

**System Logs:**
```bash
/var/log/syslog        # General system log
/var/log/auth.log      # Authentication log
/var/log/secure        # Security log (RHEL/CentOS)
```

**Web Server Logs:**
```bash
/var/log/apache2/access.log   # Apache access
/var/log/apache2/error.log    # Apache errors
/var/log/nginx/access.log     # Nginx access
```

---

## Tips for Log Analysis

### Filtering Logs

**By Time:**
- Focus on incident timeframe
- Look before and after
- Build timeline

**By Event Type:**
- Use Event IDs (Windows)
- Use log levels (Linux)
- Filter by severity

**By Source:**
- Specific IP addresses
- Specific users
- Specific systems

### Building Timeline

1. **Collect** all relevant logs
2. **Sort** by timestamp
3. **Identify** key events
4. **Correlate** across sources
5. **Document** findings

### Documentation

**Record:**
- Event timestamps
- Event IDs
- Source/Target accounts
- IP addresses
- Actions taken
- Evidence locations

---

## Key Takeaways

- **Logs are essential** for security investigations
- **Attack traces** are found in logs
- **Network logs** track incoming/outgoing traffic
- **Security logs** contain authentication events
- **Event ID 4720** = User account created
- **Event ID 4722** = User account enabled
- **Event ID 4724** = Password reset
- **Subject** = Who did the action
- **Target Account** = Who it was done to
- **Web logs** show all HTTP requests
- **GET** retrieves data, **POST** submits data
- **Timestamps** help build attack timeline
- **Correlation** across logs reveals full picture
- **Documentation** is critical for investigations
- **Pattern recognition** identifies suspicious activity

Log analysis is a fundamental skill in cybersecurity. Understanding different log types and how to analyze them helps investigate incidents, detect threats, and maintain system security
