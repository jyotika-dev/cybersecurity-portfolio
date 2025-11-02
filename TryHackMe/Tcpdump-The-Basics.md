# Tcpdump: The Basics - TryHackMe

> Learn how to use Tcpdump to capture, filter, and analyze network packets from the command line

**Room Link:** https://tryhackme.com/room/tcpdump

<img width="1583" height="882" alt="image" src="https://github.com/user-attachments/assets/6c993a84-1a6e-415b-a5dc-c43c2179ebad" />


**File:** traffic.pcap

---

## Introduction

Tcpdump is a command-line packet analyzer written in C/C++ released in the late 1980s. It's fast, stable, and uses the libpcap library (ported to Windows as winpcap).

**What you'll learn:**
- Capture packets and save to files
- Filter packets by various criteria
- Display packets in different formats
- Advanced filtering techniques

### Questions

**What is the name of the library associated with tcpdump?**
- Answer: `libpcap`

---

## Task 2: Basic Packet Capture

### Essential Commands

**Capture on interface:**
```bash
sudo tcpdump -i eth0
sudo tcpdump -i any        # All interfaces
```

**Save to file:**
```bash
sudo tcpdump -i eth0 -w capture.pcap
```

**Read from file:**
```bash
tcpdump -r capture.pcap
```

**Limit packet count:**
```bash
sudo tcpdump -i eth0 -c 10     # Capture 10 packets
```

**No name resolution:**
```bash
sudo tcpdump -i eth0 -n        # No DNS lookup
sudo tcpdump -i eth0 -nn       # No DNS or port lookup
```

**Verbose output:**
```bash
sudo tcpdump -i eth0 -v        # Verbose
sudo tcpdump -i eth0 -vv       # More verbose
```

### Examples

```bash
# Capture 50 packets verbosely on eth0
sudo tcpdump -i eth0 -c 50 -v

# Capture on WiFi and save to file
sudo tcpdump -i wlo1 -w data.pcap

# Capture on all interfaces without name resolution
sudo tcpdump -i any -nn
```

### Questions

**What option displays addresses only in numeric format?**
- Answer: `-n`

---

## Task 3: Filtering Expressions

### Host Filtering

**Filter by host:**
```bash
sudo tcpdump host example.com
sudo tcpdump src host 192.168.1.1      # Source only
sudo tcpdump dst host 192.168.1.1      # Destination only
```

### Port Filtering

**Filter by port:**
```bash
sudo tcpdump port 80
sudo tcpdump src port 443      # Source port
sudo tcpdump dst port 53       # Destination port
```

### Protocol Filtering

**Filter by protocol:**
```bash
sudo tcpdump tcp
sudo tcpdump udp
sudo tcpdump icmp
sudo tcpdump arp
```

### Logical Operators

**Combine filters:**
```bash
# AND
sudo tcpdump host 1.1.1.1 and tcp

# OR
sudo tcpdump udp or icmp

# NOT
sudo tcpdump not tcp
```

### Practical Examples

```bash
# SSH traffic on all interfaces
sudo tcpdump -i any tcp port 22

# NTP traffic on WiFi
sudo tcpdump -i wlo1 udp port 123

# HTTPS traffic with example.com
sudo tcpdump -i eth0 host example.com and tcp port 443 -w https.pcap
```

### Counting Packets

Use `wc` to count packets:
```bash
tcpdump -r traffic.pcap src host 192.168.124.1 -n | wc -l
```

### Questions

**How many packets in traffic.pcap use the ICMP protocol?**
```bash
sudo tcpdump -r traffic.pcap icmp -n | wc -l
```
- Answer: `26`

**What is the IP address of the host that asked for the MAC address of 192.168.124.137?**
```bash
sudo tcpdump -r traffic.pcap arp and host 192.168.124.137
```
- Answer: `192.168.124.148`

**What hostname (subdomain) appears in the first DNS query?**
```bash
sudo tcpdump -r traffic.pcap port 53 -A
```
- Answer: `mirrors.rockylinux.org`

---

## Task 4: Advanced Filtering

### Packet Length Filtering

**Filter by size:**
```bash
tcpdump greater 1000       # Packets > 1000 bytes
tcpdump less 100          # Packets < 100 bytes
```

### TCP Flags Filtering

**Available TCP flags:**
- `tcp-syn` - SYN flag
- `tcp-ack` - ACK flag
- `tcp-fin` - FIN flag
- `tcp-rst` - RST flag
- `tcp-push` - Push flag

**Filter examples:**
```bash
# Only SYN flag set
tcpdump "tcp[tcpflags] == tcp-syn"

# At least SYN flag set
tcpdump "tcp[tcpflags] & tcp-syn != 0"

# SYN or ACK flag set
tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"
```

