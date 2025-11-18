# Web Security Essentials - TryHackMe

> Learn fundamentals of web application security and defense

**Room Link:** https://tryhackme.com/room/websecurityessentials

<img width="1548" height="863" alt="image" src="https://github.com/user-attachments/assets/29c0dbb3-2974-4ef1-89b7-641aaa0634eb" />


---

## Task 1: Introduction

**Topics:**
- Desktop to web application shift
- Why web apps are targets
- Web infrastructure
- Security measures

**No answer needed**

---

## Task 2: Why Web?

### Evolution

**1990s:** Desktop applications (speed/connectivity limits)
**2000s:** Dynamic web apps (email, social media, banking)
**2010s:** Cloud computing and SaaS boom
**Today:** Everything in browser

### Security Tradeoffs

**Advantages:**
- Increased accessibility
- Faster updates
- Better compatibility
- Reduced resource usage

**Risks:**
- Always online = always exposed
- Connects to backend systems
- Entry point for larger attacks

### Real-World Examples

**Equifax (2017):**
- Apache vulnerability
- 150 million affected
- Database access

**Capital One (2019):**
- Misconfigured WAF
- 100+ million affected
- Cloud infrastructure exposed

### Questions

**Q1: Have applications shifted from desktop to web? (Yea/Nay)**

- Answer: `Yea`

---

**Q2: Who is responsible for securing users' data?**

Web app owner's responsibility.

- Answer: `Web App Owner`

---

## Task 3: Web Infrastructure

### Request-Response Cycle

```
User → Browser → Request → Web Server → Response → Browser → User
```

### Three Main Components

**1. Application:**
- Code, images, styles
- How site looks/functions

**2. Web Server:**
- Hosts application
- Listens for requests
- Returns responses

**3. Host Machine:**
- Underlying OS (Linux/Windows)
- Runs web server

### Common Web Servers

- **Apache** - WordPress hosting
- **Nginx** - High performance (Netflix, GitHub)
- **IIS** - Microsoft enterprise

### Questions

**Q1: What does browser send to receive webpage?**

- Answer: `Request`

---

**Q2: Web server for WordPress?**

- Answer: `Apache`

---

**Q3: What do we call OS running web server?**

- Answer: `Host Machine`

---

## Task 4: Protecting the Web

### Application Layer

**Secure Coding:**
- Avoid insecure functions
- Proper error handling
- Remove sensitive info

**Input Validation:**
- Validate user input
- Sanitize data
- Prevent injection attacks

**Access Control:**
- Role-based restrictions
- Principle of least privilege

### Web Server Layer

**Logging:**
- Access logs
- Track all requests
- Incident reconstruction

**WAF:**
- Filter malicious traffic
- Block attacks
- Rule-based protection

**CDN:**
- Reduce direct exposure
- Integrated WAF
- DDoS protection

### Host Machine Layer

**Least Privilege:**
- Low-privilege service accounts
- Minimize permissions

**System Hardening:**
- Disable unnecessary services
- Close unused ports
- Reduce attack surface

**Antivirus:**
- Endpoint protection
- Block known malware
- Detect web shells

### Universal Best Practices

**Strong Authentication:**
- Protect code, admin panels, hosts
- Multi-factor authentication

**Patch Management:**
- Update dependencies
- Update web server
- Update OS

### Access Logs

**Fields tracked:**
- Client IP address
- Timestamp
- Requested page
- Response status
- User agent

### Questions

**Q1: Concept of stopping/limiting damage?**

- Answer: `Mitigation`

---

**Q2: Control ensuring software up to date?**

- Answer: `Patch Management`

---

## Task 5: Defense Systems

### Content Delivery Network (CDN)

**Purpose:**
- Cache content closer to users
- Reduce latency
- Act as buffer

**Security Benefits:**
- IP masking (hides origin server)
- DDoS protection (absorbs traffic)
- Enforced HTTPS (TLS encryption)
- Integrated WAF

**Examples:**
- Cloudflare CDN
- Amazon CloudFront
- Azure Front Door

### Web Application Firewall (WAF)

**Types:**

**Cloud-based (Reverse Proxy):**
- Sits in front of server
- Easy deployment
- Great scalability

**Host-based:**
- Software on server
- Per-application control

**Network-based:**
- Physical/virtual appliance
- Network perimeter
- Enterprise use

### WAF Detection Methods

| Method | How it Works | Example |
|--------|-------------|---------|
| **Signature-Based** | Match known patterns | User-Agent: sqlmap/1.8.1 |
| **Heuristic-Based** | Analyze context/behavior | Long query with special chars |
| **Anomaly Analysis** | Flag deviations | Repeated login attempts |
| **IP Reputation** | Block known bad IPs | Requests from unusual locations |

### Antivirus (AV)

**Purpose:**
- Protect endpoints
- Signature-based detection
- Compare against malware database

**Role in web security:**
- Detect malicious uploads
- Block web shells
- Identify post-exploitation tools
- One layer in defense-in-depth

### Questions

**Q1: WAF type running on same system as application?**

- Answer: `Host-Based`

---

**Q2: Detection matching known malicious patterns?**

- Answer: `Signature-Based`

---

## Task 6: Practice Scenario

**Scenario:** Secure the new site "Secure-A-Site" before deployment.

**Three layers to secure:**
1. Web Application
2. Web Server
3. Host Machine

Click "View Site" button to practice.

Apply security measures learned in previous tasks.

Claim flags after completing each section.

### Questions

**Q1: Flag for securing Web Application?**

Complete application security section.

- Answer: `THM{web_app_secured!}`

---

**Q2: Flag for securing Web Server?**

Complete web server security section.

- Answer: `THM{server_security_expert!}`

---

**Q3: Flag for securing Host Machine?**

Complete host security section.

- Answer: `THM{the_final_security_layer!}`

---

## Task 7: Conclusion

**No answer needed**

---

## Quick Reference

### Web Security Layers

```
[Application Layer]
    ↓
- Input validation
- Secure coding
- Access control
    ↓
[Web Server Layer]
    ↓
- WAF
- CDN
- Logging
    ↓
[Host Layer]
    ↓
- Least privilege
- System hardening
- Antivirus
```

### Common Attack Targets

- **Application vulnerabilities** (SQLi, XSS)
- **Misconfigurations** (WAF, permissions)
- **Outdated software** (unpatched servers)
- **Weak authentication** (default credentials)

### Defense Checklist

**Application:**
- [ ] Input validation enabled
- [ ] Secure coding practices
- [ ] Role-based access control
- [ ] Error handling configured

**Web Server:**
- [ ] Access logging enabled
- [ ] WAF configured
- [ ] CDN implemented
- [ ] HTTPS enforced

**Host:**
- [ ] Services hardened
- [ ] Unnecessary ports closed
- [ ] AV installed
- [ ] Updates applied
- [ ] 
---

## Key Takeaways

- **Web apps** shifted from desktop over decades
- **Always online** = always exposed
- **Three components** need protection
- **Apache** hosts WordPress sites
- **Request-response** cycle fundamental
- **Mitigation** stops or limits damage
- **Patch management** keeps systems updated
- **CDN** masks IP and absorbs DDoS
- **WAF** has three deployment types
- **Host-based** WAF on same system
- **Signature-based** matches known patterns
- **Access logs** track all requests
- **Defense-in-depth** uses multiple layers
- **Strong auth** protects all layers

Layer defenses for comprehensive protection
Happy securing!
