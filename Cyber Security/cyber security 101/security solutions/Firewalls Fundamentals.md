A firewall acts as a "security guard" for digital devices or networks, inspecting the continuous flow of incoming and outgoing internet traffic.

## Core Functions

* **Primary Goal:** To prevent any unauthorised visitors (data/traffic) from entering a system or a network.
* **Rule-Based Filtering:** You instruct the firewall using a defined set of rules. All incoming and outgoing traffic must face the firewall first, which will then **allow** or **deny** that traffic strictly based on those rules.
* **Modern Capabilities:** Most contemporary firewalls have evolved beyond simple rule-based filtering, offering extra functionalities to further protect the device or network from the outside world.

# [[Types of Firewalls]]

Firewalls filter harmful traffic and operate on different layers of the OSI model to protect networks.

## 1. Stateless Firewall
* **OSI Layer:** Operates on Layer 3 and Layer 4.
* **Function:** Filters data *solely* based on predetermined rules (basic filtering).
* **Characteristics:**
  * Does not track previous connections; treats every packet as new.
  * Cannot apply complex policies based on connection history.
  * Processes packets very quickly, making it highly efficient for high-speed networks.

## 2. Stateful Firewall

* **OSI Layer:** Operates on Layer 3 and Layer 4.
* **Function:** Goes beyond predetermined rules by keeping track of previous connections in a "state table."
* **Characteristics:**
  * Adds security by inspecting packets based on their connection history.
  * Automatically allows or denies subsequent packets based on the established state.
  * Recognizes traffic by patterns and allows for complex rules.

## 3. Proxy Firewall (Application-Level Gateway)
* **OSI Layer:** Operates on Layer 7.
* **Function:** Acts as an intermediary between the private network and the internet.
* **Characteristics:**
  * Masks internal IP addresses to provide anonymity.
  * Inspects the actual *contents* of the data packets.
  * Provides content filtering options and application control.
  * Decrypts and inspects SSL/TLS data packets.

## 4. Next-Generation Firewall ([[NGFW]])
* **OSI Layer:** Operates from Layer 3 to Layer 7.
* **Function:** The most advanced type, offering deep packet inspection and advanced threat protection.
* **Characteristics:**
  * Includes an Intrusion Prevention System (IPS) to block real-time malicious activities.
  * Uses heuristic analysis to identify anomalies and block attack patterns before they reach the network.
  * Decrypts and inspects SSL/TLS data packets.
  * Correlates data with threat intelligence feeds to make efficient decisions.
# [[Firewall Rules]]

Firewalls give you control over network traffic by filtering data based on built-in or customized rules.

## Basic Rule Components
* **Source address:** The IP address originating the traffic.
* **Destination address:** The IP address receiving the data.
* **Port:** The port number used for the traffic.
* **Protocol:** The communication protocol being used.
* **Action:** The step taken upon identifying matching traffic.
* **Direction:** The rule's applicability to incoming or outgoing traffic.

## Types of Actions
The "Action" component dictates what happens to a matched data packet. 

### 1. Allow
Permits the defined traffic. 
*Example: Allowing all outgoing HTTP traffic to the Internet.*

| Action | Source | Destination | Protocol | Port | Direction |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Allow** | 192.168.1.0/24 | Any | TCP | 80 | Outbound |

### 2. Deny
Blocks the defined traffic, reducing the network's threat surface.
*Example: Denying incoming SSH connections to a critical server.*

| Action | Source | Destination | Protocol | Port | Direction |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Deny** | Any | 192.168.1.0/24 | TCP | 22 | Inbound |

### 3. Forward
Redirects traffic to a different network segment (used by firewalls acting as gateways).
*Example: Forwarding incoming HTTP traffic to an internal web server.*

| Action | Source | Destination | Protocol | Port | Direction |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Forward** | Any | 192.168.1.8 | TCP | 80 | Inbound |

## Directionality of Rules
Rules are categorized based on the flow of traffic they apply to:
* **Inbound Rules:** Applied *only* to incoming traffic (e.g., allowing inbound HTTP to a web server).
* **Outbound Rules:** Applied *only* to outgoing traffic (e.g., blocking outbound SMTP from non-mail servers).
* **Forward Rules:** Created to route specific traffic to an internal destination network segment.

