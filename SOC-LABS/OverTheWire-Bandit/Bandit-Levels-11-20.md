# 🔐 OverTheWire Bandit — Levels 11–20 (Summary & Notes)

**Repository:** `SOC-LABS/OverTheWire-Bandit/`  
**Range:** Levels 11 → 20  
**Purpose:** Build practical Linux investigation skills used in SOC/IR workflows (decoding, archive handling, service interaction, key handling, network reconnaissance).

---

## 📌 Overview
Levels 11–20 combine text transformations, binary/hex decoding, compression/decompression, remote service interaction (netcat/ncat), basic port scanning, and private key usage. These exercises closely mirror real-world forensic tasks (decode/untar/extract), and service-based capture/response tasks (interact with local services, handle keys securely).

> **Note:** I do **not** post challenge passwords here. If you want to keep local proof, save them in a private document.

---

## 🧭 Level-by-level Notes (11 → 20)

### Level 11 → 12
**Goal:** Apply a ROT13-style transformation to recover secret text.
- **Commands / approach:**  
  - `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`  
- **Why:** Demonstrates quick use of `tr` for character substitution (useful for simple obfuscation reversal).

---

### Level 12 → 13
**Goal:** Convert hex dump to raw, then identify and extract multiple archived/compressed layers.
- **Commands / approach (typical sequence):**  
  - Copy file to `/tmp` and work there (good practice).  
  - `xxd -r data.txt > data` → convert hex to binary.  
  - `file data` → identify file type.  
  - Rename and run appropriate tools: `gzip -d`, `bzip2 -d`, `tar xf` repeatedly until you find plain data.  
  - Inspect intermediate files with `file` and `strings` to identify formats.
- **Why:** This exercise practices iterative forensic extraction — often artifacts are nested (e.g., `xxd` → gzip → bzip2 → tar). The pattern of *identify → use correct decompressor → repeat* is common in malware/IR.

---

### Level 13 → 14
**Goal:** Use SSH key authentication to connect to another account.
- **Commands / approach:**  
  - Use provided private key with `ssh -i sshkey.private bandit14@localhost -p 2220`.
- **Why:** Shows private-key usage and `chmod 600` importance for key files. In real life, key handling and permissions are critical for secure SSH authentication.

---

### Level 14 → 15
**Goal:** Interact with a local TCP service using `nc` (netcat).
- **Commands / approach:**  
  - `cat /etc/bandit_pass/bandit14` to retrieve password (locally).  
  - `nc localhost 30000` → connect to service and receive response.  
- **Why:** Useful to practice simple TCP service interactions; many C2 or challenge services use raw sockets.

---

### Level 15 → 16
**Goal:** Connect to an SSL-enabled local service with `ncat --ssl`.
- **Commands / approach:**  
  - `ncat --ssl localhost 30001` and paste password when prompted.
- **Why:** Demonstrates SSL-wrapped plain-TCP connections — helpful to know how to interact with services running TLS but without HTTP.

---

### Level 16 → 17
**Goal:** Identify an open port range, extract a private key, and use it to SSH.
- **Commands / approach:**  
  - `nmap -p 31000-32000 localhost` → find listening port(s).  
  - Copy the RSA key (save to a file locally), set `chmod 600 bandit16.key`.  
  - `ssh -i bandit16.key bandit17@bandit.labs.overthewire.org -p 2220`
- **Why:** Practice combining port scanning and secure key-based SSH authentication. Storing keys securely and correct permissions (`chmod 600`) are critical.

---

### Level 17 → 18
**Goal:** Compare two password files to find the new password(s).
- **Commands / approach:**  
  - `diff passwords.old passwords.new` → spot differences.  
  - Try the discovered credentials to SSH into the next account.
- **Why:** `diff` is a simple but powerful forensic tool for comparing system state changes, logs, or config differences.

---

### Level 18 → 19
**Goal:** Simple file inspection & obtaining next password from a README.
- **Commands / approach:**  
  - `cat readme` or list directory to find hints → follow to next step.
- **Why:** Reinforces pattern of careful directory inspection and reading of plain text clues.

---

### Level 19 → 20
**Goal:** Execute provided helper script to read protected files.
- **Commands / approach:**  
  - `./bandit20-do ls /etc/bandit_pass` and `./bandit20-do cat /etc/bandit_pass/bandit20`
- **Why:** Some environments restrict direct file access but provide helper utilities; recognising and leveraging those is important.

---

### Level 20 → 21
**Goal:** Use `nc` to craft a two-terminal exchange to reveal password.
- **Commands / approach:**  
  - In one terminal: `echo -n 'CURRENT_PASSWORD' | nc -l -p 1234` (listen).  
  - In another terminal: use the supplied helper (`./suconnect 1234`) which will connect to that listener; paste password to match and receive next password.
- **Why:** Teaches interactive networking workflows and how to coordinate multiple terminals/services — a useful pattern for testing and debugging network interactions.

---

## 🛠 Tools & Commands You Practised
- `tr`, `xxd`, `file`, `gzip`, `bzip2`, `tar`, `strings`  
- `ssh -i keyfile`, `chmod 600` (file permission hygiene)  
- `nc`, `ncat --ssl`, `nmap -p` (service interaction & port scanning)  
- `diff` (file comparison)  
- Working directory hygiene (use `/tmp` for temporary files)

---

## 🔎 SOC / IR Takeaways
- **Layered artifact extraction** (hex → compressed archives → tar) mirrors real forensic jobs where data is intentionally or accidentally nested.  
- **Identifying file type first (`file`)** then choosing the right decompressor is a repeatable pattern.  
- **Key handling** and permissions are operationally important — incorrect permissions break SSH usage.  
- **Small networking tools (`nc`, `ncat`)** are powerful for quick service interaction and debugging; many small-scale C2 or maintenance services behave similarly.  
- **Documentation:** for each step I logged commands + reasoning; this is important for IR reports and reproducibility.

---

## 📂 Suggested Repo Layout (where to add this file)
