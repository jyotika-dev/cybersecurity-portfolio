# CAPA: The Basics - TryHackMe

> Learn about CAPA, a tool for analyzing malware capabilities

**Room Link:** https://tryhackme.com/room/capabasics

<img width="1453" height="889" alt="image" src="https://github.com/user-attachments/assets/20ac3d73-751c-475b-9b3b-dc2d54dbb2ff" />


---

## Task 1: Introduction

**No answer needed**

---

## Task 2: Tool Overview - How CAPA Works

### CAPA Command-Line Options

**Basic help:**
```bash
capa -h
```

**Verbose output:**
```bash
capa -v malware.exe
```

**Very verbose output:**
```bash
capa -vv malware.exe
```

### Verbosity Levels

| Option | Detail Level |
|--------|--------------|
| `-v` | Verbose |
| `-vv` | Very verbose |

### Questions

**What command-line option would you use if you need to check what other parameters you can use with the tool? Use the shortest format.**
- Answer: `-h`

---

**What command-line options are used to find detailed information on the malware's capabilities? Use the shortest format.**
- Answer: `-v`

---

**What command-line options do you use to find very verbose information about the malware's capabilities? Use the shortest format.**
- Answer: `-vv`

---

**What PowerShell command will you use to read the content of a file?**
- Answer: `Get-Content`

---

## Task 3: CAPA Results Part 1 - General Information, MITRE and MAEC

### File Information Table

CAPA shows file hashes and metadata:

```
md5:    3b9d26d2e7433749f2c32edb13a2b0a2
sha1:   969437df8f4ad08542ce8fc9831fc49a7765b7c5
sha256: ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c
```

### MITRE ATT&CK Mapping

CAPA maps malware behavior to MITRE ATT&CK techniques:

