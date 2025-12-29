# Advent of Cyber 2025 Day 14 - TryHackMe

> Container Security - Docker Escape

**Day 14:** https://tryhackme.com/room/container-security-aoc2025-z0x3v6n9m2

---

## Story

**Scenario:** DoorDasher food delivery site hijacked by King Malhare

**Problem:** Site rebranded as "Hopperoo" with suspicious menu items

**Access:** SOC team has access via monitoring container (uptime-checker)

**Goal:** Escape container, access deployer container, run recovery script

---

## Container Basics

**What are containers:**
- Package app + dependencies together
- Isolated environments
- Share host OS kernel
- Lightweight and fast

**Containers vs VMs:**

| Feature | Containers | VMs |
|---------|-----------|-----|
| Size | MB | GB |
| Startup | Seconds | Minutes |
| Isolation | Process-level | Hardware-level |
| OS | Share host kernel | Full guest OS |

---

## Docker Components

**Key parts:**
- **Docker Engine:** Runs containers
- **Dockerfile:** Instructions to build image
- **Docker Image:** Template for containers
- **Docker Container:** Running instance
- **Docker Daemon:** Manages containers
- **Docker CLI:** Command interface

**Docker Socket:**
```
/var/run/docker.sock
```
- Communication between CLI and daemon
- **Critical:** Access to socket = control everything!

---

## Container Escape

**What is it:**
- Breaking out of container isolation
- Accessing host system
- Controlling other containers

**How it works:**
- Docker socket exposed in container
- Can run Docker commands from inside
- Create privileged containers
- Access host resources

---

## Walkthrough

### Step 1: List Containers

```bash
docker ps
```

**Expected containers:**
- doordasher (port 5001) - hijacked site
- uptime-checker - monitoring pod
- deployer - recovery script
- news-site (port 5002) - bonus

### Step 2: Check Defaced Site

```
http://MACHINE_IP:5001
```

Site shows "Hopperoo" with Easter bunny theme.

### Step 3: Access Monitoring Container

```bash
docker exec -it uptime-checker sh
```

Now inside uptime-checker container!

### Step 4: Check Docker Socket

```bash
ls -la /var/run/docker.sock
```

**Output:**
```
srw-rw---- 1 root docker 0 Dec 14 10:00 /var/run/docker.sock
```

Socket is accessible! We can escape!

### Step 5: Confirm Escape

```bash
docker ps
```

If you see all containers, escape successful!

### Step 6: Access Deployer Container

```bash
docker exec -it deployer bash
```

Now inside deployer container with recovery script!

### Step 7: Find First Flag

```bash
ls -la /
cat /flag.txt
```

### Step 8: Run Recovery Script

```bash
sudo /recovery_script.sh
```

**Output:**
```
[*] Starting DoorDasher recovery...
[*] Stopping Hopperoo services...
[*] Restoring original configuration...
[✓] Recovery complete!
```

### Step 9: Verify Recovery

Visit `http://MACHINE_IP:5001` again.

DoorDasher should be restored!

### Step 10: Get Success Flag

```bash
cat /flag.txt
```

### Bonus: News Site Password

Visit `http://MACHINE_IP:5002` or:
```bash
curl http://MACHINE_IP:5002
```

Look for highlighted words in blog posts.

---

## Questions & Answers

### Q1: Command to list running containers?

**Answer:** `docker ps`

### Q2: File to define Docker image instructions?

**Answer:** `Dockerfile`

### Q3: What's the flag?

**Answer:** `THM{DOCKER_ESCAPE_SUCCESS}`

### Bonus: Secret code from news site?

**Answer:** `DeployMaster2025!`

(Look for color-highlighted text on port 5002)

---

## Quick Reference

### Essential Docker Commands

```bash
# List running containers
docker ps

# List all containers
docker ps -a

# Execute command in container
docker exec -it <container> <command>

# List images
docker images

# View logs
docker logs <container>

# Stop container
docker stop <container>

# Remove container
docker rm <container>
```

### Common Shells

```bash
# Lightweight shell
docker exec -it container sh

# Full bash shell
docker exec -it container bash

# Root access
docker exec -it -u root container bash
```

---

## Container Escape Explained

### Why It Works

**Normal isolation:**
```
Container → Docker Daemon → Host
   ↑______________|
   Can't communicate
```

**With socket access:**
```
Container → Socket → Docker Daemon → Host
   ↑_________________|
   Full control!
```

### Attack Chain

```
1. Start in monitoring container
    ↓
2. Find Docker socket mounted
    ↓
3. Use Docker commands from inside
    ↓
4. Access other containers
    ↓
5. Escalate privileges
    ↓
6. Control host system
```

---

## Security Issues

### Critical Vulnerability

**Mounting Docker socket:**
```bash
# DON'T DO THIS!
docker run -v /var/run/docker.sock:/var/run/docker.sock app
```

**Why dangerous:**
- Container can control Docker
- Access all other containers
- Essentially root on host
- Breaks all isolation

### Other Risks

**Privileged containers:**
```bash
docker run --privileged app  # Full host access
```

**Host namespace sharing:**
```bash
docker run --pid=host app    # See host processes
```

**Mounting host paths:**
```bash
docker run -v /:/host app    # Full filesystem access
```

---

## Defense Best Practices

### Secure Configuration

**Never mount socket:**
```bash
# ❌ Bad
-v /var/run/docker.sock:/var/run/docker.sock

# ✅ Good
Use Docker API with authentication
```

**Run as non-root:**
```dockerfile
USER nonroot
```

**Read-only filesystem:**
```bash
docker run --read-only app
```

**Drop capabilities:**
```bash
docker run --cap-drop=ALL app
```

### Image Security

**Use minimal images:**
```dockerfile
FROM alpine:latest  # Small attack surface
FROM scratch        # Minimal possible
```

**Scan for vulnerabilities:**
```bash
docker scan image:latest
```

**Don't store secrets:**
```dockerfile
# ❌ Bad
ENV PASSWORD=secret123

# ✅ Good
Use secrets management
```

---

## Dockerfile Basics

**Simple example:**
```dockerfile
FROM ubuntu:20.04
RUN apt-get update
COPY app.py /app/
CMD ["python3", "/app/app.py"]
```

**Instructions:**
- `FROM` - Base image
- `RUN` - Execute commands
- `COPY` - Copy files
- `CMD` - Default command
- `USER` - Set user
- `WORKDIR` - Set directory
- `EXPOSE` - Document ports

---

## Key Takeaways

- **Containers** are not security boundaries
- **Docker socket** access = root access
- **Never mount** socket in containers
- **Escapes are real** and exploitable
- **Run as non-root** when possible
- **Minimal images** reduce attack surface
- **Scan images** for vulnerabilities
- **Defense in depth** is essential
- **Configuration matters** for security
- **Always assume breach** mentality

---

Happy hunting!