### Binary Operations

**Understanding binary operators:**

**& (AND)** - Returns 1 only if both inputs are 1
**| (OR)** - Returns 1 unless both inputs are 0
**! (NOT)** - Inverts the bit

### Header Bytes

**Syntax:** `proto[expr:size]`
- `proto` - Protocol (arp, ether, icmp, ip, tcp, udp)
- `expr` - Byte offset (0 = first byte)
- `size` - Number of bytes (1, 2, or 4)

### Questions

**How many packets have only the TCP Reset (RST) flag set?**
```bash
sudo tcpdump -r traffic.pcap 'tcp[tcpflags] == tcp-rst' | wc -l
```
- Answer: `57`

**What is the IP address of the host that sent packets larger than 15000 bytes?**
```bash
sudo tcpdump -r traffic.pcap 'greater 15000' -n
```
- Answer: `185.117.80.53`

---

## Task 5: Displaying Packets

### Display Options

**Quick output:**
```bash
tcpdump -q        # Brief information
```

**MAC addresses:**
```bash
tcpdump -e        # Show Ethernet headers
```

**ASCII format:**
```bash
tcpdump -A        # Show packet contents in ASCII
```

**Hexadecimal format:**
```bash
tcpdump -xx       # Show packet in hex
tcpdump -X        # Show both hex and ASCII
```

### Example Output

```bash
tcpdump -r capture.pcap -X
```

Shows packets in both hex and ASCII, making it easier to see packet contents.

### Questions

**What is the MAC address of the host that sent an ARP request?**
```bash
sudo tcpdump -r traffic.pcap arp -e
```
- Answer: `52:54:00:7c:d3:5b`

---

## Task 6: Conclusion

Great job! You've learned the basics of Tcpdump for packet capture and analysis.

---

## Quick Reference

### Basic Commands

| Command | Purpose |
|---------|---------|
| `tcpdump -i eth0` | Capture on eth0 |
| `tcpdump -i any` | Capture on all interfaces |
| `tcpdump -w file.pcap` | Save to file |
| `tcpdump -r file.pcap` | Read from file |
| `tcpdump -c 10` | Capture 10 packets |
| `tcpdump -n` | No DNS resolution |
| `tcpdump -nn` | No DNS or port resolution |
| `tcpdump -v` | Verbose output |

### Filtering

| Filter | Purpose |
|--------|---------|
| `host 1.1.1.1` | Traffic to/from host |
| `src host 1.1.1.1` | Source host |
| `dst host 1.1.1.1` | Destination host |
| `port 80` | Traffic on port 80 |
| `tcp` | TCP traffic only |
| `udp` | UDP traffic only |
| `icmp` | ICMP traffic only |

### Logical Operators

| Operator | Purpose | Example |
|----------|---------|---------|
| `and` | Both conditions | `host 1.1.1.1 and tcp` |
| `or` | Either condition | `tcp or udp` |
| `not` | Exclude | `not icmp` |

### Display Options

| Option | Purpose |
|--------|---------|
| `-q` | Quick output |
| `-e` | Show MAC addresses |
| `-A` | ASCII format |
| `-xx` | Hexadecimal format |
| `-X` | Hex and ASCII |

### TCP Flags

| Flag | Purpose |
|------|---------|
| `tcp-syn` | SYN flag |
| `tcp-ack` | ACK flag |
| `tcp-fin` | FIN flag |
| `tcp-rst` | RST flag |
| `tcp-push` | Push flag |

---

## Useful Examples

```bash
# Capture HTTP traffic
sudo tcpdump -i eth0 tcp port 80 -w http.pcap

# Capture HTTPS traffic from specific host
sudo tcpdump -i eth0 host example.com and port 443

# Show DNS queries
sudo tcpdump -i any port 53 -n

# Capture SSH traffic
sudo tcpdump -i eth0 tcp port 22

# Count ICMP packets in file
tcpdump -r capture.pcap icmp | wc -l

# Show ARP requests with MAC addresses
tcpdump -r capture.pcap arp -e

# Find large packets
tcpdump -r capture.pcap 'greater 10000'

# Show TCP SYN packets only
tcpdump "tcp[tcpflags] == tcp-syn"
```

---

## Key Takeaways

- **Tcpdump** is a fast, stable command-line packet analyzer
- **-i** specifies interface, **-w** writes to file, **-r** reads from file
- **-n** avoids DNS lookups (faster)
- **Filters** let you focus on specific traffic (host, port, protocol)
- **Logical operators** (and, or, not) combine filters
- **TCP flags** can be filtered for specific connection states
- **Display options** (-A, -X, -e) show packets in different formats
- Always use `sudo` for live captures

Tcpdump is essential for network analysis and troubleshooting! 🔍
