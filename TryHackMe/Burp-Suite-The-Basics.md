# Burp Suite: The Basics - TryHackMe

> An introduction to using Burp Suite for web application pentesting

**Room Link:** https://tryhackme.com/room/burpsuitebasics

<img width="1309" height="812" alt="image" src="https://github.com/user-attachments/assets/4d447e45-8282-42d5-b9cc-d4c5fd6e4615" />

---

## Task 1: Introduction

### Room Overview

This room provides a foundational understanding of Burp Suite, focusing on:
- Introduction to Burp Suite
- Overview of its tools
- Installing Burp Suite
- Navigating and configuring the framework
- Core focus on Burp Proxy

**No answer needed**

---

## Task 2: What is Burp Suite?

### What is Burp Suite?

Burp Suite is a framework written in Java that provides tools for penetration testing of web and mobile applications. It captures and manipulates traffic between the attacker and web server.

### Editions Available

**Burp Suite Community**
- Free for non-commercial use
- Basic features
- Good for learning

**Burp Suite Professional**
- Paid license
- Advanced features
- More automation

**Burp Suite Enterprise**
- Runs on a server
- Provides continuous scanning
- Automated vulnerability detection

### Questions

**Which edition of Burp Suite runs on a server and provides constant scanning for target web apps?**
- Answer: `Burp Suite Enterprise`

---

**Burp Suite is frequently used when attacking web applications and ____ applications.**
- Answer: `mobile`

---

## Task 3: Features of Burp Suite Community

### Core Tools

**Proxy**
- Intercepts and modifies requests/responses
- Acts as intermediary between you and target

**Repeater**
- Capture and modify requests
- Resend same request multiple times

**Intruder**
- Spray endpoint with requests
- Used for brute-force attacks
- Fuzzing endpoints

**Decoder**
- Decode captured information
- Encode payloads before sending

**Comparer**
- Compare two pieces of data
- Word or byte level comparison

**Sequencer**
- Assess randomness of tokens
- Test session cookie security
- Analyze random data generation

### Extensions

- Write in Java, Python, or Ruby
- Burp Suite Extender module
- BApp Store marketplace
- Many free community extensions

### Questions

**Which Burp Suite feature allows us to intercept requests between ourselves and the target?**
- Answer: `Proxy`

---

**Which Burp tool would we use to brute-force a login form?**
- Answer: `Intruder`

---

## Task 4: Installation

### Installation Options

**Pre-installed:**
- Comes with Kali Linux
- Check with: `burpsuite`

**Manual Installation:**
- Download from PortSwigger website
- Available for Linux, macOS, Windows
- Can run as JAR file (requires Java)

**Kali Linux:**
```bash
sudo apt install burpsuite
```

**No answer needed**

---

## Task 5: The Dashboard

### Dashboard Layout

The Dashboard is split into four quadrants:

**Tasks Menu**
- Define background tasks
- Set up automated processes

**Event Log**
- Shows what Burp Suite is doing
- Starting proxy
- Connection information

**Issue Activity** (Pro only)
- Lists vulnerabilities found
- Automated scanner results

**Advisory Section** (Pro only)
- Detailed vulnerability information
- References and remediations
- Export to reports

### Questions

**What menu provides information about the actions performed by Burp Suite, such as starting the proxy, and details about connections made through Burp?**
- Answer: `Event log`

---

## Task 6: Navigation

### Menu Bar Navigation

