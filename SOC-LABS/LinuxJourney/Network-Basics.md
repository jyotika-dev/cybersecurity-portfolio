# Linux Network Basics

This file summarizes my learning from **LinuxJourney** on Linux networking fundamentals.  
https://labex.io/lesson/network-basics  
I practiced these on my local terminal and network environments.

---

## 1. Network Basics

Computer networks are interconnected devices that share resources and data. Understanding basic networking concepts is essential for system administration and cybersecurity.

**Key Concepts:**
- **LAN (Local Area Network)** - devices in same physical location
- **WAN (Wide Area Network)** - networks spanning large geographic areas  
- **Internet** - global network of interconnected networks
- **Protocols** - rules governing network communication

**Basic Commands:**
```bash
ping hostname          # test network connectivity
ping -c 4 google.com   # send 4 ping packets
wget http://example.com # download web content
curl http://example.com # retrieve web content
```

---

## 2. OSI Model

The **Open Systems Interconnection (OSI)** model is a 7-layer reference framework for network communication. Each layer has specific responsibilities and communicates with adjacent layers.

**The 7 Layers:**

**Layer 7 - Application Layer:**
- User-facing protocols (HTTP, FTP, SMTP)
- Network services to applications

**Layer 6 - Presentation Layer:**  
- Data encryption and compression
- Format translation

**Layer 5 - Session Layer:**
- Connection establishment and management
- Session control

**Layer 4 - Transport Layer:**
- End-to-end data delivery (TCP, UDP)
- Port numbers and reliability

**Layer 3 - Network Layer:**
- Packet routing and addressing (IP)
- Path determination

**Layer 2 - Data Link Layer:**
- Frame formatting and error detection
- MAC addresses and switching

**Layer 1 - Physical Layer:**
- Physical transmission medium
- Electrical signals and cables

---

## 3. TCP/IP Model

The **TCP/IP model** is a 4-layer practical implementation used by most networks today. It consolidates the OSI model into more manageable layers.

**The 4 Layers:**

**Application Layer:**
- Combines OSI layers 5, 6, and 7
- Protocols: HTTP, HTTPS, FTP, SSH, DNS, SMTP

**Transport Layer:**
- TCP (reliable, connection-oriented)
- UDP (fast, connectionless)
- Port identification

**Internet Layer:**
- IP addressing and routing
- ICMP for diagnostics

**Link Layer:**
- Physical network access
- Ethernet and WiFi protocols

---

## 4. Network Addressing

Network addressing provides unique identifiers for devices on networks.

**IP Addresses:**
```bash
# IPv4 addresses (32-bit)
192.168.1.1
10.0.0.1
172.16.0.1

# IPv6 addresses (128-bit)  
2001:db8::1
fe80::1
::1 (localhost)
```

**Subnet Concepts:**
- **Subnet mask** - defines network vs host portions
- **CIDR notation** - 192.168.1.0/24 (24-bit network mask)
- **Private IP ranges** - RFC 1918 addresses for internal networks

**Network Commands:**
```bash
ip addr show           # display network interfaces
ip addr show eth0      # show specific interface
ifconfig              # older interface configuration tool
hostname -I           # show system IP addresses
```

---

## 5. Application Layer

The Application Layer contains protocols that directly interact with users and applications.

**Common Protocols:**

**HTTP/HTTPS (Web):**
- Port 80 (HTTP), Port 443 (HTTPS)
- Web browsing and API communication

**SSH (Secure Shell):**
- Port 22
- Secure remote access and file transfer

**FTP (File Transfer):**
- Port 21 (control), Port 20 (data)
- File upload and download

**DNS (Domain Name System):**
- Port 53
- Translates domain names to IP addresses

**SMTP (Email):**
- Port 25 (sending), Port 110 (POP3), Port 143 (IMAP)

**Commands:**
```bash
netstat -tuln         # show listening services
ss -tuln             # modern replacement for netstat  
lsof -i              # show network connections
nmap localhost       # scan local ports
```

---

## 6. Transport Layer

The Transport Layer manages end-to-end communication between applications.

**TCP (Transmission Control Protocol):**
- **Connection-oriented** - establishes connection before data transfer
- **Reliable** - guarantees data delivery and order
- **Flow control** - manages data transmission rate
- **Three-way handshake** - SYN, SYN-ACK, ACK

