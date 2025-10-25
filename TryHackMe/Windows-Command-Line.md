# Windows Command Line - TryHackMe

> Learn essential Windows Command Prompt (cmd.exe) commands for system management, networking, and troubleshooting

**Room Link:** https://tryhackme.com/room/windowscommandline

<img width="1769" height="913" alt="image" src="https://github.com/user-attachments/assets/efbeacb7-3fa2-4713-addd-941a7d67cce4" />

---

## Introduction

Command Line Interfaces (CLI) might seem challenging at first, but they offer major advantages:

- **Speed** - Find information faster than clicking through menus
- **Efficiency** - Uses fewer system resources
- **Automation** - Easy to script repetitive tasks
- **Remote Management** - Manage systems from anywhere

This room focuses on using Windows Command Prompt (cmd.exe) for everyday tasks.

### Question

**What is the default command line interpreter in the Windows environment?**
- Answer: `cmd.exe`

---

## Basic System Information

### Essential Commands

**ver** - Shows OS version
```cmd
ver
```

**systeminfo** - Displays detailed system info (OS, processor, memory, etc.)
```cmd
systeminfo
```

**driverquery** - Lists installed drivers
```cmd
driverquery | more
```
(The `| more` displays output one page at a time)

**help** - Shows help for commands
```cmd
help command_name
```

**cls** - Clears the screen
```cmd
cls
```

### Questions

**What is the OS version of the Windows VM?**
```cmd
ver
```
- Answer: `10.0.20348.2655`

**What is the hostname of the Windows VM?**
```cmd
ipconfig /all
```
- Answer: `WINSRV2022-CORE`

---

## Network Troubleshooting

### Network Configuration Commands

**ipconfig** - Shows basic network info (IP address, subnet mask)
```cmd
ipconfig
```

**ipconfig /all** - Shows detailed network info (DNS, DHCP, MAC address)
```cmd
ipconfig /all
```

### Network Testing Commands

**ping** - Tests network connectivity
```cmd
ping target_name
```

**tracert** - Traces the route packets take to reach target
```cmd
tracert target_name
```

**nslookup** - Looks up IP address for a domain
```cmd
nslookup domain.com
```

**netstat** - Shows network connections and listening ports
```cmd
netstat -abon
```
- `-a` - All connections
- `-b` - Shows programs
- `-o` - Shows process IDs
- `-n` - Shows numerical addresses

### Questions

**Which command can we use to look up the server's physical address (MAC address)?**
- Answer: `ipconfig /all`

**What is the name of the service listening on port 135?**
```cmd
ipconfig /all
```
- Answer: `RpcSs`

**What is the name of the process listening on port 3389?**
```cmd
netstat -abon
```
- Answer: `TermService`

---

## File and Disk Management

### Working With Directories

**cd** - Change directory or show current directory
```cmd
cd C:\Users
```

**dir** - List files in current directory
```cmd
dir
```

**dir /a** - Show hidden files
```cmd
dir /a
```

**dir /s** - List files in current directory and subdirectories
```cmd
dir /s
```

**mkdir** - Create new directory
```cmd
mkdir folder_name
```

**rmdir** - Remove directory
```cmd
rmdir folder_name
```

**tree** - Visual representation of folder structure
```cmd
tree
```

### Working With Files

**type** - Display file contents
```cmd
type filename.txt
```

**more** - Show file contents one page at a time
```cmd
more filename.txt
```

**copy** - Copy file to new location
```cmd
copy file.txt C:\destination\
```

**move** - Move file to new location
```cmd
move file.txt C:\destination\
```

**del** or **erase** - Delete file
```cmd
del file.txt
```

**Wildcards** - Use `*` to match multiple files
```cmd
copy *.txt C:\destination\
```
(Copies all .txt files)

### Questions

**What are the file's contents in C:\Treasure\Hunt?**
```cmd
type C:\Treasure\Hunt\flag.txt
```
- Answer: `THM{CLI_POWER}`

---

## Task and Process Management

### Process Commands

**tasklist** - Show running processes
```cmd
tasklist
```

**tasklist with filter** - Find specific process
```cmd
tasklist /FI "imagename eq notepad.exe"
```

**taskkill** - Terminate a process by PID
```cmd
taskkill /PID 1234
```

### Questions

**What command would you use to find the running processes related to notepad.exe?**
- Answer: `tasklist /FI "imagename eq notepad.exe"`

**What command can you use to kill the process with PID 1516?**
- Answer: `taskkill /PID 1516`

---

## Conclusion

### Commands Covered

You've learned the most practical commands for managing Windows systems via command line. Remember that most commands support `/?` to display help:

```cmd
ipconfig /?
```

### Additional Commands (Not Covered in Detail)

- **chkdsk** - Checks disk for errors
- **driverquery** - Lists device drivers
- **sfc /scannow** - Scans and repairs system files

### Using the 'more' Command

The `more` command has two uses:

**1. Display text files:**
```cmd
more file.txt
```

**2. View long command output page by page:**
```cmd
command | more
```

### Final Questions

**The command `shutdown /s` can shut down a system. What command would you use to restart?**
- Answer: `shutdown /r`

**What command can you use to abort a scheduled system shutdown?**
- Answer: `shutdown /a`

---

## Command Reference

| Command | Purpose | Example |
|---------|---------|---------|
| `ver` | Show OS version | `ver` |
| `systeminfo` | Detailed system info | `systeminfo` |
| `ipconfig` | Network info | `ipconfig /all` |
| `ping` | Test connectivity | `ping google.com` |
| `tracert` | Trace route | `tracert google.com` |
| `nslookup` | DNS lookup | `nslookup google.com` |
| `netstat` | Network connections | `netstat -abon` |
| `cd` | Change directory | `cd C:\Users` |
| `dir` | List files | `dir /a` |
| `mkdir` | Create directory | `mkdir newfolder` |
| `type` | Display file | `type file.txt` |
| `copy` | Copy file | `copy file.txt C:\dest` |
| `move` | Move file | `move file.txt C:\dest` |
| `del` | Delete file | `del file.txt` |
| `tasklist` | Show processes | `tasklist` |
| `taskkill` | Kill process | `taskkill /PID 1234` |
| `shutdown` | Shutdown/restart | `shutdown /r` |
| `cls` | Clear screen | `cls` |
| `help` | Get help | `help command` |

---

## Key Takeaways

- **CLI is faster** than GUI for many tasks
- **ipconfig** is your friend for network info
- **tasklist** and **taskkill** manage processes
- Use **/?** with any command to get help
- Use **| more** to view long output page by page
- **Wildcards (*)** help work with multiple files at once

The Windows Command Line is a powerful tool once you get comfortable with it! 💻
