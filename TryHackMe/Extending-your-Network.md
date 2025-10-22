# Extending Your Network - TryHackMe

> Learn about port forwarding, firewalls, VPNs, and networking devices

**Room Link:** https://tryhackme.com/room/extendingyournetwork

<img width="1761" height="916" alt="image" src="https://github.com/user-attachments/assets/85270030-cc38-431d-911a-4a78e2e164fd" />


---

## Task 1: Introduction to Port Forwarding

### What is Port Forwarding?

Port forwarding is essential for connecting applications and services to the Internet. Without it, services like web servers are only accessible to devices on the same local network (intranet).

### How It Works

**Without Port Forwarding:**
- Server at 192.168.1.10 runs a web server on port 80
- Only devices on the same network can access it

**With Port Forwarding:**
- The router is configured to forward traffic from the public IP
- External networks can access the web server using the public IP (e.g., 82.62.51.70)
- The router routes external requests to the internal server

### Port Forwarding vs Firewalls

Don't confuse them:
- **Port Forwarding** - Opens specific ports
- **Firewall** - Determines if traffic can travel through those ports (even if open)

Port forwarding is configured at the network's router.

### Questions

**What device is used to configure port forwarding?**
- Answer: `Router`

---

## Task 2: Firewalls 101

### What is a Firewall?

A firewall determines what traffic is allowed to enter and exit a network. Think of it as border security for your network.

### What Firewalls Check

An administrator can configure a firewall based on:

- **Source** - Where is the traffic coming from?
- **Destination** - Where is the traffic going?
- **Port** - What port is the traffic using?
- **Protocol** - Is it UDP, TCP, or both?

Firewalls perform **packet inspection** to answer these questions.

### Types of Firewalls

**Stateful Firewall**
- Inspects the entire connection
- Tracks the state of network connections
- More secure but uses more resources
- Remembers previous packets in the connection

**Stateless Firewall**
- Inspects individual packets only
- Doesn't track connection state
- Faster and uses fewer resources
- Each packet is treated independently

### OSI Layers

Firewalls typically operate at:
- **Layer 3** (Network) - IP addresses
- **Layer 4** (Transport) - Ports and protocols

### Questions

**What layers of the OSI model do firewalls operate at?**
- Answer: `Layer 3,Layer 4`

**What category of firewall inspects the entire connection?**
- Answer: `Stateful`

**What category of firewall inspects individual packets?**
- Answer: `Stateless`

---

## Task 3: Practical - Firewall

### The Challenge

Configure the firewall to prevent a device from being overloaded by too many packets.

### Solution

In the simulation, you'll notice:
- Too many packets coming from 198.57.100.34
- This machine is trying to overload the user's machine (203.0.110.1)

**Action:**
1. Add the attacking machine's IP address to the block list
2. Add the relevant port numbers
3. The firewall will drop all packets from that source
4. Flag appears once the attack is stopped

**Flag:**
```
THM{FIREWALLS_RULE}
```

---

## Task 4: VPN Basics

### What is a VPN?

A Virtual Private Network (VPN) allows devices on separate networks to communicate securely by creating a dedicated path (tunnel) over the Internet.

### How VPNs Work

Devices connected through a VPN tunnel form their own private network. For example:
- Two offices in different locations can connect securely
- Devices on Network #1 and Network #2 can form Network #3 via VPN
- Only VPN-connected devices can communicate on this private network

### Benefits of VPNs

- **Privacy** - Your traffic is encrypted and hidden from ISPs
- **Anonymity** - Masks your IP address and location
- **Security** - Protects data from interception
- **Access** - Allows secure access to private networks remotely

### How TryHackMe Uses VPNs

TryHackMe uses VPNs to:
- Let you securely interact with vulnerable machines
- Prevent ISPs from thinking you're attacking Internet machines
- Keep vulnerable machines off the public Internet

### VPN Technologies

**PPP (Point-to-Point Protocol)**
- Only encrypts and authenticates data
- Used for authentication
- Non-routable

**PPTP (Point-to-Point Tunneling Protocol)**
- Easy to set up
- Supported by many devices
- Weak encryption (outdated)

**IPSec (Internet Protocol Security)**
- Uses the IP framework
- Encrypts IP packets
- Difficult to set up
- Strong encryption
- Supported by many devices

### Questions

**What VPN technology only encrypts & provides authentication of data?**
- Answer: `PPP`

**What VPN technology uses the IP framework?**
- Answer: `IPSec`

---

## Task 5: LAN Networking Devices

### What is a Router?

A router connects networks and passes data between them using **routing**.

**Routing** is the process of creating a path between networks so data can be delivered successfully.

**Key Features:**
- Operates at Layer 3 (Network) of the OSI model
- Has an interactive interface for configuration
- Configures rules like port forwarding and firewalling
- Chooses the most optimal path based on:
  - Shortest path
  - Most reliable path
  - Fastest medium (fiber vs copper)

Routers are dedicated devices and don't perform the same functions as switches.

### What is a Switch?

A switch provides a means of connecting multiple devices (3 to 63) using Ethernet cables.

### Layer 2 Switches

- Operate at Layer 2 (Data Link) of the OSI model
- Forward **frames** (not packets - IP info is stripped)
- Use MAC addresses to send data to the correct device
- Responsible only for sending frames to correct devices

### Layer 3 Switches

- More sophisticated than Layer 2
- Can perform some router functions
- Send frames to devices (like Layer 2)
- Route packets using the IP protocol
- Can handle IP addresses (192.168.1.1, etc.)

### VLANs (Virtual Local Area Network)

VLANs allow devices to be virtually split up on the same physical network.

**Benefits:**
- Devices share resources like Internet connection
- Treated as separate networks for security
- Rules determine how devices communicate
- Network segregation for different departments

**Example:**
- Sales Department and Accounting Department on the same switch
- Both can access Internet
- Cannot communicate with each other
- Provides security through separation

### Questions

**What is the verb for the action that a router does?**
- Answer: `Routing`

**What are the two different layers of switches? (Separate by comma)**
- Answer: `Layer2,Layer3`

---

## Task 6: Practical - Network Simulator

### The Challenge

Use the network simulator to send a TCP packet from Computer 1 to Computer 3.

### Solution

1. Deploy the static site
2. Select Computer 1 as the source
3. Select Computer 3 as the destination
4. Send a TCP packet with any data
5. Watch the three-way handshake complete
6. Data transfer happens
7. Flag displays after successful transfer

The simulator breaks down every step a packet takes from point A to point B.

### Questions

**What is the flag from the network simulator?**
- Answer: `THM{YOU'VE_GOT_DATA}`

**How many HANDSHAKE entries are there in the Network Log?**
- Answer: `5`

---

## Summary

**Key Takeaways:**

- **Port Forwarding** - Opens ports to allow external access to internal services
- **Firewalls** - Control what traffic can pass through (stateful vs stateless)
- **VPNs** - Create secure tunnels between networks over the Internet
- **Routers** - Connect networks and determine optimal paths (Layer 3)
- **Switches** - Connect devices within a network (Layer 2 or Layer 3)
- **VLANs** - Virtually separate devices on the same physical network

Understanding these technologies is essential for managing and securing networks!
