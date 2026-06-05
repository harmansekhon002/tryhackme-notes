
Inspired by military kill chains, the **Cyber Kill Chain** is a cybersecurity framework introduced by Lockheed Martin in 2011. It is designed to help organizations defend against cyber attacks by understanding the exact sequence of events involved in conducting one. 

> [!tip] The Defensive Goal
> By understanding the mechanics of each stage, an organization has a significantly better chance of breaking the chain and interrupting an attack while it is in progress. If the chain is broken at any point, the attack fails.

## The 7 Stages of an Attack

1. **Reconnaissance:** The attacker gathers information about the target (e.g., discovering email addresses, network infrastructure, or open ports).
2. **Weaponisation:** Using the gathered reconnaissance, the attacker creates a deliverable payload or modifies an existing one tailored specifically to the target system’s vulnerabilities.
3. **Delivery:** The attacker transmits the weaponized payload to the target (e.g., via phishing emails, USB drives, or malicious web links).
4. **Exploitation:** Once triggered by the victim or the system, the payload executes and actively exploits a vulnerability in the target’s operating system, application, or server.
5. **Installation:** The successful exploitation allows the attacker to install a backdoor or malware to maintain *persistence* (ongoing, undetected access) in the target’s environment.
6. **Command & Control (C2):** Using the installed backdoor, the attacker establishes a two-way communication channel to remotely control the compromised system.
7. **Actions on Objectives:** Having secured persistent access and remote control, the attacker carries out their final goals, such as data exfiltration, ransomware deployment, or pivoting to exploit other internal systems.

# Reconnaissance in Cybersecurity

> [!note] Definition
> Originating from military terminology, **reconnaissance** in cybersecurity is the information-gathering stage. An attacker collects data regarding a target’s vulnerabilities and weaknesses to discover potential entry points into their systems.

## Types of Reconnaissance

Reconnaissance is broadly categorised into two distinct approaches based on the level of interaction with the target.

| Feature | Passive Reconnaissance | Active Reconnaissance |
| :--- | :--- | :--- |
| **Interaction** | No direct interaction with the target. | Requires direct interaction with the target's systems or personnel. |
| **Stealth** | "Noiseless"; the attacker remains invisible. | "Noisy"; highly likely to leave a footprint or trigger alerts. |
| **Primary Sources** | **Open-Source Intelligence (OSINT)**, public records. | Probing tools, direct communication, physical presence. |

### 1. Passive Reconnaissance
Passive techniques rely on publicly available data to build a profile of the target without alerting them.

* **WHOIS Databases:** Can reveal contact information, registration dates, and domain owners (unless protected by privacy service add-ons).
* **DNS Databases:** Querying reveals DNS servers and the IP addresses of public servers.
* **Website Crawling & Scraping:** Automatically mapping out and extracting data from a target's public website.
* **Social Media Reconnaissance:** Gathering intelligence from personal or corporate social media profiles.
* **Google Dorking:** Using advanced search engine operators to reveal sensitive information, misconfigurations, and confidential files.

### 2. Active Reconnaissance
Active techniques are intrusive and involve directly probing the target, which often generates logs.

> [!warning] Detection Risk
> Because active reconnaissance requires direct interaction, it is much easier for defensive security teams to detect through traffic and log monitoring.

* **Network Port Scanning:** Used to identify live hosts and discover which services are running on them.
* **Vulnerability Scanning:** Probing the target’s public-facing services to identify known weaknesses and exploit opportunities.
* **Social Engineering:** Directly interacting with and manipulating the target’s personnel to extract confidential information.
* **Physical Reconnaissance:** Visiting the target’s physical premises to identify entry points, study physical security measures, and observe employee behaviour.

## Countermeasures Against Reconnaissance

> [!important] Defensive Strategy
> The core defence against reconnaissance relies on **minimising public footprint** and **active environment monitoring**.

* **Information Restriction:** Limit the amount of data exposed on public websites, social media accounts, and DNS records.
* **WHOIS Privacy:** Enable privacy services provided by domain registrars to hide administrative names and addresses.
* **Active Traffic Monitoring:** Continuously monitor network traffic to detect anomalies such as network port scans.
* **Log Analysis:** Routinely review and analyse service logs to identify, trace, and respond to active reconnaissance attempts.
# Weaponisation

> [!note] Definition
> **Weaponisation** is the stage following reconnaissance where an attacker creates a tailored payload to exploit discovered vulnerabilities. The end goal is a deliverable malicious file (a "cyber weapon").

## Key Techniques & Tools

Attackers use various methods to build and hide their payloads:

