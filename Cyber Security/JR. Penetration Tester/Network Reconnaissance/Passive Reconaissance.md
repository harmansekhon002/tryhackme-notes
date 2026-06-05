
> [!quote] Strategy
> "If you know the enemy and know yourself, your victory will not stand in doubt." — Sun Tzu

> [!note] Definition
> **Reconnaissance** is the preliminary information-gathering phase. It is the first step in frameworks like the Cyber Kill Chain, used by attackers to find weaknesses and by defenders to understand their own public exposure.

## Active vs. Passive Reconnaissance

| Feature | Passive Reconnaissance | Active Reconnaissance |
| :--- | :--- | :--- |
| **Interaction** | None. Relies strictly on publicly available info. | Direct engagement with target systems or personnel. |
| **Analogy** | Observing from a safe distance with binoculars. | Walking up to a building and testing the door locks. |
| **Examples** | DNS records, CT logs (`crt.sh`), job postings, Shodan/Censys, public GitHub repos. | Ping/ARP sweeps, Nmap port scanning, API fuzzing, phishing, in-person tailgating. |

> [!warning] Active Recon is Noisy
> Because active recon involves sending packets or talking to employees, it leaves a footprint (logs, IDS/IPS alerts). **Never perform active reconnaissance without explicit legal authorization.** ## Defensive Countermeasures

> [!tip] Monitor Your Footprint
> Defenders should proactively monitor their own passive footprint. Use OSINT tools, Shodan/Censys, and CT log watchers to minimize what attackers can discover before they ever touch your actual network.

# Domain Registration Recon: WHOIS & RDAP

> [!note] Protocol Evolution
> **WHOIS** (TCP port 43) is a legacy protocol used to query domain registration details. As of Jan 2025, ICANN officially sunsetted WHOIS for gTLDs in favor of **RDAP** (Registration Data Access Protocol), a modern, secure, and structured successor.

## Key Intelligence Gathered

Due to privacy laws (GDPR, CCPA), personal registrant details are almost always redacted. Instead, focus on the following high-value data points:

* **Dates (Creation, Update, Expiry):** Useful for estimating infrastructure age or timing renewal-themed phishing attacks.
* **Registrar:** Identifies the domain provider (e.g., Namecheap), useful for crafting realistic social engineering lures.
* **Name Servers:** Reveals authoritative DNS servers, exposing potential new targets if in scope.
* **Status Codes:** e.g., `clientTransferProhibited` indicates the domain is safely locked against unauthorized transfers.

## WHOIS vs. RDAP

| Feature | Legacy WHOIS | Modern RDAP |
| :--- | :--- | :--- |
| **Security** | Unencrypted (TCP 43) | Secure (HTTPS) |
| **Output Format** | Unstructured plain text | Structured, machine-readable **JSON** |
| **Capabilities** | Basic text response | Internationalization & granular privacy controls |

## Command-Line Execution

### 1. Legacy WHOIS Query
The built-in CLI tool is faster than web viewers and will often auto-redirect to the appropriate registrar server.
```bash
whois tryhackme.com
```

### 2. Modern RDAP Query
Use `curl` to query a public RDAP endpoint (like Verisign for `.com` domains) and pipe it through `jq` to format the JSON readability.
```bash
curl -s [https://rdap.verisign.com/com/v1/domain/tryhackme.com](https://rdap.verisign.com/com/v1/domain/tryhackme.com) | jq .
```

> [!tip] Historical Reconnaissance
> If current records are heavily redacted, use services like **Whoxy.com** to view historical WHOIS snapshots. This can expose past owners, old infrastructure, or previous name servers before privacy protections were enabled.

# Passive Reconnaissance: DNS Querying

> [!note] Core Concept
> Querying DNS records is a fully **passive** reconnaissance technique because the queries are sent to public or open resolvers, meaning you never interact directly with the target's actual servers.

## DNS Query Tools: `dig` vs. `nslookup`

Both tools translate domain names and retrieve DNS records, but `dig` is the modern standard for professionals.

| Feature | `dig` (Domain Information Groper) | `nslookup` (Name Server Lookup) |
| :--- | :--- | :--- |
| **Status** | Modern, Preferred. | Older (kept mostly for Windows compatibility). |
| **Output** | Cleaner, highly detailed, scripting-friendly. | Basic, interactive mode available. |
| **Caching Info** | Displays Time-To-Live (TTL) values by default. | Does not natively display TTL. |
## Common DNS Record Types

