<img width="1288" height="857" alt="image" src="https://github.com/user-attachments/assets/862e291b-bc78-4433-933c-a95e0189fd47" /># Phishing Prevention - TryHackMe

> Learn email security controls to prevent and detect phishing attacks

**Room Link:** https://tryhackme.com/room/phishingemails4gkxh


<img width="1288" height="857" alt="image" src="https://github.com/user-attachments/assets/1bf44ab8-af93-4188-a285-64743728b1da" />




---

## Task 1: Introduction

Phishing remains a top initial access vector for attackers. Learn security measures to prevent, detect, and mitigate phishing threats.

**Learning objectives:**
- Email security controls (SPF, DKIM, DMARC, S/MIME)
- Analyze SMTP traffic
- Anti-phishing protection measures

**No answer needed**

---

## Task 2: Sender Policy Framework (SPF)

### What is SPF?

**SPF** authenticates the sender by verifying if the sending mail server is authorized to send email for a specific domain.

**How it works:**
1. Email is sent
2. Receiving server checks sender's SPF record
3. Verifies if sending IP is authorized
4. Takes action based on result

### SPF Verification Results

| Result | Action | Meaning |
|--------|--------|---------|
| Pass, Neutral, None | Accept | Process email normally |
| SoftFail, PermError | Flag | Mark suspicious but allow |
| Fail, TempError | Reject | Discard immediately |

### SPF Record Format

**Example:** `v=spf1 ip4:127.0.0.1 include:_spf.google.com -all`

**Components:**
- `v=spf1` - SPF record version
- `ip4:127.0.0.1` - Authorized IPv4 address
- `include:_spf.google.com` - Authorized domain
- `-all` - Reject non-authorized emails

### TryHackMe SPF Record Example

**Authorized domains:**
- _spf.google.com
- email.chargebee.com
- 7168674.spf05.hubspotemail.net

**Note:** All IPs from these domains are authorized senders.

### Questions

**Based on TryHackMe's SPF record above, how many domains are authorized to send email on its behalf?**

Count the domains listed in the include statements.

- Answer: `3`

---

**What is the intended action of an email that returns a `SoftFail` verification result?**

Check the verification results table.

SoftFail = suspicious but still processed.

- Answer: `Flag`

---

## Task 3: DomainKeys Identified Mail (DKIM)

### What is DKIM?

**DKIM** uses digital signatures to verify email authenticity. Survives forwarding, making it superior to SPF.

**How it works:**
1. Sender's server signs email with private key
2. Digital signature added to email header
3. Receiving server retrieves public key from DNS
4. Verifies signature matches
5. Authenticates or rejects email

### DKIM Record Format

**Example:** `v=DKIM1; k=rsa; p=<public_key>`

**Components:**
- `v=DKIM1` - DKIM version (optional)
- `k=rsa` - Encryption algorithm (RSA standard)
- `p=` - Public key for verification

### DKIM Results

**Pass:** Signature valid, email authenticated

**Fail/PermError:** 
- Invalid signature
- Missing DNS record
- Forwarding modified email
- Misconfiguration

### Questions

**Based on the sample header above, what is the reason for the `permerror`?**

Check the DKIM header for error details

- Answer: `no key for signature`

---

## Task 4: DMARC

### What is DMARC?

**DMARC** ties SPF and DKIM together using "alignment" - ensures sender's domain matches verified domains.

**Purpose:**
- Builds on SPF and DKIM
- Defines policy for failed checks
- Provides reporting capability

### DMARC Record Format

**Example:** `v=DMARC1; p=quarantine; rua=mailto:postmaster@website.com`

**Components:**
- `v=DMARC1` - DMARC version (required)
- `p=quarantine` - Policy (what to do with failures)
- `rua=mailto:email@domain.com` - Where to send reports

### DMARC Policies

| Policy | Action |
|--------|--------|
| `none` | Monitor only, no action |
| `quarantine` | Move to spam folder |
| `reject` | Block email completely |

### Questions

**Which DMARC policy provides the greatest amount of protection by blocking emails that fail the DMARC check?**

Check the policies table.

- Answer: `p=reject`

---

