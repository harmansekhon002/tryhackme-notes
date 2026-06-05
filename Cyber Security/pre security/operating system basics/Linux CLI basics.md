The **terminal** is a text-based interface for controlling a Linux system. Instead of interacting with the graphical interface, you type commands that tell the computer exactly what to do. Cyber security professionals use it because:

- It's faster than clicking around
- It gives more control
- Many security tools only run in the terminal
# list of commands
***pwd***- present working directory
***ls*** - list
***ls - l*** - list with more details
***ls - al*** - all hidden files
***cd directory*** - go to directory or change directory
***cd..*** - go back one level
***find*** <starting_point> -name <filename> - ~ is for home dir
then the name of the file to search
cat <name of file>- to read any file on pc 
whoami - current logged in user
uname -a - full info of the current system
df -h for disk space 
The `-h` means "human readable"; it shows sizes like 2G or 500M instead of long bytes-only numbers.

**Breakdown of the Information**

- `/dev/root` is the main disk of the system with **--G total**, **12G used**, **<REDACTED>G free**, and is **17% full**.
- `tmpfs` entries are temporary filesystems stored in **RAM**, not on the physical disk.
- `/dev/shm` is a shared memory area with **1.9G** available and **0 used**.
- `/run/user/114` is similar temporary storage for another system user, also **387M total**and mostly empty.

linux stores configurational and informational files in the /etc 