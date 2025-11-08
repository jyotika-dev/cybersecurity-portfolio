# SQL Fundamentals - TryHackMe

> Learn how to perform basic SQL queries to retrieve and manage data in a database

**Room Link:** https://tryhackme.com/room/sqlfundamentals

<img width="1324" height="874" alt="image" src="https://github.com/user-attachments/assets/05e8335e-21e9-4486-9069-fb7193c4886b" />

---

## Task 1: Introduction

### What is SQL?

SQL (Structured Query Language) is a programming language that helps you:
- Query databases
- Create and modify data
- Manage database structures

### What is MySQL?

MySQL is a Database Management System (DBMS) that allows you to manage databases in a system.

### Why is SQL Important?

**Offensive Security:**
- Exploit SQL vulnerabilities
- SQL injection attacks
- Extract sensitive data

**Defensive Security:**
- Secure SQL vulnerabilities
- Reduce exploitation risks
- Retrieve information from logs

**No answer needed**

---

## Task 2: Databases 101

### What are Databases?

Databases store information that can be:
- Easily accessed
- Analyzed
- Manipulated

They often contain sensitive data like:
- Usernames and passwords
- Personal Identifiable Information (PII)

### Types of Databases

**Relational (SQL)**
- Data relates to each other
- Consistent format
- Focus on accuracy
- Uses tables, rows, and columns

**Non-Relational (NoSQL)**
- Data varies in format
- More flexible structure

### Understanding Tables

**Tables** organize data into rows and columns.

**Example:** Cyber Tools Tracker

| Tool_ID | Name | Date_Learned | Category | Description |
|---------|------|--------------|----------|-------------|
| 1 | Nmap | 2024-01-15 | Offensive | Port scanner |
| 2 | Wireshark | 2024-01-20 | Defensive | Packet analyzer |

**Rows** - Each entry (like Nmap or Wireshark)
**Columns** - Specific details (Name, Date, Category)

### Keys in Databases

**Primary Key**
- Unique identifier for each record
- Like a personal ID number
- Ensures no duplicates

**Foreign Key**
- Links tables together
- Shows relationships between tables
- Can have multiple foreign keys

### Questions

**What type of database should you consider using if the data you're going to be storing will vary greatly in its format?**
- Answer: `non-relational database`

---

**What type of database should you consider using if the data you're going to be storing will reliably be in the same structured format?**
- Answer: `relational database`

---

**In our example, once a record of a book is inserted into our "Books" table, it would be represented as a ___ in that table?**
- Answer: `row`

---

**Which type of key provides a link from one table to another?**
- Answer: `foreign key`

---

**Which type of key ensures a record is unique within a table?**
- Answer: `primary key`

---

## Task 3: SQL

### Why Use SQL?

**Benefits:**
- Speed-efficient
- Reliable organization
- Easy to learn
- Flexible
- Built-in functions for data analysis

### Logging into MySQL

```bash
mysql -u root -p
```

Password: `tryhackme`

You should see:
```
mysql>
```

### Questions

**What serves as an interface between a database and an end user?**
- Answer: `DBMS`

---

**What query language can be used to interact with a relational database?**
- Answer: `SQL`

---

## Task 4: Database and Table Statements

### Basic SQL Commands

**Show all databases:**
```sql
SHOW DATABASES;
```

**Use a specific database:**
```sql
USE database_name;
```

**Show tables in database:**
```sql
SHOW TABLES;
```

### Questions

**Using the statement you've learned to list all databases, it should reveal a database with a flag for a name; what is it?**

```sql
SHOW DATABASES;
```

Look for the flag in the database names.

- Answer: `THM{575a947132312f97b30ee5aeebba629b723d30f9}`

---

**In the list of available databases, you should also see the task_4_db database. Set this as your active database and list all tables in this database; what is the flag present here?**

```sql
USE task_4_db;
SHOW TABLES;
```

The table name itself is the flag!

- Answer: `THM{692aa7eaec2a2a827f4d1a8bed1f90e5e49d2410}`

---

## Task 5: CRUD Operations

### What is CRUD?

**C**reate - Insert new data
**R**ead - Retrieve data
**U**pdate - Modify existing data
**D**elete - Remove data

### Read Operation

**Select all data from a table:**
```sql
SELECT * FROM table_name;
```

**Select specific columns:**
```sql
SELECT column1, column2 FROM table_name;
```

### Questions

**Using the tools_db database, what is the name of the tool in the hacking_tools table that can be used to perform man-in-the-middle attacks on wireless networks?**

```sql
USE tools_db;
SELECT * FROM hacking_tools;
```

Look for a tool with "man-in-the-middle attacks" in the description.

- Answer: `Wi-Fi Pineapple`

---

