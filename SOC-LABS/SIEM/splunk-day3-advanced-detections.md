# Splunk Advanced Threat Detection Queries

## Objective
Develop advanced Splunk correlation searches to detect sophisticated attack patterns including impossible travel, failed-to-successful login sequences, and geographic anomalies. These detections help identify compromised accounts and coordinated attacks.
Created Dashboard - Brute Force Detection Suite

<img width="1857" height="923" alt="image" src="https://github.com/user-attachments/assets/53e239bc-03b0-48ae-81c1-588754ddc1ff" />
<img width="1858" height="877" alt="image" src="https://github.com/user-attachments/assets/c8d4422a-433a-4dfd-b157-47c246b8abde" />
<img width="1852" height="861" alt="image" src="https://github.com/user-attachments/assets/d0fd3f41-9152-43c2-86ce-32f80f469eee" />


---

## Detection Use Cases

### 1. Impossible Travel Detection
**Goal**: Detect accounts logging in from multiple countries within an impossible timeframe, indicating potential account compromise.

**Query**:
```spl
index=main status=200
| iplocation clientip
| stats earliest(_time) as firstLogin latest(_time) as lastLogin values(Country) as countries count by useragent
| where mvcount(countries) > 1 AND (lastLogin - firstLogin) < 600
```

**Detection Logic**:
- Tracks successful logins (status=200) across different geographic locations
- Identifies user agents appearing in multiple countries
- Flags sessions where location changes occur within 10 minutes (600 seconds)
- Multiple countries from single user agent = impossible travel scenario

**Results**: 18,252 events analyzed
- Identified user agents from China, Japan, Colombia, and Ukraine
- Rapid geographic shifts indicate credential sharing or compromise

![Impossible Travel Detection](https://github.com/user-attachments/assets/e544becf-6895-461c-8e62-fc314e8acc0c)

---

### 2. Failed Logins by Country
**Goal**: Identify high-volume failed login attempts by IP address and geographic location to detect distributed brute force attacks.

**Query**:
```spl
index=main status=401 OR status=403 OR status=404
| iplocation clientip
| stats count by clientip Country
| where count>10
| sort -count
```

**Detection Logic**:
- Filters authentication failures (401/403/404 status codes)
- Enriches with geographic IP location data
- Aggregates failure counts per IP and country
- Threshold set at >10 failures to reduce noise

**Results**: 430 events found
- Top attacking IPs: 208.91.156.11 (US - 120 attempts), 144.76.95.39 (Germany - 28 attempts)
- Geographic distribution reveals coordinated attack infrastructure

![Failed Logins by Country](https://github.com/user-attachments/assets/b6ac8b4b-f1b2-43ef-8df6-a5ad545a912d)

---

### 3. Failed → Successful Login Correlation
**Goal**: Detect credential stuffing or brute force attacks that eventually succeed, indicating successful account compromise.

**Query**:
```spl
index=main (status=200 OR status=401 OR status=403 OR status=404)
| stats values(status) as status_list count(eval(status=200)) as success count(eval(status!=200)) as failures by clientip
| where failures > 10 AND success > 0
| table clientip failures success status_list
| sort -failures
```

**Detection Logic**:
- Analyzes both successful and failed authentication attempts
- Counts failures (non-200 status) and successes (200 status) per IP
- Identifies IPs with >10 failures followed by successful logins
- Lists all observed status codes for context

**Results**: 18,682 events processed
- IP 144.76.95.39: 28 failures, 26 successes (likely compromised)
- IP 66.249.73.135: 16 failures, 840 successes (successful brute force)
- Status codes reveal attack progression (200, 404 patterns)

![Failed to Successful Login](https://github.com/user-attachments/assets/350dfd0f-f2ef-40da-972e-e0bf5c7e4159)

---

## Security Analysis

### Attack Pattern Identification

**Credential Stuffing Indicators**:
- High failure count followed by successful authentication
- IP addresses from multiple geographic regions
- Automated user agent strings

**Account Compromise Indicators**:
- Impossible travel scenarios (multiple countries in short timeframe)
- Successful logins after sustained brute force attempts
- Geographic anomalies inconsistent with user behavior

**Distributed Attack Infrastructure**:
- Failed login attempts from diverse countries (US, Germany, Poland)
- Coordinated timing patterns across different IPs
- Mixed success/failure ratios indicating credential testing

---

## Defensive Recommendations

**Immediate Response Actions**:
1. **Account lockout** for IPs showing failed→successful pattern
2. **Force password reset** for accounts with impossible travel
3. **Block IP ranges** with high failure counts
4. **Enable MFA** for all affected accounts

**Long-term Security Measures**:
- Implement rate limiting on authentication endpoints
- Deploy CAPTCHA after 3-5 failed attempts
- Enable geo-blocking for unexpected countries
- Configure adaptive authentication based on risk signals
- Establish baseline user behavior profiles

**Monitoring Enhancements**:
- Alert on >5 failures from single IP in 5 minutes
- Notify on successful login after >10 failures
- Flag geographic location changes <1 hour apart
- Track user agent changes for same account

---

## Alert Configuration

**Critical Priority Alerts**:
- Failed→Successful login pattern: Immediate investigation required
- Impossible travel detected: Force password reset and notify user
- High-volume failures (>50): Auto-block IP address

**Medium Priority Alerts**:
- Geographic anomalies: Review user activity logs
- Multiple country access: Verify with user via secondary channel
- Sustained low-level failures: Monitor for escalation

---

## Summary

These advanced correlation searches demonstrate practical SIEM threat detection capabilities by identifying:
- **Impossible travel scenarios** revealing compromised credentials
- **Geographic attack patterns** showing distributed infrastructure
- **Credential stuffing attacks** that eventually succeed
- **Account takeover indicators** requiring immediate response

The queries can be extended with additional enrichment (GeoIP ASN, threat intelligence feeds) and integrated with SOAR platforms for automated response actions.

---
