- windows is the most dominant os on the plant and thats why its always targeted by hackers and malware writers
- the current server version of windows is windows server 2025
## File system
- the file system used in modern windows is NTFS or New Technology File System
- Before NTFS, there was  **FAT16/FAT32** (File Allocation Table) and **HPFS** (High Performance File System).
- You still see FAT partitions in use today. For example, you typically see FAT partitions in USB devices, MicroSD cards, etc.  but traditionally not on personal Windows computers/laptops or Windows servers.

- NTFS is known as a journaling file system. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.

NTFS addresses many of the limitations of the previous file systems; such as: 

- Supports files larger than 4GB
- Set specific permissions on folders and files
- Folder and file compression
- Encryption ( [Encryption File System (opens in new tab)](https://docs.microsoft.com/en-us/windows/win32/fileio/file-encryption)or **EFS** )

On NTFS volumes, you can set permissions that grant or deny access to files and folders.

The permissions are:

- **Full control** 
- **Modify** 
- **Read & Execute** 
- **List folder contents** 
- **Read** 
- **Write** 

The below image lists the meaning of each permission on how it applies to a file and a folder. (credit  [Microsoft (opens in new tab)](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/bb727008\(v=technet.10\)?redirectedfrom=MSDN))

![](https://assets.tryhackme.com/additional/win-fun1/ntfs-permissions1.png)

Alternate Data Streams  (ADS) is a file attribute specific to Windows  NTFS  (New Technology File System).

Every file has at least one data stream ( `$DATA` ), and ADS allows files to contain more than one stream of data. Natively [Window Explorer (opens in new tab)](https://support.microsoft.com/en-us/windows/what-s-changed-in-file-explorer-ef370130-1cca-9dc5-e0df-2f7416fe1cb1)doesn't display ADS to the user. There are 3rd party executables that can be used to view this data, [PowerShell(opens in new tab)](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.1) also gives you the ability to view ADS for files. We will cover how you can use PowerShell to view any ADS for any files in the [Windows PowerShell](https://tryhackme.com/room/windowspowershell) room.

From a security perspective, malware writers have used ADS to hide data.

Not all its uses are malicious. For example, when you download a file from the Internet, there are identifiers written to ADS to identify that the file was downloaded from the Internet.

## windows/system32 folders
- This is where environment variables, more specifically system environment variables, come into play.  Even though not discussed yet, the system  environment variable for the Windows directory is `%windir%` .
- Per [Microsoft (opens in new tab)](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.1), " _Environment variables store information about the operating system environment. This information includes details such as the operating system path, the number of processors used by the operating system, and the location of temporary folders_ ".
- The System32 folder holds the important files that are critical for the operating system.

## User types 
- there are mainly two types of users that are administrator and standard user 
- to determine which one you are the methods are
	- One way is to click the `Start Menu` and type `Other User`. A shortcut to `System Settings > Other users` should appear.
- When a user account is created, a profile is created for the user. The location for each user profile folder will fall under is C:\Users.

For example, the user profile folder for the user account Max will be C:\Users\Max.

Another way to access this information, and then some, is using **Local User and Group Management**. 

Right-click on the Start Menu and click **Run**. Type `lusrmgr.msc`. See below

Back to lusrmgr, you should see two folders: **Users** and **Groups**. 

If you click on Groups, you see all the names of the local groups along with a brief description for each group. 

Each group has permissions set to it, and users are assigned/added to groups by the Administrator. When a user is assigned to a group, the user inherits the permissions of that group. A user can be assigned to multiple groups.

# UAC — User Account Control

**Tags:** #windows #security

---

## What Is It?

Prevents programs from auto-getting admin rights — they must ask. Even admins run as standard users by default.

---

## Two Tokens (Admins Only)

- **Standard token** — used for everything by default
- **Elevated token** — activated only when UAC prompt is approved
- Standard users must enter admin credentials to elevate

---

## The Prompt

- **Consent** — admin just clicks Yes/No
- **Credential** — standard user enters admin password
- Screen dims = **Secure Desktop** (can't be spoofed or auto-clicked by malware)

---

## UAC Levels

|Level|Behaviour|
|---|---|
|Always Notify|Prompts for everything|
|Default|Prompts for app changes only|
|No Secure Desktop|Same but no dimming|
|Never Notify|UAC off — avoid this|

---

## Integrity Levels

Low → Medium → High → System A lower-integrity process cannot touch a higher one.

---

## Limitations

UAC is **not a hard security boundary** — can be bypassed via auto-elevation abuse, DLL hijacking, or COM object abuse.

---

## Related

- [[Windows Access Tokens]]
- [[Privilege Escalation]]