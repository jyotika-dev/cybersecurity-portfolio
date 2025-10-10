# Display User and Group Information - Challenge

Completed a Linux challenge focused on displaying user identity and group memberships using basic commands.

**Type:** Challenge (independent problem-solving)  
**Difficulty:** Beginner

---

## Challenge Overview

Learn to display user identity and group information using two essential Linux commands without step-by-step guidance.

---

## Task 1: Display Current User Identity

**Objective:** Show who you're currently logged in as.

**Solution:**
```bash
whoami
```

**Output:**
```
labex
```

Simple - just shows your username. That's it.

---

## Task 2: Display User and Group Information

**Objective:** Show detailed user ID and group memberships.

**Solution:**
```bash
id
```

**Output:**
```
uid=1000(labex) gid=1000(labex) groups=1000(labex),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lpadmin),126(sambashare)
```

**What it means:**
- `uid=1000(labex)` - User ID and username
- `gid=1000(labex)` - Primary group ID
- `groups=...` - All groups you belong to (includes sudo for admin access)

---

## Commands Used

| Command | Purpose |
|---------|---------|
| `whoami` | Display current username |
| `id` | Show user ID, group ID, and all group memberships |

---

## Key Takeaways

✅ `whoami` = quick username check  
✅ `id` = detailed user and group info  
✅ Group memberships control what you can do on the system  
✅ `sudo` group = admin privileges  

---

## Why This Matters

**User permissions in Linux:**
- Every action runs as a specific user
- Groups determine what files/commands you can access
- Understanding your identity = understanding your permissions

**Real-world use:**
- Check which account you're using
- Verify admin access (sudo group)
- Troubleshoot permission issues
- Audit user accounts

---

**Challenge complete!** Two simple commands that show exactly who you are and what you can do on a Linux system.
