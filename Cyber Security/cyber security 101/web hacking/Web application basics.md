# Web Application Architecture: The Planet Analogy
A web application can be compared to a planet. The surface we see and interact with is the **Front End**, while the underlying structures and ecosystems keeping it functioning are the **Back End**.

## Front End (The Surface)
Technologies that run inside the user's web browser, focusing on layout, design, and interactivity.

| Technology          | Function                                                             | Planet Analogy                                                   |
| :------------------ | :------------------------------------------------------------------- | :--------------------------------------------------------------- |
| **HTML**            | The structural code instructing the browser on *what* to display.    | The basic DNA or bare soil/surface of the planet.                |
| **CSS**             | The styling rules for colors, fonts, and layouts.                    | The vibrant colors, landscapes, and visual beauty of the planet. |
| **JavaScript (JS)** | The logic enabling complex, interactive choices and dynamic content. | The "brain" of an organism making decisions and interacting.     |

## Back End (The Core & Ecosystem)
The behind-the-scenes engine that processes data, stores information, and secures the application.

| Component          | Function                                                                | Planet Analogy                                                |
| :----------------- | :---------------------------------------------------------------------- | :------------------------------------------------------------ |
| **Database**       | The repository where information is stored, modified, and retrieved.    | Libraries, filing cabinets, or a rich ecosystem of resources. |
| **Infrastructure** | Web servers, application servers, storage, and networking devices.      | Roads, vehicles, and fuel keeping the planet moving.          |
| **WAF**            | Web Application Firewall; filters out dangerous and malicious requests. | The ozone layer protecting the planet from harmful elements.  |
# Uniform Resource Locator (URL) Anatomy
A URL is a web address that guides your browser to specific online content (webpages, videos, images, etc.).



## URL Components
Understanding these parts is essential for web development, browsing, and identifying security threats.

| Component        | Indicator               | Description & Security Considerations                                                                                                    |
| :--------------- | :---------------------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| **Scheme**       | `http://` or `https://` | The protocol used to access the site. HTTPS encrypts the connection and is the secure standard.                                          |
| **User**         | `username@`             | Login credentials embedded directly in the URL. Rare today as it is highly insecure and exposes sensitive data.                          |
| **Host/Domain**  | `tryhackme.com`         | The unique website address. Attackers often use *typosquatting* (fake domains with slight misspellings) for phishing.                    |
| **Port**         | `:80` or `:443`         | Directs the browser to a specific service doorway on the server (HTTP = 80, HTTPS = 443).                                                |
| **Path**         | `/room/url`             | The roadmap to a specific file or page on the server. Must be secured to prevent unauthorized access to sensitive files.                 |
| **Query String** | `?search=term`          | Handles dynamic data like search terms or form inputs. Because users can modify this, it must be sanitized to prevent injection attacks. |
| **Fragment**     | `#section1`             | Jumps directly to a specific heading or element on the page. Can also be manipulated, requiring sanitization against injection.          |
# HTTP Messages
Packets of data exchanged between a user (client) and a web server. They are the foundation of client-server communication.



## Message Types
* **HTTP Requests:** Sent by the client to trigger actions on the server (e.g., requesting a webpage or submitting a login form).
* **HTTP Responses:** Sent by the server back to the client containing the requested data or the outcome of the request.

## HTTP Message Anatomy
Every HTTP message (both requests and responses) follows a strict structural format:

| Component | Description |
| :--- | :--- |
| **Start Line** | The introduction. Identifies if the message is a request (includes the method like GET/POST and URL) or a response (includes the status code like 200 OK or 404 Not Found). |
| **Headers** | Key-value pairs providing metadata and instructions for handling the message (e.g., security settings, content type, cookies). |
| **Empty Line** | A mandatory blank line that separates the headers from the body. Without it, the server or client cannot parse the message correctly, resulting in errors. |
| **Body** | The actual data payload. In a request, this is data sent *to* the server (e.g., form inputs, credentials). In a response, this is the data sent *from* the server (e.g., the HTML webpage or API data). |

## Why It Matters
* **Troubleshooting:** Helps accurately diagnose communication issues and errors between the frontend and backend.
* **Performance:** Ensures data is structured and transmitted smoothly and reliably.
* **Security:** Crucial for implementing protections and understanding how data is transmitted, making it easier to secure against interception or manipulation.

# HTTP Requests
An HTTP request is the initial message sent by a client to a web server to interact with an application.

## Request Line (Start Line)
The first line of the request that dictates the core action. 
* **Format:** `[METHOD] https://www.panynj.gov/path/en/index.html [HTTP Version]`
* **Example:** `POST /api/login HTTP/1.1`

## HTTP Methods
Tells the server what action to perform on the requested resource.

| Method | Function | Security Reminder |
| :--- | :--- | :--- |
| **GET** | Fetches data from the server. | Do not put sensitive info (tokens/passwords) in GET requests; they appear in plaintext URLs. |
| **POST** | Sends data to create or update resources. | Validate and sanitize inputs to prevent SQLi or XSS attacks. |
| **PUT** | Replaces or updates a resource entirely. | Ensure strict authorization checks before accepting changes. |
| **DELETE** | Removes a resource from the server. | Enforce strict access controls to prevent unauthorized deletion. |
| **PATCH** | Updates a *part* of a resource. | Validate data to avoid partial update inconsistencies. |
| **HEAD** | Retrieves only headers (no body). | Useful for checking metadata without downloading the full payload. |
| **OPTIONS** | Lists allowed methods for a resource. | Disable if unnecessary to limit reconnaissance. |
| **TRACE** | Echoes the received request (debugging). | Disable in production to prevent security risks (e.g., Cross-Site Tracing). |
| **CONNECT** | Establishes a secure tunnel. | Critical for encrypted HTTPS communication. |

## URL Path
Identifies the specific resource location on the server (e.g., `/api/users/123`).
* **Security Mitigation:** Must be strictly validated and sanitized to prevent unauthorized access (like Path Traversal) or injection attacks.

## HTTP Versions
The protocol iteration used for client-server communication.
* **HTTP/0.9 (1991):** The initial version; only supported GET requests.
* **HTTP/1.0 (1996):** Introduced headers and basic caching.
* **HTTP/1.1 (1997):** Brought persistent connections and chunked encoding. Highly stable and still widely used today.
* **HTTP/2 (2015):** Introduced multiplexing, header compression, and prioritization for faster performance.
* **HTTP/3 (2022):** Built on the new QUIC protocol for even quicker and more secure connections.

# HTTP Request Headers and Body

## Request Headers
Headers convey extra metadata about the request to the web server.

| Header | Example | Description |
| :--- | :--- | :--- |
| **Host** | `Host: tryhackme.com` | Specifies the target web server's name. |
| **User-Agent** | `User-Agent: Mozilla/5.0` | Provides information about the client's web browser and OS. |
| **Referer** | `Referer: https://google.com/` | Indicates the URL from which the request originated. |
| **Cookie** | `Cookie: user_type=student` | Sends previously stored session or state data back to the server. |
| **Content-Type** | `Content-Type: application/json` | Declares the format of the data sent in the request body. |

## Request Body Formats
Used primarily in `POST` and `PUT` requests to transmit data *to* the server. The format must be declared in the `Content-Type` header.

### 1. URL Encoded (`application/x-www-form-urlencoded`)
* **Structure:** Key-value pairs separated by ampersands (`&`). Special characters are percent-encoded.
* **Example Body:** `name=Aleksandra&age=27&country=US`

### 2. Form Data (`multipart/form-data`)
* **Structure:** Multiple data blocks separated by a defined boundary string (specified in the header). Essential for uploading binary data like images or files.
* **Example Body:**
```
text
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[Binary Data Here]
----WebKitFormBoundary7MA4YWxkTrZu0gW-- 
```

### 3. JSON (`application/json`)
* **Structure:** JavaScript Object Notation. Uses `"name": "value"` pairs separated by commas, all enclosed in curly braces `{}`.
* **Example Body:**
```json
{
    "name": "Aleksandra",
    "age": 27,
    "country": "US"
}
```

### XML (`application/xml`)
* **Structure:** Extensible Markup Language. Data is structured and nested inside opening `<tag>` and closing `</tag>` labels.
* **Example Body:**
```xml
<user>
    <name>Aleksandra</name>
    <age>27</age>
    <country>US</country>
</user>
```

# HTTP Responses & Status Codes
Server-generated messages indicating the outcome of a client's request.

## Status Line
The first line of an HTTP response.
* **Format:** `[HTTP Version] [Status Code] [Reason Phrase]`
* **Example:** `HTTP/1.1 200 OK`

## Status Code Categories
The three-digit number indicates the specific outcome, categorized by the first digit:

| Category | Range | Description |
| :--- | :--- | :--- |
| **Informational** | 100-199 | Server received part of request; waiting for remainder. |
| **Successful** | 200-299 | Request processed and executed exactly as expected. |
| **Redirection** | 300-399 | Resource requested has been moved to a new location. |
| **Client Error** | 400-499 | A problem with the request (e.g., bad URL, missing authorization). |
| **Server Error** | 500-599 | Server encountered an issue preventing fulfillment; not the client's fault. |

