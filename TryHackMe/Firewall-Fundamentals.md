# Firewall Fundamentals - TryHackMe

> Learn about firewalls and how they protect networks

**Room Link:** https://tryhackme.com/room/firewallfundamentals

<img width="1750" height="858" alt="image" src="https://github.com/user-attachments/assets/3c9de14f-639d-4b31-9e01-9aed5e4a1a5e" />

---

## Task 1: Introduction to Firewalls

### What is a Firewall?

A firewall is like a security guard for your network! It inspects all traffic coming in and going out, deciding what's safe and what's not.

**Think of it as:**
- A wall protecting your system
- A bouncer at a club entrance
- A checkpoint at a border

### How Firewalls Work

**Allow or Deny:**
- Permits safe traffic
- Blocks malicious traffic
- Follows predefined rules

**Protection:**
- Guards internal systems
- Checks traffic credentials
- Enforces security policies

### Why Firewalls Matter

**Security:**
- Prevent unauthorized access
- Block malicious traffic
- Protect sensitive data

**Control:**
- Monitor network activity
- Enforce policies
- Log traffic patterns

### Questions

**Which security solution inspects the incoming and outgoing traffic of a device or a network?**
- Answer: `Firewall`

---

## Task 2: Types of Firewalls

### Understanding OSI Model

Before diving into firewall types, let's quickly review the OSI Model layers:

| Layer | Name | Function |
|-------|------|----------|
| 7 | Application | User applications (HTTP, FTP) |
| 6 | Presentation | Data format, encryption |
| 5 | Session | Connection management |
| 4 | Transport | End-to-end communication (TCP/UDP) |
| 3 | Network | Routing, IP addressing |
| 2 | Data Link | MAC addressing |
| 1 | Physical | Physical connections |

### Four Types of Firewalls

### 1. Stateless Firewall

**Characteristics:**
- Basic filtering
- Doesn't track connections
- Fast and efficient
- No connection memory

**OSI Layers:** 3 (Network) & 4 (Transport)

**Pros:**
- Very fast
- Low resource usage
- Good for high-speed networks

**Cons:**
- Limited security
- Can't detect sophisticated attacks
- No context awareness

**Use case:** High-speed networks where speed is priority

### 2. Stateful Firewall

**Characteristics:**
- Tracks connection states
- Recognizes traffic patterns
- Remembers previous connections
- Complex rule support

**OSI Layers:** 3 (Network) & 4 (Transport)

**Pros:**
- Better security than stateless
- Context-aware decisions
- Monitors active connections

**Cons:**
- More resource intensive
- Slightly slower than stateless

**Use case:** Most modern networks - good balance of security and speed

### 3. Proxy Firewall

**Characteristics:**
- Inspects packet content
- Content filtering
- Application control
- SSL/TLS inspection

**OSI Layers:** 7 (Application)

**Pros:**
- Deep packet inspection
- Can filter specific content
- Hides internal network
- Application-level control

**Cons:**
- Can be slow
- Resource intensive
- May introduce latency

**Use case:** When you need to inspect application data and content

### 4. Next-Generation Firewall (NGFW)

**Characteristics:**
- Advanced threat protection
- Intrusion Prevention System (IPS)
- Heuristic analysis (detects anomalies)
- SSL/TLS decryption and inspection
- Application awareness

**OSI Layers:** 3, 4, 5, 6, 7 (Multiple layers)

**Pros:**
- Most comprehensive protection
- AI-powered threat detection
- Deep visibility
- Multiple security features

**Cons:**
- Expensive
- Complex to configure
- Resource intensive

**Use case:** Enterprise environments needing maximum security

### Questions

**Which type of firewall maintains the state of connections?**

Tracks and remembers connections = Stateful

- Answer: `stateful firewall`

---

**Which type of firewall offers heuristic analysis for the traffic?**

Heuristic analysis (anomaly detection) = Next-Generation

- Answer: `next-generation firewall`

---

**Which type of firewall inspects the traffic coming to an application?**

Application-level inspection = Proxy (Layer 7)

- Answer: `proxy firewall`

---

## Task 3: Firewall Rules

### What are Firewall Rules?

Rules are instructions that tell the firewall what to do with traffic. Think of them as a bouncer's checklist!

### Components of Firewall Rules

### 1. Source Address

**What it is:** Where the traffic is coming from

**Examples:**
- `192.168.1.100` - Specific IP
- `192.168.1.0/24` - IP range
- `any` - Any source

### 2. Destination Address

**What it is:** Where the traffic is going to

**Examples:**
- `10.0.0.50` - Internal server
- `0.0.0.0/0` - Any destination
- `192.168.1.0/24` - Local network

### 3. Port

**What it is:** The port number for the service

**Common ports:**
- `22` - SSH
- `80` - HTTP
- `443` - HTTPS
- `3389` - RDP
- `21` - FTP

