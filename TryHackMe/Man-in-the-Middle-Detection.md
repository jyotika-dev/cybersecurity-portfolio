# Man-in-the-Middle Detection - TryHackMe

> Detect and analyze MITM attacks in network traffic

**Room Link:** https://tryhackme.com/room/mitmdetection

<img width="1563" height="875" alt="image" src="https://github.com/user-attachments/assets/25bcc9b8-431a-4224-ada0-f6e03a14ca03" />


---

## Task 1: Introduction

Learn to detect MITM attacks through network monitoring and traffic analysis.

**Topics:**
- MITM attack vectors
- Indicators of compromise
- Network monitoring tools
- Incident response

**No answer needed**

---

## Task 2: Lab Connection

**Scenario:** Acme Corp detected unusual traffic suggesting MITM attack.

**Attack chain:**
1. ARP Spoofing (interception)
2. DNS Spoofing (redirection)
3. SSL Stripping (credential capture)

**Files location:** `mitm` folder on Desktop
- mitm_attack.log (complete attack chain)
- Splunk access: 10.201.12.128:8000
- Index: network_logs

**No answer needed**

---

## Task 3: MITM Overview

### What is MITM?

Attacker secretly intercepts and alters communication between two parties.

**Goals:**
- Eavesdrop on conversations
- Steal credentials
- Inject malicious content

### Attack Process

**1. Interception:**
- ARP spoofing
- DNS spoofing
- IP spoofing
- Rogue Wi-Fi

**2. Manipulation:**
- Decrypt encrypted data
- Inject malicious content
- Alter communications

### Common Techniques

- **Packet sniffing** - Capture unencrypted data
- **Session hijacking** - Steal session tokens
- **SSL stripping** - Downgrade HTTPS to HTTP
- **DNS spoofing** - Redirect to fake sites
- **Rogue AP** - Fake Wi-Fi networks

### Cyber Kill Chain Position

**Exploitation phase:**
- Exploits network protocol trust
- Intercepts communication
- Gains initial foothold

**Installation phase:**
- Delivers malicious payload
- Establishes persistence
- Injects malware

**No answer needed**

---

## Task 4: Detecting ARP Spoofing

**Gateway IP:** 192.168.10.1

### Questions

**Q1: How many ARP packets from gateway MAC?**

**Filter:**
```
arp.src.proto_ipv4 == 192.168.10.1
```

Look for legitimate gateway MAC address.

Count packets from that MAC.

- Answer: `10`

---

**Q2: What MAC address used by attacker to impersonate gateway?**

Same filter shows two MAC addresses for gateway IP.

**Red flag:** Multiple MACs claiming same IP

Look for suspicious MAC (02:fe:fe:fe:55:55).

High volume of "is-at" requests = attacker.

- Answer: `02:fe:fe:fe:55:55`

---

**Q3: How many Gratuitous ARP replies for 192.168.10.1?**

**Gratuitous ARP:**
- Unsolicited ARP reply
- Source IP = Target IP
- Announces MAC/IP mapping

Same filter from Q1.

Count gratuitous replies.

- Answer: `2`

---

**Q4: How many unique MAC addresses claimed same IP?**

From previous analysis: 2 different MACs for 192.168.10.1

- Legitimate gateway MAC
- Attacker's spoofed MAC

- Answer: `2`

---

**Q5: How many ARP spoofing packets from attacker?**

**Filter:**
```
arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1 && eth.src == 02:fe:fe:fe:55:55
```

**Breakdown:**
- opcode 2 = ARP reply
- Source IP = gateway
- Source MAC = attacker

- Answer: `14`

---

## Task 5: Unmasking DNS Spoofing

**Target domain:** corp-login.acme-corp.local

### Questions

**Q1: How many DNS responses for domain?**

**Filter:**
```
dns && dns.qry.name == "corp-login.acme-corp.local" && dns.flags.response == 1

```

Count DNS response packets.

- Answer: `211`

---

**Q2: How many DNS requests from IPs other than 8.8.8.8?**

**Filter:**
```
dns && ip.src != 8.8.8.8
```

Look for DNS queries not from Google DNS.

**Attacker IP:** 192.168.10.55 (2 requests)

- Answer: `2`

---

**Q3: What IP did attacker's forged DNS response return?**

Check DNS answers in forged responses.

Attacker returns own IP to redirect traffic.

- Answer: `192.168.10.55`

---

## Task 6: Spotting SSL Stripping

**Attacker IP:** 192.168.10.55 (from DNS investigation)

### Questions

**Q1: How many POST requests for domain?**

**Filter:**
```
http.request.method == "POST" && http.host == "corp-login.acme-corp.local" && ip.dst == 192.168.10.55
```

**Why ip.dst?**
- MITM intercepts traffic
- Data sent to attacker's IP
- Attacker is destination

- Answer: `1`

---

**Q2: What's the victim's password in plaintext?**

Right-click POST request → Follow → HTTP Stream

Check form data in request body.

**SSL stripping effect:** Credentials in cleartext.

- Answer: `Secret123!`

---

## Quick Reference

### MITM Attack Chain

```
1. ARP Spoofing → Position in network path
2. DNS Spoofing → Redirect to attacker
3. SSL Stripping → Downgrade to HTTP
4. Credential Capture → Steal plaintext data
```

### ARP Spoofing Detection

**Indicators:**
- Multiple MACs for same IP
- Gratuitous ARP replies
- High volume ARP replies
- MAC address mismatches

### DNS Spoofing Detection

**Indicators:**
- Multiple DNS answers for same query
- Different IPs returned
- Responses from non-authoritative servers
- TTL mismatches

### SSL Stripping Detection

**Indicators:**
- HTTP used instead of HTTPS
- Credentials in cleartext
- Missing TLS handshake
- Downgrade from secure connection

### ARP Opcodes

- **1** = ARP Request (Who has?)
- **2** = ARP Reply (I have)

### Detection Workflow

**1. Identify gateway:**
- Find legitimate gateway IP/MAC
- Document normal ARP behavior

**2. Detect ARP spoofing:**
- Look for duplicate MACs
- Check gratuitous ARP
- Count spoofing packets

**3. Detect DNS spoofing:**
- Compare DNS responses
- Check response sources
- Verify returned IPs

**4. Detect SSL stripping:**
- Look for HTTP instead of HTTPS
- Find cleartext credentials
- Check POST requests

**5. Correlate findings:**
- Link attacker IP across attacks
- Build timeline
- Document IOCs

### Key IOCs

**From this investigation:**
- Attacker MAC: 02:fe:fe:fe:55:55
- Attacker IP: 192.168.10.55
- Gateway IP: 192.168.10.1
- Spoofed domain: corp-login.acme-corp.local
- Stolen credentials: Secret123!

---

## Key Takeaways

- **MITM** intercepts communication between parties
- **ARP spoofing** positions attacker in network path
- **Multiple MACs** for same IP = ARP poisoning
- **Gratuitous ARP** announces fake MAC/IP mapping
- **DNS spoofing** redirects traffic to attacker
- **192.168.10.55** is attacker's IP
- **SSL stripping** downgrades HTTPS to HTTP
- **Cleartext credentials** exposed after stripping
- **Follow HTTP stream** reveals form data
- **MITM** spans Exploitation and Installation phases
- **Detect early** to disrupt attack chain
- **Correlate across** ARP, DNS, and HTTP logs

Chain multiple MITM techniques for complete attack
Happy defending!
