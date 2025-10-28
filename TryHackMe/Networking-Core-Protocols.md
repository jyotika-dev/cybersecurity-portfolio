# 🌐 TryHackMe: Networking Core Protocols Writeup 📚

**Room Link:** [https://tryhackme.com/room/networkingcoreprotocols](https://tryhackme.com/room/networkingcoreprotocols)

<img width="1587" height="1028" alt="image" src="https://github.com/user-attachments/assets/c6d951a5-69ea-4ff5-8041-c929d503ad44" />


---

## 🎯 Overview

This room, part 3 of the TryHackMe networking series, provides an excellent foundation in the **core networking protocols** that power the modern internet. It focuses on how these protocols operate behind the scenes when we perform common actions like browsing the web, sending emails, and transferring files.

**Prerequisites:** A foundational understanding of the **OSI Model** and **TCP/IP Model** is highly recommended before starting this room.

## ✨ Key Topics Covered

The lab explores the function, ports, and layers of the following essential protocols:

* **DNS (Domain Name System):** Resolving human-readable domain names to machine-readable IP addresses.
* **WHOIS:** Querying domain name registration information.
* **HTTP/HTTPS:** Accessing and securing the World Wide Web.
* **FTP (File Transfer Protocol):** Transferring files between clients and servers.
* **SMTP (Simple Mail Transfer Protocol):** Sending email messages.
* **POP3 (Post Office Protocol version 3):** Retrieving email from a mail server.
* **IMAP (Internet Message Access Protocol):** Synchronizing and managing email on a mail server.

---

## 📝 Task Walkthrough & Notes

### Task 1: Introduction 🚀

* **Action:** Start the AttackBox and the target machine.
* **Note:** Always allow 2-3 minutes for the virtual machines to fully boot up before proceeding.
* **Question:** No answer needed.

### Task 2: DNS - Remembering Addresses 🗺️

| Concept | Details |
| :--- | :--- |
| **Function** | Maps domain names (e.g., tryhackme.com) to their corresponding IP addresses. |
| **Layer** | Application Layer (Layer 7) |
| **Port** | **UDP Port 53** (primarily, but can use TCP for zone transfers) |

#### DNS Record Types (Key for Resolution)

| Record Type | Description |
| :--- | :--- |
| **A Record** | Maps a hostname to an **IPv4** address. |
| **AAAA Record** | Maps a hostname to an **IPv6** address. |
| **CNAME Record** | The **canonical name** or alias for a domain. |
| **MX Record** | Specifies the **Mail Exchanger** (mail server) for a domain. |
| **TXT Record** | Stores descriptive, **text-based information**. |

| Task Questions | Answer |
| :--- | :--- |
| What type of DNS record is used to resolve a domain name to an IPv6 address? | **AAAA** |
| What type of DNS record is used to redirect traffic for a domain to a mail server? | **MX** |

***

### Task 3: WHOIS (Domain Registration Information) 🆔

* **Function:** Used to query databases that store information about registered users or assignees of an Internet resource, like a domain name, an IP address block, or an autonomous system.
* **Tool:** The `whois` tool is used to query this database directly.

| Task Questions | Answer |
| :--- | :--- |
| When was the x.com record created? Provide the answer in YYYY-MM-DD format. | **1993-04-02** |
| When was the twitter.com record created? Provide the answer in YYYY-MM-DD format. | **2000-01-21** |

***

### Task 4: HTTP(S): Accessing the Web 🕸️

* **HTTP (HyperText Transfer Protocol):** The foundation of data communication for the World Wide Web.
    * **Port:** **TCP Port 80**
    * **Security:** Data is transferred in **plain text** (unencrypted).
* **HTTPS (HTTP Secure):** The secure version of HTTP.
    * **Port:** **TCP Port 443**
    * **Security:** Uses **SSL/TLS encryption** to secure the connection.

| Task Questions | Answer |
| :--- | :--- |
| Use telnet to access the file flag.html on MACHINE_IP. What is the hidden flag? | **THM{TELNET-HTTP}** |

***

### Task 5: FTP: Transferring Files 💾

* **FTP (File Transfer Protocol):** A standard network protocol used to transfer computer files between a client and server on a computer network.
    * **Data Port:** **TCP Port 20**
    * **Control Port:** **TCP Port 21**
    * **Note:** FTP sends credentials and data **unencrypted**.

| Task Questions | Answer |
| :--- | :--- |
| Using the FTP client ftp on the AttackBox, access the FTP server at MACHINE_IP and retrieve flag.txt. What is the flag found? | **THM{FAST-FTP}** |

***

### Task 6: SMTP: Sending Email 📤

* **SMTP (Simple Mail Transfer Protocol):** Governs the **sending** of emails.
* **Port:** **TCP Port 25** (Default)
* **Action:** You'll typically use `telnet` or `netcat` to connect to port 25 and issue commands manually.

| Task Questions | Answer |
| :--- | :--- |
| Which SMTP command indicates that the client will start the contents of the email message? | **DATA** |
| What does the email client send to indicate that the email message has been fully entered? | **.** |

***

### Task 7: POP3: Receiving Email 📥

* **POP3 (Post Office Protocol version 3):** Used by local email clients to **retrieve** email from a remote server.
* **Key Feature:** Typically **downloads** the email and then **deletes** it from the server.
* **Port:** **TCP Port 110** (Default)

| Task Questions | Answer |
| :--- | :--- |
| What does the email client send to indicate that the email message has been fully entered? | **Dovecot** |
| Use telnet to connect to MACHINE_IP’s POP3 server. What is the flag contained in the fourth message? | **THM{TELNET_RETR_EMAIL}** |

***

### Task 8: IMAP: Synchronizing Email 🔄

* **IMAP (Internet Message Access Protocol):** An alternative protocol for **receiving** email.
* **Key Feature:** Emails are **stored on the server**, and the client synchronizes the mailbox. This allows management from multiple devices.
* **Port:** **TCP Port 143** (Default)

| Task Questions | Answer |
| :--- | :--- |
| What IMAP command retrieves the fourth email message? | **FETCH 4 body[]** |

***

### Task 9: Conclusion ✅

* **Summary:** This room covered the fundamental Application Layer protocols that underpin most internet services.
* **Completion:** Room completed successfully!

| Task Questions | Answer |
| :--- | :--- |
| Room completed? | **Y** |

---

## 💡 Conclusion & Takeaways

This room effectively demonstrated the roles of the foundational Application Layer protocols. Understanding these basics is crucial for anyone pursuing a career in networking, cybersecurity, or IT administration. The practical exercises involving tools like `dig`, `whois`, and `netcat` reinforce the theoretical knowledge.

**Key Takeaway:** Protocols like DNS, HTTP, and SMTP are the invisible gears that make the internet work, each having a specific job and port to fulfill.

---
