# ( Common Analysis Platform for Artifacts )
Developed by FireEye Mandiant to automate the identification of program capabilities.
## Core Features
* **Supported Formats:** Analyses PE, ELF binaries, .NET modules, shell code, and sandbox reports.
* **Behaviour Identification:** Uses predefined rules to detect actions like network communication, file manipulation, and process injection.
* **Automated Reverse Engineering:** Encapsulates expert knowledge, allowing analysts to understand binary functionality quickly without manual code analysis.
## Key Use Cases
* Malware analysis
* Threat hunting
* Incident response
* Defensive measures

# Running and Pre-processing CAPA
CAPA analyzes executables and flags potential malicious behaviors based on predefined rules. 
## Basic Usage
* **Command:** `capa.exe .\cryptbot.bin`
* **Note:** The analysis can take several minutes. To save time during labs, it's common practice to pipe the output to a text file (e.g., `capa.exe .\cryptbot.bin > cryptbot.txt`) and read it later.
* **Reading Pre-processed Output:** `Get-Content .\cryptbot.txt` (in PowerShell).

## Command Line Options
| Option | Description | Syntax Example |
| :--- | :--- | :--- |
| `-h` / `--help` | Shows the help message and available parameters. | `capa -h` |
| `-v` / `--verbose` | Enables verbose output (more details, longer processing time). | `capa.exe .\cryptbot.bin -v` |
| `-vv` / `--vverbose` | Enables very verbose output (maximum detail). | `capa.exe .\cryptbot.bin -vv` |

## Understanding the Standard Output Structure
CAPA organizes its findings into distinct, structured tables:
1.  **File Information:** Hashes (MD5, SHA1, SHA256), OS, architecture, and the type of analysis performed.
2.  **ATT&CK Framework Mapping:** Maps the binary's capabilities directly to MITRE ATT&CK Tactics (e.g., *Defense Evasion*) and Techniques (e.g., *Virtualization/Sandbox Evasion*).
3.  **MAEC Category:** Identifies the broad category of the malware (e.g., *launcher*).
4.  **MBC (Malware Behavior Catalog):** Categorizes behaviors like Anti-Behavioral Analysis (e.g., *Virtual Machine Detection*) or Communication methods.
5.  **Capability Namespace:** A granular breakdown of specific actions the binary can perform, such as:
    * `reference anti-VM strings targeting VMWare`
    * `reference HTTP User-Agent string`
    * `run PowerShell expression`

# CAPA Results Breakdown: Part 1
CAPA organizes its output into distinct blocks to categorize a binary's characteristics, mapped against industry-standard frameworks.

## 1. File Information Block
Contains the foundational metadata and context of the analyzed file.
* **Hashes:** MD5, SHA1, SHA256 (used for unique identification and threat intelligence lookups).
* **Analysis:** The evaluation method used (e.g., static).
* **OS & Arch:** The targeted operating system (e.g., windows) and architecture (e.g., i386).
* **Format:** The file structure (e.g., pe for Portable Executable).
* **Path:** The local file path during analysis.

## 2. MITRE ATT&CK Block
Maps identified capabilities to the MITRE ATT&CK framework, a global repository of adversary behavior. 
* **Standard Format:** `Tactic :: Technique [Technique ID]`
  * *Example:* `Defense Evasion :: Obfuscated Files or Information [T1027]`
* **Sub-Technique Format:** `Tactic :: Technique :: Sub-Technique [TechniqueID.SubID]`
  * *Example:* `Defense Evasion :: Obfuscated Files or Information :: Indicator Removal from Tools [T1027.005]`
* **Purpose:** Allows analysts to rapidly understand how the malware fits into an attacker's overall playbook (e.g., how it evades defenses or establishes persistence).

## 3. MAEC Block (Malware Attribute Enumeration and Characterisation)
A standardised language used to encode and communicate specific malware attributes and functions.

| MAEC Value     | Description & Common Behaviors                                                                                                                         |
| :------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Launcher**   | Triggers specific actions on the host. Behaviors include dropping additional payloads, activating persistence mechanisms, or connecting to C2 servers. |
| **Downloader** | Reaches out to the internet to fetch additional payloads, pull updates, execute secondary stages, or retrieve configuration files.                     |
# Malware Behavior Catalogue (MBC)
MBC is a framework for labeling, similarity analysis, and standardized reporting of malware objectives and behaviors. It complements the MITRE ATT&CK framework by logging behaviors and code features specifically tailored for malware analysis.

## Output Formats in CAPA
CAPA results display MBC data in two primary formats:

