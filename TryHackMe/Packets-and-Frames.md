# Packets & Frames - TryHackMe

> Understanding how data is broken down and transmitted across networks

**Room Link:** https://tryhackme.com/room/packetsframes

<img width="1301" height="922" alt="image" src="https://github.com/user-attachments/assets/64464500-774b-4ccb-9e3d-aee500934c57" />


---

## Task 1: What are Packets and Frames?

### The Basics

Packets and frames are small pieces of data that form together to make a larger piece of information. They're different things in the OSI model:

**Packet** - Layer 3 (Network Layer)
- Contains IP addressing information
- Has an IP header and payload

**Frame** - Layer 2 (Data Link Layer)
- Encapsulates the packet
- Adds MAC addresses and other info

### The Analogy

Think of it like mailing a letter:
- The **frame** is the envelope (used to move the contents)
- The **packet** is the letter inside
- The recipient opens the envelope (frame) to see how to forward the letter (packet)

This process is called **encapsulation**.

### Why Break Data into Packets?

Packets are efficient because:
- Less chance of network bottlenecks
- Data is sent in small pieces instead of all at once
- If something fails, only small pieces need to be resent

**Example:** When loading an image from a website, it's divided into multiple packets and reconstructed on your computer.

### Packet Structure

Packets have different structures depending on the protocol. Key headers include:

- **Time to Live (TTL)** - How long the packet should live before being discarded
- **Checksum** - Ensures data integrity
- **Source/Destination IP** - Where the packet is from and going to
- **Protocol** - What protocol is being used (TCP/UDP)

### Questions

**What is the name for a piece of data when it does have IP addressing information?**
- Answer: `Packet`

**What is the name for a piece of data when it does not have IP addressing information?**
- Answer: `Frame`

---

## Task 2: TCP/IP (The Three-Way Handshake)

### What is TCP?

TCP (Transmission Control Protocol) is a set of rules for network communication. It's similar to the OSI model but simplified into 4 layers:

1. Application
2. Transport
3. Internet
4. Network Interface

### Key Features of TCP

**Connection-Based:**
- Must establish a connection before sending data
- Guarantees that data sent will be received

**Advantages:**
- Guarantees data accuracy
- Can synchronize two devices
- Performs error checking

**Disadvantages:**
- Requires a reliable connection
- Slower due to error checking
- Uses more system resources

### Important TCP Headers

- **Source Port** - Port opened by sender
- **Destination Port** - Port the application is running on
- **Sequence Number** - Order to reassemble data
- **Acknowledgement Number** - Next expected data piece
- **Checksum** - Ensures data integrity
- **Data** - The actual payload
- **Flag** - Determines how the packet should be handled

### The Three-Way Handshake

The process to establish a connection between two devices:

1. **SYN** - Client sends Initial Sequence Number (ISN) to synchronize
2. **SYN/ACK** - Server sends its ISN and acknowledges client's ISN
3. **ACK** - Client acknowledges server's ISN and sends data

**Example:**
- SYN: "Here's my starting number (0)"
- SYN/ACK: "Here's my number (5000), I got your 0"
- ACK: "I got your 5000, here's data starting at 1"

### Closing a TCP Connection

TCP closes connections to free up system resources:

1. Device sends **FIN** packet
2. Other device acknowledges with **ACK**
3. Other device sends its own **FIN**
4. First device sends final **ACK**

Both devices agree to close the connection.

### Questions

**What is the header in a TCP packet that ensures the integrity of data?**
- Answer: `Checksum`

**Provide the order of a normal Three-Way Handshake (separated by commas):**
- Answer: `SYN,SYN/ACK,ACK`

---

## Task 3: Practical - Handshake

Complete the interactive exercise to simulate a TCP three-way handshake.

Follow the prompts:
- Send SYN
- Receive SYN/ACK
- Send ACK
- Connection established!

**Flag:**
```
THM{TCP_CHATTER}
```

---

## Task 4: UDP/IP

### What is UDP?

UDP (User Datagram Protocol) is another protocol for sending data between devices.

### Key Differences from TCP

**UDP is Stateless:**
- No connection needs to be established
- No three-way handshake
- No synchronization between devices

**No Guarantees:**
- Doesn't care if data is received
- No error checking like TCP
- No data integrity safeguards

### When to Use UDP

**Good for:**
- Video streaming
- Voice calls
- Gaming
- Situations where speed matters more than perfect accuracy

**Why?** Applications can tolerate some data loss - a few dropped frames in a video call won't ruin it.

### UDP Packet Structure

Much simpler than TCP. Key headers:

- **Time to Live (TTL)** - How long packet should exist
- **Source Port** - Port opened by sender
- **Destination Port** - Port application is running on
- **Data** - The actual payload

### UDP vs TCP

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-based | Stateless |
| Speed | Slower | Faster |
| Reliability | Guaranteed delivery | No guarantees |
| Use case | File transfers, email | Streaming, voice calls |

### Questions

**What does "UDP" stand for?**
- Answer: `User Datagram Protocol`

**What type of connection is "UDP"?**
- Answer: `Stateless`

**What protocol would you use to transfer a file?**
- Answer: `TCP`

**What protocol would you use to have a video call?**
- Answer: `UDP`

---

## Task 5: Ports 101 (Practical)

Complete the practical exercise by connecting to the specified IP address and port number.

Enter the details as prompted to establish the connection.

**Flag:**
```
THM{YOU_CONNECTED_TO_A_PORT}
```

---

## Task 6: Continue Your Learning - Extending Your Network

Move on to the next room to learn more about network concepts.

---

## Summary

**Key Takeaways:**

- **Packets** have IP info (Layer 3), **Frames** don't (Layer 2)
- **TCP** is reliable but slower - used for accuracy-critical tasks
- **UDP** is fast but unreliable - used for speed-critical tasks
- **Three-Way Handshake:** SYN → SYN/ACK → ACK
- Data is broken into small pieces for efficient transmission
- Checksum ensures data integrity in TCP

Understanding packets and frames is fundamental to how networks communicate!
