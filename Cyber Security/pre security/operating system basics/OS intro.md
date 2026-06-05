## invisible manager
- the os sits between the physical hardware the users and the applications managing all these without being seen
## System Privilege Layers

Inside a modern computer, different parts of the system operate at various permission levels. Some components can communicate directly with the hardware, while regular applications run in a safer, restricted environment. This separation is intentional and helps prevent conflicts and security issues.

- **Kernel space**: The privileged, locked-down core of  fthe OS. This is where the kernel, the part of the operating system that directly manages hardware and system resources, runs. It has unrestricted access to the CPU, memory, storage, and all hardware components.
- **User space**: Where all standard applications run. Applications in the user space are deliberately prevented from accessing hardware directly. Whenever they need to open or save a file, play a sound, or connect to Wi-Fi, they must make a system call and request that the kernel act on their behalf.
## Operating System Security

It is important to understand that every OS also acts as a security foundation. Before any antivirus, firewall, or security tool is introduced, the OS is already enforcing protections in the background, some of which we covered above.

At a basic level, your operating system handles

- **Authentication**: Verifies who you are through login passwords and biometrics
- **Permissions**: Controls exactly what each user and app is allowed to read, write, or execute
- **Isolation**: Keeps every process in its own protected box (kernel/user space separation)
- **System Protection**: Safeguards critical system files
## Role of OS
1. user management
2. ram management 
3. file management
4. process management
5. device management
## os interfaces
types of interaction interfaces are:
1. GUI
2. CLI
## types of os 
1. desktop
2. server
3. mobile
4. embedded
5. virtual/cloud
**Desktop**

- **Windows**: The most widely used operating system on personal computers  
    _Windows 10 (end-of-life), Windows 11_
- **macOS**: Apple's desktop OS, known for its polished GUI and integration with other Apple devices  
    _Sonoma (14), Sequoia (15), Tahoe (26)_
- **Linux**: Not a single OS but a family of open-source operating systems called distributions  
    _Ubuntu, Debian, Fedora_
**Server**

- **Windows**: Used in large networks, data centres, and corporate environments  
    _Server 2016, 2019, 2022, 2025_
- **Linux**: The vast majority of web servers, trusted for its reliability and open-source nature  
    _Ubuntu Server, Debian, CentOS, Red Hat_
- **Unix**: Large enterprises, finance, telecom, government  
    _IBM AIX, Oracle Solaris_
**Mobile**

- **Android**: The most widely used mobile OS, which runs on phones, tablets, and smart devices  
    _Android 14 - 16, Manufacturer versions_
- **iOS**: Apple's mobile OS running on iPhones, iPads, and other devices  
    _iOS 17, 18, 26_

**Embedded and IoT Devices**

- **Embedded Linux**: Specialised OS built into devices with dedicated functions  
    _OpenWrt, Ubuntu Core, Yocto Project_
- **Real-Time OS**: Designed for apps where tasks need guaranteed response times (aircraft controls)  
    _FreeRTOS, VxWorks, QNX_

**Virtual and Cloud**

- **Cloud/VM**: Massive data centers that host websites, apps, and streaming services  
    _Ubuntu LTS, Amazon Linux, Rocky Linux_
- **Container-optimised**: Lightweight alternatives to VMs that package just the app and its dependencies  
    _Alpine Linux, Bottlerocket AWS, Flatcar Linux_
