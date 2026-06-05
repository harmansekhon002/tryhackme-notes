1. Hyper Text Transfer Protocol
2. it was developed by tim berner's lee in 1989 1991
3. it is basically a set of rules for communicating with web servers for the transmission of webpage data whether that is html images vdos etc
4. HTTPS stands for secure basically a more secure and encrypted version
5. HTTPS makes sure we are talking to the correct webserver and not a phony
# request and responses
- to access the elements or the contents of a website we first need to make request to the website through our browser
- for that we need to tell our browser what where and how to access the resources thats where a url comes in
- Uniform Resource Locator is just an instruction that tells the browser how to acces the resources of a webcontent 
## parts of a URL
1. scheme this instructs on what protocol to use for accessing the resource such as HTTP HTTPS FTP
2. user some services require authentication to log in you can put a username and password into the URL to log in 
3. host the domain name or the ip address of the server you wish to access 
4. port the place where your website is hosted usually 80 for HTTP 443 for HTTPS but can be anything between 1 65535
5. path the file name or the location of the resource you are trying to access 
6. query string extra bits of info that can be sent to the requested path 
	ex blog id 1 tells that we wish to access the blog with id 1
7.  fragment  this is a reference to a location on the actual page requested this is mostly used with long pages and has a certain part of the page link to it for faster access
## making a request
- we can get our web data with just GET/HTTP/1.1 
- but to get a better experience we use headers and these are used to store extra info to give to the web server that you are communicating
- example GET/HTTP/1.1
- ```http
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/
```

- line 1 this is the get method of request , request the home page and tell the web server that we are using the HTTP protocol v 1.1
- line 2 we tell the web server we want the website tryhackme.com
- line 3 we tell the web server we are using the firefox version 87 browser 
- line 4 we tell the web server that the web page that referred us to this one is https://tryhackme.com
- line 5 HTTP requests always end with a blank line to inform the web server that the request has finished 
- RESPONSE EXAMPLE :
- ```http
HTTP/1.1 200 OK

Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98


<html>
<head>
    <title>TryHackMe</title>
</head>
<body>
    Welcome To TryHackMe.com
</body>
</html>
```
- line 1 HTTP 1.1 is the version of the HTTP protocol the server is using and then followed by the HTTP status code in this case 200 ok which tells the request is completed successfully
- this tells us the web server software and version number
- current date and timezone of the web server
- the content type header tells the client what sort of info is going to be sent , such as HTML, images ,videos ,pdf, XML
- content type length tells the client how long the response is , this way we can confirm no data is missing 
- HTTP response contains a blank line to confirm the end of the HTTP response 
- the info that has been requested , in this ex the homepage
## HTTP methods
- GET this is used to get info from a web server
- POST this is used to submit data to the web server and potentially create new records
- PUT this is used for submitting info to web server to update info
- DELETE this is used for deleting info records from a web server
## HTTP status codes
100 to 199 info response
- these are sent to tell the client the first part of their request has been accepted and they should continue sending the rest of their request these are no longer very common
200 to 299 success
- this range of status code is used to tell the client their req was successful
300 to 399 redirection
- these are used to redirect the client's request to another response this can be a diff page or website
400 to 499 client errors
 - used to inform the client that there was an error with their request
 500 to 599 server errors
 - this is reserved for errors on the server side and usually indicate quite a major problem with the server handling the request
 ### some of the common codes
200 OK - the request was completed successfully
201 created - A resource has been created (for example a new user or new blog post).
301 moved permanently - This redirects the client's browser to a new webpage or tells search engines that the page has moved somewhere else and to look there instead.
302 found - Similar to the above permanent redirect, but as the name suggests, this is only a temporary change and it may change again in the near future.
400 bad request - This tells the browser that something was either wrong or missing in their request. This could sometimes be used if the web server resource that is being requested expected a certain parameter that the client didn't send.
401 not authorised - You are not currently allowed to view this resource until you have authorised with the web application, most commonly with a username and password.
403 forbidden - You do not have permission to view this resource whether you are logged in or not.
405 method not allowed - The resource does not allow this method request, for example, you send a GET request to the resource /create-account when it was expecting a POST request instead.
404 page not found - The page/resource you requested does not exist.
500 internal service error -The server has encountered some kind of error with your request that it doesn't know how to handle properly.
503 service unavailable - This server cannot handle your request as it's either overloaded or down for maintenance.
## Headers
- these are additional bits of data you can send to the web server while making requests
- these are not required but it will be difficult to view a website without them
Common request headers 
**these are headers that are sent from the client to the server**
Host : Some web servers host multiple websites so by providing the host headers you can tell it which one you require, otherwise you'll just receive the default website for the server.
user agent: This is your browser software and version number, telling the web server your browser software helps it format the website properly for your browser and also some elements of HTML, JavaScript and CSS are only available in certain browsers.
content length: When sending data to a web server such as in a form, the content length tells the web server how much data to expect in the web request. This way the server can ensure it isn't missing any data.
accept encoding: Tells the web server what types of compression methods the browser supports so the data can be made smaller for transmitting over the internet.
cookie:Data sent to the server to help remember your information (see cookies task for more information).
Common response headers
**these are the headers returned to the client from the web server after a request**
set cookie:Information to store which gets sent back to the web server on each request (see cookies task for more information).
cache control: How long to store the content of the response in the browser's cache before it requests it again.
content type :This tells the client what type of data is being returned, i.e., HTML, CSS, JavaScript, Images, PDF, Video, etc. Using the content-type header the browser then knows how to process the data.
content encoding:What method has been used to compress the data to make it smaller when sending it over the internet.
## cookies
- these are small pieces of data that is stored on your pc
- these are saved when we receive a set cookie header from a web server
- these are saved each time you make a request
>***HTTP is stateless(it does not keep track of the previous request)***

- these are to remind the website who you are whether you have been to the site before etc