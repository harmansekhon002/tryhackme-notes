
In hashing a hard problem quickly becomes computationally infeasible in computer science. This computational problem has its roots in mathematics as P vs NP.

In computer science, P and NP are two classes of problems that help us understand the efficiency of algorithms:

- **P (Polynomial Time)**: Class P covers the problems whose solution can be found in polynomial time. Consider sorting a list in increasing order. The longer the list, the longer it would take to sort; however, the increase in time is not exponential.
- **NP (Non-deterministic Polynomial Time)**: Problems in the class NP are those for which a given solution can be checked quickly, even though finding the solution itself might be hard. In fact, we don’t know if there is a fast algorithm to find the solution in the first place.

## Dictionary attack

John will check every possible hash for a large number of words and create a dictionary (only if we know the hashing algorithm) 
you can then compare these hashes to the one you are trying to crack and you will have the password cracked 

## Cracking Basic Hashes

Basic Syntax:

`john [options] [file path]`

- `john`: Invokes the John the Ripper program
- `[options]`: Specifies the options you want to use
- `[file path]`: The file containing the hash you’re trying to crack; if it’s in the same directory, you won’t need to name a path, just the file.

## Identifying hashes

we can use a site like hash identifier 

we can also use a tool like hash-identifier
simply download the file(done)
then type python hash-id.py [hash]
## using john with specific format

  
john --format=[format] --wordlist=[path to wordlist] [path to file]

`john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt`

Raw tells us no salt

 To check if you need to add the prefix or not, you can list all of John’s formats using `john --list=formats` and either check manually or grep for your hash type using something like `john --list=formats | grep -iF "md5"`

## Cracking Windows Authentication Hashes

Authentication hashes are very commonly cracked in 
penetration tests or red team managements 

These are stored in the operating systems 

You must be a privileged user to access these
## NTHash/NTLM

In modern windows OS the hash type is NTHash, it is also referred to as NTLM

In windows, SAM (security account manager) is used to store user account information including usernames and hashed passwords

SAM vs NTDS.dit
- SAM: Local Windows database storing local user password hashes.
- NTDS.dit: Active Directory database storing domain user password hashes.
- SAM → affects one machine.
- NTDS.dit → affects entire domain/network.

Mimikatz (basic use)
- Tool used to dump passwords and NTLM hashes from memory.
- Requires Administrator/SYSTEM privileges.

Common commands:
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords   -> dump creds from memory
lsadump::sam               -> dump SAM hashes
lsadump::dcsync /user:Administrator  -> dump domain hashes

Usage:
- Extract hashes
- Crack them OR perform Pass-the-Hash attack

## Cracking /etc/shadow

On Linux machines the passwords hashes are stored in the /etc/shadow 
It also stores other information, such as the date of last password change and password expiration information. 
It contains one entry per line for each user or user account of the system. 
This file is usually only accessible by the root user

### Unshadowing

John can be very particular about the formats it needs data in to be able to work with it; for this reason, to crack `/etc/shadow` passwords, you must combine it with the `/etc/passwd` file for John to understand the data it’s being given. To do this, we use a tool built into the John suite of tools called `unshadow`. The basic syntax of `unshadow` is as follows:

`unshadow [path to passwd] [path to shadow]`

- `unshadow`: Invokes the unshadow tool
- `[path to passwd]`: The file that contains the copy of the `/etc/passwd` file you’ve taken from the target machine
- `[path to shadow]`: The file that contains the copy of the `/etc/shadow` file you’ve taken from the target machine

**Example Usage:**

`unshadow local_passwd local_shadow > unshadowed.txt`

**FILE 1 - local_passwd**

Contains the `/etc/passwd` line for the root user:

`root:x:0:0::/root:/bin/bash`

**FILE 2 - local_shadow**

Contains the `/etc/shadow` line for the root user: `root:$6$2nwjN454g.dv4HN/$m9Z/r2xVfweYVkrr.v5Ft8Ws3/YYksfNwq96UL1FX0OJjY1L6l.DS3`
## Cracking

We can then feed the output from `unshadow`, in our example use case called `unshadowed.txt`, directly into John. We should not need to specify a mode here as we have made the input specifically for John; however, in some cases, you will need to specify the format as we have done previously using: `--format=sha512crypt`

`john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt`

## Single Crack Mode

In this mode instead of a wordlist john tries to mangle the username or make changes to try to make a good enough pass 

Consider the username “Markus”.

Some possible passwords could be:

