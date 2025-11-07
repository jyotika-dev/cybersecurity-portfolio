# Windows PowerShell - TryHackMe

> Learn about scripting and the different types of PowerShell commands to help you navigate Windows systems

**Room Link:** https://tryhackme.com/room/powershell

<img width="1598" height="910" alt="image" src="https://github.com/user-attachments/assets/9afd1992-c0bf-4095-86ef-422374ef848b" />

---

## Task 1: Introduction
### Learning Objectives

- Learn what PowerShell is and its capabilities
- Understand the basic structure of PowerShell's language
- Learn and run some basic PowerShell commands
- Understand PowerShell's applications in cyber security

**No answer needed**

---

## Task 2: What Is PowerShell

### Definition

From Microsoft: "PowerShell is a cross-platform task automation solution made up of a command-line shell, a scripting language, and a configuration management framework."

### Key Features

**Cross-Platform**
- Works on Windows, macOS, and Linux
- Originally created for Windows

**Object-Oriented**
- Handles complex data types
- Built on .NET framework
- Processes objects with properties and methods

**Better than CMD**
- CMD processes plain text
- PowerShell works with objects

### CMD vs PowerShell

| Feature | CMD | PowerShell |
|---------|-----|------------|
| Type | Text-based | Object-oriented |
| Example | `dir`, `echo` | `Get-ChildItem`, `Write-Output` |
| Output | Plain text | .NET objects |

### Questions

**What do we call the advanced approach used to develop PowerShell?**
- Answer: `object-oriented`

---

## Task 3: PowerShell Basics

### Cmdlets (Command-lets)

PowerShell uses cmdlets that follow a consistent **Verb-Noun** naming convention, making commands easier to understand.

### Common Commands

**Get-Content** - Read file content
- Similar to `type` in CMD
- Similar to `cat` in Linux

**Set-Location** - Change directory
- Similar to `cd` in CMD/Linux

**Get-Command** - List all commands
- Helps discover available cmdlets

### Launching PowerShell

- Start Menu
- Run dialog (Win + R)
- Type `powershell` in CMD

### Questions

**How would you retrieve a list of commands that start with the verb Remove?**
```powershell
Get-Command -Name Remove*
```
- Answer: `Get-Command -Name Remove*`

---

**What cmdlet has its traditional counterpart echo as an alias?**

Check aliases:
```powershell
Get-Alias -Name echo
```
- Answer: `Write-Output`

---

**What is the command to retrieve some example usage for the cmdlet New-LocalUser?**
```powershell
Get-Help New-LocalUser -examples
```
- Answer: `Get-Help New-LocalUser -examples`

---

## Task 4: Navigating the File System and Working with Files

### File System Cmdlets

PowerShell offers cmdlets for file system management with a unified approach.

**Get-ChildItem** - List directory contents
- Like `dir` in CMD
- Like `ls` in Linux

**Set-Location** - Navigate directories
- Like `cd` in CMD/Linux

**New-Item** - Create file or directory
- Similar to `mkdir` or `touch`

**Remove-Item** - Delete file/directory
- Like `del` or `rmdir` in CMD

**Get-Content** - Read file content
- Similar to `type` in CMD
- Similar to `cat` in Linux

### Example Commands

```powershell
Get-Content -Path C:\Users
```

### Questions

**What cmdlet can you use instead of the traditional Windows command type?**
- Answer: `Get-Content`

---

**What PowerShell command would you use to display the content of the "C:\Users" directory?**
```powershell
Get-ChildItem -Path C:\Users
```
- Answer: `Get-ChildItem -Path C:\Users`

---

**How many items are displayed by the command described in the previous question?**
- Answer: `4`

---

## Task 5: Piping, Filtering, and Sorting Data

### Piping

PowerShell uses the pipe operator (`|`) to send output of one cmdlet as input to another.

**Key Difference:** PowerShell pipes objects, not text!

### Comparison Operators

| Operator | Meaning | Description |
|----------|---------|-------------|
| `-eq` | Equal to | Match objects |
| `-ne` | Not equal | Exclude objects |
| `-gt` | Greater than | Strict comparison |
| `-ge` | Greater than or equal | Non-strict |
| `-lt` | Less than | Strict comparison |
| `-le` | Less than or equal | Non-strict |

### Common Commands

**Sort-Object** - Sort objects by property
- No direct CMD equivalent
- Similar to `sort` in Linux

**Where-Object** - Filter objects based on conditions
- Similar to `grep` in Linux

**Select-Object** - Select properties from objects
- Used for refining output

### Example

```powershell
Get-ChildItem | Where-Object -Property Length -gt 100
```

Lists files greater than 100 bytes.

### Questions

**How would you retrieve the items in the current directory with size greater than 100?**
```powershell
Get-ChildItem | Where-Object -Property Length -gt 100
```
- Answer: `Get-ChildItem | Where-Object -Property Length -gt 100`

---

## Task 6: System and Network Information

### System Information Cmdlets

PowerShell retrieves detailed system and network information in a structured way.

**Get-ComputerInfo** - Detailed system information
- Like `systeminfo` in CMD

