# Linux Basics - First Hands-On Lab

My first hands-on Linux lab covering terminal basics, user management, and system monitoring.

**Difficulty:** Beginner | **Completion:** ✅

---

## What I Learned

### 1. Checking Current User

```bash
whoami
```

Output: `labex`

Simple command to see who you're logged in as - useful when switching between accounts.

---

### 2. User and Group Information

```bash
id
```

Output:
```
uid=5000(labex) gid=5000(labex) groups=5000(labex),27(sudo),121(ssl-cert),5002(public)
```

Shows your user ID, group ID, and all group memberships. You can check other users too:

```bash
id root
```

Output: `uid=0(root) gid=0(root) groups=0(root)`

Root is UID 0 - the superuser with full system access.

---

### 3. Installing Software with apt

```bash
# Update package lists
sudo apt update

# Install htop
sudo apt install htop
```

- `sudo` = run with admin privileges
- `apt` = package manager for Debian/Ubuntu
- Always update before installing

---

### 4. Monitoring System with htop

```bash
htop
```

Interactive system monitor showing:
- CPU usage per core
- Memory (RAM) usage
- Running processes
- Resource consumption

**Navigation:**
- Arrow keys to move around
- Press `q` to quit
- `F9` to kill a process

---

## Commands Summary

| Command | What it does |
|---------|--------------|
| `whoami` | Show current username |
| `id` | Display user/group IDs |
| `sudo apt update` | Refresh package lists |
| `sudo apt install <package>` | Install software |
| `htop` | System monitor |

---

## Key Takeaways

✅ Terminal is powerful for system administration  
✅ sudo gives temporary admin privileges  
✅ apt makes installing software easy  
✅ htop shows what's running on your system  
✅ Every user has a UID, root is UID 0  


---

**Lab complete!** Basic Linux commands down, ready for more advanced stuff.
