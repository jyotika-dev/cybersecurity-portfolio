# TryHackMe Confidential - Walkthrough

> Hidden QR Code in PDF

**Room Link:** https://tryhackme.com/room/confidential

---

## Challenge Overview

**File:** PDF with QR code

**Problem:** QR code covered by red triangle

**Goal:** Extract and decode hidden QR code

---

## Walkthrough

### Step 1: Transfer File

**Get the PDF:**
```bash
nc < <PDF_FILE>
```

Or use:
- `wget`
- `scp`
- FTP

**Verify:**
```bash
ls
```

### Step 2: Analyze PDF

**Open PDF:**
- QR code visible
- Red triangle covering part of it
- Suspicious layer on top

### Step 3: Extract Images

**Use pdfimages:**
```bash
pdfimages -j confidential.pdf output
```

**Output files:**
- `images-000.ppm` - background + QR code
- `images-001.ppm` - red triangle layer
- `images-002.ppm` - extra data (if any)

### Step 4: View Clean QR Code

**Check extracted image:**
```bash
# View images-000.ppm
```

QR code now fully visible without triangle!

### Step 5: Decode QR Code

**Option 1: Scan with phone** 📱

**Option 2: Python decode** 🐍
```python
from PIL import Image
import pyzbar.pyzbar as pyzbar

img = Image.open('images-000.ppm')
decoded = pyzbar.decode(img)

for obj in decoded:
    print(obj.data.decode('utf-8'))
```

### Flag

**Answer:** `flag{e08e6ce2f077a1b420cfd4a5d1a57a8d}`

---

## Quick Reference

### PDF Analysis Tools

**pdfimages:**
```bash
# Extract all images
pdfimages file.pdf output

# Extract as JPEG
pdfimages -j file.pdf output

# List images only
pdfimages -list file.pdf
```

**Other tools:**
```bash
pdfinfo file.pdf          # PDF info
pdftotext file.pdf        # Extract text
pdftk file.pdf dump_data  # Metadata
exiftool file.pdf         # EXIF data
```

### QR Code Tools

**Python libraries:**
```bash
pip install pyzbar pillow
```

**Command line:**
```bash
zbarimg image.png         # Decode QR
qrencode -o qr.png "text" # Create QR
```

**Online tools:**
- zxing.org
- webqr.com

---

## Commands Used

```bash
# Transfer file
nc < confidential.pdf

# Extract images
pdfimages -j confidential.pdf images

# View images
eog images-000.ppm
# or
display images-000.ppm

# Decode QR (Python)
python3 decode_qr.py
```

---

## Python QR Decoder

**Save as `decode_qr.py`:**

```python
#!/usr/bin/env python3

from PIL import Image
import pyzbar.pyzbar as pyzbar
import sys

if len(sys.argv) < 2:
    print("Usage: python3 decode_qr.py <image>")
    sys.exit(1)

img = Image.open(sys.argv[1])
decoded = pyzbar.decode(img)

if decoded:
    for obj in decoded:
        print(obj.data.decode('utf-8'))
else:
    print("No QR code found")
```

**Usage:**
```bash
python3 decode_qr.py images-000.ppm
```

---

## Analysis Workflow

```
1. Get PDF file
    ↓
2. Open and observe
    ↓
3. Notice suspicious overlay
    ↓
4. Extract images with pdfimages
    ↓
5. View extracted layers
    ↓
6. Find clean QR code
    ↓
7. Decode QR code
    ↓
8. Get flag!
```

---

## Key Concepts

### PDF Layers

**What are layers:**
- Separate image components
- Can overlay each other
- Used to hide content
- Extractable with tools

**Why check layers:**
- Hidden information
- Redacted text
- Covered images
- Metadata

### Image Extraction

**pdfimages benefits:**
- Extracts all embedded images
- Preserves original quality
- Separates layers
- Multiple format options

**Output formats:**
- `.ppm` - Portable Pixmap
- `.pbm` - Portable Bitmap
- `.jpg` - JPEG (with -j flag)
- `.png` - PNG (with -png flag)

### QR Code Analysis

**What QR codes contain:**
- URLs
- Text
- Flags
- Credentials
- Instructions

**Security considerations:**
- Can contain malicious URLs
- May encode commands
- Sometimes obfuscated
- Always scan safely

---

## Tips & Tricks

**PDF analysis:**
- Always extract images first
- Check all layers
- Look for metadata
- Examine file structure

**Image handling:**
- Use proper viewers (eog, display)
- Check all extracted files
- Compare layers
- Look for differences

**QR decoding:**
- Scan with phone if simple
- Use Python for automation
- Check decoded content type
- Verify before clicking URLs

**File transfer:**
- Netcat for quick transfers
- SCP for secure copying
- FTP if available
- HTTP server for downloads

---

## Alternative Methods

### Method 1: GIMP

```
1. Open PDF in GIMP
2. Select layers panel
3. Hide triangle layer
4. Export clean image
5. Scan QR code
```

### Method 2: PDF.js

```
1. Open in Firefox
2. Inspect element
3. Find image URLs
4. Download directly
5. Decode QR
```

### Method 3: Inkscape

```
1. Import PDF
2. Ungroup objects
3. Delete triangle
4. Export QR image
5. Scan
```

---

## Key Takeaways

- **PDFs** can have hidden layers
- **pdfimages** extracts all images
- **Layers** can cover content
- **QR codes** encode data
- **Python** automates decoding
- **Always check** all extracted files
- **Multiple methods** to solve same problem
- **Simple tools** are powerful

---

Happy hunting!
