_PowerShell is a cross-platform task automation solution made up of a command-line shell, a scripting language, and a configuration management framework_
- its built on the .NET framework
-  Unlike older text-based command-line tools, PowerShell is object-oriented, which means it can handle complex data types and interact with system components more effectively
- Initially exclusive to Windows, PowerShell has lately expanded to support macOS and Linux, making it a versatile option for IT professionals across different operating systems.
- As IT environments evolved to include various operating systems, the need for a versatile automation tool grew. In 2016, Microsoft responded by releasing PowerShell Core, an open-source and cross-platform version that runs on Windows, macOS, and Linux.

## power in powershell
In programming, an **object** represents an item with **properties** (characteristics) and **methods**(actions)

**cmdlet** (pronounced _command-let_) is run in PowerShell, it returns objects that retain their properties and methods. This allows for more powerful and flexible data manipulation since these objects do not require additional parsing of text.

> to start powershell from cmd type powershell and click enter

Cmdlets follow a consistent `Verb-Noun` naming convention. This structure makes it easy to understand what each cmdlet does. The `Verb` describes the action, and the `Noun` specifies the object on which action is performed.

- `Get-Content`: Retrieves (gets) the content of a file and displays it in the console.
- `Set-Location`: Changes (sets) the current working directory.

To list all available cmdlets, functions, aliases, and scripts that can be executed in the current PowerShell session, we can use `Get-Command`. It’s an essential tool for discovering what commands one can use.

`Get-Help`: it provides detailed information about cmdlets, including usage, parameters, and examples.

To make the transition easier for IT professionals, PowerShell includes aliases —which are shortcuts or alternative names for cmdlets— for many traditional Windows commands. Indispensable for users already familiar with other command-line tools, `Get-Alias` lists all aliases available. For example, `dir` is an alias for `Get-ChildItem`, and `cd` is an alias for `Set-Location`.

## managing files and directories
- the get-childitem can be used to get the list of dirs
- if -path specified the dir of that path will be shown otherwise the list of the current working directory
- To navigate to a different directory, we can use the `Set-Location` cmdlet. It changes the current directory, bringing us to the specified path, akin to the `cd` command in Command Prompt.
- To create an item in PowerShell, we can use `New-Item`. We will need to specify the path of the item and its type (whether it is a file or a directory).
- Similarly, the `Remove-Item` cmdlet removes both directories and files, whereas in Windows CLI we have separate commands `rmdir` and `del`.
- We can copy or move files and directories alike, using respectively `Copy-Item` (equivalent to `copy`) and `Move-Item` (equivalent to `move`).
- Finally, to read and display the contents of a file, we can use the `Get-Content` cmdlet, which works similarly to the `type` command in Command Prompt (or `cat` in Unix-like systems).
## piping filtering and sorting data

**Piping** is a technique used in command-line environments that allows the output of one command to be used as the input for another. This creates a sequence of operations where the data flows from one command to the next. Represented by the `|` symbol, piping is widely used in the Windows CLI

In PowerShell, piping is even more powerful because it passes **objects** rather than just text. These objects carry not only the data but also the properties and methods that describe and interact with the data.

For example, if you want to get a list of files in a directory and then sort them by size, you could use the following command in PowerShell:

Terminal

```powershell
PS C:\Users\captain\Documents\captain-cabin> Get-ChildItem | Sort-Object Length

    Directory: C:\Users\captain\Documents\captain-cabin

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          9/4/2024  12:50 PM              0 captain-boots.txt
-a----          9/4/2024  12:14 PM            264 captain-hat2.txt
-a----          9/4/2024  12:14 PM            264 captain-hat.txt
-a----          9/4/2024  12:37 PM           2116 ship-flag.txt
d-----          9/4/2024  12:50 PM                captain-wardrobe
```

Here, `Get-ChildItem` retrieves the files (as objects), and the pipe (`|`) sends those file objects to `Sort-Object`, which then sorts them by their `Length` (size) property. This object-based approach allows for more detailed and flexible command sequences.

In the example above, we have leveraged the `Sort-Object` cmdlet to sort objects based on specified properties. Beyond sorting, PowerShell provides a set of cmdlets that, when combined with piping, allow for advanced data manipulation and analysis.

To filter objects based on specified conditions, returning only those that meet the criteria, we can use the `Where-Object` cmdlet. For instance, to list only `.txt` files in a directory, we can use:

Terminal

```powershell
PS C:\Users\captain\Documents\captain-cabin> Get-ChildItem | Where-Object -Property "Extension" -eq ".txt" 

    Directory: C:\Users\captain\Documents\captain-cabin

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          9/4/2024  12:50 PM              0 captain-boots.txt
-a----          9/4/2024  12:14 PM            264 captain-hat.txt
-a----          9/4/2024  12:14 PM            264 captain-hat2.txt
-a----          9/4/2024  12:37 PM           2116 ship-flag.txt
```

Here, `Where-Object` filters the files by their `Extension` property, ensuring that only files with extension equal (`-eq`) to `.txt` are listed.

The operator `-eq` (i.e. "**equal to**") is part of a set of **comparison operators** that are shared with other scripting languages (e.g. Bash, Python). To show the potentiality of the PowerShell's filtering, we have selected some of the most useful operators from that list:

