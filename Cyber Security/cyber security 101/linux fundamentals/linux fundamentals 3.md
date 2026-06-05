# text editors
## nano
It is easy to get started with Nano! To create or edit a file using nano, we simply use `nano filename` -- replacing "filename" with the name of the file you wish to edit.

Introducing Nano

```shell-session
tryhackme@linux3:/tmp# nano myfile
  GNU nano 4.8                                             myfile                                                       

^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo       M-A Mark Text
^X Exit        ^R Read File   ^\ Replace     ^U Paste Text  ^T To Spell    ^_ Go To Line  M-E Redo       M-6 Copy Text
```

Once we press enter to execute the command, `nano` will launch! Where we can just begin to start entering or modifying our text. You can navigate each line using the "up" and "down" arrow keys or start a new line using the "Enter" key on your keyboard.

Using Nano to write text

```shell-session
tryhackme@linux3:/tmp# nano myfile
  GNU nano 4.8                                             myfile                                             Modified  

Hello TryHackMe
I can write things into "myfile"


^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo       M-A Mark Text
^X Exit        ^R Read File   ^\ Replace     ^U Paste Text  ^T To Spell    ^_ Go To Line  M-E Redo       M-6 Copy Text
```

Nano has a few features that are easy to remember & covers the most general things you would want out of a text editor, including:

- Searching for text
- Copying and Pasting  
    
- Jumping to a line number
- Finding out what line number you are on

You can use these features of nano by pressing the "**Ctrl**" key (which is represented as an `^` on Linux)  and a corresponding letter. For example, to exit, we would want to press "**Ctrl**" and "**X**" to exit Nano.

## VIM

VIM is a much more advanced text editor. Whilst you're not expected to know all advanced features, it's helpful to mention it for powering up your Linux skills.

