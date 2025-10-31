# Wireshark: Packet Operations - TryHackMe

> Master packet filtering, statistics, and advanced Wireshark features

**Room Link:** https://tryhackme.com/room/wiresharkpacketoperations

<img width="1836" height="899" alt="image" src="https://github.com/user-attachments/assets/c535d12a-bb2c-4a33-8fbe-49f0f83b9b20" />

**Prerequisites:** Complete "Wireshark: The Basics" first

**File:** Exercise.pcapng



---

## Task 1: Introduction

Part 2 of the Wireshark series focuses on advanced features: statistics, filters, operators, and functions.

---

## Task 2: Statistics | Summary

### Key Statistics Features

**Resolved Addresses** - IP addresses and DNS names
```
Statistics → Resolved Addresses
```

**Protocol Hierarchy** - All protocols in tree view with percentages
```
Statistics → Protocol Hierarchy
```

**Conversations** - Traffic between two endpoints
```
Statistics → Conversations
```

**Endpoints** - Unique endpoints per protocol
```
Statistics → Endpoints
```

### Questions

**Investigate the resolved addresses. What is the IP address of the hostname starting with "bbc"?**
```
Statistics → Resolved Addresses
Search for "bbc"
```
- Answer: `199.232.24.81`

**What is the number of IPv4 conversations?**
```
Statistics → Conversations → IPv4 tab
```
- Answer: `435`

**How many bytes (k) were transferred from the "Micro-St" MAC address?**
```
Statistics → Endpoints → Ethernet tab
Enable "Name Resolution"
Sort by address, find "Micro-St"
```
- Answer: `7474`

**What is the number of IP addresses linked with "Kansas City"?**
```
Statistics → Endpoints → IPv4 tab
Sort by City, find "Kansas City"
```
- Answer: `4`

**Which IP address is linked with "Blicnet" AS Organisation?**
```
Statistics → Endpoints → IPv4 tab
Sort by Organization, find "Blicnet d.o.o"
```
- Answer: `188.246.82.7`

---

## Task 3: Statistics | Protocol Details

### Protocol-Specific Statistics

**IPv4 and IPv6** - Statistics for specific IP versions
```
Statistics → IPv4 Statistics
Statistics → IPv6 Statistics
```

**DNS** - DNS packets breakdown
```
Statistics → DNS
```

**HTTP** - HTTP packets breakdown (requests, responses)
```
Statistics → HTTP
```

### Questions

**What is the most used IPv4 destination address?**
```
Statistics → Endpoints → IPv4 tab
Sort by "Rx Packets" or "Rx Bytes"
```
- Answer: `10.100.1.33`

**What is the max service request-response time of the DNS packets?**
```
Statistics → DNS
Look at Service Stats → request-response time
```
- Answer: `0.467897`

**What is the number of HTTP Requests sent to "rad[.]msn[.]com"?**
```
Statistics → HTTP → Load Distribution
Scroll to find "rad.msn.com"
```
- Answer: `39`

---

## Task 4: Packet Filtering | Principles

### Filter Types

**Capture Filters** - Filter during capture (before saving)
**Display Filters** - Filter after capture (for viewing)

**Important:** You can't use display filter syntax for capture filters!

### Capture Filter Syntax

```
tcp port 80
host 192.168.1.1
src 10.0.0.1 and port 443
```

### Display Filter Syntax

```
tcp.port == 80
ip.addr == 192.168.1.1
ip.src == 10.0.0.1 && tcp.port == 443
```

### Comparison Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| == | Equal | ip.src == 10.0.0.1 |
| != | Not equal | tcp.port != 80 |
| > | Greater than | frame.len > 100 |
| < | Less than | ip.ttl < 10 |
| >= | Greater or equal | tcp.port >= 1024 |
| <= | Less or equal | frame.len <= 64 |

### Logical Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| and / && | Logical AND | ip.src == 10.0.0.1 and tcp.port == 80 |
| or / \|\| | Logical OR | tcp.port == 80 or tcp.port == 443 |
| not / ! | Logical NOT | not tcp.port == 80 |

### Filter Toolbar Colors

- **Green** - Valid syntax
- **Red** - Invalid syntax
- **Yellow** - Warning (valid but unusual)

---

## Task 5: Packet Filtering | Protocol Filters

### IP Filters

```
ip                    # All IP packets
ip.addr == 10.0.0.1   # Specific IP (source or dest)
ip.src == 10.0.0.1    # Source IP
ip.dst == 10.0.0.1    # Destination IP
```

### TCP/UDP Filters

```
tcp.port == 80        # TCP port 80
udp.port == 53        # UDP port 53
tcp.srcport == 443    # TCP source port
tcp.dstport == 80     # TCP destination port
```

### Application Protocol Filters