Navigate using top menu bars to switch between modules.

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl + Shift + D` | Switch to Dashboard |
| `Ctrl + Shift + T` | Switch to Target tab |
| `Ctrl + Shift + P` | Switch to Proxy tab |
| `Ctrl + Shift + I` | Switch to Intruder tab |
| `Ctrl + Shift + R` | Switch to Repeater tab |

### Questions

**Which tab Ctrl + Shift + P will switch us to?**
- Answer: `Proxy tab`

---

## Task 7: Options

### Settings Types

**Global Settings (User Settings)**
- Affect entire installation
- Persist across sessions

**Project Settings**
- Specific to current project
- Apply only during session
- Not saved in Community Edition

### Accessing Settings

Click **Settings** button in top navigation bar.

**Settings Menu Features:**
- **Search** - Find specific settings
- **Type Filter** - User vs Project settings
- **Categories** - Browse by category

### Questions

**In which category can you find a reference to a "Cookie jar"?**

Navigate to settings and look for cookie-related options.

- Answer: `Sessions`

---

**In which base category can you find the "Updates" sub-category, which controls the Burp Suite update behaviour?**
- Answer: `Suite`

---

**What is the name of the sub-category which allows you to change the keybindings for shortcuts in Burp Suite?**
- Answer: `Hotkeys`

---

**If we have uploaded Client-Side TLS certificates, can we override these on a per-project basis (yea/nay)?**

Search for "TLS" in settings to find Client TLS certificates section.

- Answer: `yea`

---

## Task 8: Introduction to the Burp Proxy

### What is Burp Proxy?

Core tool that captures and manipulates traffic between user and target web server.

### Key Features

**Intercepting Requests**
- Holds requests for inspection
- Options: forward, drop, edit, send to other modules
- Toggle with "Intercept is on" button

**Control and Analysis**
- Full control over web traffic
- Test and modify requests
- Analyze responses

**Traffic Logging**
- HTTP history tab
- WebSockets history tab
- Review captured traffic

**Proxy Settings**
- Response interception rules
- Match and Replace (regex)
- Modify requests dynamically

**No answer needed**

---

## Task 9: Connecting through the Proxy (FoxyProxy)

### Setting Up FoxyProxy

FoxyProxy is a browser extension that makes it easy to route traffic through Burp Proxy.

**Installation:**
1. Install FoxyProxy extension
2. Configure proxy settings
3. Set to 127.0.0.1:8080 (Burp's default)

**Quick Toggle:**
- Enable/disable proxy with one click
- Switch between different proxies
- Save multiple configurations

**No answer needed**

---

## Task 10: Site Map and Issue Definitions

### Target Tab Features

**Site Map**
- Displays web application structure as tree
- Automatically generates while browsing
- Maps APIs and identifies pages
- Pro version: automated crawling

**Issue Definitions**
- List of vulnerabilities Burp scans for
- Descriptions and references
- Useful for manual testing
- Great for reporting

**Scope Settings**
- Control which domains/IPs to test
- Focus testing on specific targets
- Exclude irrelevant traffic

### Questions

**What is the flag you receive after visiting the unusual endpoint?**

Browse the site at http://MACHINE_IP/ while having the site map open.

Look for a URL with random numbers and letters in the site map.

Visit that endpoint or check the response in Burp.

- Answer: `THM{NmNlZTliNGE1MWU1ZTQzMzgzNmFiNWVk}`

---

## Task 11: The Burp Suite Browser

### Built-in Chromium Browser

Burp Suite includes a pre-configured Chromium browser.

**Starting the Browser:**
- Click "Open Browser" in Proxy tab
- Automatically routes through proxy
- No manual configuration needed

**Settings:**
- Customize in project/user options
- Various browser preferences

**Linux Root User Issue:**

Running Burp as root may prevent browser from starting.

**Solutions:**
1. **Smart:** Run Burp under low-privilege user
2. **Easy:** Enable "Allow Burp's browser to run without a sandbox"
   - Settings > Tools > Burp's browser
   - Note: Less secure, use cautiously

**No answer needed**

---

## Task 12: Scoping and Targeting

### Why Use Scoping?

- Focus on specific applications
- Avoid unnecessary traffic overload
- Cleaner work environment

### Setting the Scope

**Add to Scope:**
1. Go to Target tab
2. Right-click target
3. Select "Add To Scope"
4. Choose to stop logging out-of-scope traffic

**Scope Settings:**
- Target tab > Scope sub-tab
- Include/exclude domains or IPs
- Define testing boundaries

### Limiting Intercepted Traffic

**By default:** Proxy intercepts everything

**To limit:**
1. Go to Proxy settings sub-tab
2. Under "Intercept Client Requests"
3. Enable "And URL Is in target scope"

**Result:** Only intercepts in-scope traffic

**No answer needed**

---

## Task 13: Proxying HTTPS

### TLS Certificate Issue

When intercepting HTTPS traffic, you may see certificate warnings about PortSwigger CA not being trusted.

### Solution: Install CA Certificate

**Step 1: Download Certificate**
- With Burp Proxy active
- Navigate to http://burp/cert
- Download `cacert.der`

**Step 2: Access Firefox Settings**
- Type `about:preferences` in URL bar
- Search for "certificates"
- Click "View Certificates"

**Step 3: Import Certificate**
- Click "Import"
- Select `cacert.der`

**Step 4: Set Trust**
- Check "Trust this CA to identify websites"
- Click OK

**Result:** No more certificate errors!

**Note:** AttackBox is pre-configured for this.

**No answer needed**

---

## Task 14: Example Attack

### Testing for Reflected XSS

**Target:** Support form at http://MACHINE_IP/ticket/

**Goal:** Bypass client-side filter

### Attack Steps

**1. Enable Burp Proxy**
- Ensure intercept is on

**2. Submit Legitimate Data**
```
Email: pentester@example.thm
Query: Test Attack
```

**3. Intercept the Request**
- Burp captures the request

**4. Modify the Request**
- Change email to payload:
```html
<script>alert("Succ3ssful XSS")</script>
```

**5. URL Encode Payload**
- Select payload
- Press `Ctrl + U`

**6. Forward Request**
- Click "Forward" button

**7. Check Result**
- Look for alert box
- Confirms XSS vulnerability

### What is Reflected XSS?

**Reflected XSS** occurs when:
- Malicious script injected into webpage
- Affects only the person making request
- Script reflected back by server
- Executes in victim's browser

**Potential Impact:**
- Steal cookies
- Capture session tokens
- Perform actions as victim

**No answer needed**

---

## Task 15: Conclusion

Congratulations! You've completed the Burp Suite: The Basics room!

### What You've Learned

✅ Burp Suite interface
✅ Configuration options
✅ Burp Proxy functionality
✅ Traffic interception
✅ Request modification
✅ Site mapping
✅ Basic attack techniques

### Next Steps

- Practice with Burp Suite
- Explore all features
- Experiment with settings
- Try different tools
- Continue learning web pentesting

**No answer needed**

---

## Quick Reference

### Burp Suite Editions

| Edition | Features | Cost |
|---------|----------|------|
| Community | Basic tools | Free |
| Professional | Advanced features, automation | Paid |
| Enterprise | Continuous scanning, server-based | Paid |

### Core Tools

| Tool | Purpose |
|------|---------|
| Proxy | Intercept and modify traffic |
| Repeater | Resend modified requests |
| Intruder | Brute-force and fuzzing |
| Decoder | Encode/decode data |
| Comparer | Compare data |
| Sequencer | Test randomness |

### Keyboard Shortcuts

| Shortcut | Tab |
|----------|-----|
| `Ctrl + Shift + D` | Dashboard |
| `Ctrl + Shift + T` | Target |
| `Ctrl + Shift + P` | Proxy |
| `Ctrl + Shift + I` | Intruder |
| `Ctrl + Shift + R` | Repeater |

### Proxy Configuration

| Setting | Value |
|---------|-------|
| Default Address | 127.0.0.1 |
| Default Port | 8080 |
| Certificate | http://burp/cert |

### Dashboard Quadrants

| Quadrant | Purpose |
|----------|---------|
| Tasks | Background tasks |
| Event Log | Activity information |
| Issue Activity | Vulnerabilities (Pro) |
| Advisory | Detailed info (Pro) |

---

## Common Workflows

### Basic Interception
1. Enable Burp Proxy
2. Configure browser (FoxyProxy)
3. Browse target website
4. Intercept requests
5. Modify as needed
6. Forward or drop

### Setting Up Scope
1. Target tab
2. Right-click target
3. Add to scope
4. Stop logging out-of-scope
5. Configure proxy to only intercept in-scope

### Testing for XSS
1. Find input field
2. Submit test data
3. Intercept request
4. Inject XSS payload
5. URL encode
6. Forward request
7. Check for execution

---

## Key Takeaways

- **Burp Suite is essential** for web application pentesting
- **Burp Proxy** is the core tool for traffic interception
- **Community Edition** is free and great for learning
- **Scope your targets** to avoid traffic overload
- **Certificate installation** is needed for HTTPS
- **Client-side filters** can be bypassed easily
- **Site map** helps understand application structure
- **Keyboard shortcuts** speed up workflow
- **FoxyProxy** makes browser configuration easy
- **Practice makes perfect** - experiment with features

Burp Suite is the industry standard for web application security testing. Master the basics, then explore advanced features
