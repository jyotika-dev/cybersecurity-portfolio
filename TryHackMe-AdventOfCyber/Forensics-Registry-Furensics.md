# Advent of Cyber 2025 Day 16 - TryHackMe

> Registry Forensics - Windows Registry Analysis

**Day 16 Link:** https://tryhackme.com/room/registry-forensics-aoc2025-h6k9j2l5p8

---

## Story

**Scenario:** TBFC's dispatch server (dispatch-srv01) compromised

**Attack date:** October 21, 2025

**System:** Windows Server with drone management software

**Goal:** Investigate Windows Registry to find malicious activity

---

## Windows Registry Basics

**What is it:**
- Windows configuration database
- Stores system and user settings
- Hierarchical structure
- Critical for forensics

**Registry Hives:**

| Hive | Contains | Location |
|------|----------|----------|
| SYSTEM | Services, drivers | C:\Windows\System32\config\SYSTEM |
| SOFTWARE | Installed programs | C:\Windows\System32\config\SOFTWARE |
| SAM | User accounts | C:\Windows\System32\config\SAM |
| SECURITY | Security policies | C:\Windows\System32\config\SECURITY |
| NTUSER.DAT | User preferences | C:\Users\username\NTUSER.DAT |

---

## Investigation Setup

### Location of Hives

```
C:\Users\Administrator\Desktop\Registry Hives
```

### Tool Used

**Registry Explorer** (not regedit)

**Why:**
- Opens offline hives
- Forensic analysis
- Transaction log support
- Better for investigations

---

## Loading Registry Hives

### Steps:

1. Open Registry Explorer
2. File → Load hive
3. Select SYSTEM hive
4. Hold SHIFT + click Open (includes transaction logs)
5. Repeat for SOFTWARE and other hives

**Important:** Always hold SHIFT when opening to include transaction logs!

---

## Questions & Answers

### Q1: Installed Application Name

**Path to check:**
```
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
```

**What to do:**
1. Navigate to Uninstall key
2. Check each subkey
3. Look at installation dates
4. Find app installed on Oct 21, 2025

**Answer:** `DroneManager Updater`

---

### Q2: Application Launch Path

**Path to check:**
```
HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
```

**What to do:**
1. Navigate to RecentDocs
2. Look for .exe files
3. Find DroneManager execution path
4. Check full path

**Answer:** `C:\Users\dispatch.admin\Downloads\DroneManager_Setup.exe`

---

### Q3: Persistence Mechanism

**Path to check:**
```
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
```

**What to do:**
1. Navigate to Run key
2. Look for suspicious entries
3. Find DroneManager related entry
4. Note full command line

**Answer:** `"C:\Program Files\DroneManager\dronehelper.exe" --background`

---

## Quick Reference

### Key Registry Paths

**Installed programs:**
```
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall (32-bit on 64-bit)
```

**Startup programs:**
```
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce
```

**Recent files:**
```
HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
```

**User activity:**
```
HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist
```

**Services:**
```
HKLM\SYSTEM\CurrentControlSet\Services
```

---

## Registry Explorer Navigation

**Load hive:**
```
File → Load hive → Select file → SHIFT+Open
```

**Search:**
```
Ctrl+F or Edit → Find
```

**Bookmarks:**
```
Right-click → Add bookmark
```

**Export:**
```
Right-click key → Export
```

---

## Investigation Workflow

```
1. Load Registry Hives
    ↓
2. Check Installed Programs (Uninstall key)
    ↓
3. Find Installation Date & Name
    ↓
4. Check Recent Docs (RecentDocs key)
    ↓
5. Find Execution Path
    ↓
6. Check Startup Programs (Run key)
    ↓
7. Identify Persistence Mechanism
```

---

## Forensic Artifacts Found

| Artifact | Value | Significance |
|----------|-------|--------------|
| Program | DroneManager Updater | Malicious app |
| Path | Downloads\DroneManager_Setup.exe | Initial infection |
| Persistence | dronehelper.exe --background | Auto-start malware |

---

## Common Persistence Locations

**Run keys:**
```
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

**Services:**
```
HKLM\SYSTEM\CurrentControlSet\Services
```

**Scheduled tasks (older):**
```
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks
```

**Startup folder:**
```
HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders
```

**Winlogon:**
```
HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon
```

---

## Registry Hive Files

**System location:**
```
C:\Windows\System32\config\
    ├── SYSTEM
    ├── SOFTWARE
    ├── SAM
    ├── SECURITY
    └── DEFAULT
```

**User location:**
```
C:\Users\<username>\
    ├── NTUSER.DAT
    └── AppData\Local\Microsoft\Windows\UsrClass.dat
```

**Transaction logs:**
```
Same location as hive with .LOG1, .LOG2 extensions
```

---

## Defense Tips

**Monitor for:**
- New Run key entries
- Unexpected software installations
- Executables in Downloads folder
- Auto-start programs
- Service modifications

**Best practices:**
- Application whitelisting
- Regular registry audits
- Monitor installation events
- Restrict user permissions
- Enable registry auditing

**Detection:**
- Sysmon Event ID 13 (Registry value set)
- Windows Event ID 4657 (Registry modified)
- EDR registry monitoring
- Baseline comparisons

---

## Registry Forensics Checklist

**For investigations:**
- [ ] Collect all registry hives
- [ ] Include transaction logs
- [ ] Use forensic tools (not regedit)
- [ ] Document timestamps
- [ ] Check persistence keys
- [ ] Review installed programs
- [ ] Examine user activity
- [ ] Export findings
- [ ] Create timeline
- [ ] Correlate with other logs

---

## Key Takeaways

- **Registry** stores critical forensic evidence
- **Offline analysis** prevents evidence modification
- **Registry Explorer** better than regedit for forensics
- **Run keys** show persistence mechanisms
- **Uninstall key** tracks installations
- **RecentDocs** shows user activity
- **Transaction logs** ensure complete data
- **Timestamps** help build timeline
- **Multiple keys** tell complete story
- **Always document** your findings

---

Happy hunting!
