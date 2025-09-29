# 🐧 LinuxJourney - DNS

This file summarizes my learning from **LinuxJourney** on Domain Name System fundamentals.  
https://labex.io/lesson/what-is-dns  
I practiced these on my local terminal and network environments.

---

## 🔹 What is DNS?
- **Domain Name System** → distributed database mapping hostnames to IP addresses.
- Translates human-readable names (www.google.com) to IP addresses (192.78.12.4).
- **Contact list for the Internet** → allows tracking by name instead of numbers.
- Distributed architecture → multiple organizations manage their portions.
- Essential for user-friendly internet navigation.

## 🔹 DNS Components
- **Name Server** → answers DNS queries from clients/servers.
- **Authoritative servers** → hold actual DNS records.
- **Recursive servers** → query other servers until finding answer.
- **Zone Files** → store domain information on name servers.
- **Resource Records** → entries containing host information.
- **Record structure** → name, TTL, class (IN), type (A/MX), data.

## 🔹 DNS Process
- **Local DNS Server** → first point of contact for queries.
- **Root Servers** → 13 mirrored servers handling top-level domains.
- **Top-Level Domain (TLD)** → .com, .org, .net servers.
- **Authoritative DNS Server** → contains actual zone file records.
- **Recursive resolution** → funnel down from root to authoritative server.
- ISP DNS servers typically handle recursive queries for users.

## 🔹 /etc/hosts
- **Local hostname mapping** → checked before DNS queries.
- `/etc/hosts` → maps IP addresses to hostnames locally.
- **Format** → IP_address hostname aliases.
- `/etc/resolv.conf` → DNS nameserver configuration (often auto-managed).
- Can override DNS by adding local entries.
- Security note: use firewall rules, not hosts.deny/hosts.allow.

## 🔹 DNS Setup
- **BIND** → most popular, full-featured DNS server (Berkeley Internet Name Domain).
- **DNSmasq** → lightweight, easier configuration for smaller networks.
- **PowerDNS** → flexible, database-backed (MySQL, PostgreSQL).
- Standard Linux DNS implementation → BIND.
- Choice depends on network size and feature requirements.

## 🔹 DNS Tools
- `nslookup www.google.com` → query name servers for resource records.
- `dig www.google.com` → detailed DNS information (domain information groper).
- **dig output** → query time, answer section, authority records.
- **nslookup** → simpler interface for basic queries.
- **dig** → more flexible, better for troubleshooting.

---

📌 **Applied Insight**:  
DNS knowledge is crucial in cybersecurity:
- **Reconnaissance** → DNS enumeration reveals network infrastructure and subdomains.  
- **DNS poisoning** → attackers manipulate DNS records for traffic redirection.  
- **DDoS attacks** → DNS amplification attacks exploit open resolvers.  
- **Threat hunting** → suspicious DNS queries indicate C2 communication or data exfiltration.  
- **Incident response** → DNS logs provide attack timeline and malicious domain indicators.
