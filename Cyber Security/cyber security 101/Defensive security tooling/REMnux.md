- Linux toolkit for malware analysis & reverse engineering
- Used for static, dynamic, memory & network analysis
- Comes with pre-installed security tools
- Common tools: Wireshark, Volatility, YARA, CAPA, Radare2
- Usually run in a VM for safe malware analysis

# Static File Analysis with oledump.py
`oledump.py` is a Python tool for analyzing OLE2 files (Structured Storage/Compound File Binary Format), which Microsoft uses to store documents with multiple data types. 
## 1. Initial Scan
* **Command:** `oledump.py [filename]` (e.g., `oledump.py agenttesla.xlsm`)
* **Purpose:** Lists data streams. Look for the capital letter **M** next to an index (e.g., `A4: M 688 'VBA/ThisWorkbook'`), indicating an embedded Macro.
## 2. Inspect Specific Data Stream
* **Command:** `oledump.py [filename] -s [index]` (e.g., `-s 4` selects the 4th stream).
* **Purpose:** Displays the raw contents of the selected stream (outputs as a hex dump).
## 3. Decompress VBA Macro
* **Command:** `oledump.py [filename] -s [index] --vbadecompress`
* **Purpose:** Automatically decompresses the macro hex dump into human-readable VBA script.
## 4. Payload Deobfuscation (CyberChef)
* **Identify:** Scripts often use junk characters to evade detection (e.g., `^p*o^*w*e*r...`).
* **Deobfuscate:** Paste the string into CyberChef.
* **Operation:** Chain multiple **Find / Replace** operations (Type: Simple String). Find the junk character (e.g., `*` or `^`) and leave the Replace field empty to strip them out.
## 5. Analyzing the PowerShell Payload
* **`-WindowStyle hidden`:** Runs PowerShell without displaying a visible window to the user.
* **`-executionpolicy bypass`:** Temporarily ignores security policies restricting script execution.
* **`Invoke-WebRequest -Uri [URL]`:** Downloads the malicious file/resource from the specified URL.
* **`-OutFile $TempFile`:** Saves the downloaded payload to a local temporary variable/file.
* **`Start-Process $TempFile`:** Executes the downloaded malicious binary.
* **Execution Flow:** User opens document -> Macro runs -> Obfuscated PowerShell is decoded -> PowerShell downloads the payload -> Payload is executed. 
# Dynamic Analysis with INetSim
INetSim (Internet Services Simulation Suite) is a tool for simulating network services to analyze the network behavior of malware in a controlled environment without connecting to the real internet.
## 1. Setting Up the REMnux Environment
1.  **Find the IP:** Determine your REMnux machine's IP address (e.g., using `ifconfig`).
2.  **Configure INetSim:**
    * Command: `sudo nano /etc/inetsim/inetsim.conf`
    * Action: Uncomment the `dns_default_ip` line and change its value from `0.0.0.0` to your REMnux machine's IP.
    * Save and exit (`CTRL + O`, `Enter`, `CTRL + X`).
3.  **Verify Configuration:**
    * Command: `cat /etc/inetsim/inetsim.conf | grep dns_default_ip`
4.  **Start INetSim:**
    * Command: `sudo inetsim`
    * Verify the output says "Simulation running."
## 2. Simulating Malware Behavior (From AttackBox)
1.  **Access INetSim:** Open a browser and navigate to `https://[REMnux_IP]`. Accept the self-signed certificate security risk. You will see the INetSim default page.
2.  **Simulate Payload Download:** Use `wget` to simulate malware reaching out to download secondary payloads.
    * `sudo wget https://[REMnux_IP]/second_payload.zip --no-check-certificate`
    * `sudo wget https://[REMnux_IP]/second_payload.ps1 --no-check-certificate`
3.  **Verify:** Check the download directory to confirm the files were downloaded. These are fake files served by INetSim.
## 3. Reviewing Connection Reports
1.  **Stop INetSim:** Back on the REMnux machine, stop the INetSim process.
2.  **Locate Report:** Note the report path provided in the terminal (e.g., `/var/log/inetsim/report/report.[PID].txt`).
3.  **Analyse Logs:**
    * Command: `sudo cat /var/log/inetsim/report/report.[PID].txt`
    * The log details the timestamps, protocols (e.g., HTTPS), methods (e.g., GET), URLs accessed, and the specific fake files INetSim served to the requesting machine

# Memory Evidence Preprocessing 

Preprocessing involves running analysis tools upfront and saving outputs to text files, allowing analysts to search and review evidence instantly without waiting for tool execution times.

## Volatility 3 Core Windows Plugins

Volatility extracts structured data from memory dumps (e.g., `.mem` files).
* **Command Structure:** `vol3 -f [image_name.mem] [plugin]`

| Plugin | Description |
| :--- | :--- |
| `windows.pstree.PsTree` | Lists active processes organized in a visual parent-child tree. |
| `windows.pslist.PsList` | Lists all currently active processes. |
| `windows.cmdline.CmdLine` | Shows the exact command-line arguments used to launch a process. |
| `windows.filescan.FileScan` | Scans for file objects present in memory. |
| `windows.dlllist.DllList` | Lists all loaded modules/DLLs for processes. |
| `windows.malfind.Malfind` | Identifies process memory ranges containing potentially injected/malicious code. |
| `windows.psscan.PsScan` | Scans for process structures (can find hidden/terminated processes unlike PsList). |

## Automating Volatility with a Bash Loop

To run multiple plugins consecutively and output them to text files silently:
```bash
for plugin in windows.malfind.Malfind windows.psscan.PsScan windows.pstree.PsTree windows.pslist.PsList windows.cmdline.CmdLine windows.filescan.FileScan windows.dlllist.DllList; do vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt; done