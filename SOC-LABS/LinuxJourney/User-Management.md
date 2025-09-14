# Linux User Management
https://labex.io/lesson/users-and-groups

## 1. Users and Groups

* **Users**: Individual accounts on the system, each with a unique **UID** (User ID).
* **Groups**: Collections of users, identified by a **GID** (Group ID). Groups help assign permissions collectively.

To check your current user:
```bash
whoami
id
```

To list all users and groups:
```bash
cat /etc/passwd
cat /etc/group
```

## 2. root

The **root** user is the superuser with full administrative privileges on the system.
* Its **UID is 0**.
* It can override file permissions and run system-level commands.
* Use `sudo` to execute commands with root privileges safely.

```bash
sudo ls /root
```

## 3. /etc/passwd

This file stores basic user account information.

**Format**:
```
username:x:UID:GID:comment:home_directory:shell
```

**Example entry**:
```
jyotika:x:1000:1000:Jyotika:/home/jyotika:/bin/bash
```

## 4. /etc/shadow

This file stores hashed passwords for users.
* It's readable only by the **root** user for security.
* Each entry includes the username, hashed password, and password aging information.

```bash
sudo cat /etc/shadow
```

## 5. /etc/group

This file defines group information.

**Format**:
```
group_name:x:GID:members
```

**Example**:
```
developers:x:1001:jyotika,rahul
```

## 6. User Management Tools

* `adduser` / `useradd`: Create new users.
* `passwd`: Set or change a user's password.
* `usermod`: Modify user properties.
* `deluser` / `userdel`: Delete users.
* `groupadd` / `groupdel`: Add or remove groups.
* `groups`: Show the groups a user belongs to.

**Examples**:
```bash
sudo adduser analyst
sudo passwd analyst
sudo usermod -aG sudo analyst
groups analyst
```

## 🔑Cybersecurity Applications

Misconfigured accounts (e.g., weak passwords, users with unnecessary sudo privileges) can lead to **privilege escalation**.

Cybersecurity professionals, like SOC analysts, often review `/etc/passwd`, `/etc/shadow`, and `/etc/group` for:
* Suspicious accounts
* Default or unused accounts
* Improper permissions

---
