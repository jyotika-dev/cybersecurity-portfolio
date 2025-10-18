# OSI Model - TryHackMe

> Understand the fundamental networking framework that determines how data is handled across networks

**Room Link:** https://tryhackme.com/r/room/osimodelzi

<img width="1292" height="925" alt="image" src="https://github.com/user-attachments/assets/9a4c7456-f383-4f43-beea-1caac50f9c75" />


---

## Task 1: What is the OSI Model?

The OSI (Open Systems Interconnection) model is a fundamental framework for how networked devices communicate. It ensures that data sent across a network can be understood by different devices as long as they follow the OSI model's principles.

The model has seven layers (numbered 7 to 1 from top to bottom). As data travels through each layer, specific processes occur and additional information gets added. This process is called encapsulation.

### Questions

**What does "OSI" stand for?**
- Answer: `Open Systems Interconnection`

**How many layers does the OSI model have?**
- Answer: `7`

**What is the key term for when pieces of information get added to data?**
- Answer: `Encapsulation`

---

## Task 2: Layer 7 - Application

The application layer is what you interact with most. It serves as the interface between you and the data you exchange over a network. The application layer defines protocols and rules for how users interact with data.

Examples include:
- Email clients
- Web browsers
- File transfer applications (like FileZilla)
- DNS (Domain Name System) - translates website addresses to IP addresses

### Questions

**What is the name of this layer?**
- Answer: `Application`

**What is the technical term for the software that users interact with?**
- Answer: `Graphical User Interface` (GUI)

---

## Task 3: Layer 6 - Presentation

The presentation layer marks the beginning of data standardization. It acts as a translator between the application layer and the network.

This layer:
- Transforms data into a format both sending and receiving applications can understand
- Ensures consistent data display regardless of the software used
- Handles security features like data encryption (HTTPS for secure websites)

### Questions

**What is the name of this layer?**
- Answer: `Presentation`

**What is the main purpose that this layer acts as?**
- Answer: `Translator`

---

## Task 4: Layer 5 - Session

The session layer establishes a connection (session) with the recipient computer. This session ensures a dedicated communication channel for data exchange.

Key functions:
- Synchronizes both computers before data transfer begins
- Breaks down data into manageable chunks called packets
- If the connection drops, only unsent packets need retransmission (like resuming from a game checkpoint)
- Sessions are unique - data cannot be transmitted across multiple sessions simultaneously

### Questions

**What is the name of this layer?**
- Answer: `Session`

**What is the technical term for when a connection is successfully established?**
- Answer: `Session`

**What is the technical term for "small chunks of data"?**
- Answer: `Packets`

---

## Task 5: Layer 4 - Transport

The transport layer plays a crucial role in data transmission using two primary protocols: TCP and UDP. The choice depends on specific requirements.

### TCP - Transmission Control Protocol

**Priorities:** Reliability and guaranteed delivery

**How it works:**
- Establishes a dedicated connection between devices
- Incorporates error checking
- Verifies packets are received and reassembled in the correct order

**Advantages:**
- Guarantees data accuracy
- Synchronizes devices to prevent data flooding

**Disadvantages:**
- Requires a reliable connection (missing packets render data unusable)
- Slower due to extensive reliability checks
- Can bottleneck connections

**Best for:** File sharing, web browsing, email

### UDP - User Datagram Protocol

**Priorities:** Speed and simplicity

**How it works:**
- Forgoes error checking and guaranteed delivery
- Simply sends data packets without confirmation of receipt

**Advantages:**
- Significantly faster than TCP
- Flexible for software developers
- Doesn't require dedicated connections (suitable for unstable environments)

**Disadvantages:**
- No guarantees of data reception
- Unsuitable for applications requiring complete data

**Best for:** Device discovery (ARP, DHCP), live streaming

### Questions

**What is the name of this layer?**
- Answer: `Transport`

**What does TCP stand for?**
- Answer: `Transmission Control Protocol`

