# Metasploit: Introduction - TryHackMe

> Learn the basics of Metasploit Framework - the industry-standard penetration testing tool

**Room Link:** https://tryhackme.com/room/metasploitintro

<img width="1872" height="895" alt="image" src="https://github.com/user-attachments/assets/94ec751c-925e-4027-bf42-cfdd147d1698" />


---

## Task 1: Introduction to Metasploit

### What is Metasploit?

Metasploit is an open-source penetration testing framework that helps you:
- Enumerate systems
- Test vulnerabilities
- Execute exploits
- Research vulnerabilities
- Develop exploits

### Key Components

**msfconsole** - Main command-line interface to launch Metasploit

**Modules** - Supporting modules (exploits, scanners, payloads)

**Tools** - Standalone tools (msfvenom, pattern_create, pattern_offset)

**No answer needed**

---

## Task 2: Main Components of Metasploit

### Core Concepts

**Exploit** - Code that uses a vulnerability on the target

**Vulnerability** - Weakness or flaw in a system

**Payload** - Code that runs on target after exploitation

### Module Categories

**Auxiliary**
- Scanners, crawlers, and fuzzers
- Port scanning, service enumeration

**Encoders**
- Obfuscate exploits and payloads
- Help evade signature-based antivirus
- 50/50 success rate

**Evasion**
- Attempts to evade antivirus solutions
- Different from encoders

**Exploits**
- Organized by target system
- Windows, Linux, etc.

**NOPs (No Operation)**
- Do nothing (represented as 0x90)
- Create buffers for consistent payload sizes

**Payloads**
- Code that runs on target
- Get shell, load malware, run commands

**4 Payload Categories:**

1. **Adapters** - Convert singles to different formats
2. **Singles** - Self-contained payloads (no additional downloads)
3. **Stagers** - Set up network connection, small initial size
4. **Stages** - Downloaded by stager, allows larger payloads

### Payload Naming

**Singles (inline):**
- Uses underscore `_`
- Example: `generic/shell_reverse_tcp`

**Staged:**
- Uses forward slash `/`
- Example: `windows/x64/shell/reverse_tcp`

**Post**
- Post-exploitation modules
- Used after gaining access

### Questions

**What is the name of the code taking advantage of a flaw on the target system?**
- Answer: `exploit`

**What is the name of the code that runs on the target system to achieve the attacker's goal?**
- Answer: `payload`

**What are self-contained payloads called?**
- Answer: `singles`

**Is "windows/x64/pingback_reverse_tcp" among singles or staged payloads?**
- Look for `_` (singles) or `/` (staged) between shell and reverse
- Answer: `singles`

---

## Task 3: Msfconsole

### Launching Metasploit

```bash
msfconsole
```

You'll see the `msf6 >` prompt.

### Basic Usage

**Regular commands work:**
- `ls`, `cd`, `ping`, `clear`, `history`

**Tab completion** - Supported

**Help:**
```bash
help [command]
```

### Selecting Modules

**Use command:**
```bash
use exploit/windows/smb/ms17_010_eternalblue
```

**Show options:**
```bash
show options
```

**Show compatible payloads:**
```bash
show payloads
```

**Go back:**
```bash
back
```

**Get info:**
```bash
info exploit/windows/smb/ms17_010_eternalblue
```

### Search Command

Search the Metasploit database:

```bash
search apache          # Search by name
search cve:2017        # Search by CVE
search type:exploit    # Search by type
```

### Questions

**How would you search for a module related to Apache?**
- Answer: `search apache`

**Who provided the auxiliary/scanner/ssh/ssh_login module?**
```bash
info auxiliary/scanner/ssh/ssh_login
```
- Answer: `todb`

---

## Task 4: Working with Modules

### Setting Parameters

**Syntax:**
```bash
set PARAMETER_NAME VALUE
```

**Example:**
```bash
set RHOSTS 10.10.10.10
set LPORT 4444
```

### The 5 Metasploit Prompts

**1. Regular command prompt** - Can't use Metasploit commands
```
user@machine:~$
```

