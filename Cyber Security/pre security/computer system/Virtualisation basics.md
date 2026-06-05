- earlier there was the idea that one application should run on one single server i e one server = one application 
- problems with this approach
		1. high cost
		2. low utilization
		3. slow deployment
		4. hard to scale
- a virtualization layer called the hypervisor was introduced to act as a referee between virtual machines and allow each computer to behave independently like a physical computer
## building analogy

Imagine if one single person lives alone in an entire 10-floor building:

- The person uses only one floor but must maintain the entire building: electricity, cleaning, water, and security.
- Most of the building stays empty and wasted.
- It’s expensive, inefficient, and unnecessary for his needs.

Now imagine dividing the building into **separate apartments**:

- Each apartment has its own door, walls, kitchen, and privacy.
- Different people can live independently without bothering each other.
- They all share the building’s main structure: electricity, water, and elevators, making it **cheaper and more efficient** for everyone.
- as such we can say that the building manager is the ***hypervisor***
## components of virtualization
1. hypervisor (the manager)
		its the software that creates and manages the virtual machines
		its a special piece of software that 
		- divides a physical computer into multiple virtual machines 
		- gives each virtual machine its own set of resources
		- keeps everything isolated and safe
		- manages the lifecycle of virtual machines (start stop clone pause delete)
	types of hypervisor
	1. hypervisor runs directly on the physical hardware making them fast efficient and ideal for server and professional envs
	2. hypervisor runs without an existing os making them easier to install and ideal for learning testing and small setups
- When using virtualization to test malicious files, care should be taken to ensure that the host machine does not become infected by the malware being tested in the guest machine.
- One approach is to use different operating systems for the guest and host machines, or to isolate the guest machine so that it does not communicate with the host.
## Virtual machines (the apartments)
- its a computer created by the hypervisor 
- complete isolation, independent set of resources, and can run any os 
- You can deploy VMs on your own computer using tools such as Oracle VirtualBox and VMware Workstation. 
- This type of software acts as a type 2 hypervisor and lets you run multiple operating systems, such as Windows, Linux, and macOS.
- -You need to work on a different OS like Kali Linux, but you can't buy another whole system, so you install a hypervisor and run a Kali Linux VM on it.
- You want to test whether a file is malicious, so you set up an isolated virtual machine to protect your main computer from being infected.
## containers
- A **container** is a lightweight, isolated environment that runs a single application and all the necessary components to support it. 
- Instead of bringing a whole separate operating system, a container borrows the core of the existing system by running on the kernel (the part of an operating system that communicates with the hardware and manages resources such as memory and running programs.)
- ***Because containers share this kernel, they start quickly and use fewer resources than full virtual machines, but it also means they must match the host system’s type. For example, you can’t run a Windows container on a Linux machine.***
 Containers behave like small, self-contained spaces because:
- They package the application and its dependencies (libraries, tools, versions).
- They share the host’s operating system, so they start almost instantly.
- They remain isolated from each other, so a misbehaving container doesn’t affect the others.
- They can run consistently on any machine, making them perfect for development, testing, and scalable deployments.
- The easiest way to deploy containers in a VM is using Docker.  
Docker is an open-source software platform that simplifies the process of building, deploying, and running applications using containerisation.
- In summary, VMs provide the “full apartment” with maximum separation and flexibility, while containers offer lightweight “rooms” ideal for scalable, fast-deploying applications.
## managing virtual machines
### analysing vm states
In the home screen, you should see three main sections:

- **Summary:** Provides a generic view of the state of the environment.
- **Virtual Machines:** Provides details of each VM and enables you to run actions on them.
- **Hosts:** Displays usage and performance details for each physical server.
