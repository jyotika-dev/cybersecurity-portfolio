# Advent of Cyber 2025 Day 19 - TryHackMe

> ICS/Modbus - Claus for Concern

**Day 19 Link:** https://tryhackme.com/room/ICS-modbus-aoc2025-g3m6n9b1v4

---

## Story

**Scenario:** TBFC's drone system compromised

**Problem:** Drones loading eggs instead of Christmas presents

**System:** Industrial Control System (ICS) using Modbus protocol

**Goal:** Restore system to deliver Christmas presents

---

## SCADA/Modbus Basics

**What is SCADA:**
- Supervisory Control and Data Acquisition
- Controls industrial systems
- Monitors equipment
- Used in power plants, factories, etc.

**What is Modbus:**
- Communication protocol for ICS
- Client-server architecture
- Reads/writes data to devices
- **Default port:** 502

**Components:**
- **Holding Registers (HR):** Store values
- **Coils (C):** Boolean flags (True/False)
- **PLC:** Programmable Logic Controller

---

## Questions & Answers

### Q1: What port is commonly used by Modbus TCP?

**Answer:** `502`

---

## Challenge Walkthrough

### Step 1: Reconnaissance Script

**Create script:**
```bash
nano reconnaissance.py
```

**Paste the provided Python script**

**Run script:**
```bash
python3 reconnaissance.py
```

**What it shows:**
- Current system status
- Package type (eggs = 1)
- Protection enabled (C11 = True)
- Self-destruct armed
- CCTV shows drones loading eggs

---

### Step 2: Understand System State

**Current values:**

| Register/Coil | Value | Meaning |
|---------------|-------|---------|
| HR0 | 1 | Eggs (should be 0 for Christmas) |
| HR4 | 666 | Eggsploit signature detected |
| C10 | False | Inventory verification disabled |
| C11 | True | Protection active (trap!) |
| C12 | ? | Emergency dump |
| C13 | False | Audit logging disabled |
| C14 | False | Christmas not restored |
| C15 | True | Self-destruct armed |

**Key insight:** C11 protection is a trap! Must disable BEFORE changing HR0.

---

### Step 3: Restoration Script

**Create script:**
```bash
nano restore_christmas.py
```

**Paste the provided Python script**

**What it does:**
1. Check current state
2. **Disable protection (C11 = False)** ← Critical!
3. Change package type (HR0 = 0)
4. Enable inventory verification (C10 = True)
5. Enable audit logging (C13 = True)
6. Verify restoration
7. Read flag from registers

**Run script:**
```bash
python3 restore_christmas.py
```

---

### Q2: What's the flag?

**After script runs successfully:**

**Answer:** `THM{eGgMas0V3r}`

**CCTV verification:** Drones now loading Christmas presents!

---

## Quick Reference

### Modbus Register Types

**Holding Registers (HR):**
- Store numeric values
- Read/write operations
- Example: HR0 = package type

**Coils (C):**
- Boolean values (True/False)
- On/off switches
- Example: C11 = protection enabled

### Package Types

```
0 = Christmas presents
1 = Eggs
2 = Baskets
```

### Delivery Zones

```
1-9 = Normal zones
10  = Ocean dump
```

---

## Python Modbus Commands

### Read Operations

```python
# Read holding register
result = client.read_holding_registers(address=0, count=1, slave=UNIT_ID)
value = result.registers[0]

# Read coil
result = client.read_coils(address=10, count=1, slave=UNIT_ID)
status = result.bits[0]
```

### Write Operations

```python
# Write register
client.write_register(0, 0, slave=UNIT_ID)  # HR0 = 0

# Write coil
client.write_coil(11, False, slave=UNIT_ID)  # C11 = False
```

---

## Attack Sequence

### What Happened

**Malhare's sabotage:**
```
1. Changed HR0 to 1 (eggs)
2. Disabled C10 (inventory check)
3. Disabled C13 (audit logging)
4. Enabled C11 (protection trap)
5. Armed C15 (self-destruct)
```

### The Trap

**Protection mechanism (C11):**
- If enabled and HR0 changes
- Triggers self-destruct
- System destroyed

**Correct fix order:**
```
1. Disable C11 first ← Must do this!
2. Then change HR0
3. Enable security features
4. Verify restoration
```

---

## Script Breakdown

### Reconnaissance Script

**Purpose:** Gather intelligence

**What it checks:**
- All holding registers
- All coils
- Threat assessment
- Current package type

**Safe:** Only reads, doesn't write

### Restoration Script

**Purpose:** Fix the system

**Steps:**
1. **Verify state** - Check what's wrong
2. **Disable trap** - C11 = False
3. **Fix packages** - HR0 = 0
4. **Enable security** - C10, C13 = True
5. **Verify** - Check C14 = True
6. **Get flag** - Read from HR20-31

---

## Important Notes

### Critical Order

**Wrong way (triggers trap):**
```python
# Change HR0 while C11 = True
client.write_register(0, 0)  # BOOM!
```

**Right way:**
```python
# Disable C11 first
client.write_coil(11, False)   # Safe now
client.write_register(0, 0)    # Can change
```

### Flag Location

**Registers:** HR20 through HR31

**Encoding:** Two bytes per register

**Decoding:**
```python
# Extract bytes from registers
flag_bytes = []
for reg in registers:
    flag_bytes.append(reg >> 8)    # High byte
    flag_bytes.append(reg & 0xFF)  # Low byte

# Convert to string
flag = ''.join(chr(b) for b in flag_bytes if b != 0)
```

---

## Bonus: Trigger the Trap

**What happens if you don't disable C11:**

**Modify script to:**
1. Leave C11 = True
2. Change HR0 = 0
3. Watch CCTV

**Result:** Self-destruct activates!

---

## Key Takeaways

- **Modbus** port is **502**
- **ICS systems** control physical devices
- **Read before write** - reconnaissance first
- **Order matters** - disable traps first
- **Protection mechanisms** can be traps
- **Registers** store values
- **Coils** are boolean flags
- **Always verify** after changes
- **Industrial systems** have real-world impact
- **Security** is critical for ICS

---

Happy hunting!