### 4. Protocol

**What it is:** The communication protocol

**Common protocols:**
- `TCP` - Reliable, connection-oriented
- `UDP` - Fast, connectionless
- `ICMP` - Ping, diagnostics
- `Any` - All protocols

### 5. Action

**What it is:** What the firewall should do

**Common actions:**
- `Allow/Accept` - Permit traffic
- `Deny/Drop` - Block traffic silently
- `Reject` - Block and notify sender
- `Log` - Record the traffic

### 6. Direction

**What it is:** Traffic flow direction

**Two directions:**
- `Inbound` - Coming into network
- `Outbound` - Leaving network

### Example Rule

**Block incoming SSH:**
```
Source: Any
Destination: 192.168.1.100
Port: 22
Protocol: TCP
Action: Deny
Direction: Inbound
```

This blocks all SSH attempts to server 192.168.1.100

### Rule Priority

**Important:** Rules are processed in order!
- First match wins
- More specific rules should be first
- Default rules should be last

### Questions

**Which type of action should be defined in a rule to permit any traffic?**

Permit = Allow

- Answer: `allow`

---

**What is the direction of the rule that is created for the traffic leaving our network?**

Leaving = Going out = Outbound

- Answer: `outbound`

---

## Task 4: Windows Defender Firewall

### Practical Exercise

We need to examine firewall rules in Windows Defender Firewall.

### Setup

**Step 1: Start the machine**

Click "Start Machine" and wait for it to boot.

**Step 2: Open Windows Defender Firewall**

1. Press Windows key
2. Type "Windows Defender Firewall"
3. Click to open

**Step 3: Go to Advanced Settings**

Click "Advanced Settings" on the left panel.

### Navigating the Interface

**Inbound Rules:**
- Traffic coming INTO the system
- Left sidebar > Inbound Rules

**Outbound Rules:**
- Traffic going OUT of the system
- Left sidebar > Outbound Rules

### Understanding Rule Columns

| Column | Meaning |
|--------|---------|
| Name | Rule name |
| Enabled | Active or not |
| Action | Allow or Block |
| Protocol | TCP, UDP, etc. |
| Local Port | Port on your machine |
| Remote Address | External IP |

---

### Question 1: Rule Blocking SSH

**What is the name of the rule that was created to block all incoming traffic on the SSH port?**

**Steps:**
1. Click "Inbound Rules" (incoming traffic)
2. Look for SSH or port 22
3. SSH default port = 22
4. Find BLOCK action on port 22

**Key information:**
- SSH runs on port 22
- We need a BLOCK rule
- Check the port column

**Looking at the rules:**
- Look for Action = "Block"
- Check Port = 22

The rule name is "Core Op"

- Answer: `Core Op`

---

### Question 2: Rule Allowing SSH from One IP

**A rule was created to allow SSH from one single IP address. What is the rule name?**

**Steps:**
1. Stay in "Inbound Rules"
2. Look for port 22
3. Find ALLOW action
4. Check Remote Address column (should have specific IP)

**What to look for:**
- Port 22
- Action = Allow
- Remote Address = specific IP (not "Any")

The rule name is "Infra Team"

- Answer: `Infra Team`

---

### Question 3: Allowed IP Address

**Which IP address is allowed under this rule?**

**Steps:**
1. Click on "Infra Team" rule
2. Look at Remote Address column
3. Note the IP address

The IP address allowed is visible in the Remote Address field.

- Answer: `192.168.13.7`

---

## Task 5: Linux Firewalls

### Linux Firewall Utilities

**Netfilter** - Framework providing firewall functionality

**Common utilities:**
- `iptables` - Traditional firewall tool
- `nftables` - Modern successor to iptables
- `firewalld` - High-level firewall manager
- `ufw` - Uncomplicated Firewall (easiest!)

### UFW (Uncomplicated Firewall)

UFW makes firewall configuration simple with easy-to-understand commands.

### Basic UFW Commands

**Check firewall status:**
```bash
sudo ufw status
```

**Enable firewall:**
```bash
sudo ufw enable
```

**Disable firewall:**
```bash
sudo ufw disable
```

**Allow all outgoing traffic (default):**
```bash
sudo ufw default allow outgoing
```

**Deny incoming SSH:**
```bash
sudo ufw deny 22/tcp
```
or
```bash
sudo ufw deny ssh
```

**List all rules:**
```bash
sudo ufw status numbered
```

**Delete a rule:**
```bash
sudo ufw delete [rule_number]
```

### More UFW Examples

**Allow specific port:**
```bash
sudo ufw allow 80/tcp       # Allow HTTP
sudo ufw allow 443/tcp      # Allow HTTPS
```

**Allow from specific IP:**
```bash
sudo ufw allow from 192.168.1.100
```

