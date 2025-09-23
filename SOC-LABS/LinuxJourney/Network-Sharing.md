# 🐧 LinuxJourney - Network Sharing

This file summarizes my learning from **LinuxJourney** on Linux network file sharing methods.  
https://linuxjourney.com/lesson/file-sharing-overview  
I practiced these on my local terminal and network environments.

---

## 🔹 File Sharing Overview
- **Network file sharing** → transfer data between machines on same network.
- **scp** (secure copy) → copy files over SSH connection.
- `scp file.txt user@host:/remote/path` → copy file to remote host.
- `scp user@host:/remote/file.txt /local/path` → copy file from remote host.
- `scp -r directory user@host:/remote/path` → copy directory recursively.
- Uses SSH authentication and encryption for security.

## 🔹 rsync
- **Remote synchronization** → efficient file copying and syncing.
- **Delta sync** → only copies differences, not entire files.
- **Checksum verification** → ensures data integrity.
- `rsync -zvr /local/dir /destination/dir` → sync directories locally.
- `rsync -zvr /local/dir user@host:/remote/dir` → sync to remote host.
- **Options**: `-v` (verbose), `-r` (recursive), `-h` (human-readable), `-z` (compress).

## 🔹 Simple HTTP Server
- **Python HTTP server** → quick file sharing over web.
- `python -m http.server` → start HTTP server (Python 3).
- `python -m SimpleHTTPServer` → start HTTP server (Python 2).
- **Default port**: 8000.
- **Access**: `http://IP_ADDRESS:8000` from any browser.
- Useful for quick file sharing without complex setup.

## 🔹 NFS
- **Network File System** → standard Linux network file sharing.
- **Server shares directories** → clients mount them as local filesystems.
- `sudo mount server:/directory /mount_point` → mount NFS share.
- **Automounting** → automatic mounting when accessing directory.
- **Benefits**: transparent file access, Unix permissions preserved.
- Primarily used in Linux/Unix environments.

## 🔹 Samba
- **SMB/CIFS protocol** → Windows-Linux file sharing compatibility.
- **Installation**: `sudo apt install samba`.
- **Configuration**: `/etc/samba/smb.conf`.
- `sudo smbpasswd -a username` → set Samba password.
- `smbclient //HOST/share -U user` → access Samba share from Linux.
- `sudo mount -t cifs //server/share /mount_point -o user=username` → mount Samba share.

---

📌 **Applied Insight**:  
Network sharing knowledge is essential in cybersecurity:
- **Data exfiltration**: Understanding file transfer methods helps detect unauthorized data movement.  
- **Lateral movement**: Attackers use network shares to spread across systems.  
- **Forensic analysis**: Network share logs reveal data access patterns during incidents.  
- **Security hardening**: Proper share configuration prevents unauthorized access.  
- **Incident response**: Quick file collection via scp/rsync during investigations.
