# Public Key Cryptography Basics - TryHackMe

> Learn about public key cryptography, RSA, Diffie-Hellman, SSH, and GPG

**Room Link:** https://tryhackme.com/room/publickeybasics

<img width="1545" height="932" alt="image" src="https://github.com/user-attachments/assets/7bba7c1d-8e70-4a2c-86d9-aa3c04a462d0" />


---

## Task 1: Introduction

### Why Public Key Cryptography?

In face-to-face meetings, security is natural:
- **Authentication** - You see who you're talking to
- **Authenticity** - You know the words come from them
- **Integrity** - Nothing changes their words
- **Confidentiality** - Private conversation

Online, we need cryptography to provide these same protections!

### What Public Key Crypto Provides

**Symmetric encryption** (one key) mainly protects:
- Confidentiality

**Asymmetric encryption** (public/private keys) provides:
- Authentication
- Authenticity
- Integrity
- Confidentiality

**No answer needed**

---

## Task 2: Common Use of Asymmetric Encryption

### The Problem

How do you securely share a symmetric key without anyone intercepting it?

### The Lock and Box Analogy

1. You want to send secret instructions to your friend
2. Ask your friend for a lock (only they have the key)
3. Put instructions in a box and lock it
4. Send the locked box
5. Only your friend can unlock it!

**In cryptography:**
- Lock = Server's public key
- Key = Server's private key
- Instructions = Symmetric encryption key

### How It Works

1. Server sends you its public key
2. You encrypt a symmetric key with the public key
3. Only the server can decrypt it (using private key)
4. Now you both have the same symmetric key
5. Fast symmetric encryption for the rest of the session!

### Questions

**In the analogy, what real object is analogous to the public key?**
- Answer: `Lock`

---

## Task 3: RSA

### What is RSA?

RSA (Rivest-Shamir-Adleman) is a popular asymmetric algorithm based on the difficulty of factoring large numbers.

### Key Components

- **p and q** - Two large prime numbers
- **n** - Product of p and q (n = p × q)
- **φ(n)** - Euler's Totient: φ(n) = (p-1) × (q-1)
- **e** - Public exponent (usually 65537)
- **d** - Private exponent
- **m** - Message (plaintext)
- **c** - Ciphertext (encrypted message)

### Public and Private Keys

**Public Key:** (n, e)
**Private Key:** (n, d)

### RSA in CTFs

Common CTF challenge: Given some RSA variables, decrypt a message.

