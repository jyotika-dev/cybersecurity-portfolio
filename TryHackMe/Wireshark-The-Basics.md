# Wireshark: The Basics - TryHackMe

> Learn the basics of Wireshark and how to analyze network traffic and PCAP files

**Room Link:** https://tryhackme.com/r/room/wiresharkthebasics

<img width="1792" height="912" alt="image" src="https://github.com/user-attachments/assets/9aceb61d-8626-495f-a4f1-63b2b5dfcc66" />


---

## Introduction

Wireshark is an open-source network packet analyzer tool used for sniffing and investigating live traffic and packet captures (PCAP files).

**What you'll learn:**
- Wireshark GUI and interface
- Packet dissection and analysis
- Navigating through packets
- Filtering and exporting data
- Following streams

**Files:**
- `http1.pcapng` - For following screenshots
- `Exercise.pcapng` - For answering questions

---

## Task 1: Introduction

### What is Wireshark?

Wireshark is a powerful tool for:
- Detecting network problems
- Finding security anomalies
- Investigating protocols
- Analyzing network traffic

**Important:** Wireshark is NOT an IDS - it only reads packets, doesn't modify them.

### Questions

**Which file is used to simulate the screenshots?**
- Answer: `http1.pcapng`

**Which file is used to answer the questions?**
- Answer: `Exercise.pcapng`

---

## Task 2: Tool Overview

### Wireshark GUI Components

**Toolbar** - Main menus and shortcuts for packet operations

**Display Filter Bar** - Query and filtering section

**Recent Files** - List of recently opened files

**Capture Filter and Interfaces** - Network interfaces for sniffing

**Status Bar** - Tool status and packet information

### Main Window Panes

**Packet List Pane** - Summary of each packet (source, destination, protocol)

**Packet Details Pane** - Detailed protocol breakdown

**Packet Bytes Pane** - Hex and ASCII representation

### Key Features

**Coloring Packets** - Different colors for different protocols/conditions

**Traffic Sniffing** - Blue button starts, red stops, green restarts

**Merge PCAPs** - Combine multiple capture files

**File Details** - View capture info, hash, comments

### Questions

**Read the "capture file comments". What is the flag?**
```
Statistics → Capture File Properties
Scroll down in comments section
```
- Answer: `TryHackMe_Wireshark_Demo`

**What is the total number of packets?**
- Answer: `58620`

**What is the SHA256 hash value of the capture file?**
```
Statistics → Capture File Properties
```
- Answer: `f446de335565fb0b0ee5e5a3266703c778b2f3dfad7efeaeccb2da5641a6d6eb`

---

## Task 3: Packet Dissection

### OSI Layers in Packets

Packets consist of 5-7 layers based on the OSI model:

**Layer 1 - Frame** - Physical layer details

**Layer 2 - Source [MAC]** - MAC addresses (Data Link)

**Layer 3 - Source [IP]** - IP addresses (Network)

**Layer 4 - Protocol** - TCP/UDP ports (Transport)

**Protocol Errors** - TCP reassembly info

**Layer 5 - Application Protocol** - HTTP, FTP, SMB, etc.

**Application Data** - Specific protocol data

### Viewing Packet Details

- Click on packet to see details
- Double-click opens in new window
- Click on any detail to highlight it in bytes pane

### Questions

**View packet number 38. Which markup language is used under the HTTP protocol?**
```
Go to packet 38
Expand HTTP section
```
- Answer: `eXtensible Markup Language`

**What is the arrival date of the packet? (Month/Day/Year)**
```
Check Frame section
```
- Answer: `05/13/2004`

**What is the TTL value?**
```
Expand "Internet Protocol Version 4" section
```
- Answer: `47`

**What is the TCP payload size?**
```
Expand "Transmission Control Protocol" section
```
- Answer: `424`

**What is the e-tag value?**
```
Expand "Hypertext Transfer Protocol" section
```
- Answer: `9a01a-4696-7e354b00`

---

## Task 4: Packet Navigation

### Navigation Features

**Packet Numbers** - Unique ID for each packet

**Go to Packet** - Jump to specific packet number
```
Go → Go to Packet (or Ctrl+G)
```

**Find Packets** - Search by content
```
Edit → Find Packet (or Ctrl+F)
```

Search types:
- Display filter
- Hex
- String
- Regex

**Mark Packets** - Highlight packets for investigation (shown in black)

