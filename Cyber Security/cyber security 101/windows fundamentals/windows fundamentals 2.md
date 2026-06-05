## msconfig
- the system configuration utility is for advanced troubleshooting and its main purpose is to help diagnose startup issues
- The utility has five tabs across the top. Below are the names for each tab. We will briefly cover each tab in this task. 

1. General
2. Boot
3. Services
4. Startup
5. Tools
## change UAC 
Four levels
1. always notify
2. notify for apps
3. notify without dimming 
4. never notify
## computer management
- the code is compmgmt
	- this utility has three sections system tools storage and service and applications
- we can use the task scheduler to schedule simple or complex tasks on a pc
- Event Viewer has three panes.

1. The pane on the left provides a hierarchical tree listing of the event log providers. (as shown in the image above)
2. The pane in the middle will display a general overview and summary of the events specific to a selected provider.
3. The pane on the right is the actions pane.

There are five types of events that can be logged. Below is a table from [docs.microsoft.com(opens in new tab)](https://docs.microsoft.com/en-us/windows/win32/eventlog/event-types) providing a brief description for each.

![](https://assets.tryhackme.com/additional/win-event-logs/five-event-types.png)

The standard logs are visible under Windows Logs. Below is a table from [docs.microsoft.com(opens in new tab)](https://docs.microsoft.com/en-us/windows/win32/eventlog/eventlog-key) providing a brief description for each.

![](https://assets.tryhackme.com/additional/win-event-logs/standard-event-logs.png)

a service is a special type of application that runs in the background
WMI Control configures and controls the **Windows Management Instrumentation** (WMI) service.

## system information

Windows includes a tool called Microsoft System Information (Msinfo32.exe).  This tool gathers information about your computer and displays a comprehensive view of your hardware, system components, and software environment, which you can use to diagnose computer issues

The  information in **System Summary** is divided into three sections:

- **Hardware Resources**
- **Components**
- **Software Environment**

## windows registry
The registry contains information that Windows continually references during operation, such as:

- Profiles for each user
- Applications installed on the computer and the types of documents that each can create
- Property sheet settings for folders and application icons
- What hardware exists on the system
- The ports that are being used.