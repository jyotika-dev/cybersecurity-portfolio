# Linux Fundamentals Part 3 - TryHackMe

> The finale of the Linux Fundamentals series - learn about text editors, file transfers, processes, automation, and system maintenance

**Room Link:** https://tryhackme.com/room/linuxfundamentalspart3

<img width="1749" height="927" alt="image" src="https://github.com/user-attachments/assets/93eb2e60-2845-4c73-8157-2daa013dea73" />

---

## Task 1: Introduction

Welcome to the final part of Linux Fundamentals! This room covers useful utilities and applications you'll use day-to-day, plus advancing your skills with automation, package management, and logging.

**What you'll learn:**
- Terminal text editors (Nano and VIM)
- Downloading and transferring files
- Managing processes
- Automation with cron jobs
- Package management
- System logging

**No answer needed**

---

## Task 2: Deploy Your Linux Machine

Deploy the machine and connect via SSH:

```bash
ssh tryhackme@MACHINE_IP
```

**No answer needed**

---

## Task 3: Terminal Text Editors

### Nano - The Beginner-Friendly Editor

**Creating/Editing a file:**
```bash
nano filename
```

**Key shortcuts:**
- `Ctrl + X` - Exit
- `Ctrl + O` - Save (Write Out)
- `Ctrl + K` - Cut line
- `Ctrl + U` - Paste
- `Ctrl + W` - Search

**Features:**
- Easy to learn
- Simple navigation
- Search and replace
- Line numbers
- Perfect for quick edits

### VIM - The Advanced Editor

**Opening a file:**
```bash
vim filename
```

**Why VIM?**
- Highly customizable
- Syntax highlighting for code
- Works on all terminals
- Powerful once you learn it
- Tons of resources available

**Note:** VIM has a steeper learning curve but is worth learning for power users.

### Questions

**Edit "task3" located in "tryhackme"'s home directory using Nano. What is the flag?**
```bash
nano task3
```
- Answer: `THM{TEXT_EDITORS}`

---

## Task 4: General/Useful Utilities

### Downloading Files with wget

Download files from the web via HTTP:

```bash
wget https://example.com/file.txt
```

**Example:**
```bash
wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt
```

### Transferring Files with SCP

SCP (Secure Copy) transfers files between computers using SSH.

**Copy FROM your machine TO a remote machine:**
```bash
scp local_file.txt user@remote_ip:/remote/path/
```

**Example:**
```bash
scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt
```

**Copy FROM a remote machine TO your machine:**
```bash
scp user@remote_ip:/remote/path/file.txt local_name.txt
```

**Example:**
```bash
scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt
```

### Serving Files with Python Web Server

Turn your machine into a quick web server:

```bash
python3 -m http.server
```

This serves files from your current directory on port 8000 by default.

**Downloading from the web server:**
```bash
wget http://SERVER_IP:8000/filename
```

### Questions

**Download the file http://MACHINE_IP:8000/.flag.txt onto the TryHackMe AttackBox. What are the contents?**
```bash
wget http://MACHINE_IP:8000/.flag.txt
cat .flag.txt
```
- Answer: `THM{WGET_WEBSERVER}`

**Create and download files to further apply your learning**
- Use `Ctrl + C` to stop the Python HTTP server when finished

---

## Task 5: Processes 101

### What are Processes?

Processes are programs running on your machine, managed by the kernel. Each has a unique PID (Process ID).

### Viewing Processes

**Show your current processes:**
```bash
ps
```

**Show all processes (including other users):**
```bash
ps aux
```

**Real-time process monitoring:**
```bash
top
```
- Updates every 10 seconds
- Shows CPU/memory usage
- Use arrow keys to navigate
- Press `q` to quit

### Managing Processes

**Kill a process:**
```bash
kill PID
```

**Kill signals:**
- `SIGTERM` - Gracefully kill (allows cleanup)
- `SIGKILL` - Force kill (no cleanup)
- `SIGSTOP` - Pause/suspend

**Example:**
```bash
kill 1337
```

### How Processes Start

- **PID 0** - Started at boot
- **systemd** - Init system that manages user processes
- All programs start as child processes of systemd

### Starting Services on Boot

**Start a service:**
```bash
systemctl start service_name
```

**Stop a service:**
```bash
systemctl stop service_name
```

**Enable service on boot:**
```bash
systemctl enable service_name
```

**Disable service on boot:**
```bash
systemctl disable service_name
```

### Backgrounding and Foregrounding

**Run command in background:**
```bash
command &
```

**Background current process:**
- Press `Ctrl + Z`

**Bring process to foreground:**
```bash
fg
```

### Questions

**If we launched a process where the previous ID was "300", what would the ID of this new process be?**
- Answer: `301`

**If we wanted to cleanly kill a process, what signal would we send it?**
- Answer: `SIGTERM`

**Locate the process running on the deployed instance. What flag is given?**
```bash
ps aux
```
- Answer: `THM{PROCESSES}`

