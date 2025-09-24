# 🐧 LinuxJourney - Network Basics

This file summarizes my learning from **LinuxJourney** on Linux networking fundamentals.  
https://labex.io/lesson/network-basics  
I practiced these on my local terminal and network environments.

---

## 🔹 Network Basics
- **Computer networks** → interconnected devices sharing resources and data.
- **LAN** (Local Area Network) → devices in same physical location.
- **WAN** (Wide Area Network) → networks spanning large geographic areas.
- **Internet** → global network of interconnected networks.
- **Protocols** → rules governing network communication.
- `ping hostname` → test network connectivity.

## 🔹 OSI Model
- **7-layer reference model** → conceptual framework for network communication.
- **Layer 7**: Application (HTTP, FTP, SMTP).
- **Layer 6**: Presentation (encryption, compression).
- **Layer 5**: Session (connection management).
- **Layer 4**: Transport (TCP, UDP).
- **Layer 3**: Network (IP, routing).
- **Layer 2**: Data Link (Ethernet, switches).
- **Layer 1**: Physical (cables, signals).

## 🔹 TCP/IP Model
- **4-layer practical model** → real-world network implementation.
- **Application Layer** → combines OSI layers 5-7.
- **Transport Layer** → TCP/UDP protocols.
- **Internet Layer** → IP addressing and routing.
- **Link Layer** → physical network access.
- Most networks use TCP/IP model in practice.

## 🔹 Network Addressing
- **IP addresses** → unique identifiers for network devices.
- **IPv4** → 32-bit addresses (192.168.1.1).
- **IPv6** → 128-bit addresses (2001:db8::1).
- **Subnets** → network segments with address ranges.
- **CIDR notation** → 192.168.1.0/24 (subnet mask).
- `ip addr show` → display network interfaces and addresses.

## 🔹 Application Layer
- **User-facing protocols** → web, email, file transfer.
- **HTTP/HTTPS** → web browsing (ports 80/443).
- **FTP** → file transfer (port 21).
- **SSH** → secure remote access (port 22).
- **SMTP** → email sending (port 25).
- **DNS** → domain name resolution (port 53).
- `netstat -tuln` → show listening servic
