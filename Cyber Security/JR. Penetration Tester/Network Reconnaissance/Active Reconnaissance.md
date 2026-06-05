
> [!note] Definition
> **Active reconnaissance** is the process of directly interacting with a target system or network (transmitting packets, making connections, probing services) to gather information.

## Active vs. Passive

* **Passive:** Collects public OSINT data (DNS, WHOIS, certificate logs, Shodan) without touching the target. Leaves no trace.
* **Active:** Makes direct contact (visiting sites, pinging, tracing routes, connecting to ports). Reveals live services, open ports, network paths, and banners, but leaves footprints (logs, IDS alerts, WAF blocks, honeypot triggers).

> [!info] Prerequisites
> Understanding active reconnaissance requires familiarity with core networking concepts: **TCP, UDP, ICMP, and port numbers**.

## Core Tools
* **Modern Standard:** Web browsers, `ping`, `traceroute`, `netcat`, and `curl`.
* **The Role of Telnet:** While largely obsolete for web interactions due to the dominance of HTTPS, cleartext tools like `telnet` remain essential for learning the fundamentals of banner grabbing and interacting with legacy environments.

## The Modern Landscape & Detectability
Active probing is riskier and more detectable today than ever before. 
* **Defensive Tech:** Targets heavily utilize CDNs (Cloudflare, Akamai), WAFs, zero-trust models, SIEMs, EDR solutions, and cloud-native monitoring to catch and block probes.
* **Protocol Shifts:** IPv6 adoption means many hosts now respond to `ping6` but actively filter ICMPv4. HTTPS dominates, making plaintext protocols largely obsolete for web interactions.

## Red Team vs. Blue Team Strategies
* **Red Team (Attackers):** The goal is to blend in. A browser visiting a site looks like legitimate traffic. Attackers use crafted requests, realistic User-Agent strings, and slow timing to evade basic detection.
* **Blue Team (Defenders):** The goal is early detection. Defenders monitor their own exposure by analyzing access logs, firewall drops, WAF events, and IDS alerts to catch recon attempts.

> [!danger] Critical Rule
> **Never perform active reconnaissance without explicit, signed legal authorization** (like a penetration testing contract or bug bounty scope). Unauthorized probing is illegal in most jurisdictions.

# Web Browsers for Active Reconnaissance

> [!note] Overview
> Browsers are excellent for active recon because their traffic blends in with normal user activity, making it hard for defenders to differentiate from legitimate browsing.

## Transport-Level Basics
* **Standard Ports:** TCP 80 (HTTP) usually redirects to TCP 443 (HTTPS).
* **HTTP/3 & QUIC:** Modern sites use HTTP/3 over the **QUIC** protocol (combines TCP+TLS over **UDP port 443**). Shows as `h3` in the Network tab.

* **Custom Ports:** Access non-standard ports by appending them to the URL (e.g., `https://target.com:8443/`).

## Developer Tools (`Ctrl+Shift+I` or `Cmd+Opt+I`)

* **Network Tab:** View real-time requests, responses, and headers (e.g., `Server`, `X-Powered-By`).
* **Console Tab:** Execute JavaScript in the page context and interact with the DOM.
* **Sources Tab:** Inspect JS/HTML/CSS files. Highly practical for finding hidden API endpoints, directory structures, and developer comments.
* **Application Tab:** Inspect Cookies, Local Storage, and Session Storage for accidentally exposed API keys or session tokens.
* **Security Tab:** View certificate details. Look at **Subject Alternative Names (SANs)** to discover related subdomains.

## Essential Recon Extensions

* **FoxyProxy:** Quickly switch between web proxies like Burp Suite, ZAP, or SOCKS5 tunnels.
* **User-Agent Switcher:** Emulate different devices (e.g., mobile Safari) to uncover device-specific endpoints. *Warning: Rapid switching can trigger WAFs.*
* **Wappalyzer:** Passively fingerprints the site's tech stack (CMS, servers, frameworks, DBs). *(Alternatives: BuiltWith, WhatRuns)*.