**UDP (User Datagram Protocol):**
- **Connectionless** - no connection establishment
- **Fast** - minimal overhead
- **Unreliable** - no delivery guarantees
- Used for real-time applications (video, gaming, DNS)

**Port Numbers:**
- **Well-known ports** - 0-1023 (system services)
- **Registered ports** - 1024-49151 (user applications)  
- **Dynamic ports** - 49152-65535 (temporary connections)

**Commands:**
```bash
ss -t                # show TCP connections
ss -u                # show UDP connections  
ss -p                # show process using connection
netstat -an          # show all connections
```

---

## 7. Network Layer

The Network Layer handles packet routing across different networks.

**IP Protocol Functions:**
- **Addressing** - unique identification of devices
- **Routing** - path selection across networks
- **Packet forwarding** - moving packets toward destination
- **Fragmentation** - breaking large packets into smaller ones

**ICMP (Internet Control Message Protocol):**
- Error reporting and diagnostics
- Used by ping and traceroute

**Routing:**
```bash
ip route show         # display routing table
route -n             # show numeric routing table
traceroute google.com # trace packet path
tracepath google.com  # alternative trace tool
```

**Example Routing Table:**
```bash
Destination     Gateway         Flags   Metric  Interface
0.0.0.0         192.168.1.1     UG      0       eth0
192.168.1.0     0.0.0.0         U       0       eth0
127.0.0.0       127.0.0.1       UG      0       lo
```

---

## 8. Link Layer

The Link Layer manages communication within a single network segment.

**Ethernet:**
- Most common LAN technology
- Uses CSMA/CD (Carrier Sense Multiple Access with Collision Detection)
- Frame format with headers and checksums

**MAC Addresses:**
- 48-bit hardware identifiers
- Format: AA:BB:CC:DD:EE:FF
- Unique to each network interface

**Switching:**
- **Switches** learn MAC addresses and forward frames
- **Broadcast domain** - area where broadcast frames are forwarded
- **Collision domain** - area where collisions can occur

**ARP (Address Resolution Protocol):**
- Maps IP addresses to MAC addresses
- Maintains ARP table/cache

**Commands:**
```bash
ip link show         # display network interfaces
ethtool eth0         # show interface details
arp -a              # show ARP table
ip neighbor show     # modern ARP table display
```

---

## 9. DHCP Overview

**Dynamic Host Configuration Protocol (DHCP)** automatically assigns network configuration to devices.

**DHCP Process (DORA):**
1. **Discover** - client broadcasts request for IP
2. **Offer** - server offers available IP address  
3. **Request** - client requests specific offered IP
4. **Acknowledge** - server confirms IP assignment

**DHCP Information Provided:**
- IP address
- Subnet mask
- Default gateway
- DNS servers
- Lease time

**DHCP Commands:**
```bash
dhclient eth0        # request DHCP lease
dhclient -r eth0     # release DHCP lease
cat /var/lib/dhcp/dhclient.leases  # view lease info
```

**Static vs Dynamic Configuration:**
```bash
# Static IP configuration
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip route add default via 192.168.1.1

# DHCP configuration  
sudo dhclient eth0
```

---

## Applied Insight

Network fundamentals are crucial in cybersecurity:

- **Network Security** - Understanding protocols helps secure communications and identify vulnerabilities
- **Traffic Analysis** - Packet inspection at different layers reveals malicious activity patterns  
- **Incident Response** - Network logs provide attack vectors, timelines, and lateral movement evidence
- **Penetration Testing** - Protocol knowledge enables effective security assessments and exploitation
- **Threat Hunting** - Anomalous network patterns indicate potential breaches and data exfiltration
- **Forensic Analysis** - Network artifacts help reconstruct attack sequences and data flows

**Security-focused Commands:**
```bash
tcpdump -i eth0      # capture network packets
wireshark           # graphical packet analyzer  
nmap -sS target     # TCP SYN scan
netstat -antup      # show all network connections
ss -antup           # modern network connection display
```

Understanding these networking fundamentals provides the foundation for advanced cybersecurity topics including network security, intrusion detection, forensic analysis, and penetration testing.
