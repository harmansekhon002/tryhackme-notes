![Client-server analogy](https://tryhackme-images.s3.amazonaws.com/user-uploads/66c44fd9733427ea1181ad58/room-content/66c44fd9733427ea1181ad58-1768925163790.png)

## Service, Client, Server

In our analogy, Bob and Alice choose to use the Pizza takeaway service. Alice is the client who passes her order to Bob, who then passes it to the Server at Luigi's Pizzas. In computer terms, we can translate this as Alice using, for example, a browser to navigate to a website. The browser is the **client** that requests the webpage, and the **server** is the system that serves it.

Note that the client is the one who always initiates the request.

## Request and Response

Alice requested a large pepperoni pizza from Luigi's. Be careful, this request was not made to Bob. Bob was the one who brought the request to Luigi's. What is important to realize is that if the request is not formatted correctly or the requested resource is unavailable, we will get an error response. For example, Bob returns to Alice with the message that there were no pepperoni pizzas or that the server did not understand the order.

In computer systems, we can say that Alice used a browser (the client) to request a webpage from a server, which then sent the webpage to the client.

## Protocol

One of the things we don't often realise, until we go on a trip to a foreign country, is our language. When Alice formulates her request to Bob, she uses the language Luigi's Pizza understands. Additionally, Alice uses the menu to determine which orders she can place. Bob understood the request (language and order) and went to Luigi's to bring it; he received a response he understood and brought it back to Alice.

We can compare Bob to a protocol that computer systems use to communicate. A protocol defines how a client can communicate with a server. This definition includes:

- Which commands do the client and server understand. E.g., the **get** command.
- How a request is structured. E.g., first the command and then the order.
- What syntax is used. E.g., Alice uses the English language.
- What response should be given to which type of request. E.g., a request for pizza results in receiving the available pizza.
- What response to give to faulty requests. E.g., the server at Luigi's Pizzas says: No pepereonni pizza available

## Port

A port is used to identify a specific service running on a system. When a client wants to access a service on a server, it must connect using the correct port.

Imagine that Luigi’s takeaway service requires customers to enter through a specific door. Everyone ordering takeaway uses the same door. In the same way, a service on a server listens on a specific port.

Now imagine that Luigi’s offers multiple services, such as takeaway, dining in, and delivery. Each service uses a different door: door A for takeaway, door B for the restaurant, and door C for delivery. Similarly, a single server can run multiple services at the same time, with each service identified by a different port.

## DNS

When Alice sent Bob her request, only the name of the pizza place was known. With that alone, it is not possible to reach the destination. So, Bob entered Luigi's Pizza into a GPS device, and the device returned the coordinates for Luigi's.

DNS stands for Domain Name Service and works similarly to GPS: when you enter the name of, for example, a website, DNS resolves it to server's location. These location coordinates are called an Internet Protocol (IP) address in computer terms. Imagine this IP as the address to your home (street name, house number, postal code, city, and country), but for computer systems.

Let's look at the next task to see how the client-server model applies when browsing a website.

## web communication 

Hypertext Transfer Protocol (Secure), abbreviated as HTTP(S), is a stateless client-server protocol used for the World Wide Web. This means that each request is processed independently, without the server retaining information about previous requests.

Although the protocol itself is stateless, modern websites and web applications implement mechanisms to introduce statefulness at the application level.  
For example, when you log into a website using your credentials, the server creates a session identifier (often stored in a cookie or token) that is sent with each subsequent request.  
Without these mechanisms, you would need to authenticate again with every new request, because the server would have no memory of your previous login.

## HTTP Commands

In the main specifications that define HTTP (also called Request for Comments, or RFC documents), there are 9 core commands. In HTTP lingo, we use the term method instead of command. Below you can see an overview of these methods:

- GET
- POST
- PUT
- DELETE
- PATCH
- HEAD
- OPTIONS
- CONNECT
- TRACE

on the dev tools in firefox in the network tab on any get request 

- **Scheme**: Tells us which protocol was used: HTTP or HTTPS.
- **Host**: Tells us the name of the host we request resources from.
- **Filename**: Indicates which file we requested from the host. In our request, this is "/", which actually translates to "index.html".
- **Address**: Displays the IP address where the website is hosted. In our example, we are hosting the website on the same device. That's why the address 127.0.0.1 is shown.
- **Status**: This field indicates whether the request was successful. In our example, we received a "200 OK" status, which means that the request was successful.

When a request is sent, we will get a response from the server. The response is divided into two parts: the response header and the response body. The response header contains metadata about the response, while the response body contains the requested content.