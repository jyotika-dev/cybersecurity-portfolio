# 🐧 LinuxJourney - Processes

This file summarizes my learning from **LinuxJourney** on Linux process management.  
[https://linuxjourney.com/lesson/processes](https://labex.io/lesson/monitor-processes-ps-command)
I practiced these on my local terminal and cybersecurity labs.

---

## 🔹 ps (Processes)
- `ps` → view running processes.
- `ps aux` → detailed info: PID, user, %CPU, %MEM, state, command.
- `ps -ef` → full format with parent-child hierarchy.

## 🔹 Controlling Terminal
- Each process may be linked to a **terminal** (TTY).
- Background daemons usually have **no terminal** (marked with `?` in `ps`).

## 🔹 Process Details
- **PID (Process ID):** unique number for each process.
- **PPID (Parent Process ID):** links child to parent.
- **UID/GID:** ownership of the process.
- **STAT:** process state (e.g., R = running, S = sleeping).

## 🔹 Process Creation
- `fork()` → creates a new process.
- `exec()` → replaces process memory with a new program.
- Shell creates child processes for commands.

## 🔹 Process Termination
- Processes end when:
  - Program finishes.
  - User/system sends termination signal.
- Exit codes (`echo $?`) → show process status (0 = success).

## 🔹 Signals
- Software interrupts sent to processes.
- Common signals:
  - `SIGTERM` (15): request process to terminate.
  - `SIGKILL` (9): force kill, cannot be caught.
  - `SIGSTOP`: pause process.
  - `SIGCONT`: resume process.

## 🔹 kill (Terminate)
- `kill -9 <PID>` → force terminate.
- `kill -15 <PID>` → graceful terminate.
- `pkill firefox` → kill by name.

## 🔹 Niceness
- `nice -n 10 command` → start with lower priority.
- `renice -n -5 -p <PID>` → change priority of running process.
- Lower nice value = higher priority.

## 🔹 Process States
- **R (Running)**
- **S (Sleeping)**
- **T (Stopped)**
- **Z (Zombie)** → process ended but parent didn't reap it.

## 🔹 /proc Filesystem
- Virtual FS with process info.
- `/proc/<PID>/status` → details about process.
- `/proc/cpuinfo`, `/proc/meminfo` → system info.

## 🔹 Job Control
- `command &` → run in background.
- `jobs` → list jobs in current shell.
- `fg %1` → bring job 1 to foreground.
- `bg %1` → resume job in background.
- `Ctrl+Z` → suspend process.

---

📌 **Applied Insight**:  
Understanding processes is key in cybersecurity:
- **SOC analysts** monitor rogue processes (`ps aux | grep netcat`).  
- Attackers often hide malware as **zombie/background processes**.  
- `/proc` analysis helps detect **rootkits or crypto-miners**.  
- Proper **signal handling** is critical during incident response.