**What does UDP stand for?**
- Answer: `User Datagram Protocol`

**What protocol guarantees the accuracy of data?**
- Answer: `TCP`

**What protocol doesn't care if data is received or not?**
- Answer: `UDP`

**What protocol would an email client use?**
- Answer: `TCP`

**What protocol would a file download use?**
- Answer: `TCP`

**What protocol would a video streaming application use?**
- Answer: `UDP`

---

## Task 6: Layer 3 - Network

The network layer handles routing and reassembly of data packets. Packets that were fragmented earlier are pieced back together.

This layer determines the most efficient path (route) for packets to reach their destination. Route selection considers:

- **Path Length:** Routes with fewest network devices (hops) are preferred
- **Reliability:** Routes with packet loss history are less desirable
- **Bandwidth:** Faster physical connections (like fiber optics) are prioritized

This layer uses IP addresses (like 192.168.1.100) for device identification. Routers are Layer 3 devices because they operate at this level.

### Questions

**What is the name of this layer?**
- Answer: `Network`

**Will packets take the most optimal route across a network? (Y/N)**
- Answer: `Y`

**What does OSPF stand for?**
- Answer: `Open Shortest Path First`

**What does RIP stand for?**
- Answer: `Routing Information Protocol`

**What type of addresses are dealt with at this layer?**
- Answer: `IP Addresses`

---

## Task 7: Layer 2 - Data Link

The data link layer bridges the gap between logical network addressing and physical transmission.

This layer:
- Receives packets from the network layer (containing destination IP address)
- Adds the recipient's physical address - the MAC (Media Access Control) address
- Prepares data into a format suitable for transmission over physical network media

**MAC Address:** Every network interface card (NIC) has a unique, factory-assigned MAC address burned onto the hardware. This acts as a permanent identifier and cannot be modified (though spoofing techniques exist). For local device communication, it's the MAC address that's used, not the IP address.

### Questions

**What is the name of this layer?**
- Answer: `Data Link`

**What is the name of the hardware that all networked devices have?**
- Answer: `Network Interface Card` (NIC)

---

## Task 8: Layer 1 - Physical

The physical layer is the easiest to understand. It references the physical components of hardware used in networking and is the lowest layer.

Devices use electrical signals to transfer data between each other in binary numbering (1's and 0's).

Examples:
- Ethernet cables connecting devices
- All physical hardware components

### Questions

**What is the name of this layer?**
- Answer: `Physical`

**What is the numbering system with 0's and 1's?**
- Answer: `Binary`

**What is the name of the cables used to connect devices?**
- Answer: `Ethernet Cables`

---

## Task 9: Practical - OSI Game

Play the interactive game to escape the dungeon.

**What is the flag?**
- Answer: `THM{OSI_DUNGEON_ESCAPED}`

---

## Task 10: Continue Your Learning - Packets & Frames

Move on to the "Packets and Frames" room to continue learning about networking concepts.

---

## Summary - OSI Model Layers

| Layer | Name | Purpose | Protocol Examples |
|-------|------|---------|-------------------|
| 7 | Application | User interface & applications | HTTP, DNS, SMTP |
| 6 | Presentation | Data translation & encryption | SSL/TLS, JPEG, MPEG |
| 5 | Session | Connection management | NetBIOS, SSH |
| 4 | Transport | Data delivery | TCP, UDP |
| 3 | Network | Routing & addressing | IP, OSPF, RIP |
| 2 | Data Link | MAC addressing | Ethernet, PPP |
| 1 | Physical | Hardware & signals | Cables, Hubs, Repeaters |

---

## Key Takeaways

- The OSI model has 7 layers that work together to enable network communication
- Each layer adds information (encapsulation) as data moves down
- TCP is reliable but slower - used for important data
- UDP is fast but unreliable - used for speed-critical applications
- IP addresses are used for routing, MAC addresses for local delivery
- Understanding each layer helps troubleshoot network issues