![](https://assets.tryhackme.com/additional/linux-fundamentals/part3/vim1.png)  

Some of VIM's benefits, albeit taking a much longer time to become familiar with, includes:

- Customisable - you can modify the keyboard shortcuts to be of your choosing
- Syntax Highlighting - this is useful if you are writing or maintaining code, making it a popular choice for software developers
- VIM works on all terminals where nano may not be installed
- There are a lot of resources such as [cheatsheets(opens in new tab)](https://vim.rtorr.com/), tutorials, and the sorts available to you use.

**Downloading Files (Wget)**

A pretty fundamental feature of computing is the ability to transfer files. For example, you may want to download a program, a script, or even a picture. Thankfully for us, there are multiple ways in which we can retrieve these files.

 We're going to cover the use of `wget` .  This command allows us to download files from the web via HTTP -- as if you were accessing the file in your browser. We simply need to provide the address of the resource that we wish to download. For example, if I wanted to download a file named "myfile.txt" onto my machine, assuming I knew the web address it -- it would look something like this:

`wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt`

**Transferring Files From Your Host - SCP (SSH)**

Secure copy, or SCP, is just that -- a means of securely copying files. Unlike the regular cp command, this command allows you to transfer files between two computers using the SSH protocol to provide both authentication and encryption.

Working on a model of SOURCE and DESTINATION, SCP allows you to:

- Copy files & directories from your current system to a remote system
- Copy files & directories from a remote system to your current system

Provided that we know usernames and passwords for a user on your current system and a user on the remote system. For example, let's copy an example file from our machine to a remote machine, which I have neatly laid out in the table below:

|   |   |
|---|---|
|**Variable**|**Value**|
|The IP address of the remote system|192.168.1.30|
|User on the remote system|ubuntu|
|Name of the file on the local system|important.txt|
|Name that we wish to store the file as on the remote system|transferred.txt|

With this information, let's craft our `scp` command (remembering that the format of SCP is just SOURCE and DESTINATION)

`scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`

And now let's reverse this and layout the syntax for using `scp` to copy a file from a remote computer that we're not logged into 

|   |   |
|---|---|
|**Variable**|**Value**|
|IP address of the remote system|192.168.1.30|
|User on the remote system|ubuntu|
|Name of the file on the remote system|documents.txt|
|Name that we wish to store the file as on our system|notes.txt|

The command will now look like the following: `scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt`g

# Processes 101 — Linux Fundamentals

## What is a Process?
Every single program running on Linux is a **process**.
- Opening a browser → process
- Background service → process
- Your terminal → process

---

## PID — Process ID
Every process gets a unique number called a **PID (Process ID)**

| Process | PID |
|---------|-----|
| systemd | 1 |
| Firefox | 1234 |
| Terminal | 5678 |
| SSH | 9101 |

---

## PID 1 — The Most Important Process
- PID 1 = **systemd** (modern Linux) or **init** (older Linux)
- First process Linux starts
- **Parent of ALL other processes**
- If PID 1 dies → whole system crashes
- The only process with NO parent

---

## Parent & Child Processes
````
Parent Process (PID 1)
       ↓ creates
Child Process (PID 2)
       ↓ creates
Grandchild Process (PID 3)
````

- Process that creates another = **parent**
- Newly created process = **child**
- Every process has a parent except PID 1

---

## Process States

| State | Symbol | Meaning |
|-------|--------|---------|
| Running | R | Currently executing |
| Sleeping | S | Waiting for something |
| Stopped | T | Paused/suspended |
| Zombie | Z | Finished but not cleaned up |

---

## Key Commands — Viewing Processes
````bash
# See basic running processes
ps

# See ALL processes with full details
ps aux

# Live updating process viewer
top

# Better version of top (more visual)
htop
```

### Understanding ps aux Output:
```
USER    PID  %CPU %MEM    COMMAND
root      1   0.0  0.1    systemd
john   1234   2.1  0.5    firefox
john   5678   0.1  0.1    bash
````

| Column | Meaning |
|--------|---------|
| USER | Who owns the process |
| PID | Process ID number |
| %CPU | CPU usage percentage |
| %MEM | Memory usage percentage |
| COMMAND | Program running |

---

## Key Commands — Managing Processes
````bash
# Polite kill — process can clean up
kill 1234
# or
kill -15 1234

# Force kill — no cleanup, instant
kill -9 1234

# Kill process by name
pkill firefox

# Run process in background
command &

# See background jobs
jobs

# Bring background job to foreground
fg
````

---

## Signals

| Signal | Number | What it Does |
|--------|--------|-------------|
| SIGTERM | 15 | Politely ask process to stop |
| SIGKILL | 9 | Forcefully kill immediately |
| SIGSTOP | 19 | Pause the process |
| SIGCONT | 18 | Resume paused process |

> SIGTERM = "please stop" → SIGKILL = "pulling the plug"

---

## Foreground vs Background

### Foreground
- Running visibly in terminal
- Terminal locked until it finishes
````bash
ping google.com  # terminal is locked
````

### Background
- Running invisibly behind the scenes
- Terminal stays free
````bash
ping google.com &  # & sends to background
jobs               # see background jobs
fg                 # bring back to foreground
````

---

## Systemd & Services — systemctl
````bash
# Start a service
systemctl start ssh

# Stop a service
systemctl stop ssh

# Restart a service
systemctl restart ssh

# Make service start automatically on boot
systemctl enable ssh

# Stop service from starting on boot
systemctl disable ssh

# Check service status
systemctl status ssh
````

---

## Cron Jobs — Scheduled Processes

Processes that run automatically on a schedule
````bash
# View cron jobs
crontab -l

# Edit cron jobs
crontab -e
```

### Cron Timing Format:
```
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of week (0-7)
│ │ │ └──── Month (1-12)
│ │ └────── Day of month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)
````

### Examples:
````bash
# Every day at midnight
0 0 * * * /backup.sh

# Every 5 minutes
*/5 * * * * /script.sh

# Every Monday at 9am
0 9 * * 1 /weekly.sh
````

---

## Why Processes Matter in Cybersecurity

| Scenario | Relevance |
|----------|-----------|
| Privilege Escalation | Finding vulnerable running processes |
| Persistence | Hiding malicious processes |
| Forensics | Identifying malicious processes |
| CTFs | Finding flags in running processes |
| Malware Analysis | Spotting suspicious processes |

---

## Quick Reference — Most Used Commands
````bash
ps aux              # list all processes
top                 # live process viewer
htop                # better live viewer
kill PID            # polite stop
kill -9 PID         # force stop
pkill name          # kill by name
command &           # run in background
jobs                # see background jobs
fg                  # bring to foreground
systemctl start X   # start service
systemctl stop X    # stop service
systemctl enable X  # start on boot
crontab -l          # view scheduled tasks
crontab -e          # edit scheduled tasks
````

---

## Key Concepts Summary

> **Process** = any running program
> **PID** = unique ID for each process
> **PID 1** = systemd = parent of everything
> **SIGTERM (15)** = polite stop
> **SIGKILL (9)** = force stop
> **&** = run in background
> **systemctl** = manage services
> **crontab** = schedule processes

## Automation
- we can use cron which is a linux tool to schedule tasks and actions
- We're going to be talking about the `cron` process, but more specifically, how we can interact with it via the use of `crontabs` . 
- Crontab is one of the processes that is started during boot, which is responsible for facilitating and managing cron jobs.

- A crontab is simply a special file with formatting that is recognised by the `cron` process to execute each line step-by-step. Crontabs require 6 specific values:

|   |   |
|---|---|
|Value|Description|
|MIN|What minute to execute at|
|HOUR|What hour to execute at|
|DOM|What day of the month to execute at|
|MON|What month of the year to execute at|
|DOW|What day of the week to execute at|
|CMD|The actual command that will be executed.|

  

Let's use the example of backing up files. You may wish to backup "cmnatic"'s  "Documents" every 12 hours. We would use the following formatting: 

`0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/`

- Crontabs can be edited by using `crontab -e`, where you can select an editor (such as Nano) to edit your crontab.
- we can also use specific keywords like @reboot @daily @monthly @ weekly etc for the schedule

# Managing & Removing Repositories — Linux Fundamentals 3

## Key Terms

| Term | Meaning |
|------|---------|
| **Package** | A software program ready to install |
| **Repository** | Online store where packages live |
| **APT** | Package manager for Ubuntu/Debian Linux |
| **GPG Key** | Security key proving a repository is trusted |

---

## Important File Location
```bash
/etc/apt/sources.list  # Where all repositories are stored
```

---

## APT — Core Commands
```bash
# Update package lists from repositories
apt update

# Install a package
apt install packagename

# Remove package (keeps config files)
apt remove packagename

# Remove package AND config files
apt purge packagename

# Remove unused leftover dependencies
apt autoremove

# Search for a package
apt search packagename
```

---

## Adding a Repository
```bash
# Step 1 — Add GPG security key
curl -sL https://example.com/key.gpg | apt-key add -

# Step 2 — Add repository
add-apt-repository "deb https://example.com/repo stable main"

# Step 3 — Update so APT recognises new repo
apt update

# Step 4 — Install from new repo
apt install packagename
```

---

## Removing a Repository
```bash
# Method 1 — Using command
add-apt-repository --remove "deb https://example.com/repo stable main"

# Method 2 — Manual edit
nano /etc/apt/sources.list
# Delete the repository line manually

# Always update after removing
apt update
```

---

## Remove vs Purge

| Command | Removes Software | Removes Config Files |
|---------|-----------------|---------------------|
| `apt remove` | ✅ | ❌ |
| `apt purge` | ✅ | ✅ |

> Always use **purge** for completely clean removal

---

## Why This Matters in Cybersecurity

- **Attacker persistence** → Attackers add malicious repos to keep reinstalling tools
- **Forensics** → Check sources.list for unauthorised repositories
- **Hardening** → Remove unnecessary packages to reduce attack surface
- **CTFs** → Installing tools mid-challenge using apt

---

## Quick Reference
```bash
apt update                    # refresh package lists
apt install packagename       # install software
apt remove packagename        # remove software
apt purge packagename         # remove + config files
apt autoremove               # clean unused dependencies
cat /etc/apt/sources.list    # view repositories
nano /etc/apt/sources.list   # edit repositories
```

---

## Key Concept Summary

> **Repository** = online app store for Linux
> **APT** = the tool that manages packages
> **sources.list** = file storing all repo addresses
> **GPG Key** = proof a repository is safe
> **purge** = cleanest way to remove software

# managing logs

-  Located in the /var/log directory, these files and folders contain logging information for applications and services running on your system. 
- The Operating System  (OS) has become pretty good at automatically managing these logs in a process that is known as "rotating".

I have highlighted some logs from three services running on a Ubuntu machine:
- An Apache2 web server
- Logs for the fail2ban service, which is used to monitor attempted brute forces, for example
- The UFW service which is used as a firewall
Tags: #linux #repositories #apt #packages #tryhackme #linuxfundamentals3