**Deny specific port:**
```bash
sudo ufw deny 21/tcp        # Deny FTP
```

**Allow port range:**
```bash
sudo ufw allow 6000:6007/tcp
```

**Reset all rules:**
```bash
sudo ufw reset
```

### UFW Rule Syntax

**Format:**
```bash
sudo ufw [action] [port]/[protocol]
```

**Examples:**
```bash
sudo ufw allow 22/tcp      # Allow SSH
sudo ufw deny 3389/tcp     # Deny RDP
sudo ufw reject 23/tcp     # Reject Telnet
```

### Questions

**Which Linux firewall utility is considered to be the successor of "iptables"?**

Modern replacement for iptables = nftables

- Answer: `nftables`

---

**What rule would you issue with ufw to deny all outgoing traffic from your machine as a default policy?**

**Command structure:**
```bash
ufw default [action] [direction]
```

We need to:
- Use `default` for default policy
- `deny` for blocking
- `outgoing` for traffic leaving

**Note:** Answer without `sudo`

- Answer: `ufw default deny outgoing`

---

## Quick Reference

### Firewall Types Comparison

| Type | OSI Layers | Speed | Security Level | Use Case |
|------|------------|-------|----------------|----------|
| Stateless | 3-4 | Very Fast | Basic | High-speed networks |
| Stateful | 3-4 | Fast | Good | General purpose |
| Proxy | 7 | Moderate | High | Content filtering |
| NGFW | 3-7 | Slower | Very High | Enterprise security |

### Common Ports

| Port | Service | Protocol |
|------|---------|----------|
| 20-21 | FTP | TCP |
| 22 | SSH | TCP |
| 23 | Telnet | TCP |
| 25 | SMTP | TCP |
| 53 | DNS | TCP/UDP |
| 80 | HTTP | TCP |
| 443 | HTTPS | TCP |
| 3389 | RDP | TCP |

### Rule Components

| Component | Purpose | Example |
|-----------|---------|---------|
| Source | Origin of traffic | 192.168.1.100 |
| Destination | Traffic target | 10.0.0.50 |
| Port | Service port | 22, 80, 443 |
| Protocol | Communication type | TCP, UDP, ICMP |
| Action | What to do | Allow, Deny, Reject |
| Direction | Traffic flow | Inbound, Outbound |

### UFW Commands Reference

| Command | Purpose |
|---------|---------|
| `sudo ufw status` | Check firewall status |
| `sudo ufw enable` | Turn on firewall |
| `sudo ufw disable` | Turn off firewall |
| `sudo ufw allow [port]` | Allow traffic on port |
| `sudo ufw deny [port]` | Block traffic on port |
| `sudo ufw delete [number]` | Remove rule |
| `sudo ufw reset` | Remove all rules |
| `sudo ufw default [action] [direction]` | Set default policy |

---

## Firewall Best Practices

### Rule Creation

**Start with Deny All:**
- Block everything by default
- Only allow what's needed
- Principle of least privilege

**Be Specific:**
- Use specific IPs when possible
- Limit port ranges
- Define exact protocols

**Order Matters:**
- Most specific rules first
- General rules last
- Default deny at the end

### Security Tips

**Regular Reviews:**
- Check rules periodically
- Remove unused rules
- Update as needed

**Logging:**
- Enable logging for blocked traffic
- Monitor for attack patterns
- Review logs regularly

**Testing:**
- Test rules before applying
- Verify expected behavior
- Have rollback plan

**Documentation:**
- Document each rule's purpose
- Keep change logs
- Note business justifications

---

## Common Firewall Scenarios

### Allow Web Server

```bash
# UFW
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# iptables
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

### Block All ICMP (Ping)

```bash
# UFW
sudo ufw deny proto icmp

# iptables
sudo iptables -A INPUT -p icmp -j DROP
```

### Allow SSH from Specific IP

```bash
# UFW
sudo ufw allow from 192.168.1.100 to any port 22

# iptables
sudo iptables -A INPUT -p tcp -s 192.168.1.100 --dport 22 -j ACCEPT
```

---

## Key Takeaways

- **Firewalls are security guards** for your network
- **Inspect incoming and outgoing** traffic
- **Stateless** = fast but basic
- **Stateful** = tracks connections
- **Proxy** = inspects application data
- **NGFW** = most advanced protection
- **Rules have 6 components** - source, destination, port, protocol, action, direction
- **Allow** permits traffic
- **Deny** blocks traffic
- **Inbound** = incoming traffic
- **Outbound** = outgoing traffic
- **SSH runs on port 22**
- **UFW is easiest** Linux firewall
- **nftables** replaces iptables
- **Order of rules matters** - first match wins
- **Default deny** is most secure approach

Firewalls are essential for network security. Understanding how they work and how to configure them properly is crucial for protecting systems from threats
