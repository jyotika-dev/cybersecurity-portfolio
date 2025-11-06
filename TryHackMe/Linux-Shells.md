# Linux Shells - TryHackMe

> Learn about scripting and the different types of Linux shells

**Room Link:** https://tryhackme.com/room/linuxshells

<img width="1581" height="925" alt="image" src="https://github.com/user-attachments/assets/90e6d624-fdb7-4095-a989-c38c8c2cf66b" />

---

## Task 1: Introduction to Linux Shells

### What is a Shell?

While we normally use the Graphical User Interface (GUI) for everyday tasks, the Command Line Interface (CLI) offers a more efficient and resource-friendly way to interact with your operating system. Shells provide powerful features for commands you write in the CLI.

### Learning Objectives

- Learn interaction with Linux shell
- Use basic shell commands
- Explore the types of Linux shells available
- Write some shell scripts

### Questions

**Who is the facilitator between the user and the OS?**
- Answer: `Shell`

---

## Task 2: How To Interact With a Shell?

### Understanding the Prompt

When you open the terminal, you'll see the shell prompt:

```bash
user@ubuntu:~$
```

Most Linux distributions use **Bash (Bourne Again Shell)** as their default shell.

### Basic Commands

**Check current directory:**
```bash
user@ubuntu:~$ pwd
/home/user
```

**Change directory:**
```bash
user@ubuntu:~$ cd Desktop
user@ubuntu:~/Desktop$
```

**List directory contents:**
```bash
user@ubuntu:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

**Read file contents:**
```bash
user@ubuntu:~$ cat filename.txt
this is a sample file
this is the second line of the file
```

**Search for text in file:**
```bash
user@ubuntu:~$ grep THM dictionary.txt
The flag is THM
```

The `grep` command is powerful for searching specific words or patterns inside files.

### Questions

**What is the default shell in most Linux distributions?**
- Answer: `Bash`

**Which command utility is used to list down the contents of a directory?**
- Answer: `ls`

**Which command utility can help you search for anything in a file?**
- Answer: `grep`

---

## Task 3: Types of Linux Shells

### Check Current Shell

```bash
user@ubuntu:~$ echo $SHELL
/bin/bash
```

### List Available Shells

```bash
user@ubuntu:~$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/tmux
/usr/bin/screen
/bin/zsh
/usr/bin/zsh
```

### Switch Between Shells

```bash
user@ubuntu:~$ zsh
user@ubuntu ~ $
```

### Permanently Change Default Shell

```bash
chsh -s /usr/bin/zsh
```

### Popular Shells

**Bourne Again Shell (Bash)**
- Default in most Linux distributions
- Powerful scripting capabilities
- Command history and tab completion

**Friendly Interactive Shell (Fish)**
- Auto spell correction
- Customizable themes
- Syntax highlighting

**Z Shell (Zsh)**
- Extensive customization options
- Advanced tab completion
- May run slightly slower due to features

### Questions

**Which shell comes with syntax highlighting as an out-of-the-box feature?**
- Answer: `Fish`

**Which shell does not have auto spell correction?**
- Answer: `Bash`

**Which command displays all the previously executed commands of the current session?**
- Answer: `history`

---

## Task 4: Shell Scripting and Components

### Basic Script Components

**Shebang (#!)** - Defines the interpreter (e.g., `#!/bin/bash`)
**Variables** - Store values for reuse
**Loops** - Execute commands repeatedly
**Conditional Statements** - Execute code based on conditions

### Variables

Create a script to take user input and display a welcome message:

```bash
#!/bin/bash
echo "Hey, what's your name?"
read name
echo "Welcome, $name"
```

**Give execution permissions:**
```bash
user@ubuntu:~$ chmod +x variable_script.sh
```

**Execute the script:**
```bash
user@ubuntu:~$ ./variable_script.sh
Hey, What's your name?
John
Welcome, John
```

### Loops

Create a script to display numbers 1 to 10:

```bash
#!/bin/bash
for i in {1..10};
do
    echo $i
done
```

**Execute the script:**
```bash
user@ubuntu:~$ ./loop_script.sh
1
2
3
...
10
```

### Conditional Statements

Create a script that checks authorization:

```bash
#!/bin/bash
echo "Please enter your name first:"
read name
if [ "$name" = "Stewart" ]; then
    echo "Welcome Stewart! Here is the secret: THM_Script"
else
    echo "Sorry! You are not authorized to access the secret."
fi
```

**Authorized user:**
```bash
user@ubuntu:~$ ./conditional_script.sh
Please enter your name first:
Stewart
Welcome, Stewart! Here is the secret: THM_Script
```

