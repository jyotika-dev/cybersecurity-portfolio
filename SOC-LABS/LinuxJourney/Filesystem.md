# 🐧 LinuxJourney - The Filesystem

This file summarizes my learning from **LinuxJourney** on Linux filesystem management.  
[https://linuxjourney.com/lesson/filesystem-hierarchy](https://labex.io/lesson/filesystem-hierarchy)

I practiced these on my local terminal and various Linux distributions.

---

## 🔹 Filesystem Hierarchy
- `/` → root directory, top of filesystem tree.
- `/home` → user home directories.
- `/etc` → system configuration files.
- `/var` → variable data (logs, databases).
- `/tmp` → temporary files.
- `/usr` → user programs and libraries.
- `/bin`, `/sbin` → essential system binaries.

## 🔹 Filesystem Types
- **ext4** → default Linux filesystem (journaled, reliable).
- **xfs** → high-performance filesystem for large files.
- **btrfs** → modern filesystem with snapshots and compression.
- **ntfs** → Windows filesystem (readable/writable in Linux).
- **vfat/fat32** → USB drives and compatibility.
- `cat /proc/filesystems` → show supported filesystem types.

## 🔹 Anatomy of a Disk
- **Boot sector** → contains bootloader information.
- **Partition table** → defines disk partitions (MBR or GPT).
- **Partitions** → logical divisions of disk space.
- **Filesystem** → data structure within partitions.
- `fdisk -l` → show disk and partition information.

## 🔹 Disk Partitioning
- `fdisk /dev/sda` → partition disk interactively.
- `parted /dev/sda` → advanced partitioning tool.
- **Primary partitions** → up to 4 on MBR disks.
- **Extended partition** → container for logical partitions.
- **GPT** → modern partitioning (supports >2TB, many partitions).
- `lsblk` → display block device tree.

## 🔹 Creating Filesystems
- `mkfs.ext4 /dev/sda1` → create ext4 filesystem.
- `mkfs.xfs /dev/sda2` → create XFS filesystem.
- `mkfs.vfat /dev/sda3` → create FAT32 filesystem.
- `mkswap /dev/sda4` → create swap space.
- Always partition before creating filesystem.

## 🔹 mount and umount
- `mount /dev/sda1 /mnt` → mount filesystem to directory.
- `umount /mnt` → unmount filesystem.
- `mount -t ext4 /dev/sda1 /mnt` → specify filesystem type.
- `mount -o ro /dev/sda1 /mnt` → mount read-only.
- `mount` → show currently mounted filesystems.

## 🔹 /etc/fstab
- **File system table** → automatic mounting configuration.
- Format: `device mountpoint fstype options dump pass`.
- Example: `/dev/sda1 / ext4 defaults 0 1`.
- `mount -a` → mount all filesystems in fstab.
- Edit carefully - errors can prevent system boot.

## 🔹 swap
- **Virtual memory** on disk when RAM is full.
- `swapon /dev/sda4` → enable swap partition.
- `swapoff /dev/sda4` → disable swap.
- `free -h` → show memory and swap usage.
- `mkswap /dev/sda4` → initialize swap space.

## 🔹 Disk Usage
- `df -h` → show filesystem disk space usage.
- `du -sh /home` → show directory size.
- `du -h --max-depth=1` → show subdirectory sizes.
- `ncdu` → interactive disk usage analyzer.
- `lsof` → list open files (useful when umount fails).

## 🔹 Filesystem Repair
- `fsck /dev/sda1` → check and repair filesystem.
- `fsck.ext4 /dev/sda1` → specific ext4 check.
- `e2fsck -f /dev/sda1` → force check ext4 filesystem.
- **Always unmount** before running fsck.
- Boot from live USB for root filesystem repair.

## 🔹 Inodes
- **Index nodes** → data structure storing file metadata.
- Contains: permissions, ownership, timestamps, size, block pointers.
- `ls -i` → show inode numbers.
- `df -i` → show inode usage.
- `stat filename` → detailed inode information.
- Each filesystem has limited number of inodes.

## 🔹 symlinks
- **Symbolic links** → pointers to other files/directories.
- `ln -s /path/to/file linkname` → create symbolic link.
- `ln file hardlink` → create hard link (same inode).
- `ls -l` → shows link target with `->`.
- `readlink linkname` → show target of symbolic link.
- Hard links cannot cross filesystem boundaries.

---

📌 **Applied Insight**:  
Filesystem knowledge is essential in cybersecurity:
- **Digital forensics**: Understanding inodes and file allocation for recovery.  
- **Incident response**: Analyzing `/var/log` and temporary file locations.  
- **Evidence preservation**: Proper mounting (read-only) prevents contamination.  
- **System hardening**: Separate partitions (`/tmp`, `/var`) limit attack impact.  
- **Data recovery**: `fsck` and filesystem repair for corrupted systems.
