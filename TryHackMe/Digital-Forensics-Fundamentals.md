# Digital Forensics Fundamentals - TryHackMe

> Learn about digital forensics and investigate cyber crimes

**Room Link:** https://tryhackme.com/room/digitalforensicsfundamentals

<img width="1758" height="847" alt="image" src="https://github.com/user-attachments/assets/9451bc8c-d110-409a-8524-3a10ca3fdd0f" />

---

## Task 1: Introduction to Digital Forensics

### What is Digital Forensics?

Digital forensics is the branch of forensics that investigates cyber crimes using digital devices.

### Real-World Example

**The Bank Robbery Case:**

Law enforcement raided a bank robber's home and found:
- Laptop
- Mobile phone
- Hard drive
- USB drive

The digital forensics team investigated and found:
- Digital map of the bank (planning)
- Entrance and escape routes document
- Security controls bypass plans
- Photos and videos of previous robberies
- Illegal chat groups and call records

This evidence helped convict the robber!

### What is Cyber Crime?

Any criminal activity conducted on or using a digital device:
- Hacking
- Identity theft
- Fraud
- Cyberstalking
- Data theft

### Learning Objectives

- Phases of digital forensics
- Types of digital forensics
- Evidence acquisition procedures
- Windows forensics
- Solving forensics cases

### Questions

**Which team was handed the case by law enforcement?**
- Answer: `digital forensics`

---

## Task 2: Digital Forensics Methodology

### NIST Framework

The National Institute of Standards and Technology (NIST) defines four phases of digital forensics:

### Phase 1: Collection

**What happens:**
- Identify all digital devices
- Collect data from crime scene
- Maintain proper documentation
- Ensure original data isn't tampered

**Common devices found:**
- Personal computers
- Laptops
- Digital cameras
- USB drives
- Mobile phones
- External hard drives

### Phase 2: Examination

**What happens:**
- Filter collected data
- Extract data of interest
- Remove irrelevant information

**Example:**
- Collected 10,000 photos from camera
- Only need photos from specific date
- Filter and extract only relevant photos

### Phase 3: Analysis

**What happens:**
- Analyze filtered data
- Correlate multiple evidence pieces
- Draw conclusions
- Create timeline of activities

**This is the critical phase!**

### Phase 4: Reporting

**What happens:**
- Prepare detailed report
- Document methodology
- Present findings
- Include recommendations
- Create executive summaries

**Report recipients:**
- Law enforcement
- Executive management
- Legal teams

### Types of Digital Forensics

**Computer Forensics**
- Investigates computers and laptops
- Most common type
- Desktop/laptop systems

**Mobile Forensics**
- Mobile devices investigation
- Call records
- Text messages
- GPS locations
- App data

**Network Forensics**
- Investigates networks
- Network traffic logs
- Router logs
- Firewall logs

**Database Forensics**
- Database intrusions
- Data modification
- Data exfiltration

**Cloud Forensics**
- Cloud infrastructure data
- Sometimes tricky (limited evidence)
- Cloud storage investigation

**Email Forensics**
- Email communication
- Phishing campaigns
- Fraudulent emails

### Questions

**Which phase of digital forensics is concerned with correlating the collected data to draw any conclusions from it?**
- Answer: `Analysis`

---

**Which phase of digital forensics is concerned with extracting the data of interest from the collected evidence?**
- Answer: `Examination`

---

## Task 3: Evidence Acquisition

### Best Practices for Collecting Evidence

Evidence acquisition is critical! Follow these practices:

### 1. Proper Authorization

**Why it's important:**
- Legal requirements
- Court admissibility
- Protects private data
- Follows law limits

**Without authorization:**
- Evidence inadmissible in court
- Legal consequences
- Investigation compromised

### 2. Chain of Custody

**What is it?**
A formal document tracking evidence details.

**Information tracked:**
- Description of evidence (name, type)
- Who collected it
- Date and time of collection
- Storage location
- Access times
- Who accessed evidence