**Using the tools_db database, what is the shared category for both USB Rubber Ducky and Bash Bunny?**

From the same table, find what category both tools share.

- Answer: `USB attacks`

---

## Task 6: Clauses

### Common Clauses

**DISTINCT** - Remove duplicates
```sql
SELECT DISTINCT column FROM table;
```

**ORDER BY** - Sort results
```sql
SELECT * FROM table ORDER BY column ASC;  -- Ascending
SELECT * FROM table ORDER BY column DESC; -- Descending
```

**WHERE** - Filter results
```sql
SELECT * FROM table WHERE condition;
```

### Questions

**Using the tools_db database, what is the total number of distinct categories in the hacking_tools table?**

```sql
USE tools_db;
SELECT DISTINCT category FROM hacking_tools;
```

Count the unique categories.

- Answer: `6`

---

**Using the tools_db database, what is the first tool (by name) in ascending order from the hacking_tools table?**

```sql
SELECT name FROM hacking_tools ORDER BY name ASC;
```

Take the first result.

- Answer: `Bash Bunny`

---

**Using the tools_db database, what is the first tool (by name) in descending order from the hacking_tools table?**

```sql
SELECT name FROM hacking_tools ORDER BY name DESC;
```

Take the first result.

- Answer: `Wi-Fi Pineapple`

---

## Task 7: Operators

### Comparison Operators

| Operator | Meaning |
|----------|---------|
| `=` | Equal to |
| `!=` or `<>` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

### Logical Operators

| Operator | Meaning |
|----------|---------|
| `AND` | Both conditions true |
| `OR` | Either condition true |
| `LIKE` | Pattern matching |
| `IN` | Match any in list |

### Questions

**Using the tools_db database, which tool falls under the Multi-tool category and is useful for pentesters and geeks?**

```sql
SELECT name FROM hacking_tools 
WHERE category = 'Multi-tool' 
AND description LIKE '%pentesters%';
```

- Answer: `Flipper Zero`

---

**Using the tools_db database, what is the category of tools with an amount greater than or equal to 300?**

```sql
SELECT category FROM hacking_tools 
WHERE amount >= 300;
```

- Answer: `RFID cloning`

---

**Using the tools_db database, which tool falls under the Network intelligence category with an amount less than 100?**

```sql
SELECT name FROM hacking_tools 
WHERE category = 'Network intelligence' 
AND amount < 100;
```

- Answer: `Lan Turtle`

---

## Task 8: Functions

### Aggregate Functions

**COUNT()** - Count rows
```sql
SELECT COUNT(*) FROM table;
```

**SUM()** - Add values
```sql
SELECT SUM(column) FROM table;
```

**MAX()** - Maximum value
```sql
SELECT MAX(column) FROM table;
```

**MIN()** - Minimum value
```sql
SELECT MIN(column) FROM table;
```

### String Functions

**LENGTH()** - String length
```sql
SELECT LENGTH(column) FROM table;
```

**CONCAT()** - Combine strings
```sql
SELECT CONCAT(col1, ' ', col2) FROM table;
```

**GROUP_CONCAT()** - Combine with separator
```sql
SELECT GROUP_CONCAT(column SEPARATOR ' & ') FROM table;
```

**SUBSTRING()** - Extract characters
```sql
SUBSTRING(string, start_position, length)
```

### Questions

**Using the tools_db database, what is the tool with the longest name based on character length?**

```sql
SELECT name, LENGTH(name) AS name_length 
FROM hacking_tools 
ORDER BY name_length DESC;
```

The longest name has 16 characters.

- Answer: `USB Rubber Ducky`

---

**Using the tools_db database, what is the total sum of all tools?**

```sql
SELECT SUM(amount) FROM hacking_tools;
```

- Answer: `1444`

---

**Using the tools_db database, what are the tool names where the amount does not end in 0, and group the tool names concatenated by " & "?**

This one is tricky! We need to:
1. Filter amounts that don't end in 0
2. Concatenate tool names with " & "

```sql
SELECT GROUP_CONCAT(name SEPARATOR ' & ') 
FROM hacking_tools 
WHERE SUBSTRING(amount, -1, 1) != 0;
```

**Understanding SUBSTRING(amount, -1, 1):**
- `amount` - Column to extract from
- `-1` - Start from last character
- `1` - Extract only 1 character
- `!= 0` - Filter out amounts ending in 0

- Answer: `Flipper Zero & iCopy-XS`

---

## Task 9: Conclusion

**No answer needed**

---

## Quick Reference

### Basic Commands

| Command | Purpose |
|---------|---------|
| `SHOW DATABASES;` | List all databases |
| `USE database_name;` | Select database |
| `SHOW TABLES;` | List tables in database |
| `SELECT * FROM table;` | Show all data in table |
| `DESC table;` | Describe table structure |

