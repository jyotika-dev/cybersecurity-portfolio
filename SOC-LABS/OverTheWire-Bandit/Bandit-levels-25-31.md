# 🔐 OverTheWire Bandit - Levels 25–31

## 📌 Overview
Levels 25–31 focus on **advanced shell manipulation**, **git repository analysis**, and **version control forensics**. These exercises demonstrate real-world scenarios like escaping restricted shells, analyzing git history, exploring branches and tags, and understanding repository structure for security investigations.

---

## 🧭 Level Highlights (25 → 31)

### Level 25 → 26
**Goal**: Escape a restricted shell environment using terminal manipulation and vim.

**Commands / approach**:
```bash
ssh bandit25@bandit.labs.overthewire.org -p 2220
ls
# Minimize terminal window to trigger 'more' pager
ssh -i bandit26.sshkey bandit26@localhost -p 2220
# In 'more' pager, press 'v' to open vim
# In vim: :e /etc/bandit_pass/bandit26
# To get shell access:
# :set shell=/bin/bash
# :shell
```

**Result**: Successfully escaped restricted shell and gained bash access.

**Why**: Demonstrates shell escape techniques through pagers and text editors. Understanding how restricted environments can be bypassed is crucial for both offensive and defensive security.

---

### Level 26 → 27
**Goal**: Use setuid binary to read protected password file.

**Commands / approach**:
```bash
# Already logged in from previous level
ls
./bandit27-do cat /etc/bandit_pass/bandit27
```

**Result**: Password extracted using setuid program.

**Why**: Shows how setuid binaries can be leveraged for privilege escalation. Understanding these mechanisms helps identify potential security vulnerabilities.

---

### Level 27 → 28
**Goal**: Clone git repository and extract credentials from README file.

**Commands / approach**:
```bash
ssh bandit27@bandit.labs.overthewire.org -p 2220
mkdir /tmp/jk01
cd /tmp/jk01
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
cd repo
ls -al
cat README
```

**Result**: Password found in repository README file.

**Why**: Introduces git-based challenges and demonstrates how sensitive information can be stored in repositories. Basic git operations are essential for code analysis.

---

### Level 28 → 29
**Goal**: Analyze git commit history to find accidentally committed credentials.

**Commands / approach**:
```bash
ssh bandit28@bandit.labs.overthewire.org -p 2220
mkdir /tmp/jk12
cd /tmp/jk12
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
cd repo
cat README.md
git log
# Checkout previous commit with credentials
git checkout 68314e012fbaa192abfc9b78ac369c82b75fab8f
cat README.md
```

**Result**: Password recovered from git commit history.

**Why**: Demonstrates git forensics - how deleted or modified sensitive data can be recovered from version history. Critical skill for incident response and code auditing.

---

### Level 29 → 30
**Goal**: Explore git branches to find hidden credentials.

**Commands / approach**:
```bash
ssh bandit29@bandit.labs.overthewire.org -p 2220
mkdir /tmp/jk02
cd /tmp/jk02
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
cd repo
cat README.md
git branch -r  # Show remote branches
git checkout dev
cat README.md
```

**Result**: Password found in development branch.

**Why**: Shows how different git branches may contain varying levels of sensitive information. Branch analysis is important for comprehensive repository security assessment.

---

### Level 30 → 31
**Goal**: Use git tags to locate hidden information.

**Commands / approach**:
```bash
ssh bandit30@bandit.labs.overthewire.org -p 2220
mkdir /tmp/jk03
cd /tmp/jk03
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
cd repo
cat README.md
git tag  # List available tags
git show secret
```

**Result**: Password extracted from git tag.

**Why**: Demonstrates how git tags can be used to hide or reference sensitive information. Complete git repository analysis must include tags and references.

---

### Level 31 → 32
**Goal**: Create and commit a file to remote repository while bypassing git ignore rules.

**Commands / approach**:
```bash
ssh bandit31@bandit.labs.overthewire.org -p 2220
mkdir /tmp/jk04
cd /tmp/jk04
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
cd repo
cat README.md
rm .gitignore  # Remove ignore rules
echo "May I come in?" > key.txt
git add key.txt
git commit -m "Added key.txt"
git push
```

**Result**: Successfully pushed file and received password in response.

**Why**: Shows how git ignore files can prevent certain files from being committed, and how to work around these restrictions. Understanding git workflows is important for code security analysis.

---

## 🛠 Tools & Commands You Practiced
- **Shell manipulation**: terminal resizing, pager escape, vim commands
- **Git operations**: `clone`, `log`, `checkout`, `branch -r`, `tag`, `show`
- **File operations**: `mkdir`, `rm`, `echo >`, `cat`
- **SSH**: key-based authentication, repository cloning
- **Version control forensics**: commit history analysis, branch exploration

## 🔎 Key Takeaways
- **Shell escape techniques** through pagers and editors can bypass restricted environments.
- **Git repositories** often contain sensitive information in commits, branches, or tags that may not be visible in the current working tree.
- **Version control forensics** requires comprehensive analysis of history, branches, tags, and references.
- **Temporary directories** provide clean workspace for repository analysis without affecting system state.
- **Git ignore files** can hide important files from standard operations but can be bypassed with proper permissions.
- **Repository cloning** and analysis is a standard technique in security research and incident response.

**Reference**: https://overthewire.org/wargames/bandit/