## Evading Detection
Even though standard browsing looks normal, rapid page loads, heavy DevTools interaction, modified headers, or abnormal User-Agents can trigger modern WAFs or EDR systems. Always mimic legitimate human behavior.

# Active Reconnaissance: Ping

> [!note] Concept
> Like a sonar pulse, `ping` sends a small test packet to a remote host and waits for an echo. It is the standard, lightweight first check to see if a target is online and reachable.

## How it Works

* Uses the **ICMP** (Internet Control Message Protocol).
* Sends an **ICMP Echo Request** (Type 8).
* Waits for an **ICMP Echo Reply** (Type 0).

## Command Syntax
* **Linux/macOS:** `ping -c 5 <IP>` (The `-c` flag specifies the packet count).
* **Windows:** `ping -n 5 <IP>`.
* **Force IP Version:** Use `-4` for IPv4 or `-6` for IPv6 (some systems also use `ping6`).

## Interpreting Output & OS Fingerprinting
A successful reply shows round-trip time and 0% packet loss. 

> [!tip] OS Fingerprinting via TTL
> The **TTL (Time To Live)** indicates the max number of router hops a packet can take. Every router it passes through decreases the TTL by 1. The starting TTL hints at the target's Operating System:
> * **Linux:** Typically starts at **64**.
> * **Windows:** Typically starts at **128**.
> * *Example:* A response with TTL `58` likely means it is a Linux machine (started at 64) that is 6 hops away.

## Quick Troubleshooting Reference

| Result                             | Meaning                                                                                         | Next Step                                       |
| :--------------------------------- | :---------------------------------------------------------------------------------------------- | :---------------------------------------------- |
| **Fast replies, 0% loss**          | Target is online and allows ICMP.                                                               | Proceed to port scanning.                       |
| **"Destination Host Unreachable"** | Target is down or no route exists.                                                              | Verify if the machine is powered on.            |
| **100% packet loss (No error)**    | ICMP is filtered/blocked. *(Note: Windows Firewall and cloud providers block ping by default)*. | Try TCP/UDP host discovery (e.g., using Nmap).  |
| **High latency or heavy loss**     | Network congestion or heavy filtering.                                                          | Investigate the network path with `traceroute`. |
# Active Reconnaissance: Traceroute

> [!note] Concept
> The `traceroute` command maps the network path (routers/hops) packets take from your system to a target destination. It is used to understand network topology, locate latency, and identify where filtering occurs.

## How Traceroute Works
You cannot query the full path directly. Instead, traceroute cleverly exploits the **TTL (Time To Live)** field in the IP header.

1. Traceroute sends a packet with a TTL of `1`. 
2. The very first router receives it, decrements the TTL to `0`, drops the packet, and sends back an "ICMP Time-to-Live Exceeded" message (revealing its IP address).
3. Traceroute then sends a packet with a TTL of `2`, which reaches the second router before being dropped.
4. This process incrementally continues until the packet finally reaches the target destination.

## Command Syntax
* **Linux/macOS:** `traceroute <IP>` (Defaults to UDP datagrams).
* **Windows:** `tracert <IP>` (Defaults to ICMP).
* **IPv6:** `traceroute -6 <IP>` or `traceroute6 <IP>`.
* **Bypass UDP Filters (Linux):** * `-T`: Uses TCP-based tracing.
  * `-I`: Uses ICMP-based tracing.

## Interpreting the Output
A standard output shows numbered lines (hops), the router IP/hostname, and three round-trip times (because it sends three packets per TTL by default).

* **Dynamic Routing:** Routes are not fixed. Load balancing, anycast routing (like Cloudflare), and dynamic protocols (BGP/OSPF) mean consecutive runs can take entirely different paths with different hop counts.
* **The Asterisk (`*`):** If a router is configured to suppress ICMP Time-to-Live Exceeded messages (common in secure networks), the output will show an `*` indicating a dropped or unacknowledged packet.

