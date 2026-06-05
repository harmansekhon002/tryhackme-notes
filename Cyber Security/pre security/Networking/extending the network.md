## port forwarding
- to make a website publically available we need to implement port forwarding
- it is configured at the router of a network
## firewalls
- it is a device responsible for determining which traffic enters or exits the network
-  An administrator can configure a firewall to **permit** or **deny** traffic from entering or exiting a network based on numerous factors such as:

	- Where the traffic is coming from? (has the firewall been told to accept/deny traffic from a specific network?)
	- Where is the traffic going to? (has the firewall been told to accept/deny traffic destined for a specific network?)
	- What port is the traffic for? (has the firewall been told to accept/deny traffic destined for port 80 only?)
	- What protocol is the traffic using? (has the firewall been told to accept/deny traffic that is UDP, TCP or both?)

Firewalls perform packet inspection to determine the answers to these questions.
> a firewall might be a dedicated hardware or a software like snort

We'll cover the two primary categories of firewalls in the table below:

  

|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **FirewallCategory** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Stateful             | This type of firewall uses the entire information from a connection; rather than inspecting an individual packet, this firewall determines the behaviour of a device **based upon the entire connection**.<br><br>This firewall type consumes many resources in comparison to stateless firewalls as the decision making is dynamic. For example, a firewall could allow the first parts of a TCP handshake that would later fail.<br><br>If a connection from a host is bad, it will block the entire device.                                                                                                                                      |
| Stateless            | This firewall type uses a static set of rules to determine whether or not **individual packets** are acceptable or not. For example, a device sending a bad packet will not necessarily mean that the entire device is then blocked.<br><br>Whilst these firewalls use much fewer resources than alternatives, they are much dumber. For example, these firewalls are only effective as the rules that are defined within them. If a rule is not exactly matched, it is effectively useless.<br><br>However, these firewalls are great when receiving large amounts of traffic from a set of hosts (such as a Distributed Denial-of-Service attack) |
## VPN
- Virtual Private Network
- this is used to allow devices on separate networks to communicate securely by creating a dedicated path b/w each other known as a tunnel 
- within this tunnel devices form their own private network
- ![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/418b5637e02d3fd7494affc2e9cdcc86.svg)

- Let's cover some of the other benefits offered by a VPN in the table below:

  

|   |   |
|---|---|
|**Benefit**|**Description**|
|Allows networks in different geographical locations to be connected.|For example, a business with multiple offices will find VPNs beneficial, as it means that resources like servers/infrastructure can be accessed from another office.|
|Offers privacy.|VPN technology uses encryption to protect data. This means that it can only be understood between the devices it was being sent from and is destined for, meaning the data isn't vulnerable to sniffing.<br><br>This encryption is useful in places with public WiFi, where no encryption is provided by the network. You can use a VPN to protect your traffic from being viewed by other people.|
|Offers anonymity.|Journalists and activists depend upon VPNs to safely report on global issues in countries where freedom of speech is controlled.<br><br>Usually, your traffic can be viewed by your ISP and other intermediaries and, therefore, tracked. <br><br>The level of anonymity a VPN provides is only as much as how other devices on the network respect privacy. For example, a VPN that logs all of your data/history is essentially the same as not using a VPN in this regard.|
## LAN networking devices
### router
- it connects networks and passes data between them 
- operates at layer 3 of the osi model
- it creates a path or route for data to travel
- Different protocols will decide what path should be taken, but factors include:
	
	- What path is the shortest?
	
	- What path is the most reliable?
	
	- Which path has the faster medium (e.g. copper or fibre)?
### switch
- it is a device for connecting to multiple devices at the same time
- it operates at both layer 2 and 3 
- types of switches
	- type 2
	- type 3
- A technology called **VLAN** (**V**irtual **L**ocal **A**rea **N**etwork) allows specific devices within a network to be virtually split up. This split means they can all benefit from things such as an Internet connection but are treated separately. This network separation provides security because it means that rules in place determine how specific devices communicate with each other. This segregation is illustrated in the diagram below:

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/008ae2ff118eeb5680db5fa478fd925d.svg)
