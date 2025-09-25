# 🐧 LinuxJourney - Subnetting

This file summarizes my learning from **LinuxJourney** on IP addressing and subnetting concepts.  
https://labex.io/lesson/ipv4  
I practiced these on my local terminal and network environments.

---

## 🔹 IPv4
- **32-bit addresses** → 4 octets (192.168.1.1).
- **Address classes** → A, B, C (classful addressing - mostly historical).
- **Network and host portions** → determined by subnet mask.
- **Dotted decimal notation** → human-readable format.
- `ip addr show` → display IPv4 addresses on interfaces.
- **Private address ranges** → RFC 1918 (10.x.x.x, 172.16-31.x.x, 192.168.x.x).

## 🔹 Subnets
- **Logical network segments** → divide large networks into smaller ones.
- **Subnet mask** → defines network vs host bits.
- **Network ID** → identifies the subnet.
- **Broadcast address** → last address in subnet range.
- **Host range** → usable addresses between network ID and broadcast.
- `ipcalc 192.168.1.0/24` → calculate subnet information.

## 🔹 Subnet Math
- **Binary calculations** → convert decimal to binary for subnetting.
- **Network bits** → determine number of subnets (2^n).
- **Host bits** → determine hosts per subnet (2^n - 2).
- **VLSM** → Variable Length Subnet Masking for efficiency.
- **Subnetting formula** → 2^(subnet bits) = number of subnets.
- Practice binary conversion and subnet calculations.

## 🔹 Subnetting Cheats
- **Quick reference** → common subnet masks and their values.
- **/24** → 255.255.255.0 (256 addresses, 254 hosts).
- **/25** → 255.255.255.128 (128 addresses, 126 hosts).
- **/26** → 255.255.255.192 (64 addresses, 62 hosts).
- **/27** → 255.255.255.224 (32 addresses, 30 hosts).
- **Magic number** → 256 minus subnet value for increment.

## 🔹 CIDR
- **Classless Inter-Domain Routing** → modern IP addressing method.
- **CIDR notation** → IP address followed by prefix length (/24).
- **Prefix length** → number of network bits in mask.
- **Supernetting** → combining multiple networks into larger block.
- `ip route show` → view CIDR routes in routing table.
- More efficient than classful addressing.

## 🔹 NAT
- **Network Address Translation** → maps private IPs to public IPs.
- **Static NAT** → one-to-one mapping.
- **Dynamic NAT** → pool of public IPs.
- **PAT/NAPT** → Port Address Translation (most common).
- `iptables -t nat -L` → view NAT rules on Linux.
- Enables multiple devices to share single public IP.

## 🔹 IPv6
- **128-bit addresses** → much larger address space.
- **Hexadecimal notation** → 2001:db8::1.
- **Address types** → unicast, multicast, anycast.
- **No NAT needed** → abundant address space.
- `ip -6 addr show` → display IPv6 addresses.
- **Dual stack** → running IPv4 and IPv6 simultaneously.

---

📌 **Applied Insight**:  
Subnetting knowledge is fundamental in cybersecurity:
- **Network segmentation** → isolates systems to limit attack spread.  
- **VLAN design** → proper subnetting enables secure network architecture.  
- **Firewall rules** → subnet-based access control and traffic filtering.  
- **Incident response** → subnet analysis helps trace attack paths.  
- **Network reconnaissance** → understanding target network topology through subnetting.