**HTTP:**
```
http                          # All HTTP
http.request.method == GET    # HTTP GET requests
http.response.code == 200     # HTTP 200 responses
```

**DNS:**
```
dns                          # All DNS
dns.qry.type == 1           # DNS A records
dns.qry.name contains "google"  # DNS queries with "google"
```

### Display Filter Expression

Can't remember the filter? Use the builder:
```
Analyze → Display Filter Expression
```

### Questions

**What is the number of IP packets?**
```
Filter: ip
```
- Answer: `81420`

**What is the number of packets with TTL value less than 10?**
```
Filter: ip.ttl < 10
```
- Answer: `66`

**What is the number of packets using TCP port 4444?**
```
Filter: tcp.port == 4444
```
- Answer: `632`

**What is the number of HTTP GET requests sent to port 80?**
```
Filter: http.request.method == GET && tcp.port == 80
```
- Answer: `527`

**What is the number of type A DNS queries?**
```
Analyze → Display Filter Expression
Expand DNS, find query type filter
Filter: dns.qry.type == 1
```
- Answer: `51`

---

## Task 6: Advanced Filtering

### Advanced Filter Functions

**contains** - Search for text in packet
```
http.server contains "Apache"
```

**matches** - Regex pattern matching
```
http.host matches "\.(com|net)$"
```

**in** - Check if value is in a set
```
tcp.port in {80 443 8080}
```

**upper** - Convert to uppercase
```
upper(http.server) contains "APACHE"
```

**lower** - Convert to lowercase
```
lower(http.host) contains "google"
```

**string** - Convert to string for regex
```
string(ip.ttl) matches "[02468]$"  # Even TTL
```

### Bookmarks and Buttons

Save frequently used filters:
```
Bookmark icon in filter toolbar
Or create filter buttons for one-click access
```

### Profiles

Create different profiles for different tasks:
```
Edit → Configuration Profiles
Or bottom right status bar → Profile
```

### Questions

**Find all Microsoft IIS servers. What is the number of packets that did NOT originate from port 80?**
```
Filter: http.server contains "IIS" && tcp.srcport != 80
```
- Answer: `21`

**Find all Microsoft IIS servers with version 7.5. What is the number of packets?**
```
Filter: http.server contains "IIS" && http.server contains "7.5"
```
- Answer: `71`

**What is the total number of packets that use ports 3333, 4444, or 9999?**
```
Filter: tcp.port == 3333 or tcp.port == 4444 or tcp.port == 9999
```
- Answer: `2235`

**What is the number of packets with even TTL numbers?**
```
Filter: string(ip.ttl) matches "[02468]$"
```
- Answer: `77289`

**Change profile to "Checksum Control". What is the number of Bad TCP Checksum packets?**
```
Edit → Configuration Profiles → Checksum Control
Analyze → Expert Information
```
- Answer: `34185`

**Use the existing filtering button to filter traffic. What is the number of displayed packets?**
```
Click the filter button at top right
```
- Answer: `261`

---

## Task 7: Conclusion

Great job! You've mastered Wireshark statistics, filters, operators, and functions. Continue with "Wireshark: Traffic Analysis" to investigate suspicious traffic.

---

## Quick Reference

### Common Filters

| Purpose | Filter |
|---------|--------|
| All HTTP | `http` |
| All DNS | `dns` |
| Specific IP | `ip.addr == 192.168.1.1` |
| Specific port | `tcp.port == 80` |
| HTTP GET | `http.request.method == GET` |
| DNS A records | `dns.qry.type == 1` |
| TTL less than 10 | `ip.ttl < 10` |
| Multiple ports | `tcp.port in {80 443 8080}` |

### Statistics Menu

| Menu | Shows |
|------|-------|
| Resolved Addresses | IP to hostname mappings |
| Protocol Hierarchy | Protocol breakdown |
| Conversations | Traffic between endpoints |
| Endpoints | Unique endpoints |
| DNS | DNS packet details |
| HTTP | HTTP packet details |

### Filter Operators

| Operator | Purpose | Example |
|----------|---------|---------|
| == | Equal | tcp.port == 80 |
| != | Not equal | tcp.port != 80 |
| < | Less than | ip.ttl < 10 |
| > | Greater than | frame.len > 100 |
| && / and | Logical AND | ip and tcp |
| \|\| / or | Logical OR | tcp.port == 80 or tcp.port == 443 |
| ! / not | Logical NOT | !http |

---

## Key Takeaways

- **Statistics** give you the big picture of traffic
- **Display filters** are more powerful than capture filters
- **Filter toolbar** shows syntax validity with colors
- **Advanced filters** use contains, matches, in, and regex
- **Bookmarks** save your favorite filters
- **Profiles** switch between different configurations
- **Practice** makes perfect - try different filter combinations!

Master these filtering techniques to find any packet! 🦈
