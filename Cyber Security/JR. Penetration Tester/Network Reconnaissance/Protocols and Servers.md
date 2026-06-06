> [!note] Purpose
> HTTP, FTP, POP3, SMTP, and IMAP form the foundation of web browsing, file transfers, and email. Understanding their low-level mechanics is essential for cybersecurity roles like penetration testing, network defense, and security engineering.

## Beneath the GUI

Most protocol operations are hidden by Graphical User Interfaces (GUIs). By using a simple Telnet client, you can bypass the GUI to "talk" directly to the services and observe the underlying commands in action.

## Why Learn Decades-Old Protocols?

1. **They are still in use:** Modern encrypted versions (HTTPS, SFTP, IMAPS) simply wrap the exact same underlying protocol commands in TLS encryption.
2. **Cleartext is still prevalent:** During security assessments and penetration tests, you will routinely encounter unencrypted protocols in legacy systems, internal networks, IoT devices, and misconfigured services.
3. **Protocols explain attacks:** Understanding the foundation makes vulnerabilities make sense. For example, knowing how SMTP works explains why email spoofing is possible, and understanding HTTP clarifies web application vulnerabilities.

## The Primary Vulnerability
> [!danger] Cleartext Credentials
> These protocols were originally designed for trusted academic networks. As a result, they transmit data—including passwords—in cleartext. On modern networks, this is a severe vulnerability, as anyone with access to the network traffic can capture unencrypted credentials.

# The Telnet Protocol

> [!note] Definition
> **Telnet** (TCP Port 23) is an application-layer protocol used to log into a remote computer's virtual terminal to run programs and perform system administration.

## Telnet Today

Telnet has been almost entirely replaced by SSH for interactive remote access. Finding an open Telnet port during a penetration test is a significant finding that indicates a security misconfiguration. You may still encounter it in:
* Legacy systems and older network equipment (routers, switches, industrial controllers).
* Embedded devices and IoT equipment with limited resources.
* Internal networks where security was never prioritized.
* Misconfigured systems where it was enabled but never disabled.

> [!tip] Telnet Client as a Testing Tool
> While Telnet *servers* are rare today, the Telnet *client* remains a highly useful tool. You can use it to connect to any TCP port (like port 80) to manually interact with other text-based protocols.

## How Telnet Works

1. The server listens for incoming connections on port 23.
2. The user is prompted to provide a login name.
3. The user is prompted for a password (the characters are visually hidden on the screen).
4. Upon successful authentication, the server grants a command prompt (a `$` indicates a non-root terminal).

## Why Telnet is Insecure
> [!danger] Cleartext Vulnerability
> All communication between the client and server is sent in **cleartext**. While the password is automatically hidden on the user's screen, this visual protection is meaningless because the actual network traffic travels completely unencrypted

Because the traffic is unencrypted, anyone capturing the network data can easily steal usernames and passwords. This includes:
* Attackers on the same network segment.
* Anyone who has compromised a router or switch along the network path.
* Malicious insiders with network access.
* Attackers performing a Man-in-the-Middle (MITM) attack.

**The Secure Alternative:** SSH (Secure Shell) has been the standard for over two decades because it encrypts all traffic, including credentials.

# HTTP (Hypertext Transfer Protocol)

> [!note] Concept
> **HTTP** is the protocol used to transfer web pages and files across the World Wide Web. It operates on a request-response model between a client (web browser) and a web server. 
## HTTP vs. HTTPS
HTTP transmits data in **cleartext**, meaning anyone intercepting the network traffic can read sensitive information like passwords. **HTTPS** wraps HTTP inside TLS encryption to secure it. 

> [!tip] Why learn cleartext HTTP?
> * The underlying commands and structure are identical to HTTPS.
> * Proxies like **Burp Suite** decrypt HTTPS traffic, allowing you to analyze raw HTTP.
> * You will frequently encounter plain HTTP on legacy systems and internal networks during penetration tests.
> * Understanding raw HTTP is essential for identifying web vulnerabilities.

## Manual HTTP Requests via Telnet
Because HTTP/1.1 is text-based, you can use Telnet as a manual "web browser" to interact with the server.

1. **Connect:** `telnet <IP> 80`
2. **Request:** Type `GET /index.html HTTP/1.1` (or `GET / HTTP/1.1` for the default page).
3. **Host Header:** Type `host: telnet` and press **Enter twice**.

