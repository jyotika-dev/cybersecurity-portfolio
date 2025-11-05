# John the Ripper - TryHackMe

> Learn password cracking with John the Ripper - a powerful hash cracking tool

**Room Link:** https://tryhackme.com/room/johntheripper0

<img width="1477" height="900" alt="image" src="https://github.com/user-attachments/assets/847c3e03-615f-42c2-93ac-2fea1ef49cb3" />


---

## Task 1: Introduction

John the Ripper (often called "John") is an industry-standard password cracking tool. It's fast, flexible, and works with many hash types.

**No answer needed**

---

## Task 2: Setting Up John the Ripper

### Installation

**Kali/Parrot (pre-installed):**
```bash
sudo apt-get install john
```

**BlackArch:**
```bash
packman -S john
```

**Build from source:**
```bash
git clone https://github.com/openwall/john -b bleeding-jumbo john
cd john/src/
./configure
make -s clean && make -sj4
cd ../run
```

### Questions

**What is the most popular extended version of John the Ripper?**
- Answer: `Jumbo John`

---

## Task 3: Wordlists

### Common Wordlists

**rockyou.txt** - Most popular, from 2009 RockYou breach
- Location: `/usr/share/wordlists/rockyou.txt.gz`

**Extract rockyou.txt:**
```bash
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
```

**SecLists** - Collection of many wordlists

### Questions

**What website was rockyou.txt created from a breach on?**
- Answer: `rockyou.com`

---

## Task 4: Cracking Basic Hashes

### Basic John Usage

**Automatic mode (detects hash type):**
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

**Specific format (recommended):**
```bash
john --format=[format] --wordlist=[path] hash.txt
```

### Identify Hash Type

**hash-identifier tool:**
```bash
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
python3 hash-id.py
```

### Questions

**What type of hash is hash1.txt?**
- Answer: `md5`

**What is the cracked value of hash1.txt?**
```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
```
- Answer: `biscuit`

**What type of hash is hash2.txt?**
- Answer: `SHA1`

**What is the cracked value of hash2.txt?**
```bash
john --format=raw-sha1 --wordlist=/usr/share/wordlists/rockyou.txt hash2.txt
```
- Answer: `kangeroo`

**What type of hash is hash3.txt?**
- Answer: `SHA-256`

**What is the cracked value of hash3.txt?**
```bash
john --format=raw-sha256 --wordlist=/usr/share/wordlists/rockyou.txt hash3.txt
```
- Answer: `microphone`

**What type of hash is hash4.txt?**
- Answer: `whirlpool`

**What is the cracked value of hash4.txt?**
```bash
john --format=whirlpool --wordlist=/usr/share/wordlists/rockyou.txt hash4.txt
```
- Answer: `colossal`

---

## Task 5: Cracking Windows Authentication Hashes

### What is NTHash/NTLM?

**NT** = "New Technology"
- Modern Windows password hash format
- Stored in SAM database
- Can be dumped with tools like Mimikatz

**NTLM** = Previous Windows format

### Cracking NT Hashes

```bash
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

### Questions

**What do we need to set the "format" flag to?**
- Answer: `nt`

**What is the cracked value of this password?**
```bash
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt ntlm.txt
```
- Answer: `mushroom`

---

## Task 6: Cracking /etc/shadow Hashes

### Linux Password Files

**/etc/passwd** - User account info (readable by all)
**/etc/shadow** - Password hashes (root only)

### Unshadow Utility

Combines passwd and shadow files:
```bash
unshadow /etc/passwd /etc/shadow > unshadowed.txt
```

### Crack Linux Hashes

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt
```

### Questions

**What is the root user's password?**
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt etchashes.txt
```
- Answer: `1234`

---

## Task 7: Single Crack Mode

### What is Single Mode?

Tries variations of the username as the password.

**Examples for user "john":**
- john1, john2, john123
- john$, john!, john@
- John1, JOHN, jOhN

### Using Single Mode

**Format:** `username:hash`

**Example:**
```bash
nano hash.txt
# Add: joker:7bf6d9bb82bed1302f331fc6b816aada

