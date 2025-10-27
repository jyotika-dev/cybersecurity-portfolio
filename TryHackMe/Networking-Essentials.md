# Networking Essentials - TryHackMe

> Learn about essential networking protocols like DHCP, ARP, NAT, ICMP, and routing

**Room Link:** https://tryhackme.com/room/networkingessentials

<img width="1582" height="921" alt="image" src="https://github.com/user-attachments/assets/a3417503-a33a-4fa2-a03b-226136705c96" />

---

## Introduction

Ever wondered how your computer automatically gets an IP address when you connect to Wi-Fi? Or how your data finds its way across the internet? This room answers those questions!

**What you'll learn:**
- DHCP (automatic network configuration)
- ARP (mapping IP to MAC addresses)
- NAT (sharing one public IP with multiple devices)
- ICMP (network troubleshooting with ping and traceroute)
- Routing protocols

**Prerequisites:** You should know the OSI and TCP/IP models first.

---

## Task 1: Introduction

This is part 2 of a 4-room networking series. Make sure you've completed "Networking Concepts" before starting this one.

**No answer needed**

---

## Task 2: DHCP - Give Me My Network Settings

### What is DHCP?

DHCP automatically configures your network settings so you don't have to do it manually every time you connect to a new network.

**What DHCP assigns:**
- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

**Example:** When you connect to coffee shop Wi-Fi, DHCP automatically gives your device an IP address.

### The DHCP Process (DORA)

1. **DHCP Discover** - Client broadcasts: "Is there a DHCP server here?"
2. **DHCP Offer** - Server replies: "Here's an available IP address for you"
3. **DHCP Request** - Client says: "I accept that IP address"
4. **DHCP Acknowledge** - Server confirms: "It's yours!"

### Example Packet Capture

```bash
tshark -r DHCP-G5000.pcap -n
```

Output shows:
1. DHCP Discover (0.0.0.0 → 255.255.255.255)
2. DHCP Offer (192.168.66.1 → 192.168.66.133)
3. DHCP Request (0.0.0.0 → 255.255.255.255)
4. DHCP ACK (192.168.66.1 → 192.168.66.133)

### Questions

**How many steps does DHCP use to provide network configuration?**
- Answer: `4`

**What is the destination IP address that a client uses when it sends a DHCP Discover packet?**
- Answer: `255.255.255.255` (broadcast address)

**What is the source IP address a client uses when trying to get IP network configuration over DHCP?**
- Answer: `0.0.0.0` (no IP yet)

---

## Task 3: ARP - Bridging Layer 3 to Layer 2

### What is ARP?

ARP maps IP addresses (Layer 3) to MAC addresses (Layer 2). Devices need MAC addresses to send data on the local network.

**Ethernet Frame contains:**
- Destination MAC address
- Source MAC address
- Type (like IPv4)

### How ARP Works

**ARP Request:** "Who has IP 192.168.66.1? Tell 192.168.66.89"
**ARP Reply:** "192.168.66.1 is at MAC address 44:df:65:d8:fe:6c"

### Example Packet Capture

```bash
tshark -r arp.pcapng -Nn
```

Output:
1. ARP Request: Who has 192.168.66.1? Tell 192.168.66.89
2. ARP Reply: 192.168.66.1 is at 44:df:65:d8:fe:6c

**Note:** ARP is encapsulated directly in an Ethernet frame, not in an IP packet!

### Questions

**What is the destination MAC address used in an ARP Request?**
- Answer: `ff:ff:ff:ff:ff:ff` (broadcast)

**In the example above, what is the MAC address of 192.168.66.1?**
- Answer: `44:df:65:d8:fe:6c`

---

## Task 4: ICMP - Troubleshooting Networks

### What is ICMP?

ICMP is used for network diagnostics and troubleshooting. Two main tools use ICMP:

**Ping** - Tests connectivity
**Traceroute** - Shows the path data takes to reach destination

### Ping

Sends ICMP Echo Request (Type 8) and waits for ICMP Echo Reply (Type 0).

