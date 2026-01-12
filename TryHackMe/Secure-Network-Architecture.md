# TryHackMe Secure Network Architecture - Walkthrough

> Network Security Best Practices

**Room Link:** https://tryhackme.com/room/introtosecurityarchitecture

<img width="1482" height="895" alt="image" src="https://github.com/user-attachments/assets/e390d19e-f05b-4cb0-b104-5242ac776004" />


---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Network Segmentation

### Q1: How many trunks are present in this configuration?

**What are trunks:**
- Connection between router and switch
- Configured as bridges in VyOS
- Check bridge configurations

**Answer:** `4`

### Q2: What is the VLAN tag ID for interface eth12?

**Check VLAN configuration for eth12**

**Answer:** `30`

---

## Task 3: Common Secure Network Architecture

### Security Zones Table

| Zone | Description |
|------|-------------|
| External | Outside/untrusted (internet users) |
| DMZ | Public-facing servers |
| Internal | Corporate network |
| Restricted | Critical systems (domain controllers) |

### Q1: What zone would a user connecting to public web server be in?

**Answer:** `External`

### Q2: What zone would a public web server be in?

**Answer:** `DMZ`

### Q3: What zone would a core domain controller be placed in?

**Answer:** `Restricted`

---

## Task 4: Network Security Policies and Controls

**Analyze ACL policies for packet filtering**

### Q1: Will the first packet result in drop or accept?

**Check ACL rules against first packet**

**Answer:** `accept`

### Q2: Will the second packet result in drop or accept?

**Check ACL rules against second packet**

**Answer:** `drop`

---

## Task 5: Zone-Pair Policies and Filtering

**Fill in blanks on static site using provided table**

### Q: What is the flag after filling all blanks?

**Steps:**
1. Review zone-pair policy table
2. Match policies to blanks
3. Fill in correct values
4. Submit to get flag

**Answer:** `THM{M05tly_53cure}`

---

## Task 6: Validating Network Traffic

### Q1: Does SSL inspection require man-in-the-middle proxy? (Y/N)

**SSL inspection process:**
- Intercepts encrypted traffic
- Uses SSL proxy (MitM)
- Decrypts for inspection

**Answer:** `Y`

### Q2: What platform processes data sent from SSL proxy?

**After decryption:**
- Proxy sends to UTM platform
- UTM = Unified Threat Management
- Processes and inspects traffic

**Answer:** `Unified Threat Management`

---

## Task 7: Addressing Common Attacks

### Q1: Where does DHCP snooping store leased IP addresses from untrusted hosts?

**DHCP snooping:**
- Operates at layer 2 (switch)
- Tracks untrusted hosts
- Stores in database

**Answer:** `DHCP Binding Database`

### Q2: Will a switch drop or accept DHCPRELEASE packet?

**DHCP snooping behavior:**
- Validates DHCP messages
- Drops suspicious packets

**Answer:** `Drop`

### Q3: Does dynamic ARP inspection use DHCP binding database? (Y/N)

**ARP inspection:**
- Uses DHCP binding database
- Matches IP to MAC addresses
- Validates ARP packets

**Answer:** `Y`

### Q4: Dynamic ARP inspection will match IP address and what other packet detail?

**ARP inspection checks:**
- Source IP address
- Source MAC address
- Compares to binding database

**Answer:** `MAC address`

---

## Task 8: Conclusion

**No answer needed**

---

## Quick Reference

### Network Segmentation

**VLANs (Virtual LANs):**
- Logical network separation
- Security boundaries
- Traffic isolation

**Trunks:**
- Inter-switch connections
- Carry multiple VLANs
- Tagged traffic

### Security Zones

**Common zones:**

```
External → DMZ → Internal → Restricted
(Internet) (Public) (Corporate) (Critical)
```

**Zone purposes:**
- External: Untrusted users
- DMZ: Public services
- Internal: Employee systems
- Restricted: Critical infrastructure

---

## Security Concepts

### ACLs (Access Control Lists)

**What they do:**
- Filter network traffic
- Allow or deny packets
- Based on rules

**Common criteria:**
- Source/destination IP
- Port numbers
- Protocol type

### SSL/TLS Inspection

**How it works:**
1. Proxy intercepts traffic
2. Decrypts SSL/TLS
3. Sends to UTM
4. UTM inspects content
5. Re-encrypts if clean

**Considerations:**
- Acts as MitM
- Privacy concerns
- Performance impact
- Can see plain text

### DHCP Snooping

**Purpose:**
- Prevent rogue DHCP servers
- Validate DHCP messages
- Build binding database

**How it works:**
- Trusts legitimate DHCP server
- Untrusts all other ports
- Logs IP/MAC bindings
- Drops invalid messages

### Dynamic ARP Inspection

**Purpose:**
- Prevent ARP spoofing
- Validate ARP packets
- Stop man-in-the-middle

**How it works:**
- Uses DHCP binding database
- Checks IP-to-MAC mapping
- Drops mismatched packets
- Protects against ARP attacks

---

## Network Design Principles

### Well-Designed Network

**Key features:**
- Redundancy (failover paths)
- Optimization (efficient routing)
- Security (segmentation)
- Scalability (growth ready)

### Security Considerations

**Layer 2 (Data Link):**
- VLAN segmentation
- Port security
- DHCP snooping
- ARP inspection

**Layer 3 (Network):**
- ACLs
- Routing policies
- Firewall rules

**Layer 7 (Application):**
- SSL inspection
- Content filtering
- IPS/IDS

---

## Common Attacks

### DHCP Attacks

**Rogue DHCP server:**
- Attacker runs fake DHCP
- Gives malicious config
- Can redirect traffic

**Prevention:**
- DHCP snooping
- Trusted ports only

### ARP Spoofing

**Attack method:**
- Fake ARP responses
- Redirect traffic
- Man-in-the-middle

**Prevention:**
- Dynamic ARP inspection
- Static ARP entries
- Network monitoring

---

## Best Practices

### Network Segmentation

**Do:**
- Separate by function
- Use VLANs
- Implement zones
- Restrict traffic flow

**Don't:**
- Flat networks
- Trust all devices
- Allow unrestricted access

### Security Controls

**Implement:**
- ACLs on all interfaces
- DHCP snooping
- ARP inspection
- SSL inspection (if needed)
- Regular audits

### Zone Design

**Guidelines:**
- External: No trust
- DMZ: Limited trust
- Internal: Moderate trust
- Restricted: High security

---

## Key Takeaways

- **Network segmentation** improves security
- **VLANs** create logical boundaries
- **Security zones** define trust levels
- **ACLs** filter traffic
- **DHCP snooping** prevents rogue servers
- **ARP inspection** stops spoofing
- **SSL inspection** needs MitM proxy
- **UTM** processes inspected traffic
- **Layer 2** security is important
- **Defense in depth** approach essential

---

Happy learning!
