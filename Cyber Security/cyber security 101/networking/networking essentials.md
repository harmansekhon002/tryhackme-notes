## DHCP dynamic host configuration protocol

-  DHCP is an application-level protocol that relies on UDP; the server listens on UDP port 67, and the client sends from UDP port 68. 
- **DHCP automatically gives devices their network settings** when they connect to a network.

-  Without DHCP, you’d have to manually assign an [IP address](chatgpt://generic-entity?number=1) to every device.
- Your smartphone and laptop are configured to use DHCP by default.

- DHCP follows four steps: Discover, Offer, Request, and Acknowledge (DORA):

1. **DHCP Discover**: The client broadcasts a DHCPDISCOVER message seeking the local DHCP server if one  exists.
2. **DHCP Offer**: The server responds with a DHCPOFFER message with an IP address available for the client to accept.
3. **DHCP Request**: The client responds with a DHCPREQUEST message to indicate that it has accepted the offered IP.
4. **DHCP Acknowledge**: The server responds with a DHCPACK message to confirm that the offered IP address is now assigned to this client.

## ARP address resolution protocol

- Address Resolution Protocol (ARP) makes it possible to find the MAC address of another device on the Ethernet
- If we use `tcpdump`, the packets will be displayed differently. It uses the terms ARP **Request** and ARP **Reply**
- An ARP Request or ARP Reply is not encapsulated within a UDP or even IP packet; it is encapsulated directly within an Ethernet frame
- ARP is considered layer 2 because it deals with MAC addresses

## ICMP internet control message protocol

- this protocol is used for mainly error reporting and network diagnostics
- Two popular commands rely on ICMP, and they are instrumental in network troubleshooting and network security. The commands are:

	- `ping`: This command uses ICMP to test connectivity to a target system and measures the round-trip time (RTT). In other words, it can be used to learn that the target is alive and that its reply can reach our system.
	- `traceroute`: This command is called `traceroute` on Linux and UNIX-like systems and `tracert` on MS Windows systems. It uses ICMP to discover the route from your host to the target.
## ping
-  The `ping` command sends an ICMP Echo Request (ICMP Type `8`)
- The computer on the receiving end responds with an ICMP Echo Reply (ICMP Type `0`)
- Many things might prevent us from getting a reply.
- In addition to the possibility of the target system being offline or shut down, a firewall along the path might block the necessary packets for `ping` to work. 
- In the example below, we used `-c 4` to tell the `ping` command to stop after sending four packets.
- Terminal

```shell-session
user@TryHackMe$ ping 192.168.11.1 -c 4
PING 192.168.11.1 (192.168.11.1) 56(84) bytes of data.
64 bytes from 192.168.11.1: icmp_seq=1 ttl=63 time=11.2 ms
64 bytes from 192.168.11.1: icmp_seq=2 ttl=63 time=3.81 ms
64 bytes from 192.168.11.1: icmp_seq=3 ttl=63 time=3.99 ms
64 bytes from 192.168.11.1: icmp_seq=4 ttl=63 time=23.4 ms

--- 192.168.11.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 3.805/10.596/23.366/7.956 ms
```

## Traceroute

- The Internet protocol has a field called Time-to-Live (TTL) that indicates the maximum number of routers a packet can travel through before it is dropped
- The router decrements the packet’s TTL by one before it sends it across
- When the TTL reaches zero, the router drops the packet and sends an ICMP Time Exceeded message (ICMP Type `11`). (In this context, “time” is measured in the number of routers, not seconds.)
- Some routers don’t respond; in other words, they drop the packet without sending any ICMP messages. 
- Routers that belong to our ISP might respond, revealing their private IP address. 
- Moreover, some routers respond and show their public IP address, and this would let us look up their domain name and discover their geographic location.
- Finally, there is always a possibility that an ICMP Time Exceeded message gets blocked and never reaches us.
- The traversed route might change as we rerun the command.

## Routing 
 - it is the process of deciding which path to take for the data to travel connecting the device and a user

There are many types of routing algos here are the briefs of some of them 

- OSPF(open shortest path first)
	OSPF is a routing protocol that allows routers to share information about the network topology and calculate the most efficient paths for data transmission. It does this by having routers exchange updates about the state of their connected links and networks. This way, each router has a complete map of the network and can determine the best routes to reach any destination.
- - **EIGRP (Enhanced Interior Gateway Routing Protocol)**: 
	EIGRP is a Cisco proprietary routing protocol that combines aspects of different routing algorithms. It allows routers to share information about the networks they can reach and the cost (like bandwidth or delay) associated with those routes. Routers then use this information to choose the most efficient paths for data transmission.
- - **BGP (Border Gateway Protocol)**: 
	BGP is the primary routing protocol used on the Internet. It allows different networks (like those of Internet Service Providers) to exchange routing information and establish paths for data to travel between these networks. BGP helps ensure data can be routed efficiently across the Internet, even when traversing multiple networks.
- - **RIP (Routing Information Protocol)**: 
	RIP is a simple routing protocol often used in small networks. Routers running RIP share information about the networks they can reach and the number of hops (routers) required to get there. As a result, each router builds a routing table based on this information, choosing the routes with the fewest hops to reach each destination.

## NAT network address translation
## **What it does**
- Converts **private IP addresses → public IP address**
    
- Allows multiple devices to share one internet connection
## **Why NAT is used**

- Saves public IP addresses
    
- Hides internal network (basic security)
    
- Allows many devices to access internet
## **How NAT works**

1. Device sends packet with private IP
    
2. Router replaces it with public IP
    
3. Router stores mapping in NAT table
    
4. Response comes back to router
    
5. Router forwards to correct device
## **Example**

- Laptop → 192.168.1.3
    
- Router (public IP) → 49.x.x.x
    
- Internet sees only router IP
## **NAT Table**
Stores mapping of:

- Private IP
    
- Port number
    
- Public IP
## **Important Points**

- NAT happens on router
    
- Uses port numbers to track devices
    
- Internal IPs are hidden
    
- Used in home, college, office networks