| Record Type | Purpose |
| :--- | :--- |
| **A** | Resolves a domain to an **IPv4** address. |
| **AAAA** | Resolves a domain to an **IPv6** address. |
| **CNAME** | Canonical Name (an alias pointing to another domain). |
| **MX** | Mail Exchange (servers that handle the domain's email). *Lower number = higher priority.* |
| **SOA** | Start of Authority (primary name server, admin email, zone serial number). |
| **TXT** | Arbitrary text (commonly used for SPF, DKIM, DMARC, and domain verification). |
## Command Examples

### Using `nslookup`
Specify the record type and an optional DNS resolver (e.g., `1.1.1.1`).
```bash
nslookup -type=A tryhackme.com 1.1.1.1
nslookup -type=MX tryhackme.com
```

### Using `dig` (Recommended)
Query a domain using a specific resolver (`@SERVER`) and record type.
```bash
dig @1.1.1.1 tryhackme.com MX
```

> [!tip] Security Tips
> * **Privacy (Attacker):** Use public resolvers like Cloudflare (`1.1.1.1`) to prevent your local ISP from logging your reconnaissance queries.
> * **Monitoring (Defender):** Actively monitor your DNS for unexpected changes (e.g., rogue TXT entries or new MX records), as these are strong indicators of misconfigurations or subdomain takeovers.

# Passive Subdomain Discovery

> [!note] Why Subdomains Matter
> Standard DNS queries (`dig`/`nslookup`) only resolve known names. Discovering unadvertised subdomains reveals hidden attack surfaces like forgotten services, shadow IT, development panels, and exposed APIs.

## Key Passive Discovery Tools

Passive discovery uses OSINT sources without sending any queries directly to the target.

### 1. DNSDumpster
An OSINT tool that aggregates public DNS data (search engine caches, zone transfers, certificate records) without brute-forcing, keeping the reconnaissance fully passive.
* **Features:** Discovers subdomains, resolved IPs with geolocation, standard DNS records (MX, TXT, CNAME), and generates visual relationship maps.


### 2. Certificate Transparency (CT) Logs
> [!important] The Most Effective Passive Method
> CT logs mandate the public recording of every issued SSL/TLS certificate. The **Subject Alternative Name (SAN)** field within these certificates explicitly lists the domains and subdomains they cover.

* **How to use (`crt.sh`):** Visit `crt.sh` and search using a wildcard: `%.domain.com`. 
* **Benefits:** Operates in real-time, is fully passive, and often reveals vastly more subdomains than standard OSINT aggregators.

### 3. Additional Tools
* **SecurityTrails:** Web-based platform for historical DNS and passive OSINT (limited free searches).
* **Subfinder:** A powerful command-line tool that automatically aggregates data from multiple passive sources.

## Defensive Countermeasures

> [!tip] Defender Strategy
> Security teams should actively monitor CT logs and internal subdomain lists to identify unauthorized infrastructure or **dangling DNS records**, which carry a high risk of **subdomain takeovers**.
# Shodan for Passive Reconnaissance

> [!note] Definition
> **Shodan.io** is a search engine for internet-connected devices (servers, routers, IoT, ICS). Unlike Google, which indexes web pages, Shodan scans the public internet to collect and index banners and responses from open ports.

## Intelligence Gathered per Host
* **Network Info:** IP address and ASN (Autonomous System Number).
* **Infrastructure:** Hosting provider or organization (e.g., AWS, Cloudflare).
* **Location:** Approximate geographic country and city.
* **Services:** Open ports, service types, and software version banners.
* **Tags:** Automated labels identifying features like `cdn` or known vulnerabilities (`vuln`).

## Useful Search Filters
| Filter | Purpose | Example |
| :--- | :--- | :--- |
| `hostname:` | Matches a specific domain or hostname | `hostname:tryhackme.com` |
| `org:` | Filters results by organization name | `org:"TryHackMe"` |
| `port:` / `country:` | Narrows down by open port and geographic location | `port:443 country:US` |
| `http.component:` | Identifies specific underlying technologies | `http.component:"wordpress"` |

> [!tip] Defensive Value & Alternatives
> * **Defenders** use Shodan to monitor for rogue servers, forgotten test environment machines, or unintended service exposures.
> * **Censys.io** is a powerful alternative for cross-referencing host and certificate data.