If the page doesn't exist, the server returns an `error 404`.

## Information Gathering via Headers
When you send a manual request, the server responds with HTTP headers before the HTML content. 
* **The `Server` Header:** Often reveals the web server software, version, and operating system (e.g., `Server: nginx/1.18.0 (Ubuntu)`). 
* **Recon Value:** This allows you to research known vulnerabilities for that specific version and tailor your attacks. Security-conscious admins will suppress this banner.

## Common Web Servers & Clients
* **Servers:** * **Nginx:** Most widely used, open-source, known for high performance/concurrent connections.
  * **Apache:** Highly configurable with a vast module ecosystem.
  * **IIS:** Microsoft's enterprise server (requires Windows Server license).
  * **Others:** LiteSpeed, Node.js, and Caddy (features automatic HTTPS).
* **Clients (Browsers):** Chrome, Safari, Edge, and **Firefox** (often preferred for penetration testing due to its developer tools and add-ons).

## HTTP Protocol Versions
* **HTTP/1.1:** The decades-old standard. Text-based and human-readable, making it the easiest for manual testing.
* **HTTP/2:** Introduces multiplexing, header compression, and server push. It is binary-based, making it harder to interact with manually via Telnet.
* **HTTP/3:** Uses the **QUIC** protocol (built on UDP instead of TCP) for improved performance on unreliable networks.
# File Transfer Protocol (FTP)

> [!note] Concept
> **FTP** is an early internet protocol designed for efficient file transfers between computers. It uses a **dual-connection architecture**, utilizing Port 21 for commands (control channel) and a separate port for actual file transfers (data channel).

## Security & Modern Alternatives
> [!danger] Cleartext Vulnerability
> Like HTTP and Telnet, standard FTP sends all data—including credentials and the files themselves—in **cleartext**. 

Because of this, standard FTP is mostly obsolete, replaced by secure alternatives:
* **SFTP (SSH File Transfer Protocol):** Runs over SSH (Port 22). Encrypts all traffic. The most common modern replacement.
* **FTPS (FTP Secure):** Adds TLS encryption (Port 990 for implicit, or Port 21 using `STARTTLS`).
* **SCP (Secure Copy Protocol):** Runs over SSH, but is actively being deprecated in favor of SFTP.

*Note: You will still find plain FTP on legacy systems, IoT devices, or as Anonymous FTP servers during pentests.*

## Connection Modes
FTP struggles with firewalls because of its dual-channel nature. It solves this using two modes:

* **Active Mode:** The client opens the control channel, but the **server initiates** the data connection back to the client from port 20. This often gets blocked by client-side firewalls/NAT.
* **Passive Mode (`PASV`):** The **client initiates** both the control and data connections (using ports above 1023). This is much more firewall-friendly and is the modern default.

## Manual Interaction Commands
You can use Telnet (`telnet <IP> 21`) to interact with the control channel, but you cannot download files this way because Telnet cannot automatically handle the secondary data channel.

| Command | Function |
| :--- | :--- |
| `USER <name>` / `PASS <password>` | Authentication. |
| `SYST` | Returns the system type (e.g., UNIX). |
| `PASV` | Switches to Passive Mode. |
| `TYPE A` / `TYPE I` | Switches file transfer mode to ASCII (text) or Image/Binary. |
| `STAT` | Displays server status. |

## Anonymous FTP
Historically used for public file distribution. It allows login without valid credentials.
* **Username:** `anonymous` or `ftp`
* **Password:** Any email address (or left blank).
* **Pentest Value:** Always test for anonymous login. It often reveals accidentally exposed sensitive files, config backups, or allows malicious file uploads if write access is misconfigured.

## Common Software
* **Servers:** `vsftpd` (Very Secure FTP Daemon, common on Linux), `ProFTPD`, `Pure-FTPd`, Windows IIS.
* **Clients:** The standard console `ftp` command or GUI tools like **FileZilla**. *(Note: Modern web browsers no longer support FTP).*
# Email Delivery & SMTP

> [!note] The Email Pipeline
> Delivering an email requires a sequence of specialized agents to route the message from sender to recipient.



