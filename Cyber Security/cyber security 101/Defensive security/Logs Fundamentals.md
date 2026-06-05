# [[System Logs]] Overview

Logs are the digital footprints left behind by any activity within a system. They record both normal operations and malicious actions, making them the primary source of truth for tracing an attack and identifying the responsible party.
## Key Use Cases of Logs

* **Security Events Monitoring:** Enables the real-time detection of anomalous or suspicious behaviour.
* **Incident Investigation & [[Digital Forensics]]:** Provides detailed historical records, allowing security teams to perform a precise [[Root Cause Analysis]] of an incident.
* **Troubleshooting:** Records system and application errors, helping IT teams diagnose and fix technical issues.
* **Performance Monitoring:** Offers valuable data and insights into the operational health of applications.
* **Auditing and Compliance:** Establishes a concrete, verifiable trail of activities to ensure regulatory standards and internal policies are being met.

# Types of Logs

Searching through a single, massive log file to investigate an issue is highly inefficient. To solve this, logs are segregated into specific categories based on the type of information they provide, allowing investigators to target relevant files directly.

## Common Log Categories

| Log Type | Primary Usage | Event Examples |
| :--- | :--- | :--- |
| **System Logs** | Troubleshooting OS running issues; tracks OS activities. | Startup/shutdown, driver loading, system errors, hardware events. |
| **Security Logs** | Detecting and investigating security incidents. | Authentication/authorization, policy changes, user account changes, abnormal activity. |
| **Application Logs** | Tracking specific interactive/non-interactive events inside an application. | User interaction, app changes, app updates, app errors. |
| **Audit Logs** | Compliance requirements and security monitoring. | Data access, detailed system changes, user activity, policy enforcement. |
| **Network Logs** | Troubleshooting network issues and incident investigation. | Incoming/outgoing traffic, network connections, network firewall logs. |
| **Access Logs** | Detailing access to specific resources. | Web server, database, application, and API access logs. |

> *Note: Various other log types exist depending on specific applications and the services they provide.*

## [[Log Analysis]]

**Log Analysis** is the technique of extracting valuable data from logs to identify abnormal or unusual activities. 

# [[Windows Logs]] and Event Viewer

Windows OS logs system activities into segregated files based on the log category. 

## Crucial Types of Windows Logs
* **Application:** Logs info related to running applications (errors, warnings, compatibility issues).
* **System:** Logs info related to OS operations (driver/hardware issues, startup/shutdown, services).
* **Security:** The most critical log for security. Records authentication, user account changes, and security policy changes.

## [[Event Viewer]]
Unlike raw log files, Windows OS provides a built-in GUI utility called **Event Viewer** to easily view, search, and filter logs. *(Access by typing 'Event Viewer' in the Start menu).*

### Key Event Log Fields
When analyzing a specific log entry, investigators look at several major fields:
* **Description:** Detailed information about the activity.
* **Log Name:** The specific log file name.
* **Logged:** The timestamp of the activity.
* **Event ID:** A unique numerical identifier for a specific type of activity.

## Important [[Event IDs]]
Event IDs are uniquely mapped to specific activities, making it easy to search for particular actions.

| Event ID | Description |
| :---: | :--- |
| **4624** | A user account successfully logged in. |
| **4625** | A user account failed to login. |
| **4634** | A user account successfully logged off. |
| **4720** | A user account was created. |
| **4722** | A user account was enabled. |
| **4724** | An attempt was made to reset an account’s password. |
| **4725** | A user account was disabled. |
| **4726** | A user account was deleted. |
## Filtering Logs
To quickly find malicious or specific activity, investigators use the **Filter Current Log** option in Event Viewer. 

* **How it works:** You can input specific Event IDs (e.g., `4624`) into the filter prompt to display only logs that match that ID, ignoring the noise of irrelevant events.