## Additional Techniques
> [!tip] The `mtr` Command
> Use `mtr <IP>` (My Traceroute) for a continuous, real-time view that combines the hop-by-hop mapping of traceroute with the ongoing latency/packet-loss statistics of ping.

# Active Reconnaissance: Telnet & Banner Grabbing

> [!note] The Telnet Protocol
> **Telnet** (TCP Port 23) is a legacy remote administration protocol. It transmits all data in cleartext (including passwords) and has been entirely replaced by SSH for remote management. However, its ability to connect to any TCP port makes it an excellent tool for **banner grabbing**.

## What is Banner Grabbing?

Banner grabbing involves connecting to an open TCP port and reading the server's initial text response (the "banner"). This response frequently reveals the exact software name and version running on that port, which can then be cross-referenced against vulnerability databases (like CVE or Exploit-DB).

## Command Execution Examples

* **Web Servers (Port 80):** Run `telnet <IP> 80`. To coax a response, type `GET / HTTP/1.1`, press Enter, type `host: telnet`, and press Enter twice. Look for the `Server:` string in the output (e.g., `Server: nginx/1.6.2`).
* **FTP Servers (Port 21):** Run `telnet <IP> 21`. FTP servers usually broadcast their banner immediately upon connection without requiring further input.
* **Mail Servers:** Connect to the port, but you must issue protocol-specific commands (SMTP or POP3) to extract further information.

## Modern Limitations & Alternatives

> [!warning] Encryption Blocker
> Telnet cannot handle encrypted connections. It will fail against modern TLS-wrapped services like HTTPS (Port 443) or SMTPS (Port 465).

When dealing with encrypted services, use these modern alternatives instead:
* **HTTPS:** `curl --head https://<IP>`
* **General TLS Services:** `openssl s_client -connect <IP>:<PORT>`
* **Encrypted Netcat:** `ncat --ssl <IP> <PORT>`
# Active Reconnaissance: Netcat (nc)

> [!note] Concept
> **Netcat (`nc`)** is a highly versatile networking utility that supports both TCP and UDP. It can act as a **client** to connect to ports (like Telnet) or as a **server** to listen on ports. Modern iterations like Nmap's `ncat` also support IPv6 and SSL encryption.
## 1. Banner Grabbing (Client Mode)
Netcat works identically to Telnet for banner grabbing but is generally preferred due to its flexibility.

* **Syntax:** `nc <IP> <PORT>`
* **Web Servers (Port 80):** Connect using `nc <IP> 80`, then issue `GET / HTTP/1.1` followed by `host: netcat` and press Enter twice.
* **Other Services:** Services like FTP (Port 21) or SMTP (Port 25) will often present their software banner immediately upon connection or after simple protocol-specific commands.

## 2. Listening (Server Mode)
Netcat can bind to a port and listen for incoming connections. This is highly useful for testing connectivity, simple file transfers, or establishing reverse shells.

* **Syntax:** `nc -vnlp <PORT>` *(Note: The `-p` flag must directly precede the port number).*

| Flag | Function |
| :--- | :--- |
| **`-l`** | Listen mode (acts as a server). |
| **`-p`** | Specifies the port number to listen on. |
| **`-n`** | Numeric only; skips DNS resolution (speeds up connection and avoids warnings). |
| **`-v` / `-vv`** | Verbose / Very Verbose output (crucial for debugging connections). |
| **`-k`** | Keeps the listener open even after the client disconnects. |

> [!warning] Constraints & Modern Alternatives
> * **Root Privileges:** You must use `sudo` to listen on any port below `1024`.
> * **IPv6:** Use the `-6` flag (e.g., `nc -6 -l -p 1234`).
> * **Encryption:** Standard `nc` sends data in cleartext. For transferring sensitive data or encrypted shells, use `ncat --ssl` instead.

