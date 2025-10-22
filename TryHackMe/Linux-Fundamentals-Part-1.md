# Linux Fundamentals Part 1 - TryHackMe

> Learn the basics of Linux and run essential commands on an interactive terminal

**Room Link:** https://tryhackme.com/room/linuxfundamentalspart1

<img width="1767" height="921" alt="image" src="https://github.com/user-attachments/assets/42554dab-272e-4595-9d63-13b04718db42" />

---

## Task 1: Introduction

Get ready to learn Linux fundamentals!

**No answer needed**

---

## Task 2: A Bit of Background on Linux

Linux is an open-source operating system that's widely used in servers, desktops, and various devices.

### Question

**What year was the first release of a Linux operating system?**
- Answer: `1991`

---

## Task 3: Interacting With Your First Linux Machine (In-Browser)

Deploy your first Linux machine using the in-browser terminal.

**No answer needed** - Just deploy the machine and get ready to interact with it!

---

## Task 4: Running Your First Few Commands

Time to run some basic Linux commands.

### Basic Commands

**echo** - Outputs text to the terminal
```bash
echo Hello World
```

**whoami** - Shows the current logged-in user

### Questions

**If we wanted to output the text "TryHackMe", what would our command be?**
- Answer: `echo TryHackMe`

**What is the username of who you're logged in as on your deployed Linux machine?**
- Answer: `tryhackme`

---

## Task 5: Interacting With the Filesystem!

Learn how to navigate and interact with files and directories in Linux.

### Useful Commands

**ls** - Lists files and directories in the current location
```bash
ls
```

**cd** - Changes directory
```bash
cd folder_name
```

**cat** - Displays the contents of a file
```bash
cat filename.txt
```

**pwd** - Shows the current working directory (Print Working Directory)
```bash
pwd
```

### Questions

**On the Linux machine that you deploy, how many folders are there?**
- Answer: `4`

**Which directory contains a file?**
- Answer: `folder4`

**What is the contents of this file?**
- Answer: `Hello World`

**Use the cd command to navigate to this file and find out the new current working directory. What is the path?**
```bash
cd folder4
pwd
```
- Answer: `/home/tryhackme/folder4`

---

## Task 6: Searching for Files

Learn how to search for files and text within files.

### Useful Commands

**find** - Searches for files in a directory
```bash
find -name filename.txt
```

**grep** - Searches for text within files
```bash
grep "search_term" filename.txt
```

### Questions

**Use grep on "access.log" to find the flag that has a prefix of "THM". What is the flag?**
```bash
grep "THM" access.log
```
- Answer: `THM{ACCESS}`

**And I still haven't found what I'm looking for!**
- No answer needed

---

## Task 7: An Introduction to Shell Operators

Shell operators help you chain commands together and control their behavior.

### Common Operators

**&** - Runs a command in the background
```bash
command &
```

**>** - Redirects output to a file (overwrites)
```bash
echo "text" > file.txt
```

**>>** - Redirects output to a file (appends)
```bash
echo "more text" >> file.txt
```

**&&** - Runs the second command only if the first succeeds
```bash
command1 && command2
```

### Questions

**If we wanted to run a command in the background, what operator would we want to use?**
- Answer: `&`

**If I wanted to replace the contents of a file named "passwords" with the word "password123", what would my command be?**
- Answer: `echo password123 > passwords`

**Now if I wanted to add "tryhackme" to this file named "passwords" but also keep "passwords123", what would my command be?**
- Answer: `echo tryhackme >> passwords`

**Now use the deployed Linux machine to put these into practice**
- No answer needed - Practice time!

---

## Task 8: Conclusions & Summaries

Great job! You've learned the Linux basics:
- Running basic commands
- Navigating the filesystem
- Searching for files and text
- Using shell operators

**No answer needed** - Take some time to explore and practice!

---

## Task 9: Linux Fundamentals Part 2

Ready to continue your Linux journey?

**Terminate the machine deployed in this room from task 3.**
- No answer needed

**Join Linux Fundamentals Part 2!**
- No answer needed - Continue to the next part!

---

## Summary - Commands Learned

| Command | Purpose | Example |
|---------|---------|---------|
| `echo` | Output text | `echo Hello` |
| `whoami` | Show current user | `whoami` |
| `ls` | List files/folders | `ls` |
| `cd` | Change directory | `cd folder` |
| `cat` | Display file contents | `cat file.txt` |
| `pwd` | Show current path | `pwd` |
| `find` | Search for files | `find -name file.txt` |
| `grep` | Search in files | `grep "text" file.txt` |

### Shell Operators

| Operator | Purpose | Example |
|----------|---------|---------|
| `&` | Run in background | `command &` |
| `>` | Overwrite file | `echo "text" > file` |
| `>>` | Append to file | `echo "text" >> file` |
| `&&` | Run if previous succeeds | `cmd1 && cmd2` |

---

## Key Takeaways

- Linux is an open-source operating system from 1991
- The terminal is a powerful way to interact with Linux
- File navigation uses `cd`, `ls`, and `pwd`
- File content can be viewed with `cat`
- Search using `find` for files and `grep` for text
- Shell operators help chain and control commands

Great start! Continue to Linux Fundamentals Part 2 to learn more! 🐧
