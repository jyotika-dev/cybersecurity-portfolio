# Gobuster: The Basics - TryHackMe

> This room focuses on an introduction to Gobuster, an offensive security tool used for enumeration

**Room Link:** https://tryhackme.com/room/gobusterbasics

<img width="1571" height="922" alt="image" src="https://github.com/user-attachments/assets/6c6e7949-afaf-4ecb-b7a6-833b800ad645" />

---

## Task 1: Introduction

### What is Gobuster?

Gobuster is a powerful enumeration tool used in offensive security for discovering:
- Hidden directories and files
- Subdomains
- Virtual hosts (vhosts)
- DNS records

### Why Use Gobuster?

**Fast and Efficient**
- Written in Go language
- Multi-threaded scanning
- Quick results

**Versatile**
- Multiple enumeration modes
- Flexible wordlists
- Customizable options

**Popular Choice**
- Industry standard tool
- Active development
- Large community support

**No answer needed**

---

## Task 2: Environment and Setup

### DNS Configuration

Before starting, you need to configure DNS for the machine.

**Edit DNS configuration:**
```bash
sudo nano /etc/systemd/resolved.conf
```

**Add the machine IP:**
```
DNS=MACHINE_IP
```

**Restart the service:**
```bash
sudo systemctl restart systemd-resolved
```

### Verify Setup

```bash
ping offensivetools.thm
```

You should see responses from the machine IP.

**No answer needed**

---

## Task 3: Gobuster: Introduction

### Basic Gobuster Syntax

```bash
gobuster [mode] [options]
```

### Common Modes

**dir** - Directory/File enumeration
```bash
gobuster dir -u http://target.com -w wordlist.txt
```

**dns** - Subdomain enumeration
```bash
gobuster dns -d target.com -w wordlist.txt
```

**vhost** - Virtual host enumeration
```bash
gobuster vhost -u http://target.com -w wordlist.txt
```

### Common Flags

| Flag | Purpose |
|------|---------|
| `-u` | Target URL |
| `-d` | Target domain |
| `-w` | Wordlist path |
| `-t` | Number of threads |
| `-o` | Output file |
| `-x` | File extensions |
| `-k` | Skip TLS verification |

### Questions

**What flag do we use to specify the target URL?**
- Answer: `-u`

---

**What command do we use for the subdomain enumeration mode?**
- Answer: `dns`

---

## Task 4: Use Case: Directory and File Enumeration

### Directory Enumeration

**Basic directory scan:**
```bash
gobuster dir -u http://www.offensivetools.thm -w /usr/share/wordlists/dirb/common.txt
```

**With file extensions:**
```bash
gobuster dir -u http://www.offensivetools.thm -w /usr/share/wordlists/dirb/common.txt -x php,html,js
```

**Skip TLS verification:**
```bash
gobuster dir -u https://www.offensivetools.thm -w /usr/share/wordlists/dirb/common.txt --no-tls-validation
```

### Common Options

**Status codes:**
```bash
gobuster dir -u http://target.com -w wordlist.txt -s 200,301,302
```

**Increased threads:**
```bash
gobuster dir -u http://target.com -w wordlist.txt -t 50
```

**Output to file:**
```bash
gobuster dir -u http://target.com -w wordlist.txt -o results.txt
```

### Questions

**Which flag do we have to add to our command to skip the TLS verification? Enter the long flag notation.**
- Answer: `--no-tls-validation`

---

**Enumerate the directories of www.offensivetools.thm. Which directory catches your attention?**

```bash
gobuster dir -u http://www.offensivetools.thm -w /usr/share/wordlists/dirb/common.txt
```

Look for interesting directory names.

- Answer: `secret`

---

**Continue enumerating the directory found in question 2. You will find an interesting file there with a .js extension. What is the flag found in this file?**

Enumerate the secret directory with JS extension:
```bash
gobuster dir -u http://www.offensivetools.thm/secret -w /usr/share/wordlists/dirb/common.txt -x js
```

Visit the found JS file in your browser or use curl:
```bash
curl http://www.offensivetools.thm/secret/[filename].js
```

- Answer: `THM{ReconWasASuccess}`

---

## Task 5: Use Case: Subdomain Enumeration

### DNS Enumeration

**Basic subdomain scan:**
```bash
gobuster dns -d offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

### Required Flags for DNS Mode

**-d** - Domain to enumerate
**-w** - Wordlist to use

### Advanced DNS Options

**Custom DNS server:**
```bash
gobuster dns -d offensivetools.thm -w wordlist.txt -r 8.8.8.8
```

**Show CNAMEs:**
```bash
gobuster dns -d offensivetools.thm -w wordlist.txt --show-cname
```

**Wildcard detection:**
```bash
gobuster dns -d offensivetools.thm -w wordlist.txt --wildcard
```

### Questions

**Apart from the dns keyword and the -w flag, which shorthand flag is required for the command to work?**

The domain flag is required to specify the target domain.

- Answer: `-d`

---

**Use the commands learned in this task, how many subdomains are configured for the offensivetools.thm domain?**

```bash
gobuster dns -d offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

