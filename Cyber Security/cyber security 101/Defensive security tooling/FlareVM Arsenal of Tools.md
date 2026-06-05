**Forensics, Logic Analysis, and Reverse Engineering**

## FlareVM Tool Directory
A categorised reference of specialised forensics, incident response, and malware investigation tools available within the FlareVM environment.

## Reverse Engineering & Debugging
Taking compiled products apart to understand logic (Reverse Engineering) and stepping through code to identify behavior (Debugging).

| Tool | Description |
| :--- | :--- |
| **Ghidra** | NSA-developed open-source reverse engineering suite. |
| **x64dbg** | Open-source debugger for x64 and x32 binaries. |
| **OllyDbg** | Debugger for reverse engineering at the assembly level. |
| **Radare2** | Sophisticated open-source platform for reverse engineering. |
| **Binary Ninja** | Tool for disassembling and decompiling binaries. |
| **PEiD** | Packer, cryptor, and compiler detection tool. |

## Disassemblers & Decompilers
Breaking compiled machine code back into understandable formats (assembly or high-level pseudo-code) to analyze control flow.

| Tool | Description |
| :--- | :--- |
| **CFF Explorer** | PE editor to analyze and edit Portable Executable (PE) files. |
| **Hopper** | Debugger, disassembler, and decompiler. |
| **RetDec** | Open-source decompiler for machine code. |

## Static & Dynamic Analysis
**Static Analysis:** Inspecting code without executing it. 
**Dynamic Analysis:** Observing software behavior while it runs.

| Tool | Description |
| :--- | :--- |
| **Process Hacker** | Sophisticated memory editor and process watcher. |
| **PEview** | Portable executable (PE) file viewer for analysis. |
| **Dependency Walker** | Displays an executable’s DLL dependencies. |
| **DIE (Detect It Easy)** | Packer, compiler, and cryptor detection tool. |

## Forensics & Incident Response (DFIR)
Collecting and preserving digital evidence (Forensics) and detecting/recovering from cyberattacks (Incident Response).

| Tool | Description |
| :--- | :--- |
| **Volatility** | RAM dump analysis framework for memory forensics. |
| **Rekall** | Framework for memory forensics in incident response. |
| **FTK Imager** | Disk image acquisition and analysis tool. |

## Network Analysis
Studying network traffic to uncover patterns, identify threats, and understand network behavior.

| Tool          | Description                                                                 |
| :------------ | :-------------------------------------------------------------------------- |
| **Wireshark** | Network protocol analyzer for traffic recording and deep packet inspection. |
| **Nmap**      | Vulnerability detection and network mapping tool.                           |
| **Netcat**    | Utility to read and write data across network connections.                  |

## File Analysis
Examining binary data, file structures, and permissions.

| Tool            | Description                                           |
| :-------------- | :---------------------------------------------------- |
| **FileInsight** | Program for looking through and editing binary files. |
| **Hex Fiend**   | Lightweight and quick hex editor.                     |
| **HxD**         | Binary file viewing and editing with a hex editor.    |

## Scripting & Automation
Automating repetitive tasks to increase efficiency and reduce human error.

| Tool                  | Description                                 |
| :-------------------- | :------------------------------------------ |
| **Python**            | Automation-focused modules and scripting.   |
| **PowerShell Empire** | Framework for PowerShell post-exploitation. |

## Sysinternals Suite
Advanced Windows system utilities for management, troubleshooting, and diagnosing systems.

| Tool                 | Description                                                         |
| :------------------- | :------------------------------------------------------------------ |
| **Autoruns**         | Shows executables configured to run during system boot-up.          |
| **Process Explorer** | Provides detailed information about running processes.              |
| **Process Monitor**  | Monitors and logs real-time process, thread, and registry activity. |
# Malware Analysis: Practical FlareVM Workflow
## Scenario 1: Static Analysis (`windows.exe`)
The goal of static analysis is to extract information without executing the binary.

### Tool: PEStudio
* **Hashes:** Extract MD5/SHA-1 and check against databases like VirusTotal to identify known malware or novel campaigns.
* **Metadata/Description:** Verify legitimacy. (e.g., a file claiming to be "Registry Editor" in the Downloads folder is highly suspicious, as the real `regedit.exe` lives in `C:\Windows\System32`).
* **Version/Language:** Look for unexpected foreign languages in the metadata (e.g., Russian text in an English-speaking organization's environment).
* **Rich Header:** The *absence* of a Rich Header strongly indicates the file has been packed or obfuscated to evade detection.
* **IAT (Import Address Table):** Check the "functions" or "blacklist" tab for malicious API calls:
  * `set_UseShellExecute`: Executes other processes via the OS shell (spawns child malware).
  * `CryptoStream`, `RijndaelManaged`: Cryptographic functions (AES) indicating potential ransomware or encrypted C2 communications.
### Tool: FLOSS (FireEye Labs Obfuscated String Solver)
* **Command:** `FLOSS.exe .\windows.exe > windows.txt` (via PowerShell).
* **Purpose:** Extracts and deobfuscates strings from the binary.
* **Findings:** Corroborates PEStudio findings by revealing the same imported API functions in text format.

## Scenario 2: Dynamic Analysis (`cobaltstrike.exe`)
The goal is to run the malware and observe its behavior, specifically focusing on network connections to Command and Control (C2) servers.
### Tool: Process Explorer
* **Hierarchy:** Identify the malware's parent process (e.g., launching manually puts it under `explorer.exe`). Note the Process ID (PID).
* **Network Check:** Right-click the process -> **Properties** -> **TCP/IP** tab to view destination IPs and connection states.
### Tool: Process Monitor (Procmon)

* **Purpose:** Cross-verify findings from Process Explorer with detailed event logging.
* **Filtering Setup:** To cut through system noise (CTRL + L):
  1.  Column: `Process Name`
  2.  Relation: `contains`
  3.  Value: `cobalt` (or relevant malware string)
  4.  Action: `Include` -> Click **Add** -> Click **Apply**.
* **Findings:** Confirms exact outbound network connections (e.g., connection to `47.120.46.210`), solidifying the C2 infrastructure indicators.