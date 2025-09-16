# 🐧 LinuxJourney - Devices

This file summarizes my learning from **LinuxJourney** on Linux device management.  
https://labex.io/lesson/dev-directory  
I practiced these on my local terminal and various Linux distributions.

---

## 🔹 /dev directory
- `/dev` → directory containing device files (device nodes).
- Device files provide interface to hardware and pseudo devices.
- `ls /dev` → list all available devices.
- Everything in Linux is treated as a file, including hardware devices.

## 🔹 Device Types
- **Block devices** → transfer data in blocks (hard drives, USB drives).
- **Character devices** → transfer data character by character (keyboards, mice).
- **Pseudo devices** → virtual devices not physically connected.
- `ls -l /dev` → see device types (b=block, c=character).

## 🔹 Device Names
- **SCSI devices**: `/dev/sda`, `/dev/sdb` (first, second disk).
- **Partitions**: `/dev/sda1`, `/dev/sda2` (first, second partition).
- **PATA devices** (older): `/dev/hda`, `/dev/hdb`.
- **Pseudo devices**: `/dev/null`, `/dev/zero`, `/dev/random`.
- **Network interfaces**: `eth0`, `wlan0` (not in /dev).

## 🔹 sysfs
- `/sys` → virtual filesystem exposing kernel information.
- `/sys/block/` → block device information.
- `/sys/class/net/` → network device information.
- `cat /sys/block/sda/size` → get disk size in sectors.
- Modern way to interact with kernel and device information.

## 🔹 udev
- **Device manager** for Linux kernel.
- Dynamically creates/removes device files in `/dev`.
- `/etc/udev/rules.d/` → custom udev rules.
- `udevadm info /dev/sda` → detailed device information.
- Handles hotplug events (USB insertion, etc.).

## 🔹 lsusb, lspci, lsscsi
- `lsusb` → list USB devices and controllers.
- `lspci` → list PCI devices (graphics cards, network cards).
- `lsscsi` → list SCSI devices (hard drives, SSDs).
- `lsblk` → display block devices in tree format.
- `lshw` → comprehensive hardware information.

## 🔹 dd
- **Data duplicator** → copy and convert files/devices.
- `dd if=/dev/zero of=testfile bs=1M count=100` → create 100MB file.
- `dd if=/dev/sda of=backup.img` → create disk image.
- `dd if=file.iso of=/dev/sdb` → write ISO to USB drive.
- **Caution**: Can destroy data if used incorrectly.

---

📌 **Applied Insight**:  
Device management is crucial in cybersecurity:
- **Forensic imaging**: `dd` creates bit-for-bit copies for analysis.  
- **USB monitoring**: `lsusb` and udev rules track unauthorized devices.  
- **System reconnaissance**: Device enumeration reveals hardware info.  
- **Incident response**: Understanding `/dev` helps with disk analysis and evidence collection.