* **Exploit Sourcing:** Utilizing ready-made code, editing existing exploits, or building from scratch.
* **Exploit Kits:** Automated platforms containing multiple exploits, making it easy to package malicious code into executables or documents.
* **Evasion:** Using **obfuscation** or **encryption** to bypass security scanners.
* **Concealment:** Embedding payloads inside innocuous-looking files like PDFs or MS Office documents.
* **Malicious Macros:** Leveraging MS Office features to execute a saved set of instructions as soon as the document is opened.

> [!important] The Output
> By the end of this phase, the payload is prepped for the next stage: delivery (via phishing emails, USB drives, or hosted web pages).

## Countermeasures

> [!tip] Defence Strategy
> A strong defence relies on a combination of continuous human education and strict technical limitations.

| Category                     | Defensive Actions                                                                                                                                   |
| :--------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| **User Training**            | Teach personnel to verify email legitimacy, inspect sources, and handle suspicious attachments (e.g., password-protected zip files).                |
| **Attack Surface Reduction** | Uninstall unnecessary software, remove unneeded browser plugins, and disable unused features.                                                       |
| **Policy Enforcement**       | Use tools like Windows Group Policy to restrict risky features, such as disabling MS Office macros or limiting them only to trusted/signed sources. |
# Delivery

> [!note] Definition
> **Delivery** is the process of transmitting the weaponised payload to the target environment, selecting a method based on intelligence gathered during the reconnaissance phase.

## Common Delivery Methods

Attackers continuously innovate ways to trick users or systems into accepting their exploits:

* **Phishing & Spear Phishing:** Emails containing malicious attachments (e.g., deceptive filenames like `invoice.pdf.exe`) or links. *Spear phishing* specifically targets individuals by spoofing trusted senders.
* **Malicious Links:** Hosting exploit kits on websites, often using domain spoofing or URL shortening to evade suspicion.
* **File-Sharing Platforms:** Uploading malware to trusted cloud services to exploit user familiarity.
* **Malvertising:** Placing malicious advertisements on legitimate websites to redirect traffic.
* **Smishing (SMS Phishing):** Text messages containing malicious links or download instructions.
* **Social Engineering:** Manipulating unsuspecting users into manually downloading and executing malware.
* **Physical Delivery:** Dropping infected USB drives or mailing DVDs with a convincing pretext (e.g., a catalogue) to the target's physical location.

## Countermeasures

> [!important] Defence Strategy
> Breaking the attack chain at this stage requires a combination of continuous human awareness and automated perimeter defences.

| Defence Mechanism | Purpose |
| :--- | :--- |
| **User Training** | Educating personnel on safe browsing, identifying phishing, and resisting social engineering. |
| **Email & Web Filtering** | Automatically screening and blocking suspicious links and malicious attachments. |
| **Web Application Firewalls (WAF)** | Indispensable for inspecting and blocking malicious traffic and files at the web application layer. |
| **Network Monitoring & Patching** | Essential for detecting active delivery attempts and securing the environment against known exploits. |
# Exploitation

> [!note] Definition
> **Exploitation** is the phase immediately following delivery. The attacker leverages a targeted vulnerability—such as a software flaw, weak password, or system misconfiguration—to execute code or gain unauthorized access.

## Common Exploitation Vectors

* **Authentication Targeting:** Exploiting default or weak passwords, or obtaining credentials via phishing.
* **Software Vulnerabilities:** Capitalising on unpatched flaws in client systems or network services.


	* > [!warning] Zero-Day Exploit
	  > An exploit targeting a software vulnerability that is unknown to the vendor, meaning no patch currently exists.

* **Technical Attacks:** Utilizing methods like **SQL injection** or **buffer overflows** to bypass authentication entirely and force the system into unintended behavior.

## Countermeasures

> [!tip] Defence Strategy
> Stopping exploitation requires robust access controls and proactive vulnerability management.

| Defence Mechanism                      | Purpose                                                                          |
| :------------------------------------- | :------------------------------------------------------------------------------- |
| **MFA & Strict Password Policies**     | Renders stolen credentials useless and prevents brute-force or guessing attacks. |
| **Patch Management & Scanning**        | Routinely identifies and eradicates known software vulnerabilities.              |
| **Intrusion Prevention Systems (IPS)** | Inspects network traffic to actively block known exploitation signatures.        |
| **Web Application Firewalls (WAF)**    | Filters web traffic to block application-layer attacks (e.g., SQLi, XSS, CSRF).  |
# Installation

> [!note] Core Concept: Persistence
> Following exploitation, the **Installation** phase ensures persistent access to the system. This allows the attacker to return later without repeating the initial exploitation.

## Techniques for Persistence

Attackers establish a foothold using various methods:

