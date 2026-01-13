# TryHackMe Linux System Hardening - Walkthrough

> Secure Your Linux System

**Room Link:** https://tryhackme.com/room/linuxsystemhardening

<img width="1294" height="907" alt="image" src="https://github.com/user-attachments/assets/1b75e986-b381-4a4f-8340-127482028fcd" />


---

## Overview

**What is hardening:**
- Securing a system
- Reducing attack surface
- Implementing best practices
- Ongoing maintenance

---

## Task 2: Physical Security (GRUB)

### Q1: Command to create password for GRUB bootloader?

**GRUB password:**
- Protects boot parameters
- Prevents physical tampering
- First line of defense

**Answer:** `grub2-mkpasswd-pbkdf2`

### Q2: What does PBKDF2 stand for?

**Answer:** `Password-Based Key Derivation Function 2`

---

## Task 3: Filesystem Encryption

### Q1: What does LUKS stand for?

**LUKS:**
- Encrypts drives/partitions
- Protects data at rest
- Standard Linux encryption

**Answer:** `Linux Unified Key Setup`

### Q2: Flag in the secret vault?

**Steps:**
1. Use `cryptsetup` to decrypt LUKS
2. Mount encrypted volume
3. Read flag file

**Answer:** `THM{LUKS_not_LUX}`

---

## Task 4: Firewall

### Q1: Allowed TCP port besides 22?

**Check firewall rules:**
```bash
sudo iptables -L
```

**Answer:** `12526`

### Q2: Allowed UDP port?

**Answer:** `14298`

---

## Task 5: Securing SSH

### Q: Flag in sshd_config file?

**Check SSH config:**
```bash
cat /etc/ssh/sshd_config
```

**Answer:** `THM{secure_SEA_shell}`

---

## Task 6: User Management

### Q1: Shell value to disable account?

**Disable account:**
- Edit `/etc/passwd`
- Change shell to prevent login

**Answer:** `/sbin/nologin`

### Q2: RedHat/Fedora sudoers group name?

**Answer:** `wheel`

### Q3: Debian/Ubuntu sudoers group name?

**Answer:** `sudo`

### Q4: Besides tryhackme and ubuntu, which user has sudo access?

**Check sudoers:**
```bash
getent group sudo
```

**Answer:** `blacksmith`

---

## Task 7: Secure Services

### Q: Secure replacement for TFTP and FTP (besides FTPS)?

**Secure alternatives:**
- SFTP (SSH File Transfer Protocol)
- Better than plain FTP
- Encrypted transfer

**Answer:** `SFTP`

---

## Task 8: Updates

### Q1: Command to update older Red Hat system?

**Answer:** `yum update`

### Q2: Command to update modern Fedora system?

**Answer:** `dnf update`

### Q3: Two commands to update Debian system?

**Answer:** `apt update && apt upgrade`

### Q4: What does yum stand for?

**Answer:** `Yellowdog Updater, Modified`

### Q5: What does dnf stand for?

**Answer:** `Dandified Yum`

### Q6: Flag in sources.list file?

**Check sources:**
```bash
cat /etc/apt/sources.list
```

**Answer:** `THM{not_Advanced_Persistent_Threat}`

---

## Task 9: Logging

### Q1: Command to display last 15 lines of kern.log?

**Answer:** `tail 15 kern.log`

### Q2: Command to display lines containing "denied" in secure file?

**Answer:** `grep denied secure`

---

## Quick Reference

### System Hardening Checklist

**Physical security:**
- [ ] Set GRUB password
- [ ] Encrypt filesystems
- [ ] Secure BIOS/UEFI

**Network security:**
- [ ] Configure firewall
- [ ] Harden SSH
- [ ] Close unused ports

**User management:**
- [ ] Disable unused accounts
- [ ] Strong passwords
- [ ] Limit sudo access
- [ ] Principle of least privilege

**System maintenance:**
- [ ] Remove unnecessary software
- [ ] Keep system updated
- [ ] Monitor logs
- [ ] Regular audits

---

## Essential Commands

### Firewall

```bash
# Check iptables rules
sudo iptables -L

# List all rules with numbers
sudo iptables -L --line-numbers

# Check firewalld status
sudo firewall-cmd --list-all
```

### SSH Hardening

```bash
# Edit SSH config
sudo nano /etc/ssh/sshd_config

# Restart SSH service
sudo systemctl restart sshd

# Key settings:
# PermitRootLogin no
# Port 2222 (change default)
# AllowUsers username
```

### User Management

```bash
# Check sudo group
getent group sudo
getent group wheel

# Disable user
sudo usermod -s /sbin/nologin username

# Lock account
sudo passwd -l username

# Check user details
id username
```

### Package Updates

```bash
# Debian/Ubuntu
sudo apt update
sudo apt upgrade
sudo apt full-upgrade

# Red Hat/CentOS (old)
sudo yum update

# Fedora/RHEL (new)
sudo dnf update

# Check updates available
apt list --upgradable
```

### Log Monitoring

```bash
# View last lines
tail -n 20 /var/log/syslog

# Follow log in real-time
tail -f /var/log/auth.log

# Search logs
grep "Failed" /var/log/auth.log

# Common log files
/var/log/syslog      # System messages
/var/log/auth.log    # Authentication
/var/log/kern.log    # Kernel messages
/var/log/secure      # Security (RHEL)
```

---

## Security Best Practices

### GRUB Protection

**Why:**
- Prevents boot parameter tampering
- Physical security layer
- Stops single-user mode access

**Implementation:**
```bash
grub2-mkpasswd-pbkdf2
# Add to /etc/grub.d/40_custom
# Update GRUB config
```

### LUKS Encryption

**Why:**
- Protects data at rest
- Essential for laptops
- Compliance requirement

**Usage:**
```bash
# Decrypt
cryptsetup luksOpen device name

# Mount
mount /dev/mapper/name /mnt

# Unmount and close
umount /mnt
cryptsetup luksClose name
```

### Firewall Rules

**Default policy:**
- Deny all incoming
- Allow all outgoing
- Explicitly allow needed ports

**Basic rules:**
```bash
# Allow SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Drop everything else
sudo iptables -P INPUT DROP
```

---

## Common Issues

### SSH Lockout

**Prevention:**
- Test config before restart
- Keep backup session open
- Use console access

**Recovery:**
- Boot to single-user mode
- Mount filesystem
- Fix sshd_config

### Update Problems

**Issues:**
- Broken dependencies
- Held packages
- Repository errors

**Solutions:**
```bash
# Fix broken packages
sudo apt --fix-broken install

# Clean cache
sudo apt clean

# Force update
sudo apt dist-upgrade
```

---

## Key Takeaways

- **Physical security** matters (GRUB password)
- **Encryption** protects data at rest (LUKS)
- **Firewall** controls network access
- **SSH hardening** secures remote access
- **User management** limits privileges
- **Updates** patch vulnerabilities
- **Logging** enables monitoring
- **Regular audits** maintain security
- **Remove unused** software/services
- **Defense in depth** approach

---

Happy hardening!