**Get-NetIPConfiguration** - Network interface details
- Similar to `ipconfig` in CMD

**Get-LocalUser** - List local user accounts
- Similar to `net user` in CMD

### Questions

**Other than your current user and the default "Administrator" account, what other user is enabled on the target machine?**

```powershell
Get-LocalUser
```

Look for enabled users.

- Answer: `p1r4t3`

---

**This lad has hidden his account among the others with no regard for our beloved captain! What is the motto he has so bluntly put as his account's description?**

```powershell
Get-LocalUser
```

Check the description field for user `p1r4t3`.

- Answer: `A merry life and a short one.`

---

**Can you navigate the filesystem and find the hidden treasure inside this pirate's home?**

Navigate to the user's folder:
```powershell
Set-Location C:\Users\p1r4t3
Get-ChildItem
Set-Location hidden-treasure-chest
Get-Content big-treasure.txt
```

- Answer: `THM{p34rlInAsh3ll}`

---

## Task 7: Real-Time System Analysis

### Analysis Cmdlets

PowerShell provides advanced tools to analyze processes, services, and network connections.

**Get-Process** - List running processes
- Similar to `tasklist` in CMD

**Get-Service** - Show services and statuses
- Similar to `net start` in CMD

**Get-NetTCPConnection** - Display active TCP connections
- Like `netstat` in CMD

### Questions

**In the previous task, you found a marvellous treasure carefully hidden in the target machine. What is the hash of the file that contains it?**

```powershell
Get-FileHash -Path big-treasure.txt
```

- Answer: `71FC5EC11C2497A32F8F08E61399687D90ABE6E204D2964DF589543A613F3E08`

---

**What property retrieved by default by Get-NetTCPConnection contains information about the process that has started the connection?**

```powershell
Get-NetTCPConnection
```

Look at the properties returned.

- Answer: `OwningProcess`

---

**Can you find the service name that has been tampered with? The DisplayName contains the motto "merry".**

```powershell
Get-Service | Where-Object { $_.DisplayName -like "*merry*" } | Select-Object Name, DisplayName
```

- Answer: `p1r4t3-s-compass`

---

## Task 8: Scripting

### Remote Execution

PowerShell is a powerful scripting language for automating tasks across multiple systems.

**Key Command:**
```powershell
Invoke-Command -ComputerName RemotePC -ScriptBlock {Get-Service}
```

Runs `Get-Service` on a remote computer named "RemotePC".

### Comparison

- CMD has no built-in scripting equivalent
- PowerShell is comparable to Bash scripting in Linux

### Questions

**What is the syntax to execute the command Get-Service on a remote computer named "RoyalFortune"?**

```powershell
Invoke-Command -ComputerName RoyalFortune -ScriptBlock {Get-Service}
```

- Answer: `Invoke-Command -ComputerName RoyalFortune -ScriptBlock {Get-Service}`

---

## Task 9: Conclusion

**No answer needed**

---

## Quick Reference

### Basic Cmdlets

| Cmdlet | Purpose | CMD Equivalent |
|--------|---------|----------------|
| `Get-ChildItem` | List directory | `dir` |
| `Set-Location` | Change directory | `cd` |
| `Get-Content` | Read file | `type` |
| `New-Item` | Create file/folder | `mkdir` |
| `Remove-Item` | Delete file/folder | `del` / `rmdir` |
| `Copy-Item` | Copy file | `copy` |
| `Move-Item` | Move file | `move` |

### Information Cmdlets

| Cmdlet | Purpose |
|--------|---------|
| `Get-ComputerInfo` | System information |
| `Get-LocalUser` | List local users |
| `Get-NetIPConfiguration` | Network config |
| `Get-Process` | Running processes |
| `Get-Service` | Windows services |
| `Get-NetTCPConnection` | Network connections |
| `Get-FileHash` | File hash |

### Help and Discovery

| Cmdlet | Purpose |
|--------|---------|
| `Get-Command` | List all commands |
| `Get-Help [cmdlet]` | Get help for cmdlet |
| `Get-Alias` | Show aliases |
| `Get-Member` | Show object properties |

### Comparison Operators

| Operator | Meaning |
|----------|---------|
| `-eq` | Equal to |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-ge` | Greater than or equal |
| `-lt` | Less than |
| `-le` | Less than or equal |
| `-like` | Wildcard match |

### Piping and Filtering

| Cmdlet | Purpose |
|--------|---------|
| `Where-Object` | Filter objects |
| `Select-Object` | Select properties |
| `Sort-Object` | Sort objects |
| `Measure-Object` | Calculate statistics |

### Common Patterns

**Filter files by size:**
```powershell
Get-ChildItem | Where-Object -Property Length -gt 100
```

**Search for text in files:**
```powershell
Get-Content file.txt | Where-Object { $_ -like "*search*" }
```

**Find service by display name:**
```powershell
Get-Service | Where-Object { $_.DisplayName -like "*name*" }
```

**Get file hash:**
```powershell
Get-FileHash -Path file.txt
```

**Remote execution:**
```powershell
Invoke-Command -ComputerName PC01 -ScriptBlock {Get-Service}
```

---