1.  **Detailed Format:** `OBJECTIVE :: Behavior :: Method [Identifier]`
    * *Example:* `ANTI-STATIC ANALYSIS :: Executable Code Obfuscation :: Argument Obfuscation [B0032.020]`
2.  **Standard Format:** `OBJECTIVE :: Behavior [Identifier]`
    * *Example:* `COMMUNICATION :: HTTP Communication [C0002]`

## 1. Objectives & Micro-Objectives
These are displayed under the **MBC Objective** column in CAPA.
* **Objectives:** High-level goals based on ATT&CK tactics, but tailored for malware (e.g., *Anti-Behavioral Analysis, Anti-Static Analysis, Defense Evasion, Execution, Discovery*).
* **Micro-Objectives:** Lower-level actions that aren't inherently malicious but are frequently abused by malware.

| Micro-Objective | Description |
| :--- | :--- |
| **PROCESS** | Creating processes, setting thread context, checking mutexes. |
| **MEMORY** | Allocating, freeing, or changing memory protection. |
| **COMMUNICATION** | Initiating network traffic (DNS, FTP, HTTP, ICMP, SMTP). |
| **DATA** | Checking strings, compressing, encoding/decoding data. |

## 2. Behaviors & Micro-Behaviors
These are displayed under the **MBC Behavior** column in CAPA. They represent the specific actions taken to achieve the objective.

| Category | Objective / Micro-Obj | Behavior / Micro-Behavior | ID | Explanation |
| :--- | :--- | :--- | :--- | :--- |
| **Behavior** | ANTI-BEHAVIORAL | Virtual Machine Detection | B0009 | Checks if running in a virtual environment/sandbox. |
| **Behavior** | ANTI-STATIC | Executable Code Obfuscation | B0032 | Code obscured to prevent static analysis. |
| **Behavior** | DISCOVERY | File and Directory Discovery | E1083 | Enumerating files/directories to find specific locations. |
| **Micro** | MEMORY | Allocate Memory | C0007 | Allocating memory to unpack itself or execute payloads. |
| **Micro** | PROCESS | Create Process | C0017 | Spawning child processes, often suspended or via shellcode. |
| **Micro** | FILE SYSTEM | Create / Read / Write / Delete | C0046-52 | Standard file system interactions used for dropping payloads. |

## 3. Methods (Sub-Techniques)
Methods provide specific technical details on *how* a behavior is executed.

| Behavior | Method | ID | Explanation |
| :--- | :--- | :--- | :--- |
| **Executable Code Obfuscation** | Argument Obfuscation | B0032.020 | API arguments calculated at runtime to hide intent. |
| **Executable Code Obfuscation** | Stack Strings | B0032.017 | Strings built/decrypted on the stack at runtime, then discarded. |
| **Encode Data** | Base64 / XOR | C0026.001/2 | Standard algorithms used to encode malicious data. |
| **HTTP Communication** | Read Header | C0002.014 | Reading HTTP headers during C2 communication. |
# CAPA: Capability and Namespace
CAPA organizes its findings by grouping specific malware behaviors (Capabilities/Rules) into broader categories called Namespaces. 



## Output Structure
* **Format:** `Capability (Rule Name) :: Top-Level Namespace (TLN) / Namespace`
* **Example:** `reference anti-VM strings :: anti-analysis / anti-vm / vm-detection`
  * **Capability:** `reference anti-VM strings`
  * **TLN:** `anti-analysis`
  * **Namespace:** `anti-vm / vm-detection`

## Top-Level Namespaces (TLN) Reference
TLNs represent the highest level of categorization for a binary's capabilities.

| Top-Level Namespace   | Description                                                                                    |
| --------------------- | ---------------------------------------------------------------------------------------------- |
| **anti-analysis**     | Behaviors to evade analysis (e.g., obfuscation, packing, anti-debugging, VM detection).        |
| **collection**        | Data-gathering and enumeration for exfiltration.                                               |
| **communication**     | Network interactions, C2 communications, data transmission.                                    |
| **compiler**          | Signatures identifying specific build environments or compilers.                               |
| **data-manipulation** | Data transformation (e.g., string encryption, Base64/XOR encoding).                            |
| **executable**        | Attributes within the executable file (e.g., PE sections, debug info).                         |
| **host-interaction**  | Environmental interactions (e.g., reading/writing files, memory allocation, process creation). |
| **impact**            | Potential harm caused (e.g., data destruction, exfiltration, remote access).                   |
| **internal**          | Behind-the-scenes rules used for CAPA execution (not meant for reporting).                     |
| **lib**               | Building blocks used to create other rules.                                                    |
| **linking**           | Dynamically loading external code/libraries during execution.                                  |
| **load-code**         | Runtime code injection and dynamic execution behaviors.                                        |
| **malware-family**    | Distinct characteristics or signatures linked to known malware groups.                         |
| **nursery**           | Staging ground for new, unpolished, or beta rules.                                             |
| **persistence**       | Mechanisms to maintain long-term access to a compromised system.                               |
| **runtime**           | Identifying the language or platform on which the program executes.                            |
| **targeting**         | Behaviors interacting with specific physical targets (e.g., ATMs).                             |

