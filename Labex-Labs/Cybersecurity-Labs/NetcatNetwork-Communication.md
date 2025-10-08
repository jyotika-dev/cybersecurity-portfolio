# Netcat Network Communication Lab

This document summarizes my hands-on learning with Netcat, the "Swiss Army knife" of networking tools, focusing on network communication, file transfer, and encrypted messaging.

**Lab Focus:** Network communication fundamentals using Netcat and OpenSSL encryption.

---

## Lab Overview

### Objectives
- Install and verify Netcat
- Understand ports and network listeners
- Create client-server communication
- Transfer files over network
- Implement encrypted communication with OpenSSL

---

## Part 1: Installing Netcat

### Verify Installation

```bash
nc -h
```

**Output:** OpenBSD netcat help information with command options

**Note:** Netcat was pre-installed in lab environment

---

## Part 2: Understanding Ports and Creating a Listener

### What are Ports?

**Ports are like numbered doors on a computer:**
- Range: 0-65535
- Allow multiple communications simultaneously
- Each port leads to different service/application

### Creating a Listener

**Terminal 1 (Server/Listener):**
```bash
nc -l 12345
```

**Command Breakdown:**
- `nc` - Netcat command
- `-l` - Listen mode (wait for incoming connections)
- `12345` - Port number to listen on

**Terminal 2 (Client/Connector):**
```bash
nc localhost 12345
```

**Command Breakdown:**
- `localhost` - Connect to own computer
- `12345` - Port to connect to

### Testing Communication

1. Type message in Terminal 2: `Hello`
2. Message appears in Terminal 1
3. Type message in Terminal 1: `World`
4. Message appears in Terminal 2

**Result:** Full-duplex communication (both directions simultaneously)

**End Connection:** Press `Ctrl+C` in both terminals

---

## Part 3: File Transfer with Netcat

### Creating Test File

```bash
cd ~/project
echo "Top Secret: The cake recipe is actually a lie" > secret.txt
```

### Setting Up Receiver

**Terminal 1 (Receiving end):**
```bash
cd ~/project
nc -l 12345 > received_secret.txt
```

**Explanation:**
- `-l 12345` - Listen on port 12345
- `> received_secret.txt` - Redirect incoming data to file

### Sending File

**Terminal 2 (Sending end):**
```bash
cd ~/project
nc localhost 12345 < secret.txt
```

**Explanation:**
- `localhost 12345` - Connect to listener
- `< secret.txt` - Use file as input (send contents)

### Verifying Transfer

```bash
cat received_secret.txt
```

**Output:**
```
Top Secret: The cake recipe is actually a lie
```

**Key Insight:** Replace `localhost` with remote IP address to transfer files between different computers.

---

## Part 4: Implementing Encrypted Communication

### Encryption Setup

**Algorithm:** AES-256-CBC  
**Key Derivation:** PBKDF2 with 10,000 iterations  
**Encoding:** Base64  
**Password:** `secretpassword`

### Creating Secure Sender Script

```bash
nano secure_sender.sh
```

**Script Content:**
```bash
#!/bin/bash

echo "Secure Sender - Enter messages to send. Press Ctrl+C to exit."

while true; do
  echo "Enter message:"
  read message
  encrypted=$(echo "$message" | openssl enc -aes-256-cbc -salt -base64 -pbkdf2 -iter 10000 -pass pass:secretpassword 2> /dev/null)
  echo "$encrypted" | nc -N localhost 12345
done
```

**Make Executable:**
```bash
chmod +x secure_sender.sh
```

### Creating Secure Receiver Script

```bash
nano secure_receiver.sh
```

**Script Content:**
```bash
#!/bin/bash

echo "Secure Receiver - Waiting for messages. Press Ctrl+C to exit."

while true; do
  encrypted=$(nc -l -p 12345)
  if [ ! -z "$encrypted" ]; then
    decrypted=$(echo "$encrypted" | openssl enc -aes-256-cbc -d -salt -base64 -pbkdf2 -iter 10000 -pass pass:secretpassword 2> /dev/null)
    echo "Received message: $decrypted"
  fi
done
```

**Make Executable:**
```bash
chmod +x secure_receiver.sh
```

---

## Part 5: Testing Encrypted Communication

### Starting Receiver

**Terminal 1:**
```bash
./secure_receiver.sh
```

**Output:**
```
Secure Receiver - Waiting for messages. Press Ctrl+C to exit.
```

### Starting Sender

**Terminal 2:**
```bash
./secure_sender.sh
```

**Output:**
```
Secure Sender - Enter messages to send. Press Ctrl+C to exit.
Enter message:
```

### Sending Encrypted Messages

**In Terminal 2 (Sender):**
```
Enter message: Hello, this is a secret message
```

**In Terminal 1 (Receiver):**
```
Received message: Hello, this is a secret message
```

