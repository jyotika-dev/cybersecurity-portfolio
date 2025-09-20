# 🐧 LinuxJourney - Init

This file summarizes my learning from **LinuxJourney** on Linux init systems.  
https://labex.io/lesson/sysv-overview  
I practiced these on my local terminal and various Linux distributions.

---

## 🔹 System V Overview
- **SysV Init** → traditional Unix init system (oldest).
- **PID 1** → first process started by kernel after boot.
- **Runlevels** → different system states (0-6).
- **Sequential startup** → services start one after another.
- `/etc/init.d/` → service scripts directory.
- `ps -p 1` → check if system uses SysV init.

## 🔹 System V Service
- **Service management** → `/etc/init.d/servicename {start|stop|restart}`.
- **Runlevel control** → `update-rc.d servicename defaults`.
- **Service status** → `service servicename status`.
- **Runlevel directories** → `/etc/rc0.d/` to `/etc/rc6.d/`.
- **S/K prefixes** → Start/Kill scripts with priority numbers.
- `runlevel` → show current and previous runlevel.

## 🔹 Upstart Overview
- **Event-driven** → modern replacement for SysV (Ubuntu 6.10-14.10).
- **Parallel startup** → faster boot times than SysV.
- **Event-based** → services start based on system events.
- **Job configuration** → `/etc/init/` directory.
- **Backward compatibility** → can run SysV scripts.
- `initctl` → control upstart jobs and services.

## 🔹 Upstart Jobs
- **Job definitions** → `.conf` files in `/etc/init/`.
- `initctl start jobname` → start upstart job.
- `initctl stop jobname` → stop upstart job.
- `initctl status jobname` → check job status.
- `initctl list` → list all jobs and their states.
- **Events** → startup, shutdown, filesystem mounted, etc.

## 🔹 Systemd Overview
- **Modern init system** → used by most current distributions.
- **Units** → services, sockets, devices, mount points.
- **Parallel processing** → faster boot and dependency resolution.
- **On-demand starting** → services start when needed.
- **Unified logging** → journald for centralized logs.
- `systemctl` → primary command for systemd management.

## 🔹 Systemd Goals
- **Targets** → systemd equivalent of runlevels.
- **multi-user.target** → normal multi-user system (runlevel 3).
- **graphical.target** → graphical desktop environment (runlevel 5).
- **rescue.target** → single-user rescue mode (runlevel 1).
- `systemctl get-default` → show default target.
- `systemctl set-default target` → set default boot target.

## 🔹 Power States
- **poweroff.target** → shutdown system (runlevel 0).
- **reboot.target** → restart system (runlevel 6).
- **halt.target** → halt system without power off.
- **suspend.target** → suspend to RAM.
- **hibernate.target** → suspend to disk.
- `systemctl poweroff` → shutdown system.
- `systemctl reboot` → restart system.

---

📌 **Applied Insight**:  
Init system knowledge is crucial in cybersecurity:
- **Persistence mechanisms**: Attackers modify init scripts for system-level persistence.  
- **Service enumeration**: Understanding running services helps identify attack surfaces.  
- **Incident response**: Init logs reveal system compromise and unauthorized service modifications.  
- **System hardening**: Disabling unnecessary services reduces security risks.  
- **Forensic analysis**: Service startup order and logs provide timeline of system events.
