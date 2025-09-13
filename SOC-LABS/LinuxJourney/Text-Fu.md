# 📝 Linux Text-Fu Notes

https://labex.io/lesson/stdout-standard-out-redirect
These are my notes from **Linux Text-Fu** covering standard input/output, text manipulation, and filtering commands. 
They are essential for log analysis, scripting, and cybersecurity investigations.

---

## 1. File Descriptors (I/O)

* **`stdout` (Standard Out):** The standard output stream, which displays the normal results of a command. By default, it prints text to the screen.
    ```bash
    echo "Hello World" > out.txt # Redirects stdout to a file
    ```
* **`stdin` (Standard In):** The standard input stream, which takes user input or file content as input. By default, it reads from the keyboard.
    ```bash
    cat < input.txt # Reads a file as input for cat
    ```
* **`stderr` (Standard Error):** The standard error stream, which prints command errors. It can be redirected separately from `stdout`.
    ```bash
    ls /fake/path 2> errors.txt
    ```

## 2. Pipes and Redirects

* **`|` (Pipe):** Sends the `stdout` of one command as the `stdin` to another.
* **`tee`:** Saves command output to both the screen and a file simultaneously.
    ```bash
    dmesg | grep "error" | tee errors_found.txt
    ```

## 3. Environment Variables

* **`env`:** Shows all current environment variables.
* **`echo $VARIABLE`:** Prints the value of a specific variable.
    ```bash
    env
    echo $PATH
    ```

## 4. Text Manipulation & Filtering

* **`cut`:** Extracts specific fields or columns from each line of text.
    ```bash
    cut -d":" -f1 /etc/passwd # Show only usernames from /etc/passwd
    ```
* **`paste`:** Merges lines from different files side by side.
    ```bash
    paste file1.txt file2.txt
    ```
* **`head`:** Displays the first N lines of a file.
    ```bash
    head -n 20 logfile
    ```
* **`tail`:** Displays the last N lines of a file. The `-f` flag is critical for **real-time log monitoring**.
    ```bash
    tail -f /var/log/auth.log
    ```
* **`expand` & `unexpand`:**
    * `expand`: Converts tab characters into spaces.
    * `unexpand`: Converts spaces back into tabs.
    ```bash
    expand file.txt
    unexpand -t 4 file.txt
    ```
* **`join` & `split`:**
    * `join`: Combines lines from two files based on a common field.
    * `split`: Breaks a large file into smaller, more manageable parts.
    ```bash
    join users.txt departments.txt
    split -l 1000 logfile.txt part_
    ```
* **`sort`:** Sorts lines of text alphabetically or numerically.
    ```bash
    sort -n numbers.txt
    ```
* **`tr` (Translate):** Replaces or deletes specific characters in text.
    ```bash
    tr a-z A-Z < lowercase.txt > uppercase.txt
    ```
* **`uniq` (Unique):** Removes duplicate lines from sorted input. The `-c` flag counts occurrences of each line.
    ```bash
    sort ips.txt | uniq -c
    ```
* **`wc` & `nl`:**
    * `wc` (Word Count): Counts lines, words, or characters.
    * `nl` (Number Lines): Adds line numbers to a file's output.
    ```bash
    wc -l logfile
    nl script.sh
    ```

---

## 5. Cybersecurity Applications

* **Log Analysis:** A powerful combination like `grep`, `cut`, `sort`, and `uniq` can be used to quickly identify unique IP addresses associated with failed login attempts from a log file.
* **Real-time Monitoring:** Use `tail -f` to monitor log files in real time, which is essential for detecting ongoing attacks.
* **Quick Triage:** Use `wc -l` to quickly count the number of suspicious entries in a filtered log, helping you prioritize investigations.
