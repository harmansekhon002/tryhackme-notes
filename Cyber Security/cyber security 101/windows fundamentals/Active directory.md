## Windows domains
- a **Windows domain** is a group of users and computers under the administration of a given business. 
- The main idea behind a domain is to centralise the administration of common components of a Windows computer network in a single repository called **Active Directory (AD)**. 
- The server that runs the Active Directory services is known as a **Domain Controller (DC)**.

![Windows Domain Overview](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/bebe5dfec0208bf563d01fa2dd1fb7a7.png)
## Active directory
-  This service acts as a catalogue that holds the information of all of the "objects" that exist on your network. 
- Amongst the many objects supported by AD, we have users, groups, machines, printers, shares and many others

# 🏰 Active Directory — TryHackMe Cybersecurity 101

> **Course:** TryHackMe — Cybersecurity 101  
> **Module:** Active Directory  
> **Tags:** #tryhackme #active-directory #windows #cybersecurity101 #blue-team #red-team

---

## 📌 Overview

**Active Directory (AD)** is a Microsoft directory service used to centrally manage authentication, authorization, and resources in a Windows domain environment. It is the backbone of most enterprise networks.

> [!info] Why It Matters Understanding AD is critical for both **offensive** (attacking) and **defensive** (hardening) security. Most real-world corporate environments run AD.

---

## 🗂️ Core Concepts

### Domain

- A **logical grouping** of network objects (users, computers, printers, etc.)
- Managed by a **Domain Controller (DC)**
- Named using DNS format → `corp.local`, `company.com`

### Domain Controller (DC)

- A Windows Server with **AD DS (Active Directory Domain Services)** installed
- Responsibilities:
    - Authenticates users (Kerberos/NTLM)
    - Stores the **AD database** (`ntds.dit`)
    - Enforces **Group Policy**
    - Replicates data to other DCs

### Forest & Tree

|Concept|Description|
|---|---|
|**Domain**|Single namespace (e.g. `corp.local`)|
|**Tree**|Multiple domains sharing a contiguous namespace|
|**Forest**|Collection of trees; top-level boundary of AD|

> [!tip] Forest = the highest boundary of trust in AD

---

## 🧩 AD Objects

|Object|Description|
|---|---|
|**Users**|Accounts for people/services|
|**Computers**|Joined machine accounts|
|**Groups**|Collections of users/computers|
|**OUs (Org Units)**|Containers for organizing objects + applying GPO|
|**GPOs**|Group Policy Objects — enforce settings|

### Users

- Regular users → day-to-day employees
- **Service accounts** → run applications (e.g. SQL Server)
- **KRBTGT** → special account used by Kerberos; compromise = golden ticket

### Groups

```
Security Groups  → used for permissions
Distribution Groups → used for email lists
```

#### Important Built-in Groups

|Group|Privilege Level|
|---|---|
|`Domain Admins`|Full control over the domain|
|`Enterprise Admins`|Full control over the forest|
|`Schema Admins`|Can modify AD schema|
|`Administrators`|Local admin on DCs|
|`Domain Users`|Default group for all users|
|`Domain Computers`|All joined computers|

---

## 🔐 Authentication in AD

### NTLM (NT LAN Manager)

- Challenge-response protocol
- **Older, weaker** — susceptible to Pass-the-Hash
- Flow:
    1. Client sends username
    2. Server sends a **challenge** (nonce)
    3. Client sends NTLM **hash** of password + challenge
    4. DC verifies

### Kerberos (Default in modern AD)

- **Ticket-based** authentication
- Three parties: **Client**, **KDC (Key Distribution Center)**, **Service**

```
Flow:
1. Client → AS-REQ  → KDC  (request TGT)
2. KDC   → AS-REP  → Client (encrypted TGT)
3. Client → TGS-REQ → KDC  (request service ticket using TGT)
4. KDC   → TGS-REP → Client (service ticket)
5. Client → AP-REQ  → Service (access resource)
```

#### Key Kerberos Tickets

|Ticket|Purpose|
|---|---|
|**TGT** (Ticket Granting Ticket)|Proves identity to KDC; issued by AS|
|**TGS** (Ticket Granting Service)|Grants access to a specific service|

> [!warning] Attack Surface
> 
> - **Pass-the-Ticket** → steal and reuse TGTs/TGS
> - **Kerberoasting** → request TGS for service accounts, crack offline
> - **Golden Ticket** → forge TGT using KRBTGT hash
> - **Silver Ticket** → forge TGS using service account hash
> - **AS-REP Roasting** → target accounts with pre-auth disabled

---

## 🗃️ AD DS Structure

```
Forest: corp.local
│
├── Tree: corp.local
│   ├── Domain: corp.local (root)
│   │   ├── OU: IT
│   │   │   ├── Users (alice, bob)
│   │   │   └── Computers (PC01, PC02)
│   │   ├── OU: HR
│   │   └── OU: Finance
│   └── Domain: dev.corp.local (child)
│
└── Tree: partner.local (external)
```

---

## 📋 Group Policy (GPO)

- **GPOs** are sets of policies applied to OUs, domains, or sites
- Processed in order: **LSDOU** → Local → Site → Domain → OU
- Later policies override earlier ones (OU wins over domain)

### Common GPO Settings

```
Password Policy        → min length, complexity, expiry
Account Lockout        → failed login threshold
Software Deployment    → push/remove applications
Drive Mapping          → map network shares
Startup/Logon Scripts  → run scripts on boot/login
Disable USB            → block removable media
```