## Hierarchy Example
Rules (stored as `.yml` files) are grouped under specific Namespaces, which in turn belong to a broader TLN.
* **TLN:** `Anti-Analysis`
  * **Namespace:** `anti-vm/vm-detection`
    * Rule: `reference-anti-vm-strings-targeting-virtualbox.yml`
    * Rule: `reference-anti-vm-strings-targeting-virtualpc.yml`
  * **Namespace:** `obfuscation`
    * Rule: `obfuscated-with-dotfuscator.yml`
    * Rule: `obfuscated-with-smartassembly.yml`

# CAPA: Capabilities and Rules
In CAPA, a **Capability** represents the specific behavior or rule matched during the analysis of a binary.
## Capability to Rule Mapping
The Capability name directly translates to the underlying YAML rule file by replacing spaces with hyphens.
* **Example:** The capability `reference Base64 string` is generated by the rule file `reference-base64-string.yml`.
## Complete Capability Reference Table
| Capability | Namespace | Top-Level Namespace (TLN) | Rule YAML File |
| :--- | :--- | :--- | :--- |
| `reference anti-VM strings` | `anti-vm/vm-detection` | `Anti-Analysis` | `reference-anti-vm-strings.yml` |
| `reference anti-VM strings targeting VMWare` | `anti-vm/vm-detection` | `Anti-Analysis` | `reference-anti-vm-strings-targeting-vmware.yml` |
| `reference anti-VM strings targeting VirtualBox` | `anti-vm/vm-detection` | `Anti-Analysis` | `reference-anti-vm-strings-targeting-virtualbox.yml` |
| `reference HTTP User-Agent string` | `http/client` | `Communication` | `reference-http-user-agent-string.yml` |
| `check HTTP status code` | `http` | `Communication` | `check-http-status-code.yml` |
| `reference Base64 string` | `encoding/base64` | `Data Manipulation` | `reference-base64-string.yml` |
| `encode data using XOR` | `encoding/XOR` | `Data Manipulation` | `encode-data-using-xor.yml` |
| `contain a thread local storage (.tls) section` | `pe/section/tls` | `Executable` | `contain-a-thread-local-storage-tls-section.yml` |
| `get common file path` | `file-system` | `Host-Interaction` | `get-common-file-path.yml` |
| `create directory` | `file-system/create` | `Host-Interaction` | `create-directory.yml` |
| `delete file` | `file-system/delete` | `Host-Interaction` | `delete-file.yml` |
| `read file on Windows` | `file-system/read` | `Host-Interaction` | `read-file-on-windows.yml` |
| `write file on Windows` | `file-system/write` | `Host-Interaction` | `write-file-on-windows.yml` |
| `get thread local storage value` | `process` | `Host-Interaction` *(Found in Nursery)* | `get-thread-local-storage-value.yml` |
| `allocate or change RWX memory` | `process/inject` | `Host-Interaction` | `allocate-or-change-rwx-memory.yml` |
| `create process on Windows` | `process/create` | `Host-Interaction` | `create-process-on-windows.yml` |
| `reference cryptocurrency strings` | `impact/cryptocurrency` | `Impact` *(Found in Nursery)* | `reference-cryptocurrency-strings.yml` |
| `link function at runtime on Windows` | `runtime-linking` | `Linking` | `link-function-at-runtime-on-windows.yml` |
| `parse PE header` | `load-code/pe` | `load-code` | `parse-pe-header.yml` |
| `resolve function by parsing PE exports` | `load-code/pe` | `load-code` | `resolve-function-by-parsing-pe-exports.yml` |
| `run PowerShell expression` | `load-code/PowerShell` | `load-code` | `run-powershell-expression.yml` |
| `schedule task via at` | `scheduled-tasks` | `persistence` | `schedule-task-via-at.yml` |
| `schedule task via schtasks` | `scheduled-tasks` | `persistence` | `schedule-task-via-schtasks.yml` |## The "Nursery" Exception
Some rules conceptually belong to a specific TLN (e.g., `Impact` or `Host-Interaction`) but are physically stored in the **Nursery TLN**.
* **Nursery:** A staging ground folder for rules that are currently under development, testing, or are not quite polished.
* **Examples in Nursery:** `reference cryptocurrency strings` and `get thread local storage value`.