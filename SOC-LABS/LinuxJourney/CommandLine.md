# 🐧 LinuxJourney - Command Line


This file summarizes my learning from **LinuxJourney** on basic command-line utilities.  
https://labex.io/lesson/the-shell
I practiced these on Bandit wargame (OverTheWire) and my local terminal.

---

## 🔹 Navigation & Files
- `pwd` → print working directory.
- `cd` → change directory (`cd ..` goes back).
- `ls -al` → list with details.
- `touch file.txt` → create empty file.
- `file filename` → check file type.

## 🔹 Viewing Files
- `cat file.txt` → print content.
- `less file.txt` → scroll through file.
- `history` → see past commands.

## 🔹 File Operations
- `cp file1 file2` → copy file.
- `mv old new` → move/rename file.
- `mkdir testdir` → create directory.
- `rm file.txt` → delete file.

## 🔹 Search & Help
- `find . -name "*.txt"` → find files.
- `man command` → manual page.
- `whatis command` → one-line explanation.
- `alias ll="ls -la"` → create shortcut.

---

📌 **Applied Insight**:  
These may seem like basics, but in SOC work:
- `grep`, `find`, `less` are used daily for **log analysis**.  
- Aliases improve speed during **incident investigations**.  
- Understanding `stdin/stdout/stderr` helps when redirecting logs into files or pipelines.  
