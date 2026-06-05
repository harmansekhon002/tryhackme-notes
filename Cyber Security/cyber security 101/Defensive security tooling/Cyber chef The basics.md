CyberChef is a simple, intuitive web-based application designed to help with various “cyber” operation tasks within your web browser
# CyberChef Interface Overview
CyberChef consists of four main areas: Operations, Recipe, Input, and Output.
## Operations Area
Searchable repository of data manipulation operations. Hover over operations for descriptions and Wikipedia links.

| Operation | Description |
|---|---|
| From Morse Code | Translates Morse Code to alphanumeric characters. |
| URL Encode | Encodes problematic characters into percent-encoding for URIs/URLs. |
| To Base64 | Encodes raw data into an ASCII Base64 string. |
| To Hex | Converts input string to hexadecimal bytes. |
| To Decimal | Converts input data to an ordinal integer array. |
| ROT13 | Caesar substitution cipher rotating characters (default 13). |
## Recipe Area
The core workspace to select, drag, arrange, and configure operations.
* **Save / Load / Clear Recipe:** Manage your configured operation sequences.
* **BAKE!:** Manually process the data with the current recipe.
* **Auto Bake:** Automatically processes data as inputs or recipes change.
## Input Area
Workspace to paste, type, or drag text/files for processing.
* **Add new input tab:** Create multiple tabs for different values.
* **Open folder / file as input:** Upload entire folders or individual files for processing.
* **Clear input and output:** Wipes current data from the panes.
* **Reset pane layout:** Restores default window sizes.
## Output Area
Displays the results of the processed data based on the recipe.
* **Save output to file:** Exports results as a `.dat` file.
* **Copy raw output:** Copies results directly to your clipboard.
* **Replace input with output:** Overwrites the original input with the newly processed results.
* **Maximise output pane:** Expands the output view for better visibility.
# CyberChef Thought Process
A four-step methodology for effectively analyzing and manipulating data within CyberChef.
## 1. Set a Clear Objective
Define a specific, achievable goal by answering, "What do I want to accomplish?"
* **Example:** Identifying a gibberish string found during a security investigation to uncover its hidden message.
## 2. Input Your Data
Place your raw data into the tool.
* **Action:** Paste the text or upload the file (e.g., the gibberish string) into the Input Area.
## 3. Select Operations
Choose appropriate operations based on your knowledge of the data or external research.
* **Example:** If context clues suggest encryption, select tools from the Encryption/Encoding category (e.g., ROT13, Base64, Base85, or ROT47).
## 4. Check the Output
Evaluate the results in the Output Area to answer, "Have we achieved our objective?"
* **Success:** The intended result is achieved (the string is successfully decoded).
* **Failure:** The objective is not met; you must iterate and repeat the process with different operations or parameters.

# CyberChef Operations: Extractors, Time & Data Formats
## Extractors
| Operation | Description |
|---|---|
| Extract IP addresses | Extracts all valid IPv4 and IPv6 addresses. |
| Extract URLs | Extracts URLs (requires protocol like HTTP/FTP to minimize false positives). |
| Extract email addresses | Extracts email formats (e.g., anything@domain.com). |
## Date and Time
* **UNIX timestamp:** 32-bit value representing seconds since Jan 1, 1970 UTC.

| Operation | Description |
|---|---|
| From UNIX Timestamp | Converts a UNIX timestamp to a readable datetime string. |
| To UNIX Timestamp | Parses a UTC datetime string into a UNIX timestamp. |
## Data Format & Encodings
Base encodings transform binary data into text using specific ASCII character sets.

| Operation | Description |
|---|---|
| From Base64 | Decodes ASCII Base64 string back into raw data. |
| URL Decode | Converts percent-encoded characters back to raw values. |
| From Base85 | Decodes Base85 (more efficient than Base64). |
| From Base58 | Decodes Base58 (removes l, I, 0, O to improve human readability). |
| To Base62 | Encodes data using restricted symbols, resulting in shorter strings. |
## Manual Base64 Conversion Example ("THM" -> "VEhN")

1. **Text to Binary & Merge:** Convert ASCII characters to 8-bit binary and concatenate into a 24-bit string. (e.g., T=01010100, H=01001000, M=01001101 → 010101000100100001001101).
2. **Split & Convert to Decimal:** Divide the 24-bit string into 6-bit blocks and convert each to a base-10 decimal. (e.g., 010101=21, 000100=4, 100001=33, 001101=13).
3. **Map to Base64 Index:** Match the decimals to the standard Base64 index table. (e.g., 21=V, 4=E, 33=h, 13=N → VEhN).
## Common URL Encodings (UTF-8)
| Character | Encoded |
|---|---|
| : | %3A |
| / | %2F |
| . | %2E |
| = | %3D |
| # | %23 |