# 🐧 LinuxJourney - Permissions

This file summarizes my learning from **LinuxJourney** on Linux permission system.  
[https://linuxjourney.com/lesson/file-permissions](https://labex.io/lesson/file-permissions)  
I practiced these on local terminal and cybersecurity labs.

---

## 🔹 File Permissions
- `ls -l` → view file permissions and ownership.
- **Permission format**: `rwxrwxrwx` (owner, group, others).
- `r=4`, `w=2`, `x=1` → numeric permission values.
- Example: `-rwxr-xr--` → owner: rwx, group: r-x, others: r--.

## 🔹 Modifying Permissions
- `chmod 755 script.sh` → set permissions numerically.
- `chmod u+x file.txt` → add execute for owner.
- `chmod g-w file.txt` → remove write for group.
- `chmod 644 file.txt` → common file permission (rw-r--r--).

## 🔹 Ownership Permissions
- `chown user file.txt` → change file owner.
- `chown user:group file.txt` → change owner and group.
- `ls -l` → check current ownership.

## 🔹 Umask
- `umask` → check default permission mask.
- `umask 022` → files: 644, directories: 755.
- `umask 077` → files: 600, directories: 700.

## 🔹 Special Permissions
- `chmod u+s file` → set **setuid** bit (runs as file owner).
- `chmod g+s dir` → set **setgid** bit (inherits group).
- `chmod +t /shared` → set **sticky bit** (only owner can delete).
- Symbols: `s` for setuid/setgid, `t` for sticky bit.

## 🔹 Process Permissions
- `ps aux` → view process ownership.
- Processes inherit permissions from the user who started them.
- Root processes run with elevated privileges.

---

📌 **Applied Insight**:  
These permissions are crucial in cybersecurity:
- **SOC analysts** audit `/etc/passwd`, `/etc/shadow` for misconfigurations.  
- **Sticky bit** on `/tmp` prevents users from deleting others' files.  
- **Setuid/Setgid** programs are monitored - attackers exploit them for **privilege escalation**.  
- Commands like `find / -perm /6000` help identify potential security risks.
