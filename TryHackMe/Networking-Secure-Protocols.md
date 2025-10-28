# Networking Secure Protocols - TryHackMe

> Learn how TLS, SSH, and VPN secure network traffic and protect your data

**Room Link:** https://tryhackme.com/room/networkingsecureprotocols

<img width="1478" height="929" alt="image" src="https://github.com/user-attachments/assets/e2d4d12a-fb09-42f4-b0ce-013c5e9333d6" />

---

## Introduction

In the previous room, we learned about HTTP, FTP, SMTP, and other protocols. But there's a problem - they send everything in cleartext! Anyone watching the network can see your passwords, credit card numbers, and private data.

This room covers how to secure these protocols using:
- **TLS** (Transport Layer Security)
- **SSH** (Secure Shell)
- **VPN** (Virtual Private Network)

**No answer needed**

---

## Task 2: TLS

### The Problem

In the early days, attackers could easily capture network traffic and read:
- Login passwords
- Email messages  
- Credit card numbers
- Everything sent in cleartext!

### The Solution: SSL/TLS

**SSL (Secure Sockets Layer)** - Created by Netscape in 1995
**TLS (Transport Layer Security)** - Upgrade to SSL, created in 1999

**TLS 1.3** (2018) is the current version after 20+ years of improvements.

### What TLS Provides

**Confidentiality** - Nobody can read your data
**Integrity** - Nobody can modify your data
**Authenticity** - You're talking to the real server, not a fake

### How It Works

1. Server gets a **TLS certificate** signed by a Certificate Authority (CA)
2. CAs verify the server's identity
3. Your browser trusts the CA's signature
4. TLS encrypts all traffic between you and the server

**Let's Encrypt** - Free TLS certificates!
**Self-signed certificates** - Not trusted (anyone can create them)

### Questions

**What is the protocol name that TLS upgraded and built upon?**
- Answer: `SSL`

**Which type of certificates should not be used to confirm the authenticity of a server?**
- Answer: `self-signed certificate`

---

## Task 3: HTTPS

### HTTP vs HTTPS

**HTTP** (port 80) - Everything in cleartext
**HTTPS** (port 443) - Encrypted with TLS

### How HTTP Works

1. TCP three-way handshake
2. Send HTTP requests (GET, POST, etc.)

### How HTTPS Works

1. TCP three-way handshake
2. **TLS handshake** (negotiate encryption)
3. Send HTTP requests (now encrypted!)

### What You See in Wireshark

**HTTP** - You can read everything (passwords, data, etc.)
**HTTPS** - You see "Application Data" (encrypted gibberish)

Even if you capture HTTPS traffic, you can't read it without the encryption key!

### Questions

**How many packets did the TLS negotiation and establishment take in the Wireshark HTTPS screenshots above?**
- Answer: `8`

**What is the number of the packet that contains the GET /login when accessing the website over HTTPS?**
- Answer: `10`

---

## Task 4: SMTPS, POP3S, and IMAPS

### Securing Email Protocols

Just like HTTP becomes HTTPS, email protocols get secured too:

| Protocol | Port | Secure Version | Secure Port |
|----------|------|----------------|-------------|
| SMTP | 25 | SMTPS | 465 |
| POP3 | 110 | POP3S | 995 |
| IMAP | 143 | IMAPS | 993 |

**The "S" means Secure** (using TLS)

### Key Point

All the benefits of HTTPS apply to these protocols:
- Passwords are encrypted
- Email content is encrypted
- Nobody can read or modify your emails in transit

### Questions

**If you capture network traffic, in which of the following protocols can you extract login credentials: SMTPS, POP3S, or IMAP?**
- Answer: `IMAP` (not secured by TLS)

---

## Task 5: SSH

### The Problem with TELNET

TELNET lets you remotely access systems, but it sends everything in cleartext - including your password!

### The Solution: SSH

**SSH (Secure Shell)** - Created in 1995 by Tatu Ylönen
**SSH-2** - More secure version (1996)
**OpenSSH** - Open-source implementation (1999)

### SSH Benefits

