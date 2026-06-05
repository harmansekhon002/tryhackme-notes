Each process on a device logs an event and some of these processes might be harmful while others may not be 

## Types of alerts

1. False positives
2. True positives

## Types of Security Incidents

While often generalised as "hacking," security incidents fall into specific categories based on the nature of the attack. These incidents can occur independently or simultaneously during a complex attack.

## Common Incident Categories

* **[[Malware]] Infections:** * Malicious programs designed to damage a system, network, or application. 
  * Typically delivered via files, documents, or executables.
* **Security Breaches:** * Occurs when an unauthorised entity successfully gains access to confidential, protected data.
* **Data Leaks:** * The exposure of confidential information to unauthorised entities (often used for extortion or reputational damage).
  * *Key Distinction:* Unlike targeted breaches, leaks can also happen unintentionally due to human error or misconfigurations.
* **[[Insider Threats|Insider Attacks]]:** * Malicious actions initiated by someone *within* the organization (e.g., a disgruntled employee). 
  * Extremely hazardous because insiders already possess authorized access and internal knowledge.
* **[[Denial of Service]] (DoS) Attacks:** * An attacker floods a system or network with false requests, exhausting its resources.
  * *Goal:* To violate the "Availability" pillar of cybersecurity by making the service unavailable to legitimate users.
## Assessing Severity and Impact

The severity of a specific incident cannot be universally measured; it is highly dependent on the organization's business model and infrastructure. 

> **Example:** A company that only hosts public information might suffer minimal impact from a data leak. However, if their entire revenue relies on an e-commerce platform, a **DoS attack** on that website would be financially disastrous.

# [[Incident Response]] Frameworks

Handling diverse security incidents requires a structured, generic approach. Two of the most widely adopted frameworks for guiding this process are from **[[SANS]]** and **[[NIST]]**.

## 1. The SANS Framework (PICERL)

The SANS framework breaks the incident response lifecycle down into 6 distinct phases, easily remembered by the acronym **PICERL**.

| Phase | Objective | Example |
| :---: | :--- | :--- |
| **1. Preparation** | Build necessary resources, teams, and plans before an incident occurs. | Conducting employee phishing awareness training. |
| **2. Identification** | Monitor and detect abnormal behavior indicating an incident. | Detecting massive data exfiltration from a host. |
| **3. Containment** | Minimize the attack's impact and stop its spread. | Isolating the compromised host from the network. |
| **4. Eradication** | Completely remove the threat from the environment. | Executing deep malware scans to remove malicious files. |
| **5. Recovery** | Restore affected systems from backups and verify functionality. | Reconfiguring the host and restoring exfiltrated data. |
| **6. Lessons Learned** | Identify gaps in the response to improve future processes. | Conducting a post-incident review to analyze the root cause. |

## 2. The NIST Framework

The [[NIST]] framework is highly similar to SANS but condenses the process into 4 phases. 

* **Preparation** *(Same as SANS)*
* **Detection & Analysis** *(Maps to SANS: Identification)*
* **Containment, Eradication, & Recovery** *(Groups the 3 SANS phases into one continuous operational cycle)*
* **Post-Incident Activity** *(Maps to SANS: Lessons Learned)*

## The Incident Response Plan (IRP)

Organizations use these frameworks to build their formal **Incident Response Plan**. 
* **Definition:** A structured document, formally approved by senior management, outlining the exact procedures to follow before, during, and after an incident.
* **Key Components:**
  * Roles and Responsibilities of the IR team.
  * The chosen Incident Response methodology (e.g., NIST or SANS).
  * A communication plan for stakeholders (including law enforcement).
  * Escalation paths for reporting and handling critical threats.
# Incident Response Techniques

Manually identifying abnormal behaviour during the Identification ([[SANS]]) or Detection and Analysis ([[NIST]]) phases is difficult. Security solutions are used to detect incidents, and some can even execute containment and eradication.

## Security Solutions


* **[[SIEM]] (Security Information and Event Management):** Collects all important logs in a centralised location and correlates them to identify incidents.
* **[[AV]] (Antivirus):** Regularly scans the system to detect known malicious programs.
* **[[EDR]] (Endpoint Detection and Response):** Deployed on every system to protect against advanced-level threats. It also has the capability to contain and eradicate threats.

---

## [[Playbooks]] vs. [[Runbooks]]

After an incident is identified, specific procedures are required to investigate, prevent damage, and eliminate the root cause. 

### Playbooks
Guidelines that provide step-by-step instructions for a comprehensive response to specific kinds of incidents, saving significant time.


**Example: Phishing Email Playbook**
1. Notify all stakeholders of the incident.
2. Conduct header and body analysis to determine if the email is malicious.
3. Look for and analyze any attachments.
4. Determine if anybody opened the attachments.
5. Isolate the infected systems from the network.
6. Block the email sender.

### Runbooks
The detailed, step-by-step execution of specific steps during different incidents. These execution steps may vary depending on the resources available for the investigation.