- the open systems interconnection model
- it provides a framework on how network devices will send receive and interpret data
- ![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
- the process where on each layer some process takes place on the data and some more data is added upon our data is known as encapsulation
## physical layer 1
- the lowest layer
- used for referencing physical devices used in networking 
- devices use binary numbers to transmit the data
## data link layer 2
- this layer focuses on the physical addressing of the transmission (mac address) 
- it receives the the data packet and the ip address from the network layer and adds in the mac address  
- the data link layer does the job of presenting the data in a format suitable for transmission 
## network layer 3
- the protocols of this layer determine the optimal path for data transmission that is routing 
- these protocols are
	- OSPF (Open Shortest Path First) 
	- RIP (Routing Information Protocol)

The factors that decide what route is taken is decided by the following:

- What path is the shortest? I.e. has the least amount of devices that the packet needs to travel across.
- What path is the most reliable? I.e. have packets been lost on that path before?
- Which path has the faster physical connection? I.e. is one path using a copper connection (slower) or a fibre (considerably faster)?

- everything at this layer deals with ip addresses and routers 

## transport layer 4
- it helps in the transmission of the data
- data transmission usually follows one of the below written protocols 
	- TCP
	- UDP
		### TCP
		- Transmission Control Protocol
		- this protocol is for reliability and guarantee
		- it keeps a constant connection between the devices communicating 
		- it also has error checking incorporated into it 

|                                                                                          |                                                                                                                                                   |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Advantages of TCP**                                                                    | **Disadvantages of TCP  <br>**                                                                                                                    |
| Guarantees the accuracy of data.                                                         | Requires a reliable connection between the two devices. If one small chunk of data is not received, then the entire chunk of data cannot be used. |
| Capable of synchronising two devices to prevent each other from being flooded with data. | A slow connection can bottleneck another device as the connection will be reserved on the receiving computer the whole time.                      |
| Performs a lot more processes for reliability.                                           | TCP is significantly slower than UDP because more work has to be done by the devices using this protocol.                                         |
		- it is used for mainly file sharing internet browsing and sending an email because these services require the data to be complete and accurate
### UDP 
- user datagram protocol
- it is not as advanced as tcp
- there is no error checking 
- there is no synchronisation
- 

|                                                                                                                       |                                                                                    |
| --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **Advantages of UDP**                                                                                                 | **Disadvantages of UDP**                                                           |
| UDP is much faster than TCP.                                                                                          | UDP doesn't care if the data is received.                                          |
| UDP leaves the application layer (user software) to decide if there is any control over how quickly packets are sent. | It is quite flexible to software developers in this sense.                         |
| UDP does not reserve a continuous connection on a device as TCP does.                                                 | This means that unstable connections result in a terrible experience for the user. |
- it is used in situations where only small amounts of data is to be sent 
- like protocols like ARP and DHCP 
- it is also used for video streaming where data loss isn't all that important 
- pixelated videos are just lost data across the internet

## session layer 5
- this layer is used for establishing a connection or a session between the sending and the receiving endpoint
- if a connection isn't being used this layer will close the connection to maintain and save the resources 
- it also has the capability to make checkpoints 
- sessions are always unique

## presentation layer 6 
- standardisation occurs here 
- this layer is the translator for the different data travelling to and from the application layer
- features like HTTPS occurs here
## application layer 7 
- this layer is responsible for determining how the user will interact with the data sent or received
- everyday softwares like email clients browsers or file servers etc provide a friendly gui for us to interact with data sent or received
