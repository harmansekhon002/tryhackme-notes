# Intrusion Detection System (IDS)

An **IDS** is an internal security solution that detects malicious activity that has already bypassed the network perimeter.

## Firewall vs. IDS
* **Firewall (Gatekeeper):** Acts at the boundary to block unauthorised access based on rules.
* **IDS (Surveillance):** Acts inside the network to monitor traffic patterns and detect "bad actors" who snuck past the gate.

## Key Functions
* **Passive Monitoring:** Sits "out-of-band" and observes traffic without blocking it.
* **Detection Methods:**
    * **Signature-based:** Matches traffic against a database of known threats.
    * **Anomaly-based:** Flags deviations from established "normal" behavior.
* **Alerting:** Only notifies administrators when a threat is found; it does not take action to stop the attack.

# Intrusion Detection Systems (IDS) Categories

An IDS is primarily categorized by two main factors: **Deployment Mode** (where it sits) and **Detection Mode** (how it identifies threats).

## 1. Deployment Modes
* **HIDS (Host Intrusion Detection System)**
    * **Placement:** Installed directly on individual hosts/devices.
    * **Function:** Monitors activities and potential threats localized to that specific machine.
    * **Pros:** Deep, detailed visibility into host-level activity.
    * **Cons:** Resource-intensive; difficult to scale and manage across large networks.
* **NIDS (Network Intrusion Detection System)**
    * **Placement:** Deployed at strategic points within the network.
    * **Function:** Monitors and analyzes network traffic across all connected hosts.
    * **Pros:** Provides a centralized view of network health; protects multiple hosts simultaneously without individual agents.

## 2. Detection Modes
* **Signature-Based IDS (e.g., Snort)**
    * **Mechanism:** Compares network traffic against a database of known attack patterns ("signatures").
    * **Pros:** Fast, efficient, and requires lower processing overhead. Ideal for smaller threat surfaces.
    * **Cons:** Cannot detect **zero-day** (novel/unknown) attacks since no signature exists yet.
* **Anomaly-Based IDS**
    * **Mechanism:** Learns the network's "normal" baseline behavior and flags any deviations from it.
    * **Pros:** Capable of detecting modern **zero-day** attacks.
    * **Cons:** High false-positive rate (often flags benign, unusual activity as malicious); requires manual fine-tuning to reduce noise.
* **Hybrid IDS**
    * **Mechanism:** Combines both signature and anomaly-based methods.
    * **Pros:** Leverages the speed/efficiency of signatures for known threats, while relying on anomaly detection to catch zero-days. Highly recommended for modern organisational defense
# Intrusion Detection Systems (IDS)

**Deployment Modes**
* **HIDS (Host Intrusion Detection System):** Installed individually on hosts. Provides detailed local visibility but is resource-intensive and hard to manage at scale.
* **NIDS (Network Intrusion Detection System):** Monitors entire network traffic. Provides a centralised view of malicious activities regardless of specific hosts.

**Detection Modes**
* **Signature-Based (e.g., Snort):** Detects threats using a database of known attack patterns. Efficient for known threats but unable to detect zero-day attacks.
* **Anomaly-Based:** Compares current state against a learned normal baseline. Detects zero-day attacks but generates false positives and requires manual fine-tuning.
* **Hybrid IDS:** Combines both approaches. Uses signatures for known threats for quick detection and anomaly methods for zero-day attacks.

# Snort Configuration & Rules

## 1. Key Directories & Files
* **Main Directory:** `/etc/snort/`
* **Configuration File:** `/etc/snort/snort.conf` (Defines network ranges, enabled rules, and core settings)
* **Custom Rules File:** `/etc/snort/rules/local.rules`

## 2. Snort Rule Syntax
**Format:** `[Action] [Protocol] [Source IP] [Source Port] -> [Dest IP] [Dest Port] ([Metadata])`

**Example:** `alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)`

* **Rule Header Components:**
    * **Action:** What to do when triggered (e.g., `alert`).
    * **Protocol:** Traffic type (e.g., `icmp`).
    * **Source/Dest IPs:** Origin and target IPs (e.g., `any`, `$HOME_NET`, `127.0.0.1`).
    * **Source/Dest Ports:** Origin and target ports (e.g., `any`).
    * **Direction:** `->` indicates the flow of traffic.
* **Rule Metadata Components (in parentheses):**
    * **msg:** The alert message displayed when triggered (`msg:"Loopback Ping Detected";`).
    * **sid:** Signature ID, uniquely identifies the rule (`sid:10003;`).
    * **rev:** Revision number, tracks updates to the rule (`rev:1;`).

## 3. Essential Commands

**Edit Custom Rules:**
```bash
sudo nano /etc/snort/rules/local.rules

sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf

sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf