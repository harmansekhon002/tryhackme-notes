- to ensure CIA we need to use networking secure protocols like HTTPS SSH and TLS etc
## TLS
- First the SSL was developed in 1990s (secure socket layer) its successor was SSL 2.0 
- then the internet engineering task force (IETF) developed TLS as a successor to TLS 3.0 
- it is a cryptographic protocol operating at the osi models transport layer 
### steps
- the first step for any web server or client is to get a TLS certification
- Generally, the server administrator creates a Certificate Signing Request (CSR) and submits it to a Certificate Authority (CA)
- the CA verifies the CSR and issues a digital certificate. 
- Once the (signed) certificate is received, it can be used to identify the server (or the client) to others, who can confirm the validity of the signature. 
- For a host to confirm the validity of a signed certificate, the certificates of the signing authorities need to be installed on the host. 
- In the non-digital world, this is similar to recognising the stamps of various authorities. The screenshot below shows the trusted authorities installed in a web browser.

## HTTP
- HTTP works over TCP
- lets say for a HTTP request we will have the following packets
- 3 packets for the TCP handshake
- 1 packet for the GET request
- then the required packets for data transmission
- then 3 packets for TCP termination
## HTTPS
- basically http over tls
- requesting a page over HTTPS will require the following three steps (after resolving the domain name):

1. Establish a TCP three-way handshake with the target server
2. Establish a TLS session
3. Communicate using the HTTP protocol; for example, issue HTTP requests, such as `GET / HTTP/1.1`

a TCP session is established in the first three packets, marked with `1`. 
Then, several packets are exchanged to negotiate the TLS protocol, marked with `2`. `1` and `2`
are where the **TLS negotiation and establishment** take place.
Finally, HTTP application data is exchanged, marked with `3`
## the encryption key 
- the key to decrypt the message 
- after using the key the tls and the tcp handshakes remain the same 
- however this time we can see when the GET request is issued
## SMTPS POP3S IMAPS
- The secure versions, i.e., over TLS, use the following TCP port numbers by default:

|Protocol|Default Port Number|
|---|---|
|HTTPS|443|
|SMTPS|465 and 587|
|POP3S|995|
|IMAPS|993|

TLS can be added to many other protocols; the reasoning and advantages would be similar.

## SSH
- Tatu Ylönen developed the Secure Shell (SSH) protocol and released SSH-1 in **1995** as freeware.

- OpenSSH offers several benefits. We will list a few key points:

- **Secure authentication**: Besides password-based authentication, SSH supports public key and two-factor authentication.
- **Confidentiality**: OpenSSH provides end-to-end encryption, protecting against eavesdropping. Furthermore, it notifies you of new server keys to protect against man-in-the-middle attacks.
- **Integrity**: In addition to protecting the confidentiality of the exchanged data, cryptography also protects the integrity of the traffic.
- **Tunneling**: SSH can create a secure “tunnel” to route other protocols through SSH. This setup leads to a VPN-like connection.
- **X11 Forwarding**: If you connect to a Unix-like system with a graphical user interface, SSH allows you to use the graphical application over the network.

- The screenshot below shows an example of running Wireshark on a remote Kali Linux system. The argument `-X` is required to support running graphical interfaces, for example, `ssh 192.168.124.148 -X`. (The local system needs to have a suitable graphical system installed.)

![After establishing an SSH connection to a remote server, we successfully started Wireshark, an application with a graphical user interface.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1721903514417.png)

While the TELNET server listens on port 23, the SSH server listens on port 22.

## SFTP FTPS
- SFTP is secureshell file transfer protocol
- same port number 22
-  Once logged in, you can issue commands such as `get filename` and `put filename` to download and upload files, respectively. Generally speaking, SFTP commands are Unix-like and can differ from FTP commands.
- While FTP uses port 21, FTPS usually uses port 990. It requires certificate setup, and it can be tricky to allow over strict firewalls as it uses separate connections for control and data transfer.

## VPN virtual private network
- we can access the web server of a company through a vpn by creating a tunnel for our traffic 
- Almost all companies require “private” information exchange in their virtual network. So, a VPN provides a very convenient and relatively inexpensive solution. The main requirements are Internet connectivity and a VPN server and client.

Once a VPN tunnel is established, all our Internet traffic will usually be routed over the VPN connection, i.e. via the VPN tunnel. Consequently, when we try to access an Internet service or web application, they will not see our public IP address but the VPN server’s. This is why some Internet users connect over VPN to circumvent geographical restrictions. Furthermore, the local ISP will only see encrypted traffic, which limits its ability to censor Internet access.

Finally, although in many scenarios, one would establish a VPN connection to route all the traffic over the VPN tunnel, some VPN connections don’t do this. The VPN server may be configured to give you access to a private network but not to route your traffic. Furthermore, some VPN servers leak your actual IP address, although they are expected to route all your traffic over the VPN. Depending on why you are using a VPN connection, you might need to run a few more tests, such as a DNS leak test.