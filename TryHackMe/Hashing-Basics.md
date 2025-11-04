# Hashing Basics - TryHackMe

> Learn about hashing functions, password storage, and file integrity checking

**Room Link:** https://tryhackme.com/room/hashingbasics

<img width="1576" height="928" alt="image" src="https://github.com/user-attachments/assets/58125b6a-a1a4-4f20-bfb6-075da8c328ec" />

---

## Task 1: Introduction

### What is Hashing?

Hashing is like putting data through a one-way blender - you can't reverse it!

**Key features:**
- Takes any input (text, files, passwords)
- Produces a fixed-size unique output
- One-way - can't be reversed
- Small change = completely different hash

**Common algorithms:**
- MD5 - ❌ Insecure (like a plastic lock)
- SHA-1 - ❌ Insecure
- SHA-256 - ✅ Secure (digital fortress)

**Why use hashing?**
- Store passwords safely
- Verify file integrity
- Create unique fingerprints for data

**No answer needed**

---

## Task 2: Hash Functions

### How Hash Functions Work

**Input:** Any size data
**Output:** Fixed-size digest (hash)

Even tiny changes create completely different hashes!

### Hash Properties

**Deterministic:** Same input = same hash always
**One-way:** Can't reverse the hash
**Fixed size:** MD5 = 128 bits, SHA-256 = 256 bits
**Collision resistant:** Hard to find two inputs with same hash

### Hash Collisions

When two different inputs produce the same hash (rare but possible).

**Weak algorithms:**
- MD5 - Collisions can be created intentionally
- SHA-1 - Same problem

**Strong algorithms:**
- SHA-256 - Very collision resistant

### Questions

**What is the SHA256 hash of passport.jpg in ~/Hashing-Basics/Task-2?**
```bash
sha256sum ~/Hashing-Basics/Task-2/passport.jpg
```
- Answer: `77148c6f605a8df855f2b764bcc3be749d7db814f5f79134d2aa539a64b61f02`

**What is the output size in bytes of MD5?**
```
MD5 = 128 bits = 16 bytes
```
- Answer: `16`

**If you have 8-bit hash output, how many possible hash values?**
```
2^8 = 256 possible values
```
- Answer: `256`

---

## Task 3: Insecure Password Storage

### The Hall of Shame

**RockYou (Plaintext passwords)**
- Stored millions of passwords in plain text
- Massive breach created the famous rockyou.txt wordlist
- Now used for password cracking!

**Adobe (Ancient encryption)**
- Used outdated encryption
- Stored password hints in plaintext
- Hints often revealed the password

**LinkedIn (Unsalted SHA-1)**
- Used SHA-1 without salt
- Like leaving the key under the doormat
- Easy to crack

### The Lesson

Never store passwords in plaintext or use outdated algorithms!

### Questions

**What is the 20th password in rockyou.txt?**
```bash
head -20 /usr/share/wordlists/rockyou.txt | tail -1
```
- Answer: `qwerty`

---

## Task 4: Using Hashing for Secure Password Storage

### Problems with Basic Hashing

**Problem 1:** Same password = same hash
- One cracked hash unlocks multiple accounts

**Problem 2:** Rainbow tables
- Pre-computed hash lookup tables
- Fast password cracking

### The Solution: Salting

**Salt** = Unique random value added to each password before hashing

**How it works:**
1. Password: `123456`
2. Salt: `Y4UV*^(=go_!`
3. Combined: `123456Y4UV*^(=go_!`
4. Hash the combined string
5. Store: hash + salt

Now same passwords have different hashes!

### Secure Password Storage Recipe

1. Use strong algorithm (Argon2, Bcrypt, Scrypt, PBKDF2)
2. Add unique salt to each password
3. Hash the salted password
4. Store hash + salt

### Why Not Encryption?

Encryption requires a key to decrypt. If hackers get the key, all passwords are exposed!

Hashing is one-way - no key to steal.

### Questions

**Crack the hash `4c5923b6a6fac7b7355f53bfe2b8f8c1` using the rainbow table**
- Answer: `inS3CyourP4$$`

**Crack `5b31f93c09ad1d065c0491b764d04933` using online tools**
```
Try CrackStation or Hashes.com
```
- Answer: `tryhackme`

**Should you encrypt passwords in password-verification systems? (Yea/Nay)**
- Answer: `Nay`

---

## Task 5: Recognising Password Hashes

### Linux Password Hashes

Stored in `/etc/shadow` (root only)

**Format:** `$prefix$options$salt$hash`

**Common prefixes:**

| Prefix | Algorithm |
|--------|-----------|
| `$1$` | MD5 |
| `$2a$` | Bcrypt |
| `$5$` | SHA-256 |
| `$6$` | SHA-512 |
| `$y$` | yescrypt |

### Windows Password Hashes

**NTLM** - Based on MD4
- Stored in SAM (Security Accounts Manager)
- Tools like Mimikatz can extract them

### Hash Recognition Tools

- `hashID` - Automated hash identification
- [Hashcat Example Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)

### Questions

**What is the hash size in yescrypt?**
- Answer: `256`

**What's the Hash-Mode for Cisco-ASA MD5?**
```
Check Hashcat wiki
```
- Answer: `2410`

**What algorithm is used in Cisco-IOS if it starts with $9$?**
- Answer: `scrypt`

---

## Task 6: Password Cracking

### Cracking Methods

Can't decrypt hashes - must crack them by:
1. Hash potential passwords
2. Compare with target hash
3. Match = found the password!