Count the found subdomains (including www if found).

- Answer: `4`

---

## Task 6: Use Case: Vhost Enumeration

### Virtual Host Enumeration

**Basic vhost scan:**
```bash
gobuster vhost -u http://offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

### What are Virtual Hosts?

Virtual hosts allow multiple websites to run on a single server, all sharing the same IP address but responding to different hostnames.

### Filtering Results

**Filter by status code:**
```bash
gobuster vhost -u http://offensivetools.thm -w wordlist.txt --exclude-length 0
```

**Append domain:**
```bash
gobuster vhost -u http://offensivetools.thm -w wordlist.txt --append-domain
```

### Vhost vs DNS Enumeration

| Feature | DNS | Vhost |
|---------|-----|-------|
| Requires DNS records | Yes | No |
| Finds subdomains | Yes | Yes |
| Finds virtual hosts | No | Yes |
| Method | DNS queries | HTTP headers |

### Questions

**Use the commands learned in this task to answer the following question: How many vhosts on the offensivetools.thm domain reply with a status code 200?**

```bash
gobuster vhost -u http://offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

Count the vhosts that return status code 200.

- Answer: `4`

---

## Task 7: Conclusion

Congratulations! You've learned the basics of Gobuster!

### What You've Learned

✅ Gobuster installation and setup
✅ Directory and file enumeration
✅ Subdomain discovery (DNS mode)
✅ Virtual host enumeration
✅ Common flags and options
✅ Practical enumeration techniques

### Next Steps

- Practice with different wordlists
- Combine Gobuster with other tools
- Try advanced flags and options
- Learn about custom wordlist creation
- Explore Gobuster alternatives (dirbuster, ffuf)

**No answer needed**

---

## Quick Reference

### Gobuster Modes

| Mode | Purpose | Example |
|------|---------|---------|
| `dir` | Directory/file enum | `gobuster dir -u http://target.com -w wordlist.txt` |
| `dns` | Subdomain enum | `gobuster dns -d target.com -w wordlist.txt` |
| `vhost` | Virtual host enum | `gobuster vhost -u http://target.com -w wordlist.txt` |
| `s3` | S3 bucket enum | `gobuster s3 -w wordlist.txt` |
| `gcs` | Google Cloud enum | `gobuster gcs -w wordlist.txt` |

### Common Flags

| Flag | Long Version | Purpose |
|------|--------------|---------|
| `-u` | `--url` | Target URL |
| `-d` | `--domain` | Target domain |
| `-w` | `--wordlist` | Wordlist path |
| `-t` | `--threads` | Number of threads |
| `-o` | `--output` | Output file |
| `-x` | `--extensions` | File extensions |
| `-k` | `--no-tls-validation` | Skip TLS check |
| `-q` | `--quiet` | Quiet mode |
| `-v` | `--verbose` | Verbose output |

### Directory Enumeration Flags

| Flag | Purpose |
|------|---------|
| `-x` | File extensions to search for |
| `-s` | Status codes to display |
| `-b` | Status codes to exclude |
| `-e` | Show full URLs |
| `-r` | Follow redirects |
| `-k` | Skip SSL certificate verification |

### DNS Enumeration Flags

| Flag | Purpose |
|------|---------|
| `-c` | Show CNAME records |
| `-i` | Show IP addresses |
| `--wildcard` | Force wildcard detection |
| `-r` | Custom DNS server |

### Vhost Enumeration Flags

| Flag | Purpose |
|------|---------|
| `--append-domain` | Append domain to words |
| `--exclude-length` | Exclude response length |
| `-r` | Follow redirects |

---

## Key Takeaways

- **Gobuster is fast** - Written in Go, multi-threaded
- **Multiple modes** - dir, dns, vhost for different purposes
- **Wordlists matter** - Choose appropriate wordlist size
- **-u flag** for URLs, **-d flag** for domains
- **-k flag** skips TLS verification for HTTPS
- **-x flag** adds file extensions to search
- **DNS mode** finds subdomains via DNS queries
- **Vhost mode** finds virtual hosts via HTTP headers
- **Always use wordlists** - Located in /usr/share/wordlists
- **Combine with other tools** - Use with Nmap, Burp Suite, etc.
- **Save output** - Use -o flag to save results
- **Increase threads** - Use -t flag for faster scans

Gobuster is an essential tool for web enumeration and reconnaissance. Master it to uncover hidden assets during penetration tests