**Format:** Technique Name [T####]
**Sub-technique:** [T####.###]

### MAEC Values

**Launcher:**
- Activates persistence mechanisms
- Enables malicious activity

**Downloader:**
- Fetches payloads from internet
- Retrieves additional resources

### Questions

**What is the sha256 of cryptbot.bin?**

From the file information table.

- Answer: `ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c`

---

**What is the Technique Identifier of Obfuscated Files or Information?**

Format: [T####]

- Answer: `T1027`

---

**What is the Sub-Technique Identifier of Obfuscated Files or Information::Indicator Removal from Tools?**

Sub-techniques have `.###` after the main ID.

- Answer: `T1027.005`

---

**When CAPA tags a file with this MAEC value, it indicates that it demonstrates behavior similar to, but not limited to, Activating persistence mechanisms?**

Think "enabler" - Answer: `Launcher`

---

**When CAPA tags a file with this MAEC value, it indicates that the file demonstrates behaviour similar to, but not limited to, Fetching additional payloads or resources from the internet?**

Think "retriever" - Answer: `Downloader`

---

## Task 4: CAPA Results Part 2 - Malware Behavior Catalogue

### What is MBC?

**Malware Behavior Catalogue (MBC):**
- Catalogue of malware objectives and behaviors
- Based on ATT&CK tactics
- Used for malware analysis and labeling

### Key Fields

**Objective:**
- Based on ATT&CK tactics
- Describes malware goals
- Examples: Anti-Analysis, Persistence

**Behavior:**
- Specific malware actions
- Has unique identifier

**Micro-behavior:**
- Detailed specific actions
- Format: [C####]

### Questions

**What serves as a catalogue of malware objectives and behaviours?**
- Answer: `Malware Behavior Catalogue`

---

**Which field is based on ATT&CK tactics in the context of malware behaviour?**
- Answer: `Objective`

---

**What is the Identifier of "Create Process" micro-behavior?**

Look at micro-behavior table.

- Answer: `C0017`

---

**What is the behavior with an Identifier of B0009?**

Look up behavior ID B0009.

- Answer: `Virtual Machine Detection`

---

**Malware can be used to obfuscate data using base64 and XOR. What is the related micro-behavior for this?**

Data obfuscation = Encode Data

- Answer: `Encode Data`

---

**Which micro-behavior refers to "Malware is capable of initiating HTTP communications"?**
- Answer: `HTTP Communication`

---

## Task 5: CAPA Results Part 3 - Namespaces

### Understanding Namespaces

**Top-Level Namespace (TLN):**
- Main category of rules
- Examples: Anti-Analysis, Persistence

**Namespace:**
- Sub-category under TLN
- More specific grouping

**Example hierarchy:**
```
Anti-Analysis (TLN)
  └── anti-vm/vm-detection (Namespace)
      └── obfuscation (Namespace)
```

### Common Namespaces

**Anti-Analysis:**
- Obfuscation techniques
- Packing
- Anti-debugging

**Persistence:**
- Maintaining access
- Long-term presence

**Nursery:**
- Rules in development
- Not fully polished

### Questions

**Which top-level Namespace contains a set of rules specifically designed to detect behaviors, including obfuscation, packing, and anti-debugging techniques exhibited by malware to evade analysis?**
- Answer: `Anti-Analysis`

---

**Which namespace contains rules to detect virtual machine (VM) environments? Note that this is not the TLN.**

Sub-namespace under Anti-Analysis.

- Answer: `Anti-vm/vm-detection`

---

**Which Top-Level Namespace contains rules related to behaviors associated with maintaining access or persistence within a compromised system?**

Hint is in the question - maintaining access.

- Answer: `Persistence`

---

**Which namespace addresses techniques such as String Encryption, Code Obfuscation, Packing, and Anti-Debugging Tricks?**

Child namespace of Anti-Analysis.

- Answer: `obfuscation`

---

**Which Top-Level Namespace is a staging ground for rules that are not quite polished?**
- Answer: `Nursery`

---

## Task 6: CAPA Results Part 4 - Capability

### Rules and Capabilities

**Rule name = Capability name**

**Format conversion:**
- Spaces → Dashes in YAML filename
- `check HTTP status code` → `check-http-status-code.yml`

**Reverse:**
- Dashes → Spaces in capability name
- `reference-anti-vm-strings.yml` → `reference anti-VM strings`

### Questions

**What rule yaml file was matched if the Capability or rule name is check HTTP status code?**

Spaces → Dashes

- Answer: `check-http-status-code`

---

**What is the name of the Capability if the rule YAML file is reference-anti-vm-strings.yml?**

Dashes → Spaces

- Answer: `reference anti-VM strings`

---

**Which TLN includes the Capability or rule name run PowerShell expression?**

PowerShell = Code execution

- Answer: `load-code`

---

**Check the conditions inside the check-for-windows-sandbox-via-registry.yml rule file. What is the value of the API that ends in Ex?**

Look for registry API ending in "Ex".

- Answer: `RegOpenKeyEx`

---

## Task 7: More Information, More Fun

### CAPA Web Explorer

Better than command line - interactive web interface!

**Features:**
- Visual analysis
- Filter results
- Export options

### Output Options

**JSON output:**
```bash
capa malware.exe -j output.json
```

### Questions

**Which parameter allows you to output the result of CAPA into a .json file?**
- Answer: `-j`

---

**What tool allows you to interactively explore CAPA results in your web browser?**
- Answer: `CAPA Web Explorer`

---

**Which feature of this CAPA Web Explorer allows you to filter options or results?**
- Answer: `Global Search Box`

---

## Quick Reference

### CAPA Commands

| Command | Purpose |
|---------|---------|
| `capa -h` | Help |
| `capa -v` | Verbose |
| `capa -vv` | Very verbose |
| `capa -j` | JSON output |

### ID Formats

| Type | Format | Example |
|------|--------|---------|
| Technique | T#### | T1027 |
| Sub-technique | T####.### | T1027.005 |
| Behavior | B#### | B0009 |
| Micro-behavior | C#### | C0017 |

### Key Namespaces

| Namespace | Purpose |
|-----------|---------|
| Anti-Analysis | Evasion techniques |
| Persistence | Maintain access |
| Nursery | Rules in development |
| obfuscation | Code hiding |
| anti-vm | VM detection |

### MAEC Values

| Value | Behavior |
|-------|----------|
| Launcher | Enables persistence |
| Downloader | Fetches payloads |

---

## Key Takeaways

- **CAPA** analyzes malware capabilities
- **-h** for help
- **-v** for verbose, **-vv** for very verbose
- **-j** exports to JSON
- **MBC** = Malware Behavior Catalogue
- **Objective** based on ATT&CK tactics
- **T####** = Technique ID
- **T####.###** = Sub-technique ID
- **Anti-Analysis** detects evasion
- **Persistence** maintains access
- **Nursery** = unpolished rules
- **Rule name** uses dashes in filename
- **CAPA Web Explorer** = interactive analysis
- **Global Search Box** filters results

CAPA makes malware analysis easier by automatically detecting capabilities and mapping them to known frameworks
Happy analyzing