### Tools

**Hashcat** - GPU-powered (fast!)
**John the Ripper** - CPU-based

### Hashcat Syntax

```bash
hashcat -m <hash_type> -a <attack_mode> hashfile wordlist
```

**Common hash types:**
- `-m 0` - MD5
- `-m 1000` - NTLM
- `-m 1400` - SHA-256
- `-m 1800` - SHA-512
- `-m 3200` - Bcrypt

**Example:**
```bash
hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

### GPU vs CPU

**GPUs:**
- Thousands of cores
- Much faster for hashing
- Best for Hashcat

**CPUs:**
- Fewer cores
- Slower but universal
- Works in VMs

### Questions

**Crack `$2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01pMSrddr1ZG`**
```bash
hashcat -m 3200 -a 0 ~/Hashing-Basics/Task-6/hash1.txt /usr/share/wordlists/rockyou.txt
```
- Answer: `85208520`

**Crack SHA2-256 `9eb7ee7f551d2f0ac684981bd1f1e2fa4a37590199636753efe614d4db30e8e1`**
```bash
hashcat -m 1400 -a 0 ~/Hashing-Basics/Task-6/hash2.txt /usr/share/wordlists/rockyou.txt
```
- Answer: `halloween`

**Crack `$6$GQXVvW4EuM$ehD6jWiMsfNorxy5SINsgdlxmAEl3.yif0/c3NqzGLa0P.S7KRDYjycw5bnYkF5ZtB8wQy8KnskuWQS3Yr1wQ0`**
```bash
hashcat -m 1800 -a 0 ~/Hashing-Basics/Task-6/hash3.txt /usr/share/wordlists/rockyou.txt
```
- Answer: `spaceman`

**Crack `b6b0d451bbf6fed658659a9e7e5598fe`**
```bash
hashcat -m 0 -a 0 ~/Hashing-Basics/Task-6/hash4.txt /usr/share/wordlists/rockyou.txt
```
- Answer: `funforyou`

---

## Task 7: Hashing for Integrity Checking

### File Integrity

Hash files to verify they haven't been tampered with.

**Process:**
1. Download file
2. Calculate hash
3. Compare with official hash
4. Match = file is authentic!

**Example:**
```bash
sha256sum file.iso
```

If your hash matches the official one, the file is safe.

### Finding Duplicates

Same file = same hash

Hash all files and compare - identical hashes mean duplicate files!

### HMAC (Keyed-Hash Message Authentication Code)

Adds a secret key to hashing for:
- **Authentication** - Verify source
- **Integrity** - Verify no tampering

**Format:**
```
HMAC = Hash(Key + Message)
```

### Questions

**What is SHA256 hash of libgcrypt-1.11.0.tar.bz2 in ~/Hashing-Basics/Task-7?**
```bash
sha256sum ~/Hashing-Basics/Task-7/libgcrypt-1.11.0.tar.bz2
```
- Answer: `09120c9867ce7f2081d6aaa1775386b98c2f2f246135761aae47d81f58685b9c`

**What's the hashcat mode for HMAC-SHA512 (key = $pass)?**
- Answer: `1750`

---

## Task 8: Conclusion

### Hashing vs Encoding vs Encryption

**Hashing:**
- One-way (irreversible)
- Fixed output size
- For passwords, integrity
- Example: SHA-256

**Encoding:**
- Reversible transformation
- For data compatibility
- Example: Base64, UTF-8
- NOT for security!

**Encryption:**
- Reversible (with key)
- For confidentiality
- Example: AES, RSA
- Requires key management

### Base64 Encoding

**Encode:**
```bash
echo "Hello" | base64
```

**Decode:**
```bash
echo "SGVsbG8K" | base64 -d
```

### Questions

**Decode `RU5jb2RlREVjb2RlCg==` from decode-this.txt in ~/Hashing-Basics/Task-8**
```bash
base64 -d ~/Hashing-Basics/Task-8/decode-this.txt
```
- Answer: `ENcodeDEcode`

---

## Quick Reference

### Hash Algorithms

| Algorithm | Bits | Secure? | Use Case |
|-----------|------|---------|----------|
| MD5 | 128 | ❌ No | Legacy only |
| SHA-1 | 160 | ❌ No | Legacy only |
| SHA-256 | 256 | ✅ Yes | General purpose |
| SHA-512 | 512 | ✅ Yes | High security |
| Bcrypt | 184 | ✅ Yes | Password hashing |

### Hashcat Modes

| Hash Type | Mode |
|-----------|------|
| MD5 | 0 |
| SHA-256 | 1400 |
| SHA-512 | 1800 |
| NTLM | 1000 |
| Bcrypt | 3200 |

### Common Commands

```bash
# Calculate hashes
md5sum file.txt
sha256sum file.txt

# Crack with Hashcat
hashcat -m 1400 -a 0 hash.txt rockyou.txt

# Crack with John
john --wordlist=rockyou.txt hash.txt

# Base64 encode/decode
base64 file.txt
base64 -d encoded.txt
```

---

## Key Takeaways

- **Hashing** is one-way - can't be reversed
- **Never use MD5 or SHA-1** for security
- **SHA-256** is the current standard
- **Salt passwords** to prevent rainbow table attacks
- **Bcrypt, Argon2, Scrypt** are best for passwords
- **Hashcat** uses GPU for fast cracking
- **Hashing** verifies file integrity
- **Encoding ≠ Hashing ≠ Encryption**

Hashing is essential for password security and data integrity! 🔐