> [!note] GPO Abuse If you can write/modify a GPO linked to a sensitive OU (e.g. Domain Controllers), you can achieve **code execution** on every machine in that OU.

---

## 🔍 Key AD Files & Paths

|File / Path|Description|
|---|---|
|`C:\Windows\NTDS\ntds.dit`|AD database — contains all user hashes|
|`C:\Windows\System32\config\SYSTEM`|Registry hive needed to decrypt `ntds.dit`|
|`C:\Windows\SYSVOL\`|Shared folder replicated to all DCs; holds GPOs & scripts|
|`\\domain\NETLOGON\`|Logon scripts location|
|`\\domain\SYSVOL\`|GPT (Group Policy Templates)|

> [!danger] High-Value Target `ntds.dit` + `SYSTEM` hive = all domain hashes → full domain compromise

---

## 🛠️ Useful AD Commands

### PowerShell (AD Module)

```powershell
# Get domain info
Get-ADDomain

# List all users
Get-ADUser -Filter * | Select Name, SamAccountName

# List Domain Admins
Get-ADGroupMember -Identity "Domain Admins"

# Find all computers
Get-ADComputer -Filter * | Select Name, OperatingSystem

# Get all GPOs
Get-GPO -All

# Find users with password never expires
Get-ADUser -Filter {PasswordNeverExpires -eq $true} -Properties PasswordNeverExpires
```

### CMD / Net Commands

```cmd
net user /domain                    # List all domain users
net group "Domain Admins" /domain   # List Domain Admins
net localgroup administrators       # Local admins
nltest /domain_trusts               # List domain trusts
whoami /groups                      # Current user groups
```

### LDAP Query (PowerShell)

```powershell
# LDAP search for all users
([adsisearcher]"(objectClass=user)").FindAll()
```

---

## 🌐 Trust Relationships

- Allow users in one domain to access resources in another
- Types:
    - **One-way trust** → A trusts B (B's users can access A's resources)
    - **Two-way trust** → mutual access
    - **Transitive** → if A trusts B and B trusts C → A trusts C
    - **Non-transitive** → limited to direct relationship

> [!warning] Lateral Movement via Trusts Trusts can be exploited for cross-domain/cross-forest privilege escalation if misconfigured.

---

## ⚔️ Common AD Attack Techniques

### Enumeration

```powershell
# BloodHound / SharpHound (graph-based AD recon)
Invoke-BloodHound -CollectionMethod All

# PowerView
Get-NetDomain
Get-NetUser
Get-NetGroup
Get-NetComputer
Find-LocalAdminAccess    # Find machines where current user is local admin
```

### Credential Attacks

|Attack|Tool|Description|
|---|---|---|
|**Password Spraying**|`Spray-Passwords.ps1`, `kerbrute`|Try 1 password against many users|
|**Kerberoasting**|`Rubeus`, `GetUserSPNs.py`|Request TGS for SPN accounts, crack hash|
|**AS-REP Roasting**|`Rubeus`, `GetNPUsers.py`|Request TGT for accounts w/o pre-auth|
|**Pass-the-Hash**|`mimikatz`, `impacket`|Authenticate using NTLM hash|
|**Pass-the-Ticket**|`Rubeus`, `mimikatz`|Authenticate using stolen Kerberos ticket|

### Privilege Escalation

|Technique|Description|
|---|---|
|**GPO Abuse**|Modify a GPO linked to a privileged OU|
|**ACL Abuse**|Exploit misconfigured AD ACEs (e.g. WriteDACL, GenericAll)|
|**DCSync**|Replicate DC data to pull password hashes|
|**Golden Ticket**|Use KRBTGT hash to forge TGTs indefinitely|

---

## 🛡️ Hardening / Defence

```
✔ Tier 0/1/2 Admin Model (Privileged Access Workstations)
✔ Disable legacy protocols (NTLM where possible)
✔ Enable Protected Users security group
✔ Enable Kerberos AES encryption
✔ Audit AD ACLs regularly (PingCastle, BloodHound)
✔ Enable LAPS (Local Administrator Password Solution)
✔ Monitor for DCSync events (Event ID 4662)
✔ Restrict use of Domain Admin accounts
✔ Enable Credential Guard
✔ Rotate KRBTGT password periodically
```

> [!tip] Detection: Key Event IDs
> 
> |Event ID|Meaning|
> |---|---|
> |`4624`|Successful logon|
> |`4625`|Failed logon|
> |`4648`|Logon using explicit credentials|
> |`4662`|Object accessed (DCSync detection)|
> |`4768`|TGT requested (Kerberoasting indicator)|
> |`4769`|Service ticket requested|
> |`4771`|Kerberos pre-auth failed|
> |`4776`|NTLM authentication|

---

## 🔗 Related Notes

- [[Kerberos Authentication]]
- [[NTLM & Pass-the-Hash]]
- [[BloodHound & SharpHound]]
- [[Privilege Escalation Windows]]
- [[PowerView Cheatsheet]]
- [[Mimikatz Usage]]
- [[THM - Windows Fundamentals]]

---

## 📚 Resources

- [TryHackMe — Active Directory Basics](https://tryhackme.com/room/winadbasics)
- [HackTricks — AD](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology)
- [PayloadsAllTheThings — AD](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Active%20Directory%20Attack.md)
- [BloodHound](https://github.com/BloodHoundAD/BloodHound)
- [Impacket](https://github.com/fortra/impacket)