```bash
ping 192.168.11.1 -c 4
```

Output shows:
- 4 packets transmitted
- 4 received
- 0% packet loss
- Round-trip time statistics

**What ping tells you:**
- Is the target online?
- How fast is the connection?
- Is there packet loss?

### Traceroute

Shows each router your data passes through to reach the destination.

```bash
traceroute example.com
```

**How it works:**
- Uses TTL (Time-to-Live) field
- Each router decrements TTL by 1
- When TTL reaches 0, router sends ICMP Time Exceeded message
- This reveals the router's IP address

Output shows the path with 16+ hops from your device to example.com.

### Questions

**Using the example images above, how many bytes were sent in the echo (ping) request?**
- Answer: `40`

**Which IP header field does the traceroute command require to become zero?**
- Answer: `TTL`

---

## Task 5: Routing

### What is Routing?

Routing protocols help routers determine the best path for data to travel.

### Common Routing Protocols

**OSPF (Open Shortest Path First)**
- Finds the shortest path
- Shares network topology information
- Used in enterprise networks

**EIGRP (Enhanced Interior Gateway Routing Protocol)**
- Cisco proprietary protocol
- Uses multiple metrics to find best path

**BGP (Border Gateway Protocol)**
- Main routing protocol of the Internet
- ISPs use it to exchange routing info

**RIP (Routing Information Protocol)**
- Simple protocol
- Finds route with fewest hops
- Used in smaller networks

### Questions

**Which routing protocol discussed in this task is a Cisco proprietary protocol?**
- Answer: `EIGRP`

---

## Task 6: NAT

### What is NAT?

NAT (Network Address Translation) allows multiple devices on a private network to share a single public IP address.

**Why it's useful:**
- Conserves public IP addresses (IPv4 has limited addresses)
- Allows entire office/home to access Internet with one public IP

### How NAT Works

**Example:** Office with 50 computers:
- All 50 devices share one public IP: 212.3.4.5
- Router maintains translation table
- Maps internal IP + port to external IP + port

**Translation Example:**
- Laptop internal: 192.168.0.129:15401
- Appears as external: 212.3.4.5:19273
- Router does this translation seamlessly

### Questions

**In the network diagram above, what is the public IP that the phone will appear to use when accessing the Internet?**
- Answer: `212.3.4.5`

**Approximately how many thousand simultaneous TCP connections can the router maintain?**
- Answer: `65` (65,535 ports available, roughly 65 thousand)

---

## Task 7: Closing Notes

You've learned the protocols that make networks work! Even though we use the Internet daily, these protocols work behind the scenes to keep everything running smoothly.

### What You Learned

✅ **DHCP** - Automatic network configuration (DORA process)
✅ **ARP** - Mapping IP addresses to MAC addresses
✅ **ICMP** - Network troubleshooting (ping, traceroute)
✅ **Routing** - How data finds its path (OSPF, EIGRP, BGP, RIP)
✅ **NAT** - Sharing one public IP with multiple devices

### Final Question

**Click on the View Site button and follow the instructions to obtain the flag.**
- Answer: `THM{computer_is_happy}`

---

## Quick Reference

| Protocol | Purpose |
|----------|---------|
| **DHCP** | Automatically assigns IP addresses |
| **ARP** | Maps IP addresses to MAC addresses |
| **ICMP** | Network diagnostics (ping/traceroute) |
| **NAT** | Shares public IP with private network |
| **OSPF** | Routing protocol (shortest path) |
| **EIGRP** | Cisco routing protocol |
| **BGP** | Internet routing protocol |

---

## Key Takeaways

- **DHCP** uses 4 steps: Discover, Offer, Request, Acknowledge
- **ARP** broadcasts to find MAC addresses (ff:ff:ff:ff:ff:ff)
- **Ping** uses ICMP Echo Request/Reply
- **Traceroute** reveals router paths using TTL
- **NAT** allows multiple devices to share one public IP
- These protocols work together to make networks functional

Understanding these essentials helps you troubleshoot network issues! 🌐