**Packet Comments** - Add notes to specific packets

**Export Packets** - Save specific packets separately

**Export Objects** - Extract files (HTTP, SMB, TFTP, etc.)

**Time Display Format** - Change how timestamps are shown
```
View → Time Display Format → UTC Date and Time of Day
```

**Expert Info** - Wireshark's analysis suggestions
```
Analyze → Expert Information
```

### Questions

**Search the "r4w" string in packet details. What is the name of artist 1?**
```
Edit → Find Packet
Search: r4w
```
- Answer: `r4w8173`

**Go to packet 12 and read the comments. What is the answer?**
```
Right-click → Packet Comment
Scroll up to see full comment
```
- Answer: `911cd574a42865a956ccde2d04495ebf`

**Go to packet 39765, export JPEG bytes. What is the MD5 hash?**
```
Go to packet 39765
Right-click on JPEG section → Export Packet Bytes
Save as image.jpg

Terminal:
cd Desktop
md5sum image.jpg
```
- Answer: `911cd574a42865a956ccde2d04495ebf`

**Find the .txt file inside the capture. What is the alien's name?**
```
File → Export Objects → HTTP
Filter for .txt
Save note.txt

Terminal:
cd Desktop
cat note.txt
```
- Answer: `PACKETMASTER`

**Look at the expert info section. What is the number of warnings?**
```
Analyze → Expert Information
```
- Answer: `1636`

---

## Task 5: Packet Filtering

### Filtering Methods

**Apply as Filter** - Filter specific values
```
Right-click on field → Apply as Filter
```

**Conversation Filter** - View all packets in a conversation
```
Right-click → Conversation Filter
or
Analyze → Conversation Filter
```

**Colourise Conversation** - Highlight related packets without filtering
```
Right-click → Colorize Conversation
or
View → Colorize Conversation
```

**Prepare as Filter** - Create filter without applying it
```
Right-click → Prepare as Filter
```

**Apply as Column** - Add field as column to packet list
```
Right-click → Apply as Column
```

**Follow Stream** - Reconstruct and view application-level data
```
Right-click → Follow → TCP/UDP/HTTP Stream
or
Analyze → Follow → TCP/UDP/HTTP Stream
```

- Server packets: Blue
- Client packets: Red

### Questions

**Go to packet 4. Right-click on "Hypertext Transfer Protocol" and apply it as a filter. What is the filter query?**
```
Right-click on HTTP → Apply as Filter
```
- Answer: `http`

**What is the number of displayed packets?**
```
Check status bar at bottom
```
- Answer: `1089`

**Go to packet 33790 and follow the stream. What is the total number of artists?**
```
Right-click packet 33790 → Follow → TCP Stream
Use Find to search for "artist"
```
- Answer: `3`

**What is the name of the second artist?**
```
In the stream window, search for "artist=2"
```
- Answer: `Blad3`

---

## Task 6: Conclusion

Congrats! You've learned Wireshark basics. Continue with "Wireshark: Packet Operations" to deepen your skills.

---

## Quick Reference

### Essential Shortcuts

| Action | Shortcut |
|--------|----------|
| Go to Packet | Ctrl + G |
| Find Packet | Ctrl + F |
| Start Capture | Ctrl + E |
| Stop Capture | Ctrl + E |
| Save | Ctrl + S |

### Key Menus

| Menu | Purpose |
|------|---------|
| File → Merge | Combine PCAP files |
| File → Export Objects | Extract files |
| Edit → Find Packet | Search packets |
| View → Time Display Format | Change timestamp format |
| Analyze → Follow Stream | View reconstructed traffic |
| Analyze → Expert Information | View Wireshark's analysis |
| Statistics → Capture File Properties | View file details |

### Packet Panes

| Pane | Shows |
|------|-------|
| Packet List | Summary of all packets |
| Packet Details | Protocol breakdown (OSI layers) |
| Packet Bytes | Hex and ASCII data |

---

## Key Takeaways

- **Wireshark** is a powerful packet analyzer, not an IDS
- **Three panes** show different views of packet data
- **Packet dissection** follows OSI model layers
- **Filtering** is essential for finding relevant traffic
- **Follow Stream** reconstructs application-level data
- **Export Objects** extracts files from captures
- **Expert Info** provides analysis suggestions
- **Golden Rule:** "If you can click it, you can filter it"

Wireshark is an essential tool for network analysis and troubleshooting! 🦈
