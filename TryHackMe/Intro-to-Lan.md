# Intro to LAN - TryHackMe

> Learn about LAN topologies, switches, routers, subnetting, ARP, and DHCP

**Room Link:** https://tryhackme.com/room/introtolan
<img width="1299" height="920" alt="image" src="https://github.com/user-attachments/assets/14a73310-dda9-48d7-ae2c-1d03b6450a63" />


---

## Overview

This room covers the fundamentals of how local area networks work, including the technologies and designs that power them.

---

## Task 1: Introducing LAN Topologies

### What is a Network Topology?

A topology refers to the design or layout of a network. There are three main types to know.

**Star Topology**
- Devices connect individually to a central device (switch or hub)
- Most common today
- Pros: Reliable, scalable, easy to add devices
- Cons: Expensive, requires more maintenance, central point of failure

**Bus Topology**
- All devices connect to a single backbone cable
- Pros: Cost-efficient, easy to set up
- Cons: Slow if many devices request data, single point of failure, hard to troubleshoot

**Ring Topology**
- Devices connect in a loop, forming a ring
- Pros: Little cabling needed, less dependent on dedicated hardware
- Cons: Not efficient for data travel, entire network fails if one device breaks

### Questions

**What does LAN stand for?**
- Answer: `Local Area Network`

**What is the verb given to the job that Routers perform?**
- Answer: `Routing`

**What device is used to centrally connect multiple devices on the local network and transmit data to the correct location?**
- Answer: `Switch`

**What topology is cost-efficient to set up?**
- Answer: `Bus Topology`

**What topology is expensive to set up and maintain?**
- Answer: `Star Topology`

**Complete the interactive lab. What is the flag given at the end?**
- Answer: `THM{TOPOLOGY_FLAWS}`

---

## Task 2: A Primer on Subnetting

### What is Subnetting?

Subnetting is dividing a network into smaller pieces. It helps categorize and assign specific parts of a network. This is done using a subnet mask.

### IP Address Structure

An IP address has four sections called octets. Same with subnet masks - they're 32 bits and range from 0-255.

Subnets use IP addresses in three ways:

**Network Address** - Identifies the start of the network
- Example: A device with IP 192.168.1.100 is on network 192.168.1.0

**Host Address** - Identifies a specific device on the subnet
- Example: 192.168.1.1

**Default Gateway** - The device that can send info to another network
- Usually uses first (.1) or last (.254) host address
- Receives data destined for devices outside the current network

### Questions

**What is the technical term for dividing a network up into smaller pieces?**
- Answer: `Subnetting`

**How many bits are in a subnet mask?**
- Answer: `32`

**What is the range of a section (octet) of a subnet mask?**
- Answer: `0-255`

**What address is used to identify the start of a network?**
- Answer: `Network Address`

**What address is used to identify devices within a network?**
- Answer: `Host Address`

**What is the name used to identify the device responsible for sending data to another network?**
- Answer: `Default Gateway`

---

## Task 3: ARP

### What is ARP?

The Address Resolution Protocol (ARP) allows devices to identify themselves on a network. It associates a MAC address (physical identifier) with an IP address (logical identifier).

### How ARP Works

Each device keeps a cache of MAC addresses associated with other devices. When devices communicate, they use ARP to find the MAC address of the target device.

**ARP Request** - A broadcast asking if any device has a specific IP address

**ARP Reply** - The device confirms it has that IP address

Once a device receives an ARP reply, it stores this information in its cache as an ARP entry.

### Questions

**What does ARP stand for?**
- Answer: `Address Resolution Protocol`

**What category of ARP Packet asks a device whether or not it has a specific IP address?**
- Answer: `Request`

**What address is used as a physical identifier for a device on a network?**
- Answer: `MAC Address`

**What address is used as a logical identifier for a device on a network?**
- Answer: `IP Address`

---

## Task 4: DHCP

### What is DHCP?

IP addresses can be assigned manually or automatically using a DHCP server. Most networks use DHCP for automatic assignment.

### DHCP Process

When a device connects to a network without a manual IP assignment, it goes through this process:

1. **DHCP Discover** - Device broadcasts to find a DHCP server
2. **DHCP Offer** - DHCP server replies with an available IP address
3. **DHCP Request** - Device confirms it wants that IP address
4. **DHCP ACK** - Server acknowledges the assignment is complete

The device can now use the assigned IP address.

### Questions

**What type of DHCP packet is used by a device to retrieve an IP address?**
- Answer: `DHCP Discover`

**What type of DHCP packet does a device send once it has been offered an IP address by the DHCP server?**
- Answer: `DHCP Request`

**What is the last DHCP packet that is sent to a device from a DHCP server?**
- Answer: `DHCP ACK`

---

## Summary

This room covers the essentials of LAN networks:
- Network topologies determine how devices connect
- Subnetting organizes networks into manageable pieces
- ARP helps devices find each other using MAC addresses
- DHCP automatically assigns IP addresses to devices

These concepts form the foundation of how local networks operate!

---

## Task 5: Continue Your Learning - OSI Model

No specific questions - move on to the next learning topic.