## Delivery Components
* **MUA (Mail User Agent):** The email client (e.g., Outlook, Thunderbird, Webmail).
* **MSA (Mail Submission Agent):** Receives mail from the sender's MUA and checks for errors.
* **MTA (Mail Transfer Agent):** Routes and delivers mail across the internet (server-to-server).
* **MDA (Mail Delivery Agent):** Stores the email in the recipient's mailbox until they retrieve it.

## SMTP (Simple Mail Transfer Protocol)
SMTP is used strictly for **sending** and routing emails. *(POP3 and IMAP are used for receiving).*

### Ports & Encryption
* **Port 25 (Traditional):** Used for MTA-to-MTA (server-to-server) communication. Often blocked for residential networks to prevent spam. Uses cleartext or negotiates encryption via `STARTTLS`.
* **Port 587 (Submission):** Used by MUAs to submit mail to their MSA. The recommended standard for clients. Requires authentication and `STARTTLS`.
* **Port 465 (SMTPS):** Implicit TLS. Encryption begins immediately upon connection.

## Manual Interaction
Because standard SMTP on Port 25 is text-based, you can use Telnet to act as an email client (`telnet <IP> 25`). 
* **Key Commands:** `helo` (initiate), `mail from:` (sender), `rcpt to:` (recipient), `data` (begins the message body). To end and send the message, type a single period `.` on a new line.

## Security Implications & Spoofing
> [!danger] The Spoofing Flaw
> SMTP was built for trusted networks and has **no built-in sender verification**. An attacker using Telnet can manually specify *any* address in the `mail from:` field, and the server will accept it. This fundamental lack of verification is exactly how email spoofing and phishing work.

* **Pentest Value:** Security professionals test for misconfigured "open relays" (servers that forward spam), attempt spoofing to test awareness, and analyze email headers to trace the true origin of messages.
# POP3 (Post Office Protocol version 3)

> [!note] Concept
> **POP3** is a protocol used by an email client to download messages from a Mail Delivery Agent (MDA) server. 

## Ports & Encryption
* **Port 110 (Default):** Unencrypted cleartext (can optionally be upgraded using the `STLS` command).
* **Port 995 (POP3S):** Encrypted with implicit TLS from the start. This is the modern requirement.

## Core Behavior: "Download and Delete"
By default, POP3 downloads emails directly to your local device and deletes them from the server.
* **Pros:** Minimizes server storage requirements; excellent for offline access or local archiving.
* **Cons:** Emails are isolated to a single device. There is no synchronization across multiple devices (if the device breaks, the emails are gone unless backed up locally). *Note: For multi-device synchronization, IMAP is preferred.*

## Manual Interaction & Commands
Because Port 110 is unencrypted, you can manually interact with the server using Telnet (`telnet <IP> 110`).

| Command | Function |
| :--- | :--- |
| `USER <username>` | Identifies the user. |
| `PASS <password>` | Authenticates the password. |
| `STAT` | Returns the total number of messages and the total inbox size. |
| `LIST` | Lists all new messages alongside their file sizes. |
| `RETR <n>` | Retrieves and reads message number `n`. |
| `DELE <n>` | Marks message number `n` for deletion. |
| `QUIT` | Ends the session and permanently deletes any marked messages. |

## Security Implications
> [!danger] Cleartext Vulnerability
> When running on Port 110, the `USER` and `PASS` commands are transmitted in cleartext. During a penetration test, intercepting this network traffic allows you to sniff valid credentials. Attackers can leverage these credentials to read sensitive communications, intercept password reset links, or attempt password reuse on other corporate systems.

# POP3 (Post Office Protocol version 3)

> [!note] Concept
> **POP3** is a protocol used by an email client to download messages from a Mail Delivery Agent (MDA) server. 



## Ports & Encryption
* **Port 110 (Default):** Unencrypted cleartext (can optionally be upgraded using the `STLS` command).
* **Port 995 (POP3S):** Encrypted with implicit TLS from the start. This is the modern requirement.

## Core Behavior: "Download and Delete"
By default, POP3 downloads emails directly to your local device and deletes them from the server.
* **Pros:** Minimizes server storage requirements; excellent for offline access or local archiving.
* **Cons:** Emails are isolated to a single device. There is no synchronization across multiple devices (if the device breaks, the emails are gone unless backed up locally). *Note: For multi-device synchronization, IMAP is preferred.*