# [[Windows Defender Firewall]]

Windows Defender is Microsoft's built-in firewall for the Windows operating system, providing essential tools to manage and restrict incoming and outgoing network traffic.

## Network Profiles
Windows uses Network Location Awareness (NLA) to determine your current network and apply the appropriate security profile. You can configure different rules for each profile:
* **Private networks:** Applied when connected to trusted networks, like your home network.
* **Guest or public networks:** Applied when connected to untrusted networks, like coffee shops. *Security Tip: Configure this profile to block all incoming connections while allowing only essential outgoing connections.*

## Main Dashboard Options
From the main firewall dashboard, you can quickly configure basic settings:
* **Allow an app or feature:** Selectively check or uncheck installed applications to permit or deny them network access across your profiles.
* **Turn Windows Defender Firewall on or off:** Modify the firewall state for each profile. Instead of turning it off (which is not recommended), you can choose to block all incoming connections.
* **Restore Defaults:** Reverts all firewall settings back to their original state.

## Creating Custom Rules (Advanced Settings)
For granular control, you can create custom inbound and outbound rules using the **Advanced Settings** panel. 

### Example: Blocking Outbound Web Traffic (HTTP/HTTPS)
If you want to prevent the system from browsing the internet, you can create an outbound rule blocking ports 80 and 443. 

**Steps to create the rule:**
1. **Navigate:** Open **Advanced Settings** -> click **Outbound Rules** -> click **New Rule**.
2. **Rule Type:** Select **Custom** and click Next.
3. **Program:** Select **All programs** and click Next.
4. **Protocols and Ports:** * Protocol type: **TCP**
   * Remote port: **Specific ports**
   * Port numbers: `80,443` *(no spaces between comma-separated ports)*
5. **Scope:** Leave local and remote IP addresses as they are (Any) and click Next.
6. **Action:** Select **Block the connection** and click Next.
7. **Profile:** Keep all network profiles checked (Domain, Private, Public) and click Next.
8. **Name:** Assign a name and optional description, then click Finish.

Once applied, the system will immediately block access to any web pages operating on ports 80 or 443.

# [[Netfilter]] and [[UFW]] (Uncomplicated Firewall)

**Netfilter** is the core Linux framework for packet filtering, NAT, and connection tracking. While powerful, its native syntax (like `iptables`) is complex, leading to the creation of easier management utilities.

## Linux Firewall Utilities
* **iptables:** The classic, most widely used utility for Netfilter.
* **nftables:** The modern successor to `iptables` with enhanced filtering.
* **firewalld:** Uses "zones" to manage predefined rule sets.
* **UFW:** A beginner-friendly interface that translates simple commands into `iptables` rules.

---

## [[UFW]] Command Reference

### System Management
* **Check Status:** `sudo ufw status`
* **Enable Firewall:** `sudo ufw enable`
* **Disable Firewall:** `sudo ufw disable`

### Rule Configuration
* **Set Default Policy:** `sudo ufw default allow outgoing`  
  *(Sets the baseline to permit all outbound traffic)*
* **Deny Specific Port/Protocol:** `sudo ufw deny 22/tcp`  
  *(Blocks incoming SSH traffic on TCP port 22)*

### Rule Maintenance
* **List Rules with ID Numbers:** `sudo ufw status numbered`  
  *(Essential for identifying which rule number to delete)*
* **Delete a Rule:** `sudo ufw delete [number]`  
  *(Example: `sudo ufw delete 2` removes the second rule in the list)*

---

## Comparison Table

| Utility       | Complexity | Key Feature                                 |
| :------------ | :--------- | :------------------------------------------ |
| **iptables**  | High       | Granular control; industry standard.        |
| **nftables**  | High       | Faster; successor to iptables.              |
| **firewalld** | Medium     | Dynamic zones; good for laptops/servers.    |
| **UFW**       | Low        | Human-readable commands; beginner-friendly. |