### CRUD Operations

| Operation | SQL Command |
|-----------|-------------|
| Create | `INSERT INTO table VALUES (...);` |
| Read | `SELECT * FROM table;` |
| Update | `UPDATE table SET col=val WHERE ...;` |
| Delete | `DELETE FROM table WHERE ...;` |

### Clauses

| Clause | Purpose | Example |
|--------|---------|---------|
| `DISTINCT` | Remove duplicates | `SELECT DISTINCT category FROM tools;` |
| `WHERE` | Filter results | `SELECT * FROM tools WHERE amount > 100;` |
| `ORDER BY` | Sort results | `SELECT * FROM tools ORDER BY name ASC;` |
| `LIMIT` | Limit results | `SELECT * FROM tools LIMIT 10;` |
| `GROUP BY` | Group results | `SELECT category, COUNT(*) FROM tools GROUP BY category;` |

### Comparison Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `=` | Equal | `WHERE name = 'Nmap'` |
| `!=` | Not equal | `WHERE amount != 0` |
| `>` | Greater than | `WHERE amount > 100` |
| `<` | Less than | `WHERE amount < 50` |
| `>=` | Greater or equal | `WHERE amount >= 300` |
| `<=` | Less or equal | `WHERE amount <= 100` |

### Logical Operators

| Operator | Purpose | Example |
|----------|---------|---------|
| `AND` | Both true | `WHERE cat='Tool' AND amount>100` |
| `OR` | Either true | `WHERE cat='Tool' OR cat='Multi'` |
| `LIKE` | Pattern match | `WHERE desc LIKE '%wireless%'` |
| `IN` | Match list | `WHERE cat IN ('Tool', 'Multi')` |
| `NOT` | Negate | `WHERE NOT amount = 0` |

### Aggregate Functions

| Function | Purpose | Example |
|----------|---------|---------|
| `COUNT()` | Count rows | `SELECT COUNT(*) FROM tools;` |
| `SUM()` | Add values | `SELECT SUM(amount) FROM tools;` |
| `AVG()` | Average | `SELECT AVG(amount) FROM tools;` |
| `MAX()` | Maximum | `SELECT MAX(amount) FROM tools;` |
| `MIN()` | Minimum | `SELECT MIN(amount) FROM tools;` |

### String Functions

| Function | Purpose | Example |
|----------|---------|---------|
| `LENGTH()` | String length | `SELECT LENGTH(name) FROM tools;` |
| `CONCAT()` | Combine strings | `SELECT CONCAT(name, ' - ', cat);` |
| `GROUP_CONCAT()` | Combine with separator | `SELECT GROUP_CONCAT(name SEPARATOR ', ');` |
| `SUBSTRING()` | Extract characters | `SUBSTRING(name, 1, 3)` |
| `UPPER()` | To uppercase | `SELECT UPPER(name) FROM tools;` |
| `LOWER()` | To lowercase | `SELECT LOWER(name) FROM tools;` |

---

## Common Query Patterns

### Basic Selection
```sql
SELECT column1, column2 FROM table_name;
```

### With Condition
```sql
SELECT * FROM table_name WHERE condition;
```

### With Sorting
```sql
SELECT * FROM table_name ORDER BY column ASC/DESC;
```

### With Multiple Conditions
```sql
SELECT * FROM table_name 
WHERE condition1 AND condition2;
```

### Pattern Matching
```sql
SELECT * FROM table_name 
WHERE column LIKE '%pattern%';
```

### Counting
```sql
SELECT COUNT(*) FROM table_name 
WHERE condition;
```

### Grouping
```sql
SELECT category, COUNT(*) 
FROM table_name 
GROUP BY category;
```

---

## Key Takeaways

- **SQL is essential** for both offensive and defensive security
- **Relational databases** use tables with related data
- **Primary keys** uniquely identify records
- **Foreign keys** link tables together
- **CRUD operations** - Create, Read, Update, Delete
- **SELECT** is used to retrieve data
- **WHERE** filters results based on conditions
- **ORDER BY** sorts results (ASC/DESC)
- **DISTINCT** removes duplicates
- **Operators** (`=`, `!=`, `>`, `<`, `LIKE`) filter data
- **Functions** (`COUNT`, `SUM`, `MAX`, `LENGTH`) analyze data
- **GROUP_CONCAT** combines multiple values
- **SUBSTRING** extracts specific characters
- Always end SQL statements with semicolon (`;`)
- SQL is **case-insensitive** but use UPPERCASE for keywords

Understanding SQL is crucial for database management, web application security, and penetration testing. Master the basics first, then move on to advanced topics like SQL injection
