# Cryptography Basics - TryHackMe

> Learn the fundamentals of cryptography and symmetric encryption

**Room Link:** https://tryhackme.com/room/cryptographybasics

<img width="1570" height="925" alt="image" src="https://github.com/user-attachments/assets/cee12640-201c-43ba-8871-509267382c1d" />

---

## Task 1: Introduction

Get ready to learn about cryptography - the science of securing information!

**No answer needed**

---

## Task 2: Importance of Cryptography

### Why Cryptography Matters

Cryptography protects sensitive information in everyday life:
- Online banking transactions
- Secure messaging (WhatsApp, Signal)
- Password storage
- Credit card payments
- HTTPS websites

### Industry Standards

Organizations must follow security standards when handling sensitive data.

**PCI DSS (Payment Card Industry Data Security Standard)** - Required for handling credit card information securely.

### Questions

**What is the standard required for handling credit card information?**
- Answer: `PCI DSS`

---

## Task 3: Plaintext to Ciphertext

### Basic Cryptography Concepts

**Plaintext** - The original readable message
- Example: "HELLO WORLD"

**Ciphertext** - The encrypted, unreadable message
- Example: "KHOOR ZRUOG"

**Encryption** - The process of converting plaintext to ciphertext
- Uses an algorithm and a key

**Decryption** - The process of converting ciphertext back to plaintext
- Uses the same or related key

### The Process

```
Plaintext → [Encryption + Key] → Ciphertext
Ciphertext → [Decryption + Key] → Plaintext
```

### Questions

**What do you call the encrypted plaintext?**
- Answer: `ciphertext`

**What do you call the process that returns the plaintext?**
- Answer: `decryption`

---

## Task 4: Historical Ciphers

### Caesar Cipher

One of the oldest and simplest ciphers, named after Julius Caesar.

**How it works:**
- Shifts each letter by a fixed number of positions
- Example with shift of 3:
  - A → D
  - B → E
  - C → F

**Decryption:**
- Shift back by the same amount
- Or try all 25 possible shifts (brute force)

### Example

Encrypted: `XRPCTCRGNEI`

To decrypt, try different shifts or use online tools like [CyberChef](https://gchef.org/) or [dCode](https://www.dcode.fr/caesar-cipher).

Shift by 17 (or -9):
- X → I
- R → C
- P → A
- etc.

### Questions

**Knowing that `XRPCTCRGNEI` was encrypted using Caesar Cipher, what is the original plaintext?**
- Answer: `ICANENCRYPT`

---

## Task 5: Types of Encryption

### Symmetric Encryption

Uses the **same key** for encryption and decryption.
- Faster than asymmetric
- Key must be shared securely

### Common Algorithms

**DES (Data Encryption Standard)**
- Created in 1970s
- 56-bit key
- **Insecure** - can be cracked
- No longer recommended

**3DES (Triple DES)**
- Applies DES three times
- More secure than DES
- Still being phased out

**AES (Advanced Encryption Standard)**
- Adopted in 2001
- Uses 128, 192, or 256-bit keys
- **Current standard** - very secure
- Used everywhere (HTTPS, VPNs, file encryption)

### Questions

**Should you trust DES? (Yea/Nay)**
- Answer: `Nay`

**When was AES adopted as an encryption standard?**
- Answer: `2001`

---

## Task 6: Basic Math

Cryptography relies on mathematical operations.

### XOR (Exclusive OR) - ⊕

Returns 1 if bits are different, 0 if same:

| A | B | A ⊕ B |
|---|---|-------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

**Properties:**
- A ⊕ A = 0
- A ⊕ 0 = A
- A ⊕ B = B ⊕ A (commutative)

### Modulo (%)

Returns the remainder after division.

**Examples:**
- 10 % 3 = 1 (10 ÷ 3 = 3 remainder 1)
- 60 % 12 = 0 (60 ÷ 12 = 5 remainder 0)
- 7 % 5 = 2 (7 ÷ 5 = 1 remainder 2)

### Questions

**What's 1001 ⊕ 1010?**
```
  1001
⊕ 1010
------
  0011
```
- Answer: `0011`

**What's 118613842 % 9091?**
```
118613842 ÷ 9091 = 13045 remainder 3565
```
- Answer: `3565`

**What's 60 % 12?**
```
60 ÷ 12 = 5 remainder 0
```
- Answer: `0`

---

## Task 7: Summary

### Key Concepts Learned

✅ **Cryptography** protects sensitive information
✅ **PCI DSS** is the standard for credit card security
✅ **Plaintext** → Encryption → **Ciphertext**
✅ **Decryption** converts ciphertext back to plaintext
✅ **Caesar Cipher** - Historical shift cipher
✅ **DES** - Insecure, don't use
✅ **AES** - Modern, secure standard (adopted 2001)
✅ **XOR** and **Modulo** - Basic crypto math operations

**No answer needed** - Make sure you understand these concepts before moving on!

---

## Quick Reference

### Encryption Algorithms

| Algorithm | Year | Key Size | Status |
|-----------|------|----------|--------|
| DES | 1970s | 56-bit | ❌ Insecure |
| 3DES | 1990s | 168-bit | ⚠️ Being phased out |
| AES | 2001 | 128/192/256-bit | ✅ Secure |

### Key Terms

| Term | Definition |
|------|------------|
| **Plaintext** | Original readable message |
| **Ciphertext** | Encrypted unreadable message |
| **Encryption** | Converting plaintext to ciphertext |
| **Decryption** | Converting ciphertext to plaintext |
| **Key** | Secret value used in encryption/decryption |
| **Symmetric** | Same key for encryption and decryption |
| **Caesar Cipher** | Simple shift cipher |
| **XOR** | Exclusive OR operation |
| **Modulo** | Remainder after division |

### Math Operations

**XOR (⊕):**
- 0 ⊕ 0 = 0
- 0 ⊕ 1 = 1
- 1 ⊕ 0 = 1
- 1 ⊕ 1 = 0

**Modulo (%):**
- Returns remainder
- Example: 17 % 5 = 2

---

## Key Takeaways

- **Cryptography** is essential for protecting data
- **Never use DES** - it's outdated and insecure
- **AES** is the current standard for symmetric encryption
- **Caesar Cipher** is easy to break (good for learning)
- **XOR** and **Modulo** are fundamental operations in crypto
- **PCI DSS** compliance is required for handling credit cards

Understanding these basics is the foundation for advanced cryptography! 🔒
