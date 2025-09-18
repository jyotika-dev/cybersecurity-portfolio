# 🐧 LinuxJourney - Boot the System

This file summarizes my learning from **LinuxJourney** on Linux boot process.  
https://labex.io/lesson/boot-process-overview  
I practiced these on my local terminal and virtual machines.

---

## 🔹 Boot Process Overview
- **Multi-stage process** → hardware initialization to user login.
- **Sequence**: BIOS/UEFI → Bootloader → Kernel → Init → User Space.
- Each stage hands control to the next component.
- Understanding boot process helps troubleshoot system failures.
- `dmesg` → view kernel boot messages.

## 🔹 Boot Process: BIOS
- **BIOS/UEFI** → first software that runs on power-on.
- **Power-On Self Test (POST)** → hardware component checks.
- **Boot device selection** → finds bootable storage device.
- **Master Boot Record (MBR)** → first 512 bytes of bootable disk.
- **UEFI** → modern replacement for BIOS (supports GPT, secure boot).
- `dmidecode` → show BIOS/hardware information.

## 🔹 Boot Process: Bootloader
- **GRUB2** → most common Linux bootloader.
- **Stage 1** → MBR loads Stage 1.5 or Stage 2.
- **Stage 2** → presents boot menu, loads kernel.
- `/boot/grub/grub.cfg` → GRUB configuration file.
- `grub-update` → regenerate GRUB configuration.
- Bootloader can load multiple operating systems.

## 🔹 Boot Process: Kernel
- **Kernel loading** → bootloader loads kernel into memory.
- **initramfs** → temporary root filesystem for early boot.
- **Hardware detection** → kernel probes and initializes devices.
- **Root filesystem mounting** → switches from initramfs to real root.
- `/proc/cmdline` → kernel boot parameters.
- `uname -a` → show kernel version and information.

## 🔹 Boot Process: Init
- **Init system** → first user-space process (PID 1).
- **systemd** → modern init system (most distributions).
- **SysV Init** → traditional init system (older systems).
- **Runlevels/Targets** → different system states.
- `systemctl` → control systemd services and targets.
- `ps -p 1` → show init process information.

---

📌 **Applied Insight**:  
Boot process knowledge is crucial in cybersecurity:
- **Forensic analysis**: Boot logs reveal system compromise indicators.  
- **Rootkit detection**: Understanding boot sequence helps identify boot-level malware.  
- **System hardening**: Secure boot and bootloader protection prevent unauthorized modifications.  
- **Incident response**: Boot process failures often indicate system compromise or hardware issues.  
- **Persistence mechanisms**: Attackers target boot process for maintaining access.
