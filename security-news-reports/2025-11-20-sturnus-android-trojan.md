# Sturnus Android Trojan Quietly Captures Encrypted Chats and Hijacks Devices (2025)
**Date**: November 20, 2025  
**Source**: The Hacker News  
**Author**: Ravie Lakshmanan  

---

## Summary

Researchers have uncovered a powerful new Android banking trojan named **Sturnus**, capable of credential theft, device takeover, and bypassing encrypted messaging apps by capturing decrypted screen content.  
The malware is still in an early evaluation stage but already demonstrates advanced capabilities such as overlay attacks, Accessibility Service abuse, remote control through WebSockets, and high-fidelity device monitoring.

Its targets include financial institutions in **Southern and Central Europe**, with tailored region-specific overlays designed to steal banking credentials.

---

## Technical Details

**Malware Family**: Sturnus (Android Banking Trojan)

**Key Features**:
- Captures decrypted messages from WhatsApp, Telegram, and Signal using screen content analysis.
- Performs overlay attacks with fake login screens for banks.
- Remote control via VNC-like sessions over WebSocket.
- Accessibility Service abuse for:
  - Keystroke capturing  
  - UI element extraction  
  - Automated navigation  
- Blocks uninstallation by auto-navigating users away from device admin settings.
- Uses mixed communication protocols: plaintext, AES, and RSA.

**Distribution Artifacts**:
- Google Chrome impersonation: `com.klivkfbky.izaybebnx`
- Preemix Box impersonation: `com.uvxuthoq.noscjahae`

**Targets**:
- Banking apps in Southern and Central European regions.
- High-value financial applications.

**Impact**:
- Full device takeover  
- Credential theft  
- Remote interaction with UI  
- Collection of sensor, network, and hardware data  
- Ability to push background actions while showing a fake full-screen OS update screen  

---

## MITRE ATT&CK Mapping (Relevant Techniques)

| Phase             | Technique                                   | ID                     |
|------------------|---------------------------------------------|------------------------|
| Initial Access    | Malicious mobile applications               | T1475                  |
| Execution         | Abuse of Android Accessibility Services     | T1546.001              |
| Credential Access | Overlay attacks & keylogging                | T1056.001 / T1056      |
| Command & Control | WebSocket / HTTP-based C2                   | T1071 / T1095          |
| Impact            | Device takeover & UI manipulation           | T1499 / T1642          |

---

## My Key Takeaways

- **Sturnus is a next-generation mobile trojan**—its ability to capture decrypted content from secure messengers is a major leap in mobile surveillance capabilities.
- Overlay attacks remain the primary attack vector for Android financial fraud campaigns.
- Its **mixed encryption patterns** and mimicry-inspired name reflect a focus on stealth and adaptability.
- Blocking uninstallation by auto-redirecting users shows **elevated persistence mechanisms**.
- The malware is still not widespread, meaning threat actors are preparing for a **larger and more coordinated deployment**.

---

## Why This Incident Matters

Sturnus represents a growing trend of **highly interactive mobile malware**, capable not just of stealing credentials but of **actively controlling the device in real time**.  
For SOC analysts, mobile researchers, and fraud detection teams, this case reinforces the need for:

- Deep mobile threat detection strategies  
- Monitoring Accessibility Service misuse  
- Hardening financial apps against overlay attacks  
- Behavioral anomaly detection for UI automation  
- Awareness of evolving Android banking threats

While still in early testing, Sturnus shows all the hallmarks of a future large-scale financial malware campaign targeting European banking systems.

---
