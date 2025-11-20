# Detecting Web DDoS - TryHackMe

> Learn to detect and analyze denial-of-service attacks

**Room Link:** https://tryhackme.com/room/detectingwebddos

<img width="1549" height="855" alt="image" src="https://github.com/user-attachments/assets/fe32c8d1-dce4-43d7-845d-856a1f0f8653" />


---

## Task 1: Introduction

**Topics:**
- DoS and DDoS attacks
- Attacker motives
- Log analysis techniques
- Detection and mitigation

**No answer needed**

---

## Task 2: DoS and DDoS Attacks

### Denial-of-Service (DoS)

Single machine attacking target.

**Goal:** Overwhelm service to make it unavailable.

**Example:** Abuse search form with malformed input to crash application.

### Distributed DoS (DDoS)

**Uses botnet** - army of compromised devices.

**Advantage:** Multiple sources = more traffic = harder to block.

### Attack Types

| Type | Description |
|------|-------------|
| **Slowloris** | Partial HTTP requests tie up resources |
| **HTTP Flood** | Massive HTTP request volume |
| **Cache Bypass** | Force origin server response |
| **Oversized Query** | Resource-intensive requests |
| **Login/Form Abuse** | Overload authentication |
| **Input Validation** | Exploit poor input handling |

### Questions

**Q1: Attack class disrupting availability?**

- Answer: `Denial-of-Service`

---

**Q2: Network of compromised machines?**

- Answer: `Botnet`

---

## Task 3: Attack Motives

### Why Attack?

| Motive | Goal | Example |
|--------|------|---------|
| **Financial Loss** | Stop sales/revenue | E-commerce during holidays |
| **Extortion** | Ransom payment | Threaten bank with DDoS |
| **Hacktivism** | Political protest | Government sites |
| **Distraction** | Cover other attacks | DDoS while hacking elsewhere |
| **Competition** | Hurt rival business | Product launch disruption |
| **Denial of Wallet** | Rack up cloud costs | Repeated AWS requests |
| **Reputation** | Lose customer trust | Game server crashes |

### Real Examples

**BBC (2015):**
- New Year's Eve attack
- Hours offline
- New World Hacking test

**Microsoft (2023):**
- Azure, OneDrive, Outlook down
- Anonymous Sudan claimed
- HTTP flooding + Slowloris

### Questions

**Q1: Motive making customers lose confidence?**

- Answer: `Reputational Damage`

---

**Q2: Most likely motive for Microsoft 2023 attack?**

- Answer: `Hacktivism`

---

## Task 4: Log Analysis

### Key Indicators

| Indicator | Example | Description |
|-----------|---------|-------------|
| **High Request Rate** | 1000 GET /login | Flood resource-heavy pages |
| **Odd User-Agents** | curl/7.6.88 | Automated tools |
| **Geographic Anomalies** | IPs worldwide | Botnet distribution |
| **Burst Timestamps** | 50 requests/second | Automation pattern |
| **Server Errors (5xx)** | 503 spike | Resources maxed out |
| **Logic Abuse** | ?limit=999999 | Overload queries |

### Targeted Resources

**Common targets:**
- `/login` - Authentication processes
- `/search` - Database queries
- `/api` - Critical endpoints
- `/register` - Database writes
- `/checkout` - Payment processing

### Log Pattern

**Normal:**
```
10.10.10.100 GET /index.html 200
10.10.10.101 GET /products 200
```

**Attack:**
```
203.0.113.55 GET /login.php 200 (repeated)
203.0.113.55 GET /login.php 200
203.0.113.55 GET /login.php 200
```

**Service Down:**
```
10.10.10.100 GET /index.html 503
10.10.10.101 GET /products 503
```

### Questions

**Q1: Attacker's IP in access.log?**

Check for repeated requests pattern.

- Answer: `203.12.23.195`

---

**Q2: Repeatedly targeted page?**

- Answer: `/login`

---

**Q3: Error code legitimate users receive?**

- Answer: `503`

---

## Task 5: Leveraging SIEMs

**Access:** http://10.10.40.78:8000

### Splunk Analysis

**Basic search:**
```
index="main"
```

**Fields available:**
- uri
- clientip
- useragent
- status

### Questions

**Q1: Most frequently requested uri?**

Click uri field, check top value.

- Answer: `/search`

---

**Q2: clientip with most requests?**

Click clientip field.

- Answer: `203.0.113.7`

---

**Q3: How many IPs in botnet?**

**Search:**
```
index="main" clientip="203*"
```

Count unique IPs starting with 203.

- Answer: `60`

---

**Q4: Most common useragent by attackers?**

Click useragent field.

- Answer: `Java/1.8.0_181`

---

**Q5: Peak requests per second?**

**Search:**
```
index="main" | timechart span=1s count by 203.0.113.7
```

Click Visualization, hover over peak.

- Answer: `207`

---

**Q6: First legitimate IP receiving 503?**

**Search:**
```
index="main" AND status="503" AND clientip=10*
```

Go to last page (earliest entries).

- Answer: `10.10.0.27`

---

## Task 6: Defense

### Application Defenses

**Secure Development:**
- Input validation
- Sanitize queries
- Error handling

**Challenges:**
- CAPTCHA puzzles
- JavaScript challenges
- Bot detection

### Network Defenses

**CDN Benefits:**
- Cache content at edge
- Absorb attack traffic
- Load balancing
- Geographic distribution

**WAF Features:**
- Inspect traffic
- Block/challenge/allow
- Rate limiting
- Custom rules

**Example rule:**
```
IF requests to /login > 5 per minute THEN block
```

### Real Mitigation

**Google (2023):**
- 398 million requests/second mitigated

**Cloudflare:**
- 11.5 Tbps attack (35 seconds)
- Largest ever recorded

### Bypass Techniques

**Attackers try:**
- Random query parameters (?a=abcd)
- Change User-Agents
- Spoof referrers
- Geographic diversity

### Questions

**Q1: Challenge blocking bots with puzzle?**

- Answer: `CAPTCHA`

---

**Q2: CDN feature spreading traffic?**

- Answer: `Load-balancing`

---

## Task 7: Conclusion

**No answer needed**

---

## Quick Reference

### DoS vs DDoS

```
DoS  = Single attacker → Target
DDoS = Botnet (many devices) → Target
```

### Detection Checklist

- [ ] High request rate
- [ ] Unusual User-Agents
- [ ] Geographic anomalies
- [ ] Timestamp bursts
- [ ] 5xx error spikes
- [ ] Logic abuse in queries

### Defense Layers

```
[User Request]
    ↓
[CDN - Cache & Filter]
    ↓
[WAF - Inspect & Block]
    ↓
[Rate Limiting]
    ↓
[Origin Server]
```

---

## Key Takeaways

- **DoS** = single source attack
- **DDoS** uses **botnet** for scale
- **Slowloris** sends partial requests
- **HTTP Flood** = volume attack
- **Hacktivism** politically motivated
- **503** = Service Unavailable
- **High request rate** key indicator
- **Odd User-Agents** reveal automation
- **Geographic anomalies** suggest botnet
- **SIEM** makes analysis efficient
- **Splunk** visualizes patterns
- **CAPTCHA** challenges bots
- **CDN** absorbs attack traffic
- **WAF** filters malicious requests
- **Load balancing** prevents overload

Detect patterns early to mitigate DDoS
Happy defending