- Markus1, Markus2, Markus3 (etc.)
- MArkus, MARkus, MARKus (etc.)
- Markus!, Markus$, Markus* (etc.)

This approach is done by a set of mangling rules which define how the username will be mutated and thus generating a wordlist

## GECOS

General Electric Comprehensive Operating System
The fifth field in etc shadow or passwd is GECOS which contains general information about the user like full name office number and telephone number 
John uses this field to generate another dictionary 

## using single crack mode

`john --single --format=[format] [path to file]`

- `--single`: This flag lets John know you want to use the single hash-cracking mode
- `--format=[format]`: As always, it is vital to identify the proper format.

**Example Usage:**
a  
john --single --format=raw-sha256 hashes.txt

#identification #hashes
# Hash Identification

## What is Hash Identification
Determining which hashing algorithm produced a hash before cracking it.
## Common Hash Lengths
- 32 → MD5 / NTLM
- 40 → SHA1
- 56 → SHA224
- 64 → SHA256
- 96 → SHA384
- 128 → SHA512

## Prefix Indicators
- $1$ → MD5 (Linux)
- $2a$ / $2b$ → bcrypt
- $5$ → SHA256 (Linux)
- $6$ → SHA512 (Linux)

## Tools
- [hashid](chatgpt://generic-entity?number=2)
- [hash-identifier](chatgpt://generic-entity?number=3)
- [CyberChef](chatgpt://generic-entity?number=4)

### Example
5f4dcc3b5aa765d61d8327deb882cf99
- Length = 32
- Hex characters
- Likely MD5 or NTLM
- Use context to confirm

# Custom Rules

Many companies often have forced password complexity to combat dictionary attacks

Custom rules are defined in the john.conf file

his file can be found in `/opt/john/john.conf`on the TryHackMe Attackbox. It is usually located in `/etc/john/john.conf` if you have installed John using a package manager or built from source with `make`.

We then use a regex style pattern match to define where the word will be modified; again

- `Az`: Takes the word and appends it with the characters you define
- `A0`: Takes the word and prepends it with the characters you define
- `c`: Capitalises the character positionally

 We must define what characters should be appended, prepended or otherwise included. We do this by adding character sets in square brackets `[ ]` where they should be used. These follow the modifier patterns inside double quotes `" "`. Here are some common examples:

- `[0-9]`: Will include numbers 0-9  
    
- `[0]`: Will include only the number 0
- `[A-z]`: Will include both upper and lowercase  
    
- `[A-Z]`: Will include only uppercase letters
- `[a-z]`: Will include only lowercase letters

Please note that:

- `[a]`: Will include only `a`
- `[!£$%@]`: Will include the symbols `!`, `£`, `$`, `%`, and `@`

Putting this all together, to generate a wordlist from the rules that would match the example password `Polopassword1!` (assuming the word `polopassword` was in our wordlist), we would create a rule entry that looks like this:

`[List.Rules:PoloPassword]`

`cAz"[0-9] [!£$%@]"`

Utilises the following:

- `c`: Capitalises the first letter
- `Az`: Appends to the end of the word
- `[0-9]`: A number in the range 0-9
- `[!£$%@]`: The password is followed by one of these symbols

## Cracking zip protected files 

To crack zip files we will use the ***zip2john*** to convert the zip files into a hash format 

Syntax

zip2john [options] [zip file] > [output file]

- `[options]`: Allows you to pass specific checksum options to `zip2john`; this shouldn’t often be necessary
- `[zip file]`: The path to the Zip file you wish to get the hash of
- `>`: This redirects the output from this command to another file
- `[output file]`: This is the file that will store the output

**Example Usage**

`zip2john zipfile.zip > zip_hash.txt`

## Cracking a password protected RAR archive

The basic syntax is as follows:

`rar2john [rar file] > [output file]`

- `rar2john`: Invokes the `rar2john` tool
- `[rar file]`: The path to the RAR file you wish to get the hash of
- `>`: This redirects the output of this command to another file
- `[output file]`: This is the file that will store the output from the command  
    

**Example Usage**

`/opt/john/rar2john rarfile.rar > rar_hash.txt`

## Cracking a SSH keys with john 

Originally to log in to the SSH we require the login password but we can also configure it to use the hash of the pass or the private key 

However doing so will often require a password to access the private key

we will use the tool ssh2john to break this password 

this can be found in the python3 /opt/john/ssh2john.py 
ssh2john [id_rsa private key file] > [output file]

