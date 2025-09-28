# 🐧 LinuxJourney - Network Troubleshooting

This file summarizes my learning from **LinuxJourney** on network troubleshooting tools and techniques.  
https://labex.io/lesson/icmp  
I practiced these on my local terminal and network environments.

---

## 🔹 ICMP
- **Internet Control Message Protocol** → part of TCP/IP for error messages and diagnostics.
- **Message structure** → type, code, checksum fields.
- **Common ICMP types**:
  - Type 0: Echo Reply
  - Type 3: Destination Unreachable
  - Type 8: Echo Request
  - Type 11: Time Exceeded
- **Destination unreachable codes**:
  - Code 0: Network Unreachable
  - Code 1: Host Unreachable
- Essential for network debugging and connectivity testing.

## 🔹 ping
- **Basic connectivity test** → sends ICMP echo requests (Type 8).
- `ping -c 3 www.google.com` → send 3 packets and stop.
- **Key output fields**:
  - **icmp_seq** → packet sequence number (missing = packet loss)
  - **ttl** → Time To Live hop counter
  - **time** → roundtrip time in milliseconds
- **64 bytes** → default packet size.
- One packet per second by default.

## 🔹 traceroute
- **Path discovery** → shows route packets take to destination.
- **TTL manipulation** → incremental TTL values reveal each hop.
- `traceroute google.com` → trace path to destination.
- **ICMP Time Exceeded** → routers return this when TTL expires.
- **Three columns** → three packet round-trip times per hop.
- **30 hops max** → default maximum hop limit.
- Builds router list until reaching destination.

## 🔹 netstat
- **Network statistics** → comprehensive network connection information.
- `/etc/services` → well-known port definitions.
- **Common ports**:
  - 21: FTP
  - 22: SSH
  - 25: SMTP
  - 53: DNS
  - 80: HTTP
  - 443: HTTPS
- `netstat -at` → show all TCP connections.
- **Connection states**:
  - LISTENING → awaiting connections
  - ESTABLISHED → active connection
  - TIME_WAIT → closing cleanup
  - CLOSE_WAIT → remote shutdown

## 🔹 Packet Analysis
- **tcpdump** → command-line packet analyzer.
- `sudo apt install tcpdump` → installation.
- `sudo tcpdump -i wlan0` → capture on specific interface.
- **Output format**:
  - Timestamp → when packet captured
  - Protocol → IP, ARP, etc.
  - Source > Destination → packet direction
  - Sequence numbers → TCP packet ordering
  - Length → packet size in bytes
- `sudo tcpdump -w /file/path` → save capture to file.
- **Alternative**: Wireshark for GUI analysis.

---

📌 **Applied Insight**:  
Network troubleshooting skills are essential in cybersecurity:
- **Incident response** → trace attack paths using traceroute and packet analysis.  
- **Network forensics** → tcpdump captures provide evidence of malicious traffic.  
- **Threat detection** → unusual ping patterns or connection states indicate potential attacks.  
- **Service enumeration** → netstat reveals running services and potential attack surfaces.  
- **Performance analysis** → ping and traceroute help identify network bottlenecks during security incidents.
