# 🐧 LinuxJourney - Packages

This file summarizes my learning from **LinuxJourney** on Linux package management.  
[https://linuxjourney.com/lesson/software-distribution ](https://labex.io/lesson/software-distribution)
I practiced these on my local terminal and various Linux distributions.

---

## 🔹 Software Distribution
- Software distributed as **source code** or **pre-compiled packages**.
- **Source code**: compile yourself (flexible but complex).
- **Packages**: pre-built binaries (easy to install and manage).
- Package managers handle installation, updates, and dependencies.

## 🔹 Package Repositories
- **Repositories** → central servers storing packages.
- `apt update` → refresh package list from repos.
- `/etc/apt/sources.list` → repository configuration (Debian/Ubuntu).
- `/etc/yum.repos.d/` → repository config directory (Red Hat/CentOS).

## 🔹 tar and gzip
- `tar -cvf archive.tar files/` → create archive.
- `tar -xvf archive.tar` → extract archive.
- `tar -czvf archive.tar.gz files/` → compress with gzip.
- `tar -xzvf archive.tar.gz` → extract compressed archive.
- Common for **source code distribution**.

## 🔹 Package Dependencies
- Software often depends on **libraries** and other packages.
- **Dependency hell**: conflicts between package versions.
- Package managers automatically resolve dependencies.
- `apt-cache depends package` → show dependencies.

## 🔹 rpm and dpkg
- **Low-level package managers**.
- `rpm -i package.rpm` → install RPM package.
- `rpm -qa` → list all installed packages.
- `dpkg -i package.deb` → install DEB package.
- `dpkg -l` → list installed packages.
- **Don't handle dependencies automatically**.

## 🔹 yum and apt
- **High-level package managers** with dependency resolution.
- **YUM (Red Hat/CentOS/Fedora)**:
  - `yum install package` → install package.
  - `yum update` → update all packages.
  - `yum remove package` → uninstall package.
  - `yum search keyword` → search packages.
- **APT (Debian/Ubuntu)**:
  - `apt install package` → install package.
  - `apt update && apt upgrade` → update system.
  - `apt remove package` → uninstall package.
  - `apt search keyword` → search packages.

## 🔹 Compile Source Code
- **Download source code** → usually as `.tar.gz`.
- **Standard compilation process**:
  ```bash
  ./configure     # configure build
  make           # compile source
  sudo make install  # install binaries
  ```
- **Prerequisites**: compiler (`gcc`), build tools (`make`), development headers.
- `apt install build-essential` → install build tools (Ubuntu).

---

📌 **Applied Insight**:  
Package management is crucial in cybersecurity:
- **Vulnerability management**: keeping packages updated prevents exploits.  
- **SOC teams** monitor for **unauthorized software installations**.  
- **Source compilation** allows custom security hardening.  
- **Package integrity** verification prevents **supply chain attacks** (`rpm --verify`, `debsums`).