john --single --format=raw-md5 hash.txt
```

### Questions

**What is Joker's password?**
```bash
# Edit file to: joker:7bf6d9bb82bed1302f331fc6b816aada
john --single --format=raw-md5 hash7.txt
```
- Answer: `Jok3r`

---

## Task 8: Custom Rules

### What are Custom Rules?

Define patterns for password variations.

**Configuration:** `/etc/john/john.conf`

### Using Rules

```bash
john --wordlist=[path] --rule=RuleName hash.txt
```

### Questions

**What do custom rules allow us to exploit?**
- Answer: `Password complexity predictability`

**What rule adds all capital letters to the end of the word?**
- Answer: `Az"[A-Z]"`

**What flag calls a custom rule called "THMRules"?**
- Answer: `--rule=THMRules`

---

## Task 9: Cracking Password Protected Zip Files

### Convert Zip to Hash

**zip2john utility:**
```bash
zip2john secure.zip > hash.txt
```

### Crack the Hash

```bash
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

### Extract and View

```bash
unzip secure.zip
cat flag.txt
```

### Questions

**What is the password for secure.zip?**
```bash
zip2john secure.zip > secure.txt
john secure.txt --wordlist=/usr/share/wordlists/rockyou.txt
```
- Answer: `pass123`

**What is the contents of the flag?**
```bash
unzip secure.zip
cat flag.txt
```
- Answer: `THM{ZIP_CRACK}`

---

## Task 10: Cracking Password Protected RAR Archives

### Convert RAR to Hash

**rar2john utility:**
```bash
rar2john secure.rar > hash.txt
```

### Crack the Hash

```bash
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

### Extract and View

```bash
unrar e secure.rar
cat flag.txt
```

### Questions

**What is the password for secure.rar?**
```bash
rar2john secure.rar > secure2.txt
john secure2.txt --wordlist=/usr/share/wordlists/rockyou.txt
```
- Answer: `password`

**What is the contents of the flag?**
```bash
unrar e secure.rar
cat flag.txt
```
- Answer: `THM{RAR_CRACK}`

---

## Task 11: Cracking SSH Keys with John

### Convert SSH Key to Hash

**ssh2john utility:**
```bash
sudo python /usr/share/john/ssh2john.py id_rsa > hash.txt
```

**Note:** May need Python 2 installed:
```bash
sudo apt-get install python
```

### Crack the Hash

```bash
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

### Questions

**What is the SSH key password?**
```bash
sudo python /usr/share/john/ssh2john.py idrsa.id_rsa > id.txt
john --wordlist=/usr/share/wordlists/rockyou.txt id.txt
```
- Answer: `mango`

---

## Task 12: Further Reading

Check out the [OpenWall John Wiki](https://www.openwall.com/john/) for more info!

**No answer needed**

---

## Quick Reference

### Common John Commands

| Purpose | Command |
|---------|---------|
| Crack hash | `john --wordlist=rockyou.txt hash.txt` |
| Specify format | `john --format=raw-md5 hash.txt` |
| Single mode | `john --single hash.txt` |
| Custom rules | `john --rule=RuleName hash.txt` |
| Show cracked | `john --show hash.txt` |

### Hash Formats

| Hash Type | Format |
|-----------|--------|
| MD5 | `raw-md5` |
| SHA-1 | `raw-sha1` |
| SHA-256 | `raw-sha256` |
| SHA-512 | `sha512crypt` |
| NTLM | `nt` |
| Bcrypt | `bcrypt` |

### Conversion Tools

| File Type | Tool | Command |
|-----------|------|---------|
| ZIP | zip2john | `zip2john file.zip > hash.txt` |
| RAR | rar2john | `rar2john file.rar > hash.txt` |
| SSH Key | ssh2john | `python ssh2john.py id_rsa > hash.txt` |
| Linux Shadow | unshadow | `unshadow passwd shadow > hash.txt` |

---

## Key Takeaways

- **John the Ripper** is the industry standard for password cracking
- **rockyou.txt** is the most popular wordlist
- **Specify format** for best results (`--format=`)
- **Single mode** tries username-based passwords
- **Custom rules** exploit password patterns
- **Conversion tools** handle ZIP, RAR, SSH keys
- **unshadow** combines Linux passwd and shadow files
- **NT format** for Windows password hashes

John is essential for password security testing! 🔓
