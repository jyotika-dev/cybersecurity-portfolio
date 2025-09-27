# 🐧 LinuxJourney - Network Config

This file summarizes my learning from **LinuxJourney** on Linux network configuration and management.  
https://labex.io/lesson/network-interfaces  
I practiced these on my local terminal and network environments.

---

## 🔹 Network Interfaces
- **Physical and virtual interfaces** → connect system to networks.
- `ip link show` → display all network interfaces.
- `ifconfig` → older tool for interface configuration.
- **Interface naming** → eth0, wlan0, enp0s3 (predictable naming).
- `ip addr add 192.168.1.100/24 dev eth0` → assign IP address.
- `ip link set eth0 up` → bring interface up.
- `ip link set eth0 down` → bring interface down.

## 🔹 route
- **Routing table management** → control packet forwarding decisions.
- `route -n` → display routing table in numeric format.
- `ip route show` → modern command to view routes.
- `route add default gw 192.168.1.1` → add default gateway.
- `ip route add 10.0.0.0/8 via 192.168.1.1` → add static route.
- `route del -net 192.168.2.0/24` → delete specific route.
- **Persistent routes** → configured in network files for permanent setup.

## 🔹 dhclient
- **DHCP client** → automatically obtain network configuration.
- `dhclient eth0` → request IP address from DHCP server.
- `dhclient -r eth0` → release current DHCP lease.
- `dhclient -v eth0` → verbose DHCP negotiation output.
- `/var/lib/dhcp/dhclient.leases` → stored lease information.
- `/etc/dhcp/dhclient.conf` → DHCP client configuration.
- **DORA process** → Discover, Offer, Request, Acknowledge.

## 🔹 Network Manager
- **High-level network management** → desktop and server networking.
- `nmcli` → command-line interface for NetworkManager.
- `nmcli device status` → show device status.
- `nmcli connection show` → list network connections.
- `nmcli connection up profile_name` → activate connection.
- `nmcli device wifi list` → scan for wireless networks.
- `nmtui` → text-based user interface for network configuration.

## 🔹 arp
- **Address Resolution Protocol** → maps IP to MAC addresses.
- `arp -a` → display ARP table entries.
- `ip neighbor show` → modern command for ARP table.
- `arp -d 192.168.1.1` → delete ARP entry.
- `arp -s 192.168.1.1 aa:bb:cc:dd:ee:ff` → add static ARP entry.
- **ARP cache** → temporary storage of IP-to-MAC mappings.
- **Gratuitous ARP** → announces IP address ownership.

---

📌 **Applied Insight**:  
Network configuration knowledge is crucial in cybersecurity:
- **Network forensics** → ARP tables reveal device communication patterns.  
- **MITM detection** → ARP spoofing attacks manipulate IP-to-MAC mappings.  
- **Network segmentation** → proper routing configuration isolates network segments.  
- **Incident response** → interface and routing analysis helps trace attack paths.  
- **System hardening** → disable unused interfaces and configure secure routing policies.