**Unauthorized user:**
```bash
user@ubuntu:~$ ./conditional_script.sh
Please enter your name first:
Alex
Sorry! You are not authorized to access the secret.
```

### Comments

Adding comments makes your code readable:

```bash
#!/bin/bash
# Asking the user to enter a value.
echo "Please enter your name first:"
# Storing the user input value in a variable.
read name
# Checking if the name equals our required name.
if [ "$name" = "Stewart" ]; then
    # If it equals, display welcome message
    echo "Welcome Stewart! Here is the secret: THM_Script"
else
    echo "Sorry! You are not authorized to access the secret."
fi
```

### Questions

**What is the shebang used in a Bash script?**
- Answer: `#!/bin/bash`

**Which command gives executable permissions to a script?**
- Answer: `chmod +x`

**Which scripting functionality helps us configure iterative tasks?**
- Answer: `Loops`

---

## Task 5: The Locker Script

### Requirement

Create a script that verifies user credentials before opening a locker:

**Correct Details:**
- Username: John
- Company name: Tryhackme
- PIN: 7385

### Complete Script

```bash
#!/bin/bash 
# Defining the variables
username=""
companyname=""
pin=""

# Defining the loop
for i in {1..3}; do
    # Defining the conditional statements
    if [ "$i" -eq 1 ]; then
        echo "Enter your Username:"
        read username
    elif [ "$i" -eq 2 ]; then
        echo "Enter your Company name:"
        read companyname
    else
        echo "Enter your PIN:"
        read pin
    fi
done

# Checking if the user entered the correct details
if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
    echo "Authentication Successful. You can now access your locker, John."
else
    echo "Authentication Denied!!"
fi
```

### Script Execution

**Incorrect credentials:**
```bash
user@ubuntu:~$ ./locker_script.sh
Enter your Username:
John
Enter your Company name:
Tryhackme
Enter your PIN:
1349
Authentication Denied!!
```

**Correct credentials:**
```bash
user@ubuntu:~$ ./locker_script.sh
Enter your Username:
John
Enter your Company name:
Tryhackme
Enter your PIN:
7385
Authentication Successful. You can now access your locker, John.
```

### Questions

**What would be the correct PIN to authenticate in the locker script?**
- Answer: `7385`

---

## Task 6: Practical Exercise

### Objective

Find a flag in log files using a shell script.

### Setup

**Become root user:**
```bash
user@ubuntu:~$ sudo su
[sudo] password for user: user@Tryhackme
root@tryhackme:/home/user#
```

### Script Details

- **Flag:** thm-flag01-script
- **Directory:** /var/log
- **Hint:** Look for empty double quotes `""` and fill them

### Execution

```bash
./flag_hunt.sh
```

### Finding Additional Information

```bash
grep "cat" /var/log/authentication.log
```

### Questions

**Which file has the keyword?**
- Answer: `authentication.log`

**Where is the cat sleeping?**
- Answer: `under the table`

---

## Task 7: Conclusion

**No answer needed**

---

## Quick Reference

### Basic Commands

| Command | Purpose |
|---------|---------|
| `pwd` | Print working directory |
| `cd [directory]` | Change directory |
| `ls` | List directory contents |
| `cat [file]` | Display file contents |
| `grep [pattern] [file]` | Search for pattern in file |
| `echo $SHELL` | Show current shell |
| `history` | Show command history |

### Shell Script Components

| Component | Syntax | Purpose |
|-----------|--------|---------|
| Shebang | `#!/bin/bash` | Define interpreter |
| Variable | `name="value"` | Store data |
| Input | `read variable` | Get user input |
| Output | `echo "text"` | Display text |
| If statement | `if [ condition ]; then ... fi` | Conditional execution |
| For loop | `for i in {1..10}; do ... done` | Iterate |
| Comments | `# comment` | Add notes |

### File Permissions

| Command | Purpose |
|---------|---------|
| `chmod +x script.sh` | Make script executable |
| `./script.sh` | Execute script |
| `sudo su` | Become root user |

### Popular Shells

| Shell | Key Features |
|-------|-------------|
| **Bash** | Default shell, powerful scripting, command history |
| **Fish** | Auto spell correction, syntax highlighting, themes |
| **Zsh** | Extensive customization, advanced completion |

---

## Key Takeaways

- **CLI is more efficient** than GUI for many tasks
- **Bash** is the default shell in most Linux distributions
- **grep** is powerful for searching patterns in files
- **Variables** store values for reuse in scripts
- **Loops** automate repetitive tasks
- **Conditional statements** control script flow
- **chmod +x** makes scripts executable
- **Comments** make code readable and maintainable
- Always use **shebang** (`#!/bin/bash`) at the start of scripts
