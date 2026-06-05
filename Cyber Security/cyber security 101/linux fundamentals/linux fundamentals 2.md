
**What is SSH & how Does it Work?**

- Secure Shell or SSH simply is a protocol between devices in an encrypted form. 
- Using cryptography, any input we send in a human-readable format is encrypted for travelling over a network -- where it is then unencrypted once it reaches the remote machine
- we can connect to a remote device using the ssh command for which we need the username password and the ip of the device 
- ls -a to display all the files including the hidden files
- -help to get a formatted version of the man page
- man -command to get the info about that command 

More specifically, the following commands:

|   |   |   |
|---|---|---|
|Command|Full Name|Purpose|
|touch|touch|Create file|
|mkdir|make directory|Create a folder|
|cp|copy|Copy a file or folder|
|mv|move|Move a file or folder|
|rm|remove|Remove a file or folder|
|file|file|Determine the type of a file|

_Protip: Similarly to using cat, we can provide full file paths, i.e. directory1/directory2/note for all of these commands_

- cp source file destination file 
- mv same
- login -l or -login or - can be used to do a full switch and switch to users home directory 
- ls -l shows full info of a file along with the owner permissions and others
## permissions
different users might have diff permissions like
- r read
- w write
- x execute
each permission has a numeric value 
- r 4
- w 2
- x 1
these permission come in groups like
rwx-r-rw
7-4-6
# important files and folders
- /etc contains important system files and folders
		- sudoers list of users and groups that have permission to run sudo 
		- passwd contains passwords in encrypted form known as sha512
- /var it contains data of the apps or services running on the system that needs to be frequently accessed
- /root it contains the data of the root or the home user
- /tmp it contains temporary data that gets deleted after the machine restarts and is only used once or twice