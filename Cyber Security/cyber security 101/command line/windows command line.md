- command line interface are basic command or text based systems 

There are many other advantages to using a CLI besides speed and efficiency. We will mention a few:

- **Lower resource usage**: CLIs require fewer system resources than graphics-intensive GUIs. In other words, you can run your CLI system on older hardware or systems with limited memory. If you are using cloud computing, your system will require lower resources, which in turn will lower your bill.
- **Automation**: While you can automate GUI tasks, creating a batch file or script with the commands you need to repeat is much easier.
- **Remote management**: CLI makes it very convenient to use SSH to manage a remote system such as a server, router, or an IoT device. This approach works well on slow network speeds and systems with limited resources.

## commands
- set The `set` command is used to **display, create, or modify environment variables** in the current CMD session or to check the path from the command line
- ver this command is used to determine the current os version
- systeminfo to show the info of the system 
- help to the see the info of a command
- cls to clear the current screen
## network commands
- ipconfig to the network info ipconfig /all for more info
- ping is used to send a packet to a certain target and know if we can connect to the target from our machine
- `tracert`, which stands for _trace route_. The command `tracert target_name` traces the network route traversed to reach the target. Without getting into more details, it expects the routers on the path to notify us if they drop a packet because its time-to-live (TTL) has reached zero.
- nslookup target will get us the address of the website through the use of the dns
- `netstat`This command displays current network connections and listening ports
	- `-a` displays all established connections and listening ports
	- `-b` shows the program associated with each listening port and established connection
	- `-o` reveals the process ID (PID) associated with the connection
	- `-n` uses a numerical form for addresses and port numbers
## disk and storage commands
- cd to see the current dir
- dir to see the list of the available dir
	- - `dir /a` - Displays hidden and system files as well.
	- `dir /s` - Displays files in the current directory and all subdirectories.
- tree You can type `tree` to visually represent the child directories and subdirectories.
- cd target to change the dir 
- cd.. to go back one level
- mkdir directory_name to create dir
- rmdir target to remove dir
- type to display the content of a text file
- more to display more of the data
- copy to copy data
- move to move data
- del or erase to delete a file
- We can use the wildcard character `*` to refer to multiple files. For example, `copy *.md C:\Markdown` will copy all files with the extension `md` to the directory `C:\Markdown`.
## task and process managent commands
- tasklist to show the running tasks
	- Let’s say that we want to search for tasks related to `sshd.exe`, we can do that with the command `tasklist /FI "imagename eq sshd.exe"`. Note that `/FI` is used to set the filter _image name equals_`sshd.exe`.
	- With the process ID (PID) known, we can terminate any task using `taskkill /PID target_pid`. For example, if we want to kill the process with PID `4567`, we would issue the command `taskkill /PID 4567`.
- `chkdsk`: checks the file system and disk volumes for errors and bad sectors.
- `driverquery`: displays a list of installed device drivers.
- `sfc /scannow`: scans system files for corruption and repairs them if possible.