**Process:**
1. Message typed in sender terminal
2. Automatically encrypted with AES-256-CBC
3. Sent over network via Netcat
4. Received and decrypted automatically
5. Displayed in plaintext in receiver terminal

**End Communication:** Press `Ctrl+C` in both terminals

---

## Script Breakdown

### Sender Script Workflow

```bash
# 1. Prompt for user input
read message

# 2. Encrypt message
encrypted=$(echo "$message" | openssl enc -aes-256-cbc -salt -base64 -pbkdf2 -iter 10000 -pass pass:secretpassword 2> /dev/null)

# 3. Send via Netcat
echo "$encrypted" | nc -N localhost 12345
```

**Encryption Parameters:**
- `-aes-256-cbc` - AES encryption with 256-bit key
- `-salt` - Add random data for security
- `-base64` - Encode for safe transmission
- `-pbkdf2` - Key derivation function
- `-iter 10000` - 10,000 iterations for key strengthening
- `-pass pass:secretpassword` - Encryption password

### Receiver Script Workflow

```bash
# 1. Listen for incoming data
encrypted=$(nc -l -p 12345)

# 2. Check if data received
if [ ! -z "$encrypted" ]; then

# 3. Decrypt message
decrypted=$(echo "$encrypted" | openssl enc -aes-256-cbc -d -salt -base64 -pbkdf2 -iter 10000 -pass pass:secretpassword 2> /dev/null)

# 4. Display decrypted message
echo "Received message: $decrypted"
```

**Decryption Parameters:**
- `-d` - Decryption mode
- All other parameters must match sender

---

## Key Netcat Commands

### Basic Usage

| Command | Description |
|---------|-------------|
| `nc -h` | Display help information |
| `nc -l PORT` | Listen on specified port |
| `nc HOST PORT` | Connect to host on port |
| `nc -l PORT > file` | Receive data and save to file |
| `nc HOST PORT < file` | Send file contents |
| `nc -N HOST PORT` | Close connection after EOF |

### Common Options

| Flag | Purpose |
|------|---------|
| `-l` | Listen mode |
| `-p` | Specify port (listen mode) |
| `-N` | Close connection after sending |
| `-v` | Verbose output |
| `-z` | Scan mode (port scanning) |

---

## Understanding Network Communication

### Client-Server Model

**Server (Listener):**
- Waits for incoming connections
- Listens on specific port
- Accepts data from clients

**Client (Connector):**
- Initiates connection
- Connects to server's IP and port
- Sends data to server

### Localhost Communication

**localhost = 127.0.0.1**
- Refers to own computer
- Used for testing
- No external network required

### Remote Communication

**Replace localhost with remote IP:**
```bash
# Sender connects to remote receiver
nc 192.168.1.100 12345 < secret.txt

# Receiver listens on network interface
nc -l 12345 > received_file.txt
```

---

## Security Considerations

### Encryption Benefits

✅ **Confidentiality:** Data unreadable if intercepted  
✅ **Integrity:** Detects message tampering  
✅ **Authentication:** Shared password verifies parties  

### Best Practices

**For Production Use:**
- Use SSH tunneling or VPN for secure channels
- Implement proper key management
- Use public-key cryptography (asymmetric encryption)
- Add message authentication codes (MAC)
- Rotate encryption keys regularly

---

## Lab Completion Summary

### Skills Acquired

✅ Netcat installation and verification  
✅ Port-based network communication  
✅ Client-server connection establishment  
✅ File transfer over network  
✅ Bash scripting for automation  
✅ OpenSSL encryption integration  
✅ Encrypted messaging implementation  

### Commands Mastered

```bash
# Installation verification
nc -h

# Basic communication
nc -l 12345              # Listen
nc localhost 12345       # Connect

# File transfer
nc -l 12345 > file.txt   # Receive file
nc localhost 12345 < file.txt  # Send file

# Encrypted communication
./secure_sender.sh       # Send encrypted messages
./secure_receiver.sh     # Receive and decrypt
```

### Files Created

- `secret.txt` - Test file for transfer
- `received_secret.txt` - Received file copy
- `secure_sender.sh` - Encryption and sending script
- `secure_receiver.sh` - Receiving and decryption script

---

## Key Takeaways

### Netcat Fundamentals

- Versatile networking tool for TCP/UDP communication
- Simple syntax for quick network operations
- Full-duplex communication capability
- Cross-platform compatibility

### Network Communication

- Ports enable multiple simultaneous connections
- Client-server model fundamental to networking
- Localhost useful for testing before deployment
- File transfer possible with simple redirects

### Encryption Integration

- OpenSSL provides strong cryptographic functions
- Symmetric encryption requires shared secret
- Scripts automate complex encryption workflows
- Base64 encoding ensures safe transmission

---

---

**Lab completed successfully!** Gained foundational understanding of network communication, file transfer, and encrypted messaging using Netcat and OpenSSL integration.