* **OS Mechanisms:** Creating scheduled tasks or services (Windows), and cron jobs or daemons (Linux).
* **Malicious Tooling:** Deploying backdoors, rootkits, or other malware.
* **LOLBins:** Abusing "Living-off-the-land" binaries (legitimate, built-in OS tools).
* **Web Shells:** Small scripts deployed on web servers to execute OS commands remotely, often camouflaged within standard HTTPS traffic.

## Countermeasures

> [!tip] Defence Strategy
> Defending against installation requires strict execution controls and continuous monitoring for unauthorized system changes.

| Defence Mechanism            | Purpose                                                                                                                          |
| :--------------------------- | :------------------------------------------------------------------------------------------------------------------------------- |
| **EDR & Process Monitoring** | Detects suspicious process creation, sensitive file modifications, and unexpected network connections.                           |
| **System Audits**            | Compares the system against a secure baseline to identify and revert unauthorized changes (e.g., new user accounts or services). |
| **Application Allowlisting** | Blocks unauthorized software by restricting execution strictly to approved applications.                                         |
# Command and Control (C2)

> [!note] Core Concept: Covert Communication
> Following the installation of persistent access, **Command and Control (C2)** establishes a discreet communication channel between the compromised system and the attacker's infrastructure.

## C2 Tactics & Infrastructure

Attackers use various methods to blend in and evade detection:

* **Standard Protocols:** Camouflaging traffic using common application-layer protocols (**HTTP, HTTPS, DNS, SMTP**).
* **DNS Tunnelling:** Encoding C2 instructions within standard DNS requests to bypass firewalls.
* **Legitimate Services:** Leveraging social media platforms or cloud services (e.g., Dropbox, Google Docs) to issue commands and exfiltrate data without raising suspicion.

### Resilience Techniques

To prevent their infrastructure from being easily blocked or taken down, attackers use:

| Technique | Mechanism |
| :--- | :--- |
| **DGA (Domain Generation Algorithms)** | Automatically generates thousands of random domains. The attacker registers a tiny fraction (~1%), and malware iterates through the list to find active servers. |
| **Fast Flux** | Associates a single domain name with hundreds of frequently swapping IP addresses (often compromised IoT devices acting as proxies) to evade IP-based blocking. |

## Countermeasures

> [!tip] Defence Strategy
> Disrupting C2 requires deep traffic analysis, decryption capabilities, and anomaly detection.

| Defence Mechanism               | Purpose                                                                                                                                     |
| :------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------ |
| **Network Monitoring**          | Firewalls, IDS, and IPS watch for unusual traffic volumes, patterns, and connections to known malicious IPs.                                |
| **DNS Analysis**                | Identifies DNS tunnelling by detecting unusually long queries or high-volume requests to suspicious or unknown domains.                     |
| **Web & Encryption Inspection** | Monitors web traffic for suspicious connections, filters malicious URLs, and decrypts HTTPS traffic to inspect for hidden C2 communication. |
| **Honeypots**                   | Decoy systems deployed to safely attract, detect, and analyze attacker C2 behavior.                                                         |
# Actions on Objectives

> [!note] The Final Phase
> After establishing a C2 channel, the attacker executes their primary goals. This phase represents the actual realization of the threat actor's motives, whether that involves theft, disruption, or destruction.

## Attacker Goals & Techniques

Depending on the threat actor's motives, their actions will vary significantly:

* **Data Exfiltration:** Stealing sensitive information for industrial or political espionage.
* **Financial Gain:** Deploying ransomware or executing fraudulent wire transfers.
* **Destructive Attacks:** Deleting or corrupting data to halt system operations.
* **Lateral Movement:** Stealthily compromising additional systems within the network to expand access.
* **ICS Manipulation:** Targeting and altering Industrial Control Systems.
* **Long-Term Persistence:** Maintaining quiet, ongoing access to strike at a later time.

## Countermeasures

> [!important] Defence Strategy
> Minimising impact at this late stage requires isolating systems, strictly limiting access, and maintaining robust recovery protocols.

| Defence Mechanism                     | Purpose                                                                                                                     |
| :------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------- |
| **Data Loss Prevention (DLP)**        | Detects and blocks the unauthorized extraction of sensitive data.                                                           |
| **Reliable Backups**                  | Essential for full system recovery following ransomware or destructive attacks.                                             |
| **Network Segmentation**              | Isolates critical systems to prevent attackers from moving laterally across the network.                                    |
| **Access Controls (Least Privilege)** | Limits access so compromised accounts cannot reach highly sensitive data.                                                   |
| **User Activity Monitoring & EDR**    | Detects behavioral anomalies (e.g., midnight queries) and suspicious endpoint actions (e.g., unauthorized file encryption). |
