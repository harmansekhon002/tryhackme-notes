
Forensics is the application of methods and procedures to investigate and solve crimes

The branch of forensics that investigates cyber crimes is known as **digital forensics**. **Cyber crime** is any criminal activity conducted on or using a digital device

# [[Digital Forensics]] Fundamentals

The [[NIST]] framework defines a standard 4-phase process for digital investigations:

## 1. The 4 Phases
1. **Collection:** Securely acquire device data without tampering; maintain strict documentation.
2. **Examination:** Filter collected data to extract only relevant artifacts (e.g., specific dates/users).
3. **Analysis:** Correlate the filtered evidence to build a chronological timeline and draw conclusions.
4. **Reporting:** Document findings and methodology, including an executive summary for stakeholders.

## 2. Specialised Domains
* **[[Computer Forensics]]:** Investigating PCs and laptops.
* **Mobile Forensics:** Extracting SMS, call logs, and GPS from smart devices.
* **[[Network Forensics]]:** Analyzing network traffic logs to track threats across an infrastructure.
* **Cloud Forensics:** Investigating cloud-hosted data (challenging due to limited physical access).
* **Database Forensics:** Investigating unauthorized data modification or exfiltration.
* **Email Forensics:** Analyzing communications to trace phishing or fraudulent campaigns.

## Evidence Acquisition

Securely collecting evidence without tampering with the original data is a critical phase in digital forensics. While extraction methods vary by device type, core best practices universally apply.

## Key Acquisition Practices

* **Proper Authorisation:**
  * Prior legal approval must be obtained before data collection.
  * *Why:* Forensic data is highly sensitive. Evidence collected without authorisation violates legal limits and is typically inadmissible in court.
* **[[Chain of Custody]]:**
  * A formal document that tracks the complete lifecycle of evidence to prove its integrity, accountability, and reliability in court.
  
  * **Key details recorded:** 
	1. Evidence description (name, type)
    2. Name of the collector
    3. Date and time of collection
    4. Storage location
    5. Access logs (who accessed it and when)
* **Use of Write Blockers:**
  * Essential tools placed between the suspect's storage device and the forensic workstation.
  
  * *Why:* Prevents the workstation's background tasks from accidentally modifying the original evidence (e.g., altering file timestamps during extraction), ensuring the data remains exactly as it was found.

# [[Windows Forensics]]: Evidence Acquisition

Windows PCs and laptops are the most frequently analysed devices in digital investigations. Investigators secure evidence by creating **forensic images** (exact, bit-by-bit copies of the system).

## 1. Types of Forensic Images



* **Memory Image (Volatile Data):**
  * Captures data actively running in RAM (e.g., open files, running processes, current network connections).
  * **Critical Rule:** Must be acquired *first*. This data is permanently lost if the system is shut down or restarted.
* **Disk Image (Non-Volatile Data):**
  * Captures all data residing on physical storage devices (HDD, SSD).
  * Contains permanent files like documents, media, and browsing history that survive system reboots.

## 2. Common Acquisition & Analysis Tools

### Disk Forensics
* **FTK Imager:** A widely used, GUI-based tool capable of both **acquiring** disk images in various formats and performing basic **analysis**.
* **[[Autopsy]]:** A powerful, open-source platform dedicated to extensive disk **analysis**. Key features include keyword searching, deleted file recovery, and metadata extraction.

### Memory Forensics
* **DumpIt:** A lightweight, command-line interface (CLI) utility designed specifically for **acquiring** memory (RAM) images from Windows systems.
* **[[Volatility]]:** An industry-standard, open-source framework for **analyzing** memory images. It utilizes specialized plugins to extract specific artifacts and supports multiple operating systems (Windows, Linux, macOS, Android).
