# 🐧 Advanced Text-Fu

https://labex.io/lesson/regular-expressions-regex
This file summarizes key concepts and commands for advanced Linux text manipulation, focusing on powerful tools like regular expressions and text editors. 
These skills are vital for advanced log analysis, system administration, and digital forensics.

---

## 🔹 Regular Expressions (Regex)

**Regex** is a powerful language for searching and manipulating text based on patterns. It's a fundamental skill for anyone working with data.

| Component | Description | Example |
| :--- | :--- | :--- |
| **`.`** | Matches any single character. | `a.t` matches "cat," "hat," "bat" |
| **`*`** | Matches zero or more occurrences of the preceding character. | `go*l` matches "gl," "gol," "gool" |
| **`^`** | Matches the beginning of a line. | `^Error` matches lines that start with "Error" |
| **`$`** | Matches the end of a line. | `login$` matches lines that end with "login" |
| **`[]`** | Matches any single character within the brackets. | `[a-z]` matches any lowercase letter |
| **`[^]`** | Matches any single character NOT in the brackets. | `[^0-9]` matches any non-digit character |
| **`()`** | Groups patterns together. | `(ab)+` matches "ab," "abab," "ababab" |

---

## 🔹 Vim (Vi Improved)

**Vim** is a highly efficient, keyboard-driven text editor. Its power comes from its different modes, which allow for rapid navigation and editing without using a mouse.

### **Modes**
* **Normal Mode:** The default mode for navigation, search, and issuing commands.
* **Insert Mode:** For inserting and typing text.
* **Visual Mode:** For selecting blocks of text.

### **Basic Commands**
* **`h` `j` `k` `l`**: Move the cursor (left, down, up, right).
* **`i`**: Switch to Insert mode.
* **`dd`**: Delete the current line.
* **`yy`**: Copy the current line.
* **`p`**: Paste the copied or deleted text.

### **Search, Save, and Exit**
* **`/pattern`**: Search forward for a pattern.
* **`:w`**: Save the file.
* **`:q`**: Quit the editor.
* **`:wq`**: Save and quit.
* **`:q!`**: Quit without saving.

---

## 🔹 Emacs

**Emacs** is another powerful, extensible text editor known for its extensive features and customization.

### **Basic Commands**
* **`Ctrl` + `x`, `Ctrl` + `f`**: Open a file.
* **`Ctrl` + `s`**: Search forward.
* **`Ctrl` + `k`**: Delete a line (cut).
* **`Ctrl` + `y`**: Yank (paste) deleted text.
* **`Ctrl` + `g`**: Exit a command.

### **File & Buffer Management**
* **`Ctrl` + `x`, `Ctrl` + `s`**: Save the current file.
* **`Ctrl` + `x`, `b`**: Switch to a different buffer (file).
* **`Ctrl` + `x`, `Ctrl` + `c`**: Exit Emacs.

---

### **📌 Cybersecurity Applications**

Advanced text editors and regex are critical for **Incident Response (IR)** and **Threat Hunting**.

* **Vim/Emacs for Forensics:** A forensics investigator might use Vim to quickly open and analyze large log files on a compromised server without needing a GUI.
* **Regex for IOCs:** A SOC analyst can use regex with `grep` to hunt for Indicators of Compromise (IOCs) such as specific IP address patterns or malicious domain names within massive log datasets.
* **Scripting:** Security professionals use these tools in shell scripts to automate log parsing, audit report generation, and data filtering.