**Useful tools:**
- [RsaCtfTool](https://github.com/Ganapati/RsaCtfTool)
- rsatool

### Questions

**Knowing that p = 4391 and q = 6659. What is n?**
```
n = p × q
n = 4391 × 6659
n = 29,239,669
```
- Answer: `29239669`

**Knowing that p = 4391 and q = 6659. What is φ(n)?**
```
φ(n) = (p-1) × (q-1)
φ(n) = (4391-1) × (6659-1)
φ(n) = 4390 × 6658
φ(n) = 29,228,620
```
- Answer: `29228620`

---

## Task 4: Diffie-Hellman Key Exchange

### What is Diffie-Hellman?

Allows two parties to establish a shared secret over an insecure channel without transmitting the secret itself.

### How It Works

**Public values (known to everyone):**
- p = large prime number
- g = generator

**Private values (kept secret):**
- a = Alice's private key
- b = Bob's private key

**Process:**
1. Alice calculates: A = g^a mod p (sends to Bob)
2. Bob calculates: B = g^b mod p (sends to Alice)
3. Alice calculates key: K = B^a mod p
4. Bob calculates key: K = A^b mod p
5. Both get the same key!

### Questions

**Consider p = 29, g = 5, a = 12. What is A?**
```
A = g^a mod p
A = 5^12 mod 29
A = 7
```
- Answer: `7`

**Consider p = 29, g = 5, b = 17. What is B?**
```
B = g^b mod p
B = 5^17 mod 29
B = 9
```
- Answer: `9`

**Knowing p = 29, a = 12, and B = 9, what is Bob's key?**
```
Key = B^a mod p
Key = 9^12 mod 29
Key = 24
```
- Answer: `24`

**Knowing p = 29, b = 17, and A = 7, what is Alice's key?**
```
Key = A^b mod p
Key = 7^17 mod 29
Key = 24
```
- Answer: `24`

(Both keys match - success!)

---

## Task 5: SSH

### What is SSH?

SSH (Secure Shell) securely connects to remote systems using public key authentication.

### SSH Key Algorithms

**Common algorithms:**
- **RSA** - Traditional, widely supported
- **Ed25519** - Modern, fast, secure (recommended)
- **ECDSA** - Elliptic curve variant
- **DSA** - Older, less secure

### Generating SSH Keys

```bash
ssh-keygen -t ed25519
```

Creates two files:
- `id_ed25519` - Private key (keep secret!)
- `id_ed25519.pub` - Public key (share this)

### Using SSH Keys

**Connect with specific key:**
```bash
ssh -i private_key user@host
```

**Copy public key to server:**
```bash
ssh-copy-id user@host
```

### SSH Key Permissions

Private key must have correct permissions:
```bash
chmod 600 private_key
```

Only owner can read/write (600 or stricter).

### Authorized Keys

**~/.ssh/authorized_keys** - Contains public keys allowed to access the server

### Questions

**Check the SSH private key in ~/Public-Crypto-Basics/Task-5. What algorithm does the key use?**
```bash
ssh-keygen -l -f ~/Public-Crypto-Basics/Task-5
```
- Answer: `RSA`

---

## Task 6: Digital Signatures and Certificates

### Digital Signatures

Verify:
- **Authenticity** - Message genuinely from sender
- **Integrity** - Message hasn't been altered

**How it works:**
1. Sender signs message with private key
2. Receiver verifies with sender's public key

### TLS Certificates

Used for HTTPS to verify website identity.

**Certificate contains:**
- Website's public key
- Website's domain name
- Certificate Authority (CA) signature

**Certificate Authorities:**
- Trusted third parties
- Sign certificates to verify authenticity
- Browsers trust major CAs

**Free certificates:**
- [Let's Encrypt](https://letsencrypt.org/) - Free TLS certificates

### Questions

**What does a remote web server use to prove itself to the client?**
- Answer: `Certificate`

**What would you use to get a free TLS certificate for your website?**
- Answer: `Let's Encrypt`

---

## Task 7: PGP and GPG

### What are PGP and GPG?

**PGP (Pretty Good Privacy)** - Software for encryption and digital signatures

**GPG (GnuPG)** - Open-source implementation of OpenPGP standard

### GPG Uses

- Encrypt emails
- Sign emails (verify integrity)
- Encrypt files
- Verify file authenticity

### Generating GPG Keys

```bash
gpg --full-gen-key
```

Choose:
- Algorithm (ECC recommended)
- Key expiration
- Your name and email

### GPG Operations

**Import key:**
```bash
gpg --import backup.key
```

**Decrypt message:**
```bash
gpg --decrypt message.gpg
```

**Encrypt message:**
```bash
gpg --encrypt --recipient user@email.com message.txt
```

**Sign message:**
```bash
gpg --sign message.txt
```

### GPG in CTFs

Protected keys can be cracked with:
- John the Ripper
- gpg2john

### Questions

**Use GPG to decrypt the message in ~/Public-Crypto-Basics/Task-7. What secret word does the message hold?**
```bash
gpg --decrypt ~/Public-Crypto-Basics/Task-7
```
- Answer: `Pineapple`

---

## Task 8: Conclusion

Great job learning public key cryptography fundamentals!

**No answer needed**

---

## Quick Reference

### Asymmetric vs Symmetric

| Type | Keys | Speed | Use Case |
|------|------|-------|----------|
| Symmetric | 1 (shared) | Fast | Bulk encryption |
| Asymmetric | 2 (public/private) | Slow | Key exchange, signatures |

### RSA Components

| Variable | Meaning |
|----------|---------|
| p, q | Prime numbers |
| n | p × q |
| φ(n) | (p-1) × (q-1) |
| e | Public exponent |
| d | Private exponent |
| m | Plaintext message |
| c | Ciphertext |

### SSH Commands

| Command | Purpose |
|---------|---------|
| `ssh-keygen -t ed25519` | Generate key pair |
| `ssh -i key user@host` | Connect with key |
| `ssh-copy-id user@host` | Copy public key |
| `chmod 600 private_key` | Set correct permissions |

### GPG Commands

| Command | Purpose |
|---------|---------|
| `gpg --full-gen-key` | Generate key pair |
| `gpg --import key` | Import key |
| `gpg --decrypt file.gpg` | Decrypt file |
| `gpg --encrypt file` | Encrypt file |
| `gpg --sign file` | Sign file |

---

## Key Takeaways

- **Asymmetric encryption** uses two keys: public and private
- **Public key** encrypts, **private key** decrypts
- **RSA** based on difficulty of factoring large numbers
- **Diffie-Hellman** allows key exchange without transmitting the key
- **SSH** uses public key authentication for secure remote access
- **Certificates** verify website identity (HTTPS)
- **GPG** encrypts and signs emails/files
- **Let's Encrypt** provides free TLS certificates
- Asymmetric crypto is slower but essential for secure key exchange

Public key cryptography is the foundation of modern internet security! 🔐
