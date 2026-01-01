# Advent of Cyber 2025 Day 20 - TryHackMe

> Race Conditions - Toy to The World

**Day 20 Link:** https://tryhackme.com/room/race-conditions-aoc2025-d7f0g3h6j9

---

## Story

**Scenario:** TBFC's SleighToy limited edition (only 10 pieces)

**Problem:** More than 10 people bought it successfully

**Exploit:** Multiple requests at exact same time

**Goal:** Exploit race condition to buy more than available stock

---

## Race Condition Basics

**What is it:**
- Multiple actions happen simultaneously
- System can't handle concurrent requests
- Order of execution matters
- Results in unexpected behavior

**Example:**
```
User 1 checks stock: 1 item left ✓
User 2 checks stock: 1 item left ✓
User 1 buys item
User 2 buys item
Result: Both succeed, stock = -1
```

**Types:**

| Type | Description |
|------|-------------|
| TOCTOU | Check then use, data changes between |
| Shared Resource | Multiple users modify same data |
| Atomicity Violation | Operation split into parts |

---

## Setup

### Burp Suite Configuration

1. Open Firefox
2. Click FoxyProxy icon
3. Select Burp profile
4. Launch Burp Suite
5. Proxy tab → Intercept OFF

---

## Challenge Walkthrough

### Part 1: SleighToy Exploit

#### Step 1: Normal Purchase

**Access webapp:**
```
http://MACHINE_IP
```

**Login:**
- Username: `attacker`
- Password: `attacker@123`

**Make purchase:**
1. Add "SleighToy Limited Edition" to cart
2. Go to checkout
3. Click "Confirm & Pay"
4. Note the order ID

#### Step 2: Capture Request

**In Burp Suite:**
1. Proxy tab → HTTP history
2. Find `POST /process_checkout` request
3. Right-click → Send to Repeater

#### Step 3: Create Tab Group

**In Repeater:**
1. Right-click request tab
2. "Add tab to group"
3. "Create tab group"
4. Name it "cart"

#### Step 4: Duplicate Requests

**Create copies:**
1. Right-click request tab
2. Select "Duplicate tab"
3. Create 15 copies total

(More than available stock)

#### Step 5: Send Parallel Requests

**Execute attack:**
1. Select all tabs in group
2. Click "Send group in parallel (last-byte sync)"

**Result:**
- Multiple orders confirmed
- Stock goes negative
- Race condition exploited!

#### Step 6: Get Flag

**Check orders page:**

Visit orders → See multiple successful orders

### Q1: Flag for SleighToy?

**Answer:** `THM{WINNER_OF_R@CE007}`

---

### Part 2: Bunny Plush Exploit

#### Step 1: New Purchase

**Add to cart:**
- "Bunny Plush (Blue)"
- Go to checkout
- Capture request in Burp

#### Step 2: Close Previous Group

**In Repeater:**
1. Close "cart" tab group
2. Create new group for Bunny Plush

#### Step 3: Repeat Attack

**Same steps:**
1. Send to Repeater
2. Create tab group "Cart"
3. Duplicate 10+ times
4. Send in parallel
5. Check orders page

**Result:**
- Multiple Bunny Plush orders
- Negative stock
- Flag obtained!

### Q2: Flag for Bunny Plush?

**Answer:** `THM{WINNER_OF_Bunny_R@ce}`

---

## Quick Reference

### Burp Suite Steps

**Capture request:**
```
1. Proxy → HTTP history
2. Find POST /process_checkout
3. Right-click → Send to Repeater
```

**Create attack:**
```
1. Repeater → Create tab group
2. Duplicate tab multiple times
3. Send group in parallel
```

**Parallel send:**
```
Select: "Send group in parallel (last-byte sync)"
This ensures all requests hit at same time
```

---

## Attack Visualization

### Normal Flow (No Race)

```
Request 1 → Check stock (10) → Buy (9) → Done
Request 2 → Check stock (9) → Buy (8) → Done
Request 3 → Check stock (8) → Buy (7) → Done
```

### Race Condition Flow

```
Request 1 → Check stock (10) ───┐
Request 2 → Check stock (10) ───┤ All check at once
Request 3 → Check stock (10) ───┘

Request 1 → Buy (stock: 9)
Request 2 → Buy (stock: 8)  
Request 3 → Buy (stock: 7)

But all saw stock = 10!
All succeed even if only 10 available
```

---

## Why It Works

**Problem:**
1. Check stock
2. Process payment
3. Update stock

**Time gap between steps!**

**Multiple requests:**
- All check stock before any update
- All see "stock available"
- All process successfully
- Stock becomes negative

---

## Mitigation Strategies

### Atomic Operations

- Perform all steps as single operation
- Check and update together
- No gap between steps

### Database Locks

- Lock stock record during transaction
- Other requests must wait
- Ensures one-at-a-time processing

### Idempotency Keys

- Use unique order IDs
- Prevent duplicate processing
- Check if order exists first

### Rate Limiting

- Limit requests per second
- Block rapid repeated attempts
- Prevent automated attacks

---

## Common Targets

**Where race conditions happen:**

**E-commerce:**
- Limited stock items
- Flash sales
- Discount codes

**Banking:**
- Account transfers
- Balance checks
- Withdrawals

**Gaming:**
- Item duplication
- Currency exploits
- Loot claiming

**Voting/Polls:**
- Multiple votes
- Like/upvote systems

---

## Burp Suite Tips

### Tab Groups

**Why use them:**
- Send multiple requests simultaneously
- Better timing control
- Easier management

**Last-byte sync:**
- Holds all requests
- Releases at exact same time
- Maximizes race condition chance

### Request Count

**How many duplicates:**
- More than available stock
- 10-20 copies usually enough
- Too many = slower processing

---

## Detection Signs

**How to spot race conditions:**

**As attacker:**
- Multiple successful responses
- Negative stock values
- Duplicate transactions
- Inconsistent data

**As defender:**
- Multiple orders same millisecond
- Negative inventory
- Concurrent transaction logs
- Database deadlocks

---

## Key Concepts

### Concurrency

**Definition:**
- Multiple things happening at once
- Need proper synchronization
- Database transactions help

### Atomicity

**Definition:**
- All-or-nothing operation
- Either completes fully or not at all
- No partial states

### Critical Section

**Definition:**
- Code that accesses shared resource
- Must be protected
- Only one process at a time

---

## Real-World Examples

**Past incidents:**

**Airline tickets:**
- Oversold flights
- Multiple bookings same seat

**Concert tickets:**
- More tickets sold than seats
- Duplicate confirmations

**Limited editions:**
- Sold out instantly
- More orders than stock

**Banking:**
- Double withdrawals
- Negative balances

---

## Key Takeaways

- **Race conditions** occur with concurrent requests
- **Timing** is critical for exploitation
- **Burp Suite** enables parallel requests
- **Tab groups** send requests simultaneously
- **Last-byte sync** maximizes success
- **Atomic operations** prevent issues
- **Database locks** ensure consistency
- **Always validate** stock before commit
- **Rate limiting** reduces risk
- **Real impact** on business operations

---

Happy hunting!
