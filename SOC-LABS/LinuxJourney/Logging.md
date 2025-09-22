# 🐧 LinuxJourney - Logging

This file summarizes my learning from **LinuxJourney** on Linux logging systems.  
https://linuxjourney.com/lesson/system-logging  
I practiced these on my local terminal and production environments.

---

## 🔹 System Logging
- **Centralized logging** → system events recorded in log files.
- **Log locations** → primarily in `/var/log/` directory.
- **Log levels** → Emergency, Alert, Critical, Error, Warning, Notice, Info, Debug.
- **Syslog daemon** → handles system-wide logging (rsyslog/syslog-ng).
- `ls -la /var/log/` → view available log files.
- Logs provide audit trail and troubleshooting information.

## 🔹 syslog
- **Syslog protocol** → standard for message logging.
- **rsyslog** → modern syslog implementation (most Linux systems).
- `/etc/rsyslog.conf` → main configuration file.
- **Facilities** → mail, daemon, kern, user, auth, syslog, etc.
- **Priority levels** → 0 (emergency) to 7 (debug).
- `logger "test message"` → send message to syslog.
- Remote logging capability for centralized log management.

## 🔹 General Logging
- `/var/log/messages` → general system messages (RHEL/CentOS).
- `/var/log/syslog` → general system messages (Debian/Ubuntu).
- `/var/log/dmesg` → kernel ring buffer messages.
- `/var/log/boot.log` → system boot messages.
- `journalctl` → query systemd journal logs.
- `tail -f /var/log/messages` → monitor logs in real-time.

## 🔹 Kernel Logging
- **dmesg** → display kernel messages from boot.
- `dmesg | grep error` → filter kernel error messages.
- `dmesg -w` → follow kernel messages in real-time.
- `/proc/kmsg` → kernel message interface.
- **Kernel ring buffer** → circular buffer for kernel messages.
- `journalctl -k` → show kernel messages via systemd.

## 🔹 Authentication Logging
- `/var/log/auth.log` → authentication events (Debian/Ubuntu).
- `/var/log/secure` → authentication events (RHEL/CentOS).
- **SSH logins** → successful and failed login attempts.
- **sudo usage** → privilege escalation events.
- `last` → show recent user logins.
- `lastlog` → show last login for each user.
- `who` → currently logged-in users.

## 🔹 Managing Log Files
- **Log rotation** → prevents logs from consuming disk space.
- `logrotate` → utility for automatic log rotation.
- `/etc/logrotate.conf` → main logrotate configuration.
- `/etc/logrotate.d/` → service-specific rotation configs.
- **Compression** → old logs compressed with gzip.
- `logrotate -f /etc/logrotate.conf` → force log rotation.
- **Retention policies** → specify how long to keep logs.

---

📌 **Applied Insight**:  
Logging is fundamental to cybersecurity operations:
- **SIEM integration**: Logs feed Security Information and Event Management systems.  
- **Incident response**: Authentication logs reveal unauthorized access attempts.  
- **Forensic analysis**: System logs provide timeline of events during breaches.  
- **Threat detection**: Anomalous patterns in logs indicate potential attacks.  
- **Compliance**: Log retention and management meet regulatory requirements (PCI DSS, SOX, HIPAA).
