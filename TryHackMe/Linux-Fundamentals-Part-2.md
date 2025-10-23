# Linux Fundamentals Part 2 - TryHackMe

> Continue learning Linux by connecting remotely via SSH and mastering file operations and permissions

**Room Link:** https://tryhackme.com/room/linuxfundamentalspart2

<img width="1576" height="923" alt="image" src="https://github.com/user-attachments/assets/3c524ab5-1860-4df5-b7e1-82bb6a24bfd3" />

---

## Overview

Part 2 takes your Linux skills further by moving from the in-browser terminal to real SSH connections. You'll learn:

- Connecting to remote machines via SSH
- Using command flags and arguments
- Advanced file operations (copy, move)
- Understanding file permissions
- Switching between users
- Key Linux directories

---

## Task 1: Introduction

Welcome to part 2! This room builds on the basics from part 1 and introduces you to remote connections and more advanced Linux concepts.

**What you'll learn:**
- Remote machine control via SSH
- Command flags and switches
- File copying and moving
- File permissions and security
- Running scripts and executables

**No answer needed**

---

## Task 2: Accessing Your Linux Machine Using SSH (Deploy)

### What is SSH?

SSH (Secure Shell) allows you to connect to and control a remote Linux machine securely over the network.

### Connecting via SSH

Deploy the machine and connect using:

```bash
ssh tryhackme@MACHINE_IP
```

When prompted, enter the password: `tryhackme`

**No answer needed** - Just deploy and connect!

---

## Task 3: Introduction to Flags and Switches

### What are Flags?

Flags (or switches) are options you add to commands to change their behavior. They usually start with `-` or `--`.

### Using Manual Pages

The `man` command shows the manual for any command:

```bash
man ls
```

This shows all available flags and how to use them.

### Questions

**Explore the manual page of the ls command**
- No answer needed

**What directional arrow key would we use to navigate down the manual page?**
- Answer: `down`

**What flag would we use to display the output in a "human-readable" way?**
```bash
ls -h
```
- Answer: `-h`

---

## Task 4: Filesystem Interaction Continued

### New Commands

**touch** - Creates a new empty file
```bash
touch filename.txt
```

**file** - Determines the file type
```bash
file filename.txt
```

**mv** - Moves or renames files
```bash
mv source destination
```

**cp** - Copies files
```bash
cp source destination
```

### Questions

**How would you create the file named "newnote"?**
- Answer: `touch newnote`

**On the deployable machine, what is the file type of "unknown1" in "tryhackme's" home directory?**
```bash
file unknown1
```
- Answer: `ASCII text`

**How would we move the file "myfile" to the directory "myfolder"?**
- Answer: `mv myfile myfolder`

**What are the contents of this file?**
```bash
cat important
```
- Answer: `THM{FILESYSTEM}`

---

## Task 5: Permissions 101

### Understanding File Permissions

In Linux, every file and directory has permissions that control who can read, write, or execute them.

### Viewing Permissions

```bash
ls -l
```

Shows permissions like: `-rwxr-xr--`
- First character: file type (- for file, d for directory)
- Next 3: owner permissions (rwx = read, write, execute)
- Next 3: group permissions
- Last 3: other users' permissions

### Switching Users

**su** - Switch User
```bash
su username
```

### Questions

**On the deployable machine, who is the owner of "important"?**
```bash
ls -l important
```
- Answer: `user2`

**What would the command be to switch to the user "user2"?**
- Answer: `su user2`

**Now switch to this user "user2" using the password "user2"**
```bash
su user2
# Enter password: user2
```
- No answer needed

**Output the contents of "important", what is the flag?**
```bash
cat important
```
- Answer: `THM{SU_USER2}`

---

## Task 6: Common Directories

### Important Linux Directories

**/etc** - Configuration Files
- Stores system configuration files
- Examples: `passwd`, `shadow` files
- Like the system's library of essential settings

**/var** - Variable Data
- Data that changes frequently
- Logs and application data
- Like a notebook for program notes

**/root** - Root User's Home
- Home directory for the root (admin) user
- Not the same as `/` (root directory)
- Like the administrator's office

**/tmp** - Temporary Files
- Stores temporary data
- Cleared on system restart
- Like a whiteboard that gets erased

### Questions

**Read me!**
- No answer needed

**What is the directory path that we would expect logs to be stored in?**
- Answer: `/var/log`

**What root directory is similar to how RAM on a computer works?**
- Answer: `/tmp`

**Name the home directory of the root user**
- Answer: `/root`

**Now apply your learning and navigate through these directories on the deployed Linux machine**
- No answer needed - Explore!

---

## Task 7: Conclusions and Summaries

Great job completing part 2! Here's what you've learned:

✅ Connecting to Linux machines remotely using SSH
✅ Using flags and switches to enhance commands
✅ Using manual pages (`man`) to find help
✅ Creating, moving, and copying files
✅ Understanding file permissions
✅ Switching between users with `su`
✅ Navigating important Linux directories

**Tip:** Revisit this room to reinforce your understanding. Practice makes perfect!

**No answer needed**

---

## Task 8: Linux Fundamentals Part 3

Continue your journey with Linux Fundamentals Part 3!

**No answer needed** - Move on to the next part!

---

## Command Reference

| Command | Purpose | Example |
|---------|---------|---------|
| `ssh` | Connect to remote machine | `ssh user@ip` |
| `man` | View command manual | `man ls` |
| `touch` | Create empty file | `touch file.txt` |
| `file` | Check file type | `file document.txt` |
| `mv` | Move/rename file | `mv file.txt folder/` |
| `cp` | Copy file | `cp file.txt backup.txt` |
| `su` | Switch user | `su username` |
| `ls -l` | List with permissions | `ls -l` |

---

## Important Directories

| Directory | Purpose |
|-----------|---------|
| `/etc` | Configuration files |
| `/var` | Variable data and logs |
| `/root` | Root user's home |
| `/tmp` | Temporary files (cleared on reboot) |
| `/var/log` | System logs |

---

## Key Takeaways

- **SSH** lets you control remote Linux machines securely
- **Flags** modify command behavior (like `-h` for human-readable)
- **Manual pages** (`man`) are your friend for learning commands
- **File permissions** control who can access files
- **`su`** lets you switch between users
- **Important directories** each have specific purposes in Linux

Continue practicing these concepts and move on to Part 3! 🐧