## Common Status Codes & Reason Phrases
| Code | Reason Phrase | Explanation |
| :--- | :--- | :--- |
| **100** | Continue | Initial request received; ready for remainder. |
| **200** | OK | Request successful; requested resource is included. |
| **301** | Moved Permanently | Resource relocated; subsequent requests should use the new URL provided. |
| **404** | Not Found | Resource not found at the requested URL. |
| **500** | Internal Server Error | Generic error on the server side prevented processing. |
# HTTP Response Headers and Body
HTTP response headers are key-value pairs sent by the web server to provide metadata and instruct the client on how to handle the response.



## Crucial Response Headers
Essential information required for the client and server to process the response correctly.

| Header | Example | Description & Security Considerations |
| :--- | :--- | :--- |
| **Date** | `Date: Fri, 23 Aug 2024 10:43:21 GMT` | Exact date and time the server generated the response. |
| **Content-Type** | `Content-Type: text/html; charset=utf-8` | Declares the content format (HTML, JSON) and character set for proper rendering. |
| **Server** | `Server: nginx` | Identifies the server software. **Security:** Often obfuscated or removed to prevent information disclosure to attackers. |

## Other Common Response Headers
Provides additional instructions for client/browser handling and session management.

| Header | Example | Description & Security Considerations |
| :--- | :--- | :--- |
| **Set-Cookie** | `Set-Cookie: sessionId=123xyz` | Instructs the client to store a cookie. **Security:** Always use `HttpOnly` (blocks JS access) and `Secure` (HTTPS only) flags. |
| **Cache-Control** | `Cache-Control: max-age=600` | Dictates caching behavior. Use `no-cache` to prevent the browser from caching sensitive information. |
| **Location** | `Location: /index.html` | Used with 3xx redirects to specify the new URL. **Security:** Input must be strictly validated to prevent Open Redirect vulnerabilities. |

## Response Body
The actual data payload sent back to the client (e.g., the HTML document, JSON API data, or images).
* **Security Mitigation:** To prevent injection attacks like Cross-Site Scripting (XSS), you must always sanitize and escape any data (especially user-generated content) before including it in the response body.

# HTTP Security Headers
Security headers add an essential layer of defense to web applications, mitigating common vulnerabilities like Cross-Site Scripting (XSS), clickjacking, and MIME sniffing. 
*(Tip: You can use [securityheaders.io](https://securityheaders.io/) to audit a website's headers).*



## 1. Content-Security-Policy (CSP)
Defines which domains are considered safe to load resources from, preventing attackers from injecting malicious scripts from external sites.
* **Example:** `Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'`

| Directive | Description |
| :--- | :--- |
| `'self'` | Special keyword meaning "only the current domain." |
| `default-src` | The fallback policy if a specific resource type isn't explicitly defined. |
| `script-src` | Specifies allowed sources for loading JavaScript (e.g., self and a trusted CDN). |
| `style-src` | Specifies allowed sources for loading CSS stylesheets. |

## 2. Strict-Transport-Security (HSTS)


Ensures that web browsers *always* connect to the website over an encrypted HTTPS connection, preventing downgrade attacks.
* **Example:** `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload`

| Directive | Description |
| :--- | :--- |
| `max-age` | Expiry time (in seconds) for the browser to remember this rule. |
| `includeSubDomains` | Applies the HTTPS enforcement to all subdomains. |
| `preload` | Allows the site to be hardcoded into a browser's preload list, enforcing HTTPS even on the user's very first visit. |

## 3. X-Content-Type-Options
Prevents the browser from trying to "guess" (sniff) the file type (MIME type) and forces it to strictly use the declared `Content-Type`.
* **Example:** `X-Content-Type-Options: nosniff`

| Directive | Description |
| :--- | :--- |
| `nosniff` | Instructs the browser to explicitly trust the server's `Content-Type` header and not attempt MIME sniffing. |

## 4. Referrer-Policy
Controls how much information is sent to a destination web server when a user clicks a hyperlink navigating away from your site.

| Directive                         | Description                                                                                              |
| :-------------------------------- | :------------------------------------------------------------------------------------------------------- |
| `no-referrer`                     | Completely disables the Referer header; no origin info is sent.                                          |
| `same-origin`                     | Sends referrer info *only* if navigating within the same website/domain.                                 |
| `strict-origin`                   | Sends referrer info only if the security level matches (e.g., HTTPS to HTTPS).                           |
| `strict-origin-when-cross-origin` | Sends full URL path for same-origin requests, but only the domain name for cross-origin secure requests. |