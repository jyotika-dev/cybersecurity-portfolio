# 🐧 LinuxJourney - Kernel

This file summarizes my learning from **LinuxJourney** on Linux kernel fundamentals.  
https://labex.io/lesson/kernel-overview  
I practiced these on my local terminal and various Linux distributions.

---

## 🔹 Overview of the Kernel
- **Kernel** → core of the operating system, manages hardware and system resources.
- **Bridge** between applications and hardware components.
- **Memory management** → allocates and deallocates RAM for processes.
- **Process scheduling** → decides which processes get CPU time.
- **Device drivers** → kernel modules that communicate with hardware.
- `uname -r` → show current kernel version.

## 🔹 Privilege Levels
- **Kernel space** → privileged mode, direct hardware access.
- **User space** → restricted mode, limited system access.
- **Ring 0** → kernel mode (highest privilege).
- **Ring 3** → user mode (lowest privilege).
- **System calls** → interface between user and kernel space.
- Applications cannot directly access hardware from user space.

## 🔹 System Calls
- **syscalls** → kernel functions accessible from user space.
- `open()`, `read()`, `write()`, `close()` → file operations.
- `fork()`, `exec()`, `wait()` → process management.
- `malloc()`, `free()` → memory allocation (via libc).
- `strace command` → trace system calls made by program.
- `syscalls` provide controlled access to kernel functionality.

## 🔹 Kernel Installation
- **Package manager** → `apt install linux-image-generic` (Ubuntu).
- **From source** → download, configure, compile kernel.
- **Steps**: `make menuconfig`, `make`, `make modules_install`, `make install`.
- **initramfs** → temporary filesystem for boot process.
- `update-grub` → update bootloader configuration.
- Always keep backup kernel for recovery.

## 🔹 Kernel Location
- `/boot/vmlinuz-*` → compressed kernel image.
- `/boot/initrd.img-*` → initial ramdisk image.
- `/boot/System.map-*` → kernel symbol table.
- `/boot/config-*` → kernel compilation configuration.
- `ls /boot` → view kernel files.
- GRUB bootloader loads kernel from `/boot` directory.

## 🔹 Kernel Modules
- **Loadable modules** → extend kernel functionality without reboot.
- **Device drivers** → most hardware support via modules.
- `lsmod` → list loaded kernel modules.
- `modinfo module_name` → show module information.
- `modprobe module_name` → load kernel module.
- `rmmod module_name` → unload kernel module.
- `/lib/modules/$(uname -r)/` → module storage location.

---

📌 **Applied Insight**:  
Kernel knowledge is fundamental in cybersecurity:
- **Rootkit detection**: Understanding kernel space helps identify malicious modules.  
- **System hardening**: Kernel parameters and modules affect security posture.  
- **Forensic analysis**: `strace` reveals program behavior and system interactions.  
- **Incident response**: Kernel logs and module analysis detect compromised systems.  
- **Vulnerability research**: Many exploits target kernel vulnerabilities for privilege escalation.