**Why it matters:**
- Creates evidence trail
- Proves integrity
- Prevents tampering
- Maintains accountability
- Court admissibility

**Example scenario:**
Without chain of custody, if evidence goes missing, no one can be held accountable!

### 3. Write Blockers

**What is a write blocker?**
A tool that prevents any changes to original evidence.

**Problem without write blockers:**
- Forensic workstation might alter files
- Timestamps get changed
- Metadata modified
- Evidence integrity compromised

**With write blockers:**
- Original evidence stays intact
- No alterations possible
- Data integrity maintained
- Analysis results accurate

**Essential for digital forensics team!**

### Questions

**Which tool is used to ensure data integrity during the collection?**
- Answer: `write blocker`

---

**What is the name of the document that has all the details of the collected digital evidence?**
- Answer: `chain of custody`

---

## Task 4: Windows Forensics

### Why Windows Forensics?

Windows is the most common OS found in:
- Personal computers
- Laptops
- Business systems
- Criminal investigations

### Types of Forensic Images

### 1. Disk Image

**Contains:**
- All data on storage device (HDD/SSD)
- Non-volatile data (survives restart)

**Examples:**
- Media files
- Documents
- Browser history
- Installed programs
- User files

### 2. Memory Image

**Contains:**
- Data in RAM
- Volatile data (lost on restart)

**Examples:**
- Open files
- Running processes
- Network connections
- Encryption keys
- Passwords

**Priority:** Take memory image FIRST before system powers off!

### Popular Windows Forensics Tools

### FTK Imager

**Purpose:** Disk image acquisition and analysis

**Features:**
- User-friendly GUI
- Multiple image formats
- Can analyze disk images
- Both acquisition and analysis

**Use case:** Create disk images from Windows systems

### Autopsy

**Purpose:** Digital forensics platform

**Features:**
- Open-source
- Extensive image analysis
- Keyword search
- Deleted file recovery
- File metadata analysis
- Extension mismatch detection

**Use case:** Import and analyze disk images

### DumpIt

**Purpose:** Memory image acquisition

**Features:**
- Command-line interface
- Quick memory capture
- Multiple formats
- Easy to use

**Use case:** Create memory dumps from Windows

### Volatility

**Purpose:** Memory image analysis

**Features:**
- Open-source
- Powerful plugins
- Multiple OS support (Windows, Linux, macOS, Android)
- Extensive artifact analysis

**Use case:** Analyze memory dumps for processes, network connections, etc.

### Questions

**Which type of forensic image is taken to collect the volatile data from the operating system?**
- Answer: `Memory Image`

---

## Task 5: Practical Example of Digital Forensics

### The Case: Kidnapped Cat (Gado)

The kidnapper sent a ransom document. Let's investigate!

### Setup

**On AttackBox:**
```bash
cd /root/Rooms/introdigitalforensics
```

Files provided:
- `ransom-letter.pdf` - Ransom document
- `letter-image.jpg` - Image from kidnapper

### What is Metadata?

Data about data! Every file contains hidden information:
- Creation date
- Modification date
- Author name
- GPS coordinates (photos)
- Camera model
- Software used

### Analyzing PDF Files

**Tool:** `pdfinfo`

**Shows:**
- Creator software
- Producer
- Creation date
- Modification date
- Page count
- File size

### Analyzing Image Files

**Tool:** `exiftool`

**Shows:**
- Camera model
- Date and time
- GPS coordinates
- Camera settings (ISO, aperture, etc.)
- Smartphone model

### EXIF Data

**What is EXIF?**
Exchangeable Image File Format - metadata standard for images.

**Common metadata:**
- Camera/smartphone model
- Capture date and time
- Photo settings
- GPS coordinates (latitude/longitude)

**GPS coordinates reveal where photo was taken!**

---

### Question 1: Find the PDF author

**Command:**
```bash
pdfinfo ransom-letter.pdf
```

Look for the "Author" field in the output.

**Output:**
```
Author:         Ann Gree Shepherd
```

- Answer: `Ann Gree Shepherd`

---