## Manual Interaction & Commands
Because Port 110 is unencrypted, you can manually interact with the server using Telnet (`telnet <IP> 110`).

| Command | Function |
| :--- | :--- |
| `USER <username>` | Identifies the user. |
| `PASS <password>` | Authenticates the password. |
| `STAT` | Returns the total number of messages and the total inbox size. |
| `LIST` | Lists all new messages alongside their file sizes. |
| `RETR <n>` | Retrieves and reads message number `n`. |
| `DELE <n>` | Marks message number `n` for deletion. |
| `QUIT` | Ends the session and permanently deletes any marked messages. |

## Security Implications
> [!danger] Cleartext Vulnerability
> When running on Port 110, the `USER` and `PASS` commands are transmitted in cleartext. During a penetration test, intercepting this network traffic allows you to sniff valid credentials. Attackers can leverage these credentials to read sensitive communications, intercept password reset links, or attempt password reuse on other corporate systems.

# IMAP (Internet Message Access Protocol)

> [!note] Concept
> **IMAP** is a sophisticated email protocol designed to keep messages synchronized across multiple devices and mail clients using a **server-side storage** model. 
## Why IMAP is the Standard
IMAP has largely replaced POP3 because modern users access email across multiple devices (phones, laptops, tablets).
* Emails remain on the server and are accessible from anywhere.
* Read/unread status, folders, and flags synchronize seamlessly across all clients.
* Deleting an email on one device removes it everywhere.
* Searches are performed server-side, saving local bandwidth and storage.

## Ports & Encryption
* **Port 143 (Default):** Unencrypted cleartext. Many servers support upgrading to TLS via the `STARTTLS` command.
* **Port 993 (IMAPS):** Encrypted with implicit TLS from the start. This is strictly required by major modern providers (Gmail, Outlook, Yahoo).

## Manual Interaction & Commands
You can interact with a plaintext IMAP server using Telnet (`telnet <IP> 143`). 

> [!warning] The Tag Requirement
> Unlike POP3 or SMTP, IMAP requires every command to be preceded by a random string or **tag** (e.g., `c1`, `c2`, `a001`) so the client and server can track specific replies.

### Command Reference

| Command | Description | Example Syntax |
| :--- | :--- | :--- |
| **`LOGIN`** | Authenticates the user. | `c1 LOGIN username password` |
| **`LIST`** | Lists all mailbox folders (e.g., INBOX, Trash, Sent). | `c2 LIST "" "*"` |
| **`SELECT`** | Opens a folder for read/write access. | `c3 SELECT INBOX` |
| **`EXAMINE`** | Opens a folder for read-only access (checking for new mail). | `c4 EXAMINE INBOX` |
| **`FETCH`** | Retrieves a specific message by its number. | `c5 FETCH 1 BODY[]` |
| **`SEARCH`** | Searches for messages matching specific criteria. | `c6 SEARCH criteria` |
| **`STORE`** | Marks a message with a specific flag (e.g., read). | `c7 STORE 1 +FLAGS (\Seen)` |
| **`LOGOUT`** | Ends the session. | `c8 LOGOUT` |

### Analyzing the Server Response
When you connect, the server's initial response includes a `CAPABILITY` list. This is highly useful for reconnaissance:
* `IMAP4rev1`: The IMAP version.
* `STARTTLS`: Confirms the server supports encryption upgrades.
* `IDLE`: Shows the server supports push notifications for new mail.
* `ACL`: Indicates Access Control List support.

## Security & Exploitation Implications

> [!danger] Cleartext & High-Value Compromise
> On Port 143, the `LOGIN` command transmits the username and password in cleartext, making it vulnerable to network sniffing.

Compromising an IMAP account is significantly more valuable to an attacker than a POP3 account due to the server-side storage model:
* **Persistent Access:** Emails remain on the server, allowing the attacker to continuously monitor new incoming mail indefinitely.
* **Historical Data:** The attacker gains access to the entire mailbox history (potentially years of sensitive corporate communications).
* **Password Reset Abuse:** Attackers can actively search the server for password reset links to pivot into other accounts.
* **Business Email Compromise (BEC):** Persistent access facilitates invoice fraud, executive impersonation, and data theft.
* **Lateral Movement:** Inboxes frequently contain internal network documentation, credentials, and VPN profiles useful for penetrating deeper into an organization.