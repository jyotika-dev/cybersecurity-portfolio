# 🎣 Phishing Investigation Lab – LetsDefend

This lab is based on LetsDefend’s **Phishing Module**, where I explored how phishing attacks are created, delivered, and analyzed by SOC analysts.

---

## 📌 Objectives
- Understand common phishing attack methods.
- Learn how to investigate email headers and attachments.
- Explore static & dynamic analysis of phishing files.
- Identify advanced techniques attackers use to bypass detection.

---

## 🛠️ Lab Sections & Key Learnings

### 1. Introduction to Phishing
- Phishing is one of the **most common attack vectors** against organizations.
- Goal: trick users into giving credentials, clicking malicious links, or downloading malware.

---

### 2. Information Gathering
- Phishing emails often provide **initial clues** (sender address, subject line, urgency).
- SOC analysts collect evidence:  
  - Suspicious domains  
  - Links to external services  
  - File attachments  

---

### 3. Email Headers & Analysis
- Email headers = **first step in investigation**.
- Learned how to extract:  
  - `Return-Path` (sender identity)  
  - `Received-From` (mail server hops)  
  - `SPF/DKIM/DMARC` checks (authenticity)  

💡 Key Insight: Even if the content looks valid, header mismatches reveal spoofing.

---

### 4. Static Analysis
- Opened phishing attachments (safely in sandbox).  
- Looked for:  
  - **Macros in Word docs**  
  - Suspicious URLs inside PDFs  
  - File hashes to check on VirusTotal  

---

### 5. Dynamic Analysis
- Detonated suspicious files in a controlled environment.  
- Observed network traffic:  
  - C2 callbacks  
  - Credential exfiltration attempts  
  - Dropped malware binaries  

---

### 6. Additional Techniques
- Attackers abuse **legitimate services**:  
  - **Cloud storage** (Google Drive, OneDrive) for malware delivery.  
  - **Free subdomains** (WordPress, Wix, Blogspot) to host phishing pages.  
  - **Form apps** for fake logins.  

💡 Key Insight: Phishing isn’t just shady emails anymore. It blends into **trusted platforms**.

---

### 7. Phishing Quiz (Validation)
- Completed LetsDefend’s phishing quiz.  
- Tested knowledge on **header analysis, file inspection, and attacker tricks**.  

---

## 📊 Outcome
✅ Understood end-to-end **phishing investigation workflow**.  
✅ Hands-on practice with **headers, static/dynamic analysis, attacker techniques**.  
✅ Learned that phishing evolves — attackers abuse **trust in known services**.  

---

## 💡 Takeaways
- Always validate sender domains with **headers + DNS checks**.  
- Cloud/legit services don’t equal safety — attackers exploit that trust.  
- Combining static + dynamic analysis gives **full attack visibility**.  

---

📖 **Reference:** [LetsDefend – Phishing Training](https://app.letsdefend.io/training/lesson_detail/additional-techniques)