- `-ne`: "**not equal**". This operator can be used to exclude objects from the results based on specified criteria.
- `-gt`: "**greater than**". This operator will filter only objects which exceed a specified value. It is important to note that this is a strict comparison, meaning that objects that are equal to the specified value will be excluded from the results.
- `-ge`: "**greater than or equal to**". This is the non-strict version of the previous operator. A combination of `-gt` and `-eq`.
- `-lt`: "**less than**". Like its counterpart, "greater than", this is a strict operator. It will include only objects which are strictly below a certain value.
- `-le`: "**less than or equal to**". Just like its counterpart `-ge`, this is the non-strict version of the previous operator. A combination of `-lt` and `-eq`.

Below, another example shows that objects can also be filtered by selecting properties that match (`-like`) a specified pattern:

Terminal

```powershell
PS C:\Users\captain\Documents\captain-cabin> Get-ChildItem | Where-Object -Property "Name" -like "ship*"  

    Directory: C:\Users\captain\Documents\captain-cabin

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          9/4/2024  12:37 PM           2116 ship-flag.txt
```

The next filtering cmdlet, `Select-Object`, is used to select specific properties from objects or limit the number of objects returned. It’s useful for refining the output to show only the details one needs.

Terminal

```powershell
PS C:\Users\captain\Documents\captain-cabin> Get-ChildItem | Select-Object Name,Length 

Name              Length
----              ------
captain-wardrobe
captain-boots.txt 0
captain-hat.txt   264
captain-hat2.txt  264
ship-flag.txt     2116
```

The cmdlets pipeline can be extended by adding more commands, as the feature isn’t limited to just piping between two cmdlets. As an exercise, try and build a pipeline of cmdlets to sort and filter the output with the goal of displaying the largest file in the `C:\Users\captain\Documents\captain-cabin` directory.

Terminal

```powershell
Get-ChildItem | Sort-Object Length -Descending | Select-Object -First 1

    Directory: C:\Users\captain\Documents\captain-cabin

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          9/4/2024  12:37 PM           2116 ship-flag.txt
```

The last in this set of filtering cmdlets is `Select-String`. This cmdlet searches for text patterns within files, similar to `grep` in Unix-based systems or `findstr` in Windows Command Prompt. It’s commonly used for finding specific content within log files or documents.

Terminal

```powershell
PS C:\Users\captain\Documents\captain-cabin> Select-String -Path ".\captain-hat.txt" -Pattern "hat" 

captain-hat.txt:8:Don't touch my hat!
```

The `Select-String` cmdlet fully supports the use of regular expressions ([regex(opens in new tab)](https://learn.microsoft.com/en-us/dotnet/standard/base-types/regular-expressions)). This advanced feature allows for complex pattern matching within files, making it a powerful tool for searching and analysing text data.
## system and network information
- The `Get-ComputerInfo` cmdlet retrieves comprehensive system information, including operating system information, hardware specifications, BIOS details, and more
-   Essential for managing user accounts and understanding the machine’s security configuration, `Get-LocalUser` lists all the local user accounts on the system. The default output displays, for each user, username, account status, and description.
- `Get-NetIPConfiguration` provides detailed information about the network interfaces on the system, including IP addresses, DNS servers, and gateway configurations.
- In case we need specific details about the IP addresses assigned to the network interfaces, the `Get-NetIPAddress` cmdlet will show details for all IP addresses configured on the system, including those that are not currently active.

## real time system analysis
- get-process gives the list of running tasks on the system
- `Get-Service` allows the retrieval of information about the status of services on the machine, such as which services are running, stopped, or paused. It is used extensively in troubleshooting by system administrators, but also by forensics analysts hunting for anomalous services installed on the system.
- `Get-NetTCPConnection` displays current TCP connections, giving insights into both local and remote endpoints. This cmdlet is particularly handy during an incident response or malware analysis task, as it can uncover hidden backdoors or established connections towards an attacker-controlled server.
-  `Get-FileHash` as a useful cmdlet for generating file hashes, which is particularly valuable in incident response, threat hunting, and malware analysis, as it helps verify file integrity and detect potential tampering.

In the output above, you can see two streams that are attached to the file `C:\House\house_log.txt`:

1. `:$DATA` is the default data stream of every NTFS file. It contains the normal file contents and is not an ADS.
    
2. `housinginginfo` is the Alternate Data Stream (ADS) added to this file. It appears as `house_log.txt:housinginfo`, which means an extra hidden stream named housinginfo is attached to this file.
## scripting
**Scripting** is the process of writing and executing a series of commands contained in a text file, known as a script, to automate tasks that one would generally perform manually in a shell, like PowerShell.

- For **blue team** professionals such as incident responders, malware analysts, and threat hunters, PowerShell scripts can automate many different tasks, including log analysis, detecting anomalies, and extracting indicators of compromise (IOCs). These scripts can also be used to reverse-engineer malicious code (malware) or automate the scanning of systems for signs of intrusion.
    
- For the **red team**, including penetration testers and ethical hackers, PowerShell scripts can automate tasks like system enumeration, executing remote commands, and crafting obfuscated scripts to bypass defences. Its deep integration with all types of systems makes it a powerful tool for simulating attacks and testing systems’ resilience against real-world threats.
    
- Staying in the context of cyber security, **system administrators** benefit from PowerShell scripting for automating integrity checks, managing system configurations, and securing networks, especially in remote or large-scale environments. PowerShell scripts can be designed to enforce security policies, monitor systems health, and respond automatically to security incidents, thus enhancing the overall security posture.

- The second example demonstrates that we don't need to know how to script to benefit from the power of `Invoke-Command`. In fact, by appending the `-ScriptBlock { ... }` parameter to the cmdlet's syntax, we can execute any command (or sequence of commands) on the remote computer. The result would be the same as if we were typing the commands in a local PowerShell session on the remote computer itself.