## Task 5: S/MIME

### What is S/MIME?

**S/MIME** = Secure/Multipurpose Internet Mail Extensions

Standard protocol for digitally signed and encrypted messages.

### Two Main Components

**1. Digital Signatures**
- Authentication - Confirms sender identity
- Non-repudiation - Sender can't deny sending
- Data Integrity - Detects tampering

**2. Encryption**
- Confidentiality - Only recipient can read
- Data Integrity - Detects changes in transit

### How It Works

**Public Key Cryptography:**
1. Bob gets digital certificate with public key
2. Bob signs email with his private key
3. Mary decrypts with Bob's public key
4. Mary sends her certificate to Bob
5. Both have each other's certificates

**Future emails:** Both can sign and encrypt messages.

### Questions

**Which S/MIME component ensures that only the intended recipient can read the contents of an email message?**

Check the two main components.

Which one provides confidentiality?

- Answer: `Encryption`

---

## Task 6: Analyzing SMTP Responses

### SMTP Response Codes

**Common codes:**
- **220** - Service ready
- **250** - OK, action completed
- **421** - Service unavailable
- **450** - Mailbox unavailable
- **550** - Mailbox not found
- **552** - Message size exceeded
- **554** - Transaction failed

### Questions

**Which Wireshark filter can you use to narrow down your results based on SMTP response codes?**

SMTP protocol has specific filter syntax.

Filter for SMTP responses.

- Answer: `smtp.response.code`

---

**How many packets in the capture contain the SMTP response code `220 Service ready`?**

Use filter: `smtp.response.code == 220`

Count the results.

- Answer: `19`

---

**One SMTP response indicates that an email was blocked by `spamhaus.org`. What response code did the server return?**

Search for "spamhaus" in packet details.

Check the response code.

- Answer: `553`

---

**Based on the packet from the previous question, what is the full `Response code:` message?**

Look at the full response text in packet details.

Copy the complete message.

- Answer: `Requested action not taken: mailbox name not allowed (553)`

---

**Search for response code `552`. How many messages were blocked for presenting potential security issues?**

Use filter: `smtp.response.code == 552`

Count the packets.

- Answer: `6`

---

## Task 7: Inspecting Emails and Attachments

### Internet Message Format (IMF)

**Used to examine:**
- Sender and recipient fields
- Content type
- Attachments
- Message headers

### Questions

**How many SMTP packets are available for analysis?**

Use filter: `smtp`

Count total packets.

- Answer: `512`

---

**What is the name of the attachment in packet `270`?**

Navigate to packet 270.

Look for attachment name in IMF.

- Answer: `document.zip`

---

**According to the message in packet `270`, which Host IP address is not responding, making the message undeliverable?**

Read the message body in packet 270.

Find the unresponsive host IP.

- Answer: `212.253.25.152`

---

**By filtering for `imf`, which email client was used to send the message containing the attachment `attachment.scr`?**

Use filter: `imf`

Look for User-Agent or X-Mailer field.

Find the client that sent attachment.scr.

- Answer: `Microsoft Outlook Express 6.00.2600.0000`

---

**Which type of encoding is used for this potentially malicious attachment?**

Check the Content-Transfer-Encoding field.

Common encoding for attachments.

- Answer: `base64`

---

## Task 8: How Organizations Stop Phishing

### Technical Defenses

**Email Filtering:**
- IP/domain reputation checks
- Block or quarantine suspicious messages

**Secure Email Gateways (SEGs):**
- Detect impersonation
- Identify spoofing
- Catch advanced phishing

**Link Rewriting:**
- Replace suspicious URLs
- Redirect through safe proxy
- Scan before user clicks

**Sandboxing:**
- Isolate suspicious files/links
- Test in virtual environment
- Detect malicious behavior safely

### User-Facing Tools

**Trust & Warning Indicators:**
- "External Sender" banners
- "Suspicious Link" warnings
- Trusted sender markers

**Phishing Reporting:**
- In-email report buttons
- Easy escalation to security team

**User Awareness Training:**
- Identify phishing attempts
- Recognize social engineering
- Safe email practices