### Question 2: Find where the image was taken

**Command:**
```bash
exiftool letter-image.jpg | grep GPS
```

or

```bash
exiftool letter-image.jpg
```

Look for GPS Position in the output.

**Output:**
```
GPS Position : 51 deg 31' 4.00" N, 0 deg 5' 48.30" W
```

**Finding the location:**
1. Copy GPS coordinates
2. Format for search: `51°31'4.0"N 0°05'48.3"W`
3. Search on Google Maps or Bing Maps

The location is revealed!

- Answer: `Milk Street`

---

### Question 3: Find the camera model

**Command:**
```bash
exiftool letter-image.jpg | grep Camera
```

or

```bash
exiftool letter-image.jpg
```

Look for "Camera Model Name" in the output.

**Output:**
```
Camera Model Name       : Canon EOS R6
```

- Answer: `Canon EOS R6`

---

## Quick Reference

### Digital Forensics Phases

| Phase | Purpose | Key Activities |
|-------|---------|----------------|
| Collection | Gather evidence | Identify devices, collect data, document |
| Examination | Filter data | Extract relevant data, remove noise |
| Analysis | Draw conclusions | Correlate evidence, create timeline |
| Reporting | Document findings | Create report, present findings |

### Types of Digital Forensics

| Type | Focus Area |
|------|-----------|
| Computer | Desktop/laptop systems |
| Mobile | Smartphones and tablets |
| Network | Network traffic and logs |
| Database | Database intrusions |
| Cloud | Cloud infrastructure |
| Email | Email communications |

### Forensic Image Types

| Type | Data | Volatility | Priority |
|------|------|------------|----------|
| Disk Image | Storage data | Non-volatile | Second |
| Memory Image | RAM data | Volatile | First! |

### Essential Tools

| Tool | Purpose | Platform |
|------|---------|----------|
| FTK Imager | Disk imaging | Windows |
| Autopsy | Disk analysis | Cross-platform |
| DumpIt | Memory imaging | Windows |
| Volatility | Memory analysis | Cross-platform |
| pdfinfo | PDF metadata | Linux |
| exiftool | Image metadata | Cross-platform |

### Metadata Analysis Commands

**PDF Analysis:**
```bash
pdfinfo document.pdf
```

**Image Analysis:**
```bash
exiftool image.jpg
```

**Filter specific data:**
```bash
exiftool image.jpg | grep GPS
exiftool image.jpg | grep Camera
exiftool image.jpg | grep Date
```

---

## Common Use Cases

### PDF Investigation

**Check document creator**

**Check creation date**

**Check author**

### Image Investigation

**Extract GPS coordinates**

**Get camera details**

**Check capture date**

**Get all EXIF data**

---

## Important Concepts

### Evidence Collection Best Practices

1. **Get authorization first**
2. **Maintain chain of custody**
3. **Use write blockers**
4. **Document everything**
5. **Preserve original evidence**
6. **Work on copies, not originals**

### Why Metadata Matters

**Documents reveal:**
- Who created them
- When they were created
- What software was used
- Modification history

**Images reveal:**
- Where photos were taken
- What device was used
- When photos were captured
- Camera settings used

---

## Key Takeaways

- **Digital forensics** investigates cyber crimes
- **NIST defines 4 phases** - Collection, Examination, Analysis, Reporting
- **Analysis phase** is where conclusions are drawn
- **Examination phase** filters relevant data
- **Chain of custody** tracks evidence handling
- **Write blockers** prevent evidence tampering
- **Memory images** must be taken first (volatile!)
- **Disk images** contain non-volatile data
- **Metadata reveals hidden information** about files
- **GPS coordinates** in images show location
- **pdfinfo** analyzes PDF metadata
- **exiftool** analyzes image EXIF data
- **Every digital action leaves traces**
- **Proper authorization** is legally required
- **Documentation** is critical for court admissibility

Digital forensics is essential for investigating cyber crimes. Understanding evidence collection, analysis, and metadata examination helps solve cases and bring criminals to justice