**2. Msfconsole prompt** - No module selected
```
msf6 >
```

**3. Context prompt** - Module selected
```
msf6 exploit(windows/smb/ms17_010_eternalblue) >
```

**4. Meterpreter prompt** - Connected to target
```
meterpreter >
```

**5. Shell on target** - Command shell on target system
```
C:\Windows\system32>
```

### Common Parameters

**RHOSTS** - Remote host (target IP)
- Single IP: `10.10.10.10`
- Range: `10.10.10.1-10`
- CIDR: `10.10.10.0/24`
- File: `file:/path/to/targets.txt`

**RPORT** - Remote port (target port)

**PAYLOAD** - Payload to use

**LHOST** - Local host (attacker IP)

**LPORT** - Local port (for reverse shell)

**SESSION** - Session ID for post-exploitation

### Managing Parameters

**Set parameter:**
```bash
set LPORT 6666
```

**Clear parameter:**
```bash
unset LPORT
```

**Clear all:**
```bash
unset all
```

**Set globally (persists between modules):**
```bash
setg RHOSTS 10.10.10.10
```

**Clear global:**
```bash
unsetg RHOSTS
```

### Running Modules

**Execute:**
```bash
exploit
# or
run
```

**Execute and background:**
```bash
exploit -z
```

**Check if vulnerable:**
```bash
check
```

### Managing Sessions

**Background session:**
```bash
background
# or
CTRL+Z
```

**List sessions:**
```bash
sessions
```

**Interact with session:**
```bash
sessions -i [ID]
```

### Questions

**How would you set the LPORT value to 6666?**
- Answer: `set lport 6666`

**How would you set the global value for RHOSTS to 10.10.19.23?**
- Answer: `setg rhosts 10.10.19.23`

**What command would you use to clear a set payload?**
- Answer: `unset payload`

**What command do you use to proceed with the exploitation phase?**
- Answer: `exploit`

---

## Task 5: Summary

Metasploit provides modules for each exploitation phase:
1. Finding exploits
2. Customizing exploits
3. Exploiting vulnerable services

**No answer needed**

---

## Quick Reference

### Essential Commands

| Command | Purpose |
|---------|---------|
| `msfconsole` | Launch Metasploit |
| `search [term]` | Search for modules |
| `use [module]` | Select module |
| `show options` | View parameters |
| `show payloads` | Show compatible payloads |
| `set [param] [value]` | Set parameter |
| `setg [param] [value]` | Set globally |
| `exploit` or `run` | Execute module |
| `check` | Check if vulnerable |
| `sessions` | List sessions |
| `sessions -i [ID]` | Interact with session |
| `background` | Background session |
| `back` | Return to msfconsole |
| `info [module]` | Module information |

### Common Parameters

| Parameter | Purpose |
|-----------|---------|
| RHOSTS | Target IP address |
| RPORT | Target port |
| LHOST | Attacker IP |
| LPORT | Attacker port |
| PAYLOAD | Payload to use |
| SESSION | Session ID |

### Payload Types

| Type | Naming | Example |
|------|--------|---------|
| Singles | `shell_reverse` | `generic/shell_reverse_tcp` |
| Staged | `shell/reverse` | `windows/x64/shell/reverse_tcp` |

### Module Categories

| Category | Purpose |
|----------|---------|
| Auxiliary | Scanners, fuzzers |
| Encoders | Obfuscate payloads |
| Evasion | Evade antivirus |
| Exploits | Attack code |
| NOPs | Buffer creation |
| Payloads | Code for target |
| Post | Post-exploitation |

---

## Key Takeaways

- **Metasploit** is the industry-standard pentesting framework
- **msfconsole** is the main interface
- **Modules** perform specific tasks (exploit, scan, payload)
- **Singles** are self-contained, **staged** are multi-part
- **Set parameters** before running exploits
- **RHOSTS** = target, **LHOST** = attacker
- **Sessions** manage connections to targets
- **Search** finds relevant modules quickly
- **setg** sets global parameters (persist between modules)
- **Tab completion** and **help** are your friends

Metasploit is essential for penetration testing! 🎯