**Phishing Simulations:**
- Controlled phishing campaigns
- Test employee awareness
- Reinforce training

### Questions

**A security team wants to implement a control to detect hidden malware inside email attachments. They need a way to analyze suspicious files without risking infection on real systems. Which protective technique would allow them to observe a file's behavior safely?**

Which technique isolates and tests files in virtual environment?

- Answer: `Sandboxing`

---

## Task 9: Conclusion

**No answer needed**

---

## Quick Reference

### Email Authentication Comparison

| Protocol | Purpose | What It Checks |
|----------|---------|----------------|
| **SPF** | Sender authorization | Is sending IP allowed? |
| **DKIM** | Message integrity | Valid digital signature? |
| **DMARC** | Policy enforcement | SPF and DKIM alignment? |
| **S/MIME** | Encryption & signing | End-to-end security |


### DKIM Components

```
Private Key (on sending server)
    ↓
Digital Signature (added to email)
    ↓
Public Key (in DNS DKIM record)
    ↓
Verification (receiving server checks)
```

### DMARC Policies

**none:**
- Monitor only
- No action taken
- Generate reports

**quarantine:**
- Move to spam folder
- User can review
- Moderate protection

**reject:**
- Block completely
- Never reaches inbox
- Maximum protection

### SMTP Response Code Categories

| Code Range | Meaning |
|------------|---------|
| 2xx | Success |
| 3xx | Intermediate success |
| 4xx | Temporary failure |
| 5xx | Permanent failure |

### Common SMTP Codes

- **220** - Service ready
- **250** - Requested action completed
- **354** - Start mail input
- **421** - Service not available
- **450** - Mailbox unavailable
- **451** - Local error
- **452** - Insufficient storage
- **550** - Mailbox unavailable
- **551** - User not local
- **552** - Exceeded storage
- **553** - Mailbox name invalid
- **554** - Transaction failed

### Wireshark Filters

**SMTP traffic:**
```
smtp
```

**Specific response code:**
```
smtp.response.code == 220
```

**IMF (email content):**
```
imf
```

**Email attachments:**
```
imf.content_type contains "attachment"
```

### Attachment Encoding

**base64:**
- Most common
- Binary data to text
- Email-safe format

**quoted-printable:**
- Human-readable
- ASCII characters
- Less common

### Phishing Defense Layers

```
[Technical Controls]
    ↓
Email Filtering (IP/Domain reputation)
    ↓
SEG (Advanced detection)
    ↓
Link Rewriting (URL protection)
    ↓
Sandboxing (Behavior analysis)
    ↓
[User Controls]
    ↓
Warning Indicators (Visual cues)
    ↓
Reporting Tools (Easy escalation)
    ↓
Training (Awareness)
    ↓
Simulations (Practice)
```

### Email Header Analysis Checklist

**Authentication:**
- [ ] SPF result
- [ ] DKIM result
- [ ] DMARC result

**Sender verification:**
- [ ] From address
- [ ] Return-Path
- [ ] Reply-To
- [ ] Originating IP

**Content:**
- [ ] Subject line
- [ ] Attachments
- [ ] Links/URLs
- [ ] Encoding type

### Red Flags

**SPF Failures:**
- Unauthorized sending IP
- Missing SPF record
- Too many DNS lookups

**DKIM Failures:**
- Invalid signature
- Missing public key
- Modified message

**DMARC Failures:**
- SPF and DKIM both fail
- Domain alignment fails
- Policy not enforced

---

## Key Takeaways

- **SPF** verifies sending server authorization
- **DKIM** uses digital signatures for integrity
- **DMARC** enforces alignment policies
- **S/MIME** provides end-to-end encryption
- **3 domains** authorized in TryHackMe SPF
- **SoftFail** flags but accepts email
- **Reject policy** provides maximum protection
- **Encryption** ensures message confidentiality
- **smtp.response.code** filters SMTP responses
- **220** indicates service ready
- **554** typically blocks spam
- **552** blocks security issues
- **base64** encoding common for attachments
- **Sandboxing** tests files safely
- **Layer defenses** for comprehensive protection

Defense in depth stops phishing at multiple levels
Happy defending!