**What command would we use to stop the service "myservice"?**
- Answer: `systemctl stop myservice`

**What command would we use to start the same service on boot-up?**
- Answer: `systemctl enable myservice`

**What command would we use to bring a previously backgrounded process back to the foreground?**
- Answer: `fg`

---

## Task 6: Maintaining Your System - Automation

### Cron Jobs for Automation

Cron runs scheduled tasks automatically. Tasks are defined in crontabs.

### Crontab Format

```
MIN HOUR DOM MON DOW CMD
```

| Field | Description | Values |
|-------|-------------|--------|
| MIN | Minute | 0-59 |
| HOUR | Hour | 0-23 |
| DOM | Day of Month | 1-31 |
| MON | Month | 1-12 |
| DOW | Day of Week | 0-7 (0 and 7 = Sunday) |
| CMD | Command to run | Any command |

### Crontab Examples

**Backup every 12 hours:**
```
0 */12 * * * cp -R /home/user/Documents /var/backups/
```

**Run at reboot:**
```
@reboot /path/to/script.sh
```

**Wildcard (*) means "any"** - Don't care about that specific field

### Editing Crontab

```bash
crontab -e
```

**Useful Resources:**
- [Crontab Generator](https://crontab-generator.org/)
- [Cron Guru](https://crontab.guru/)

### Questions

**When will the crontab on the deployed instance run?**
- Answer: `@reboot`

---

## Task 7: Maintaining Your System - Package Management

### What are Packages?

Packages are software submitted by developers to repositories (repos). Ubuntu uses `apt` to manage packages.

### Managing Repositories

**Update package list:**
```bash
sudo apt update
```

**Install software:**
```bash
sudo apt install package_name
```

**Remove software:**
```bash
sudo apt remove package_name
```

### Adding Third-Party Repositories

**1. Add GPG key (for trust):**
```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
```

**2. Add repository:**
```bash
sudo add-apt-repository ppa:repository_name
```

**3. Update package list:**
```bash
sudo apt update
```

**4. Install the software:**
```bash
sudo apt install sublime-text
```

### Removing Repositories

```bash
sudo add-apt-repository --remove ppa:repository_name
```

Or manually delete the file from `/etc/apt/sources.list.d/`

**No specific questions for this task**

---

## Task 8: Maintaining Your System - Logs

### Where are Logs?

Logs are stored in `/var/log/`

### Common Log Files

- **Apache web server:** `/var/log/apache2/`
- **Authentication:** `/var/log/auth.log`
- **System logs:** `/var/log/syslog`
- **Firewall (UFW):** `/var/log/ufw.log`

### Important Log Types

**Access logs** - Who accessed what (for web servers)
**Error logs** - What went wrong

### Viewing Logs

```bash
cat /var/log/apache2/access.log
```

Logs help with:
- Monitoring system health
- Diagnosing performance issues
- Investigating security incidents
- Tracking user activity

### Questions

**What is the IP address of the user who visited the site?**
```bash
cat /var/log/apache2/access.log
```
- Answer: `10.9.232.111`

**What file did they access?**
- Answer: `catsanddogs.jpg`

---

## Task 9: Conclusions & Summaries

Congratulations on completing the Linux Fundamentals series! 🎉

### What You've Learned

✅ Terminal text editors (Nano and VIM)
✅ Downloading files with wget
✅ Transferring files with SCP
✅ Serving files with Python HTTP server
✅ Managing processes with ps, top, and kill
✅ Backgrounding and foregrounding processes
✅ Automating tasks with cron jobs
✅ Managing packages with apt
✅ Reading system logs

### Continue Learning

Check out these TryHackMe rooms:
- **The Find Command** - Advanced file searching
- **Bash Scripting** - Automate tasks with scripts
- **Regular Expressions** - Pattern matching

**No answer needed**

---

## Command Reference

| Command | Purpose |
|---------|---------|
| `nano file` | Edit file with Nano |
| `vim file` | Edit file with VIM |
| `wget URL` | Download file from web |
| `scp source dest` | Securely copy files |
| `python3 -m http.server` | Start web server |
| `ps` | Show processes |
| `ps aux` | Show all processes |
| `top` | Real-time process view |
| `kill PID` | Kill process |
| `systemctl start service` | Start service |
| `systemctl stop service` | Stop service |
| `systemctl enable service` | Enable on boot |
| `fg` | Foreground process |
| `Ctrl + Z` | Background process |
| `command &` | Run in background |
| `crontab -e` | Edit cron jobs |
| `apt update` | Update package list |
| `apt install pkg` | Install package |
| `apt remove pkg` | Remove package |

---

## Key Takeaways

- **Nano** is beginner-friendly, **VIM** is powerful
- **wget** downloads, **SCP** transfers securely
- **Processes** have PIDs and can be managed with signals
- **Cron jobs** automate repetitive tasks
- **apt** manages software packages
- **Logs** in `/var/log/` help monitor and troubleshoot

Great job completing the Linux Fundamentals series! 🐧
