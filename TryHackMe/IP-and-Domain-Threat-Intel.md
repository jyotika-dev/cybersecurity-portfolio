# IP and Domain Threat Intel - TryHackMe

> Investigate suspicious IPs and domains using threat intelligence enrichment

**Room Link:** https://tryhackme.com/room/ipanddomainthreatenintel

<img width="1574" height="928" alt="image" src="https://github.com/user-attachments/assets/818ffc10-a784-43a5-a68c-827fc5e7220c" />


---

## Scenario

SOC flagged suspicious artifacts for investigation:

**Domains:**
- advanced-ip-sccanner[.]com

**IPs:**
- 166[.]1[.]160[.]118
- 64[.]31[.]63[.]194
- 69[.]197[.]185[.]26
- 85[.]188[.]1[.]133

**Task:** Enrich with OSINT and determine if malicious

---

## Task 2: DNS Enrichment

### Domain Analysis

**Domain:** advanced-ip-sccanner[.]com

**Red flag:** Typosquatting (legitimate: Advanced IP Scanner)

### Questions

**Q1: IP addresses for A Record of advanced-ip-sccanner[.]com?**

Format: IP-1, IP-2

Use nslookup.io or similar tool.

- Answer: `172.67.189.143,104.21.9.202`

---

**Q2: Nameserver addresses associated with IP? (Defanged)**

Check NS records.

- Answer: `jaziel[.]ns[.]cloudflare[.]com, summer[.]ns[.]cloudflare[.]com`

---

## Task 3: IP Enrichment

### IP: 64[.]31[.]63[.]194

### RDAP Lookup

**Tool:** client.rdap.org

**Info gathered:**
- NetRange: 64.31.0.0 – 64.31.63.255
- Organisation: Netriplex LLC
- Entity: NOC2791-ARIN

### Questions

**Q1: When was IP logged for registration? (UTC: MM/DD/YY, H:MM:SS AM/PM)**

Check RDAP registration date.

- Answer: `12/27/2010, 3:51:03 PM`

---

**Q2: Roles assigned to entity NOC2791-ARIN?**

Check entity roles in RDAP output.

- Answer: `administrative,technical`

---

**Q3: Country name for IP 64[.]31[.]63[.]194?**

Geolocation lookup.

- Answer: `France`

---

**Q4: ASN for IP 64[.]31[.]63[.]194?**

Check bgpview.io or ipinfo.io.

- Answer: `AS136258`

---

## Task 4: Service Identification

### IP: 85[.]188[.]1[.]133

### Shodan Reconnaissance

**Tool:** shodan.io

**Check for:**
- Primary service
- Open ports
- Service banners

### Questions

**Q1: Primary service on IP 85[.]188[.]1[.]133?**

- Answer: `FTP`

---

**Q2: How many open ports identified?**

- Answer: `6`

---

**Q3: TLS certificate fingerprint for IP? (Use censys.io)**

Check certificate details.

- Answer: `48d6057099841bd18809fd61aa990b17779176de7799f301dac24879da553456`

---

**Q4: Certificate Transparency log entries on crt.sh? (Yay/Nay)**

Search fingerprint on crt.sh.

- Answer: `Yay`

---

## Task 5: Reputation and Passive DNS

### IP: 166[.]1[.]160[.]118

### VirusTotal Check

**Look for:**
- Communicating files
- Associated malware
- Detection ratio

### WHOIS Lookup

**Historical data:**
- Organisation info
- NetRange
- Location

### Questions

**Q1: File linked to IP 166[.]1.160[.]118?**

Check VirusTotal relations tab.

- Answer: `ff4c287c60ede1990442115bddd68201d25a735458f76786a938a0aa881d14ef.exe`

---

**Q2: Organisation from historical WHOIS lookups?**

- Answer: `Ace Data Centers, Inc`

---

## Task 7: Challenge

### IP: 170[.]130[.]202[.]134

**Q1: RIR associated with IP?**

Regional Internet Registry.

- Answer: `ARIN`

---

**Q2: ASN connected to IP?**

- Answer: `AS62904`

---

### Domain: santagift[.]shop

**Q1: Number of NS records for domain?**

Use dig or DNSdumpster.

- Answer: `4`

---

**Q2: Start of Authority (SOA) NS?**

Check SOA record.

- Answer: `ns-298.awsdns-37.com`

---

**Q3: When was domain registered? (DD/MM/YYYY)**

WHOIS lookup.

- Answer: `30/10/2022`

---

## Quick Reference

### OSINT Tools

**DNS Lookup:**
- nslookup.io
- DNSdumpster.com
- dig command

**IP Enrichment:**
- client.rdap.org (RDAP)
- ipinfo.io
- bgpview.io
- ipgeolocation.com

**Service Discovery:**
- shodan.io
- censys.io
- crt.sh (certificates)

**Reputation:**
- VirusTotal
- Cisco Talos
- AbuseIPDB

**WHOIS:**
- whois.domaintools.com
- whois.domains

### Investigation Flow

```
Suspicious Indicator
    ↓
DNS Resolution (A, NS, SOA)
    ↓
IP Enrichment (RDAP, ASN, GeoIP)
    ↓
Service Discovery (Shodan, Censys)
    ↓
Reputation Check (VT, Talos)
    ↓
Historical Analysis (WHOIS, Passive DNS)
    ↓
Verdict: Benign or Malicious
```

### Key Data Points

**Domain:**
- A records (IPs)
- NS records (nameservers)
- SOA records (authority)
- Registration date
- WHOIS info

**IP:**
- RIR (ARIN, RIPE, etc.)
- ASN (Autonomous System)
- Geolocation (country)
- NetRange
- Organisation
- Registration date

**Services:**
- Open ports
- Running services
- TLS certificates
- Service banners
- CVEs

**Reputation:**
- VT detections
- Associated malware
- Historical activity
- Abuse reports

### Common Commands

```bash
# DNS lookup
nslookup domain.com
dig domain.com ANY

# WHOIS
whois domain.com
whois 1.2.3.4

# Certificate check
openssl s_client -connect ip:443
```

### Red Flags

**Domains:**
- Typosquatting
- Recent registration
- Privacy-protected WHOIS
- Parked domains
- Suspicious TLDs (.shop, .xyz)

**IPs:**
- Hosting providers (disposable)
- Multiple open ports
- Known malware communication
- Suspicious ASN
- VPN/proxy services

### Enrichment Checklist

**DNS:**
- [ ] A records resolved
- [ ] NS records identified
- [ ] SOA record found
- [ ] Registration date checked

**IP:**
- [ ] RDAP lookup done
- [ ] ASN identified
- [ ] Geolocation verified
- [ ] Organisation found

**Services:**
- [ ] Open ports scanned
- [ ] Primary service ID'd
- [ ] TLS cert checked
- [ ] Banners analyzed

**Reputation:**
- [ ] VT checked
- [ ] Associated files found
- [ ] Historical WHOIS done
- [ ] Abuse reports reviewed

---

## Key Takeaways

- **DNS records** reveal infrastructure
- **Typosquatting** common phishing tactic
- **RDAP** provides authoritative IP data
- **ASN** shows network ownership
- **Shodan** reveals exposed services
- **Censys** checks TLS certificates
- **crt.sh** logs certificate transparency
- **VirusTotal** links IPs to malware
- **Historical WHOIS** shows changes
- **Hosting providers** = red flag
- **Recent domains** suspicious
- **Multiple open ports** unusual
- **Always enrich** before verdict

Context is everything
Happy hunting