✅ **Secure authentication** - Password, public key, or 2FA
✅ **Confidentiality** - All traffic encrypted
✅ **Integrity** - Data can't be modified
✅ **Tunneling** - Route other protocols through SSH
✅ **X11 Forwarding** - Run graphical apps remotely

### Using SSH

```bash
ssh username@hostname
```

If your username matches, just use:
```bash
ssh hostname
```

**Port 23** - TELNET (insecure)
**Port 22** - SSH (secure)

### Questions

**What is the name of the open-source implementation of the SSH protocol?**
- Answer: `OpenSSH`

---

## Task 6: SFTP and FTPS

### Two Ways to Secure FTP

**SFTP (SSH File Transfer Protocol)**
- Part of SSH protocol suite
- Uses **port 22** (same as SSH)
- Easy to set up

**FTPS (FTP Secure)**
- FTP secured with TLS
- Uses **port 990**
- Requires TLS certificate
- More complex setup

### Using SFTP

```bash
sftp username@hostname
```

Commands:
- `get filename` - Download file
- `put filename` - Upload file

### Questions

**Click View Site and follow instructions to get the flag.**
- Answer: `THM{Protocols_secur3d}`

---

## Task 7: VPN

### What is a VPN?

VPN (Virtual Private Network) creates a secure connection over the Internet.

**Virtual** - Uses existing Internet infrastructure
**Private** - Encrypts your traffic

### VPN Use Cases

**1. Connect Company Offices**
- Main branch + remote branches
- All sites can access shared resources securely

**2. Remote Worker Access**
- Employees work from home
- Connect securely to company network

**3. Privacy and Security**
- Hide your real IP address
- ISP can't see your traffic (only encrypted data)
- Bypass geographical restrictions

### How VPN Works

1. VPN client on your device
2. VPN server at destination
3. **VPN tunnel** encrypts all traffic between them

**Example:**
- You connect to VPN server in Japan
- Websites see you as located in Japan
- Your ISP only sees encrypted traffic

### Important Notes

⚠️ Some VPN servers leak your real IP
⚠️ Some countries ban VPN usage
⚠️ Always check local laws before using VPNs

### Questions

**What would you use to connect various company sites so remote users can access resources at the main branch?**
- Answer: `VPN`

---

## Task 8: Closing Notes & Challenge

### Three Ways to Secure Network Traffic

1. **TLS** - Secures HTTP, SMTP, POP3, IMAP, etc.
2. **SSH** - Secure remote access and file transfers
3. **VPN** - Connect entire networks securely

### Challenge: Decrypt TLS Traffic

The machine has a browser configured to log TLS keys. Use Wireshark to decrypt HTTPS traffic and find login credentials.

**Steps:**
1. Open `randy-chromium.pcapng` in Wireshark (Documents folder)
2. Right-click → Protocol Preferences → Transport Layer Security
3. Open TLS preferences
4. Browse to `ssl-key.log` file (Documents folder)
5. Click OK - Wireshark will decrypt TLS traffic
6. Find the packet with login credentials

### Questions

**One of the packets contains login credentials. What password did the user submit?**
- Answer: `THM{B8WM6P}`

---

## Protocol Ports Reference

### Insecure Protocols

| Protocol | Port |
|----------|------|
| HTTP | 80 |
| FTP | 21 |
| SMTP | 25 |
| POP3 | 110 |
| IMAP | 143 |
| TELNET | 23 |

### Secure Protocols

| Protocol | Port |
|----------|------|
| HTTPS | 443 |
| FTPS | 990 |
| SMTPS | 465 |
| POP3S | 995 |
| IMAPS | 993 |
| SSH | 22 |
| SFTP | 22 |

---

## Key Takeaways

- **TLS** adds encryption to existing protocols (just add "S"!)
- **Self-signed certificates** can't prove server authenticity
- **HTTPS** is HTTP + TLS (secure web browsing)
- **SSH** replaces insecure TELNET
- **SFTP** is part of SSH (port 22)
- **FTPS** is FTP + TLS (port 990)
- **VPN** creates secure tunnels over the Internet
- Without encryption, anyone can read your passwords and data!

Understanding these secure protocols is essential for protecting your data online! 🔒
