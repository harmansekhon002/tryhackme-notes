# Introduction to Burp Suite
Burp Suite is a Java-based framework and the industry-standard tool for hands-on security assessments and penetration testing of web and mobile applications (including APIs).

## How It Works
At its core, Burp Suite acts as an interception proxy. It captures all HTTP/HTTPS traffic flowing between your web browser and the target web server. 
* **The Advantage:** This fundamental capability allows you to intercept, view, route, and manually modify web requests *before* they reach the server, as well as manipulate the server's responses *before* they are loaded by your browser.

## Burp Suite Editions
There are three main editions of the framework, each tailored to different testing needs and environments.

| Edition | Description & Key Features |
| :--- | :--- |
| **Community** | The free version for non-commercial use. Provides the core interception and manual testing capabilities. |
| **Professional** | The unrestricted, paid version preferred by industry professionals for advanced manual testing. **Key upgrades include:** <br>• Automated vulnerability scanner <br>• Unrestricted, fast fuzzer/brute-forcer (Intruder) <br>• Project saving and formal report generation <br>• Built-in API for external tool integration <br>• Unrestricted access to add new extensions <br>• Access to Burp Suite Collaborator (a dedicated request catcher) |
| **Enterprise** | A server-hosted solution designed for continuous, automated scanning. Unlike Community/Pro (which are used on a local machine for manual attacks), Enterprise acts like Nessus for web apps, periodically scanning targets for potential vulnerabilities. |
# Burp Suite Community Edition: Core Features
While the Community Edition has some limitations compared to Professional, it still provides an impressive, highly valuable suite of tools for web application testing.

## Built-in Tools
| Feature | Primary Function | Use Case |
| :--- | :--- | :--- |
| **Proxy** | The core interception tool. Captures and modifies HTTP/HTTPS requests and responses in transit. | Manually manipulating traffic between your browser and the target web application. |
| **Repeater** | Captures, modifies, and resends the same request multiple times. | Crafting payloads through trial and error (e.g., testing for SQLi or verifying endpoint logic flaws). |
| **Intruder** | Sprays endpoints with numerous automated requests. *(Note: Rate-limited in the Community Edition).* | Brute-forcing login forms or fuzzing parameters for vulnerabilities. |
| **Decoder** | Transforms data formats directly within the framework. | Quickly decoding captured data (like Base64 cookies) or encoding a payload before sending it. |
| **Comparer** | Compares two pieces of data at either the word or byte level. | Easily identifying slight differences in server responses after sending modified requests. |
| **Sequencer** | Assesses the randomness (entropy) of supposed randomly generated data. | Testing if session cookies or tokens are predictable and vulnerable to attack. |

## Extensibility (Extender & BApp Store)


Because Burp Suite has a Java codebase, its functionality can be heavily expanded using custom extensions.
* **Languages Supported:** Extensions can be written in Java, Python (using the Jython interpreter), or Ruby (using the JRuby interpreter).
* **The BApp Store:** Burp Suite's official marketplace for downloading third-party modules. 
* **Extender Module:** Allows you to quickly load these extensions into your framework (for example, adding `Logger++` to enhance built-in logging). *Note: While some extensions require a Professional license, many are fully compatible with the Community Edition.*

# Burp Suite: Startup & Dashboard Overview
Launching Burp Suite for the first time can seem overwhelming, but the setup and interface are designed to be straightforward once you know where to look.

## 1. The Startup Process
When launching Burp Suite Community Edition, the setup process is very simple:
1. **Accept Terms:** Accept the initial terms and conditions.
2. **Project Selection:** Because project saving is restricted in the Community Edition, default to a Temporary Project and click **Next**.
3. **Configuration:** Keep the default settings (these are suitable for almost all manual testing scenarios) and click **Start Burp**.

## 2. The Dashboard Quadrants
The main dashboard is divided into four primary sections (moving counter-clockwise from the top-left):

| Quadrant | Description | Community vs Pro |
| :--- | :--- | :--- |
| **Tasks** | Defines background operations Burp performs while you work. | Community defaults to "Live Passive Crawl" (logging visited pages). Pro adds automated on-demand scans. |
| **Event Log** | A system log showing actions performed by Burp itself (e.g., proxy starting, connection errors). | Available in both. |
| **Issue Activity** | A live feed of vulnerabilities identified by the automated scanner, ranked by severity. | **Pro Only.** (Community will likely show this as empty). |
| **Advisory** | Provides deep dives into the vulnerabilities found in the Issue Activity quadrant, including references and remediation steps. | **Pro Only.** |

## 3. Built-in Help System

Throughout the framework, you will see **Question Mark (`?`) icons**. 
* **Function:** Clicking these icons opens a dedicated window explaining the specific feature or tab you are currently looking at. It is an invaluable built-in resource for learning the tool!

# Burp Suite: Navigation & Interface Management
Navigating Burp Suite is primarily done through its tiered menu system at the top of the application window. Understanding this layout and utilizing keyboard shortcuts will significantly speed up your workflow.

## 1. The Menu System
The interface is organized into a hierarchy of modules and their specific settings.
* **Module Selection (Top Menu Bar):** The main row of tabs across the top (e.g., Dashboard, Target, Proxy, Intruder, Repeater). Clicking these switches the core tool you are using.
* **Sub-Tabs (Second Menu Bar):** When a main module is selected, a second row of tabs appears directly below it. These contain the specific features and settings for that module (e.g., under the **Proxy** module, you will find sub-tabs for *Intercept*, *HTTP history*, *WebSockets history*, and *Proxy settings*).

## 2. Window Management (Detaching Tabs)


If you have multiple monitors or prefer viewing different tools side-by-side, you can break the interface apart.
* **How to Detach:** Click **Window** in the very top application menu (above the Module Selection bar) and select **Detach**. The currently active tab will pop out into its own standalone window.
* **How to Reattach:** You can re-dock the window into the main framework using the same **Window** menu on the detached screen.

## 3. Global Keyboard Shortcuts
Using hotkeys to jump between modules is much faster than clicking through the menu bars. Here are the default shortcuts for navigating to key tabs:

| Shortcut | Destination Tab |
| :--- | :--- |
| `Ctrl + Shift + D` | **Dashboard** |
| `Ctrl + Shift + T` | **Target** tab |
| `Ctrl + Shift + P` | **Proxy** tab |
| `Ctrl + Shift + I` | **Intruder** tab |
| `Ctrl + Shift + R` | **Repeater** tab |
# Burp Suite: Settings & Configuration
Before diving into specific modules, it is important to understand how to configure the framework. Burp Suite splits its configuration into two main categories.

## 1. Types of Settings
| Setting Type | Description | Community Edition Limitation |
| :--- | :--- | :--- |
| **Global (User) Settings** | Affects the entire Burp Suite installation. These act as your baseline configuration and are applied every time you start the application. | *Fully supported. Settings persist across restarts.* |
| **Project Settings** | Specific to the current, active session you are working on. | *Because Community Edition cannot save projects, these settings will be lost when you close the application.* |

## 2. Navigating the Settings Menu
To access the configuration options, click the **Settings** button located in the top navigation bar. This opens a dedicated Settings window.

The left-hand menu of this window helps you navigate the extensive options:
* **Search:** A highly useful search bar to quickly find specific settings using keywords instead of digging through menus.
* **Type Filter:** Allows you to toggle the view between just User settings, just Project settings, or both.
* **Categories:** Groups settings logically by the tool or feature they affect (e.g., Network, User interface, Suite).
## 3. Module-Specific Shortcuts

You don't always have to navigate through the main Settings menu to make a change. Many individual tools within Burp Suite have built-in shortcut buttons. 
* **Example:** If you are actively working in the **Proxy** tab, clicking the **Proxy settings** button will open the main Settings window and instantly jump to the relevant proxy configuration section, saving you time.

# Burp Suite: The Proxy Tool
The Burp Proxy is the fundamental core of Burp Suite. It acts as a "Man-in-the-Middle" (MITM) between your browser and the target web server, allowing you to capture, manipulate, and route HTTP/HTTPS and WebSocket traffic.

## 1. Core Functionality
| Feature | Description |
| :--- | :--- |
| **Intercepting Requests** | When enabled, requests are held back and displayed in the Proxy tab *before* reaching the server. You can modify them, click **Forward** to send them, **Drop** to discard them, or send them to other modules. |
| **The Intercept Toggle** | You can quickly enable or disable this holding pattern by clicking the **Intercept is on / Intercept is off** button. |
| **HTTP History (Logging)** | Even when interception is turned *off*, Burp Suite silently captures and logs all requests and responses in the **HTTP history** sub-tab. This is crucial for retrospective analysis. |
| **WebSocket Support** | Burp also captures and logs continuous WebSocket communication in the **WebSockets history** sub-tab. |
## 2. Notable Proxy Settings
Clicking the **Proxy settings** button opens a dedicated configuration menu for the Proxy module, offering extensive control over how traffic is handled.

| Setting | Functionality |
| :--- | :--- |
| **Response Interception** | By default, the proxy only intercepts outgoing *requests*. It does not intercept incoming server *responses*. You can change this by checking "Intercept responses based on the following rules" to catch and modify data before your browser renders it. |
| **Match and Replace** | Allows you to use Regular Expressions (regex) to automatically find and modify specific strings in incoming or outgoing traffic (e.g., automatically faking a specific User-Agent or dynamically changing cookie values on the fly). |
# Burp Suite: The Target Tab
The Target tab is essential for organizing your assessment. It does more than just control what you are testing; it actively maps the application and provides a built-in encyclopedia of vulnerabilities.
## Target Sub-Tabs
| Sub-Tab | Function & Details |
| :--- | :--- |
| **Site map** | Displays a hierarchical tree structure of the target web application. Every page or API endpoint you visit while the proxy is active is automatically mapped here. *Pro feature:* Automated crawling. *Community use:* Manual enumeration and visual mapping. |
| **Issue definitions** | A built-in dictionary of web vulnerabilities. Even without the Pro scanner, you can use this extensive list (complete with descriptions and references) for reporting and reference during manual testing. |
| **Scope settings** | Allows you to explicitly **Include** or **Exclude** specific domains/IP addresses. This filters the proxy history and site map so you only see traffic relevant to your actual target, blocking out background noise. |

## Practical Challenge: Mapping the Target
To see how the Site map populates in real-time, follow these steps in your AttackBox:

1. Ensure your Burp Proxy Intercept is **OFF** (so traffic flows freely), but ensure your browser is still routing traffic through Burp.
2. Open the browser and navigate to the target IP: `http://10.48.128.108/`
3. Manually click through and visit every page linked on the homepage.
4. Go back to Burp Suite and navigate to **Target -> Site map**. 
5. Expand the folder tree for `10.48.128.108`. You should see a list of all the pages you just visited.
6. Look through the list for an endpoint that seems very unusual or out of place compared to the others.
7. Visit that unusual endpoint in your browser (or view its **Response** directly in the Site map) to find your clue!

# Burp Suite: The Built-in Browser
If manually configuring an external web browser (like Firefox with FoxyProxy) seems tedious, Burp Suite provides an easier alternative. It includes a built-in Chromium browser that is pre-configured to automatically route all its traffic through the Burp Proxy.

## 1. Launching the Browser
To use the built-in browser:
1. Navigate to the **Proxy** module.
2. Click the **Open Browser** button.
3. A Chromium window will open. Any navigation or requests made within this specific window will automatically be intercepted and logged by Burp Suite.

## 2. Troubleshooting: Linux "Root" Sandbox Error
If you are running Burp Suite on a Linux machine as the `root` user (which is how the TryHackMe AttackBox operates by default), you will likely get an error when clicking "Open Browser." 

**The Cause:** Chromium browsers require a "sandbox" environment to run securely, and they refuse to create this sandbox when executed with root-level privileges.

**The Solutions:**

| Method | Instructions | Security Implications |
| :--- | :--- | :--- |
| **The Smart Option** | Create a new, low-privilege user account on your Linux machine and run Burp Suite from that account. | **High Security.** This is the recommended best practice for real-world environments. |
| **The Easy Option** | Navigate to **Settings -> Tools -> Burp's browser** and check the box that says **"Allow Burp's browser to run without a sandbox"**. | **Low Security.** Bypassing the sandbox means if the browser is compromised, the attacker gains root access to your machine. *(Note: This is generally fine to do in the isolated, temporary THM AttackBox environment, but avoid doing this on your personal host machine).* |
# Burp Suite: Target Scoping
Without scoping, Burp Suite captures and logs *all* background traffic from your browser, which quickly becomes overwhelming. Scoping allows you to filter out the noise and focus strictly on the specific web application you are testing.

## 1. Defining the Target Scope
The easiest way to define your scope is directly from the Site map once you have visited the target.
1. Navigate to the **Target -> Site map** tab.
2. Right-click on your target's domain or IP address in the left-hand list.
3. Select **Add to scope**.
4. **The Prompt:** Burp will ask if you want to stop sending out-of-scope items to the history or other tools. It is highly recommended to click **Yes**.

*Note: You can manually manage your scope, including explicitly excluding specific subdomains or IP ranges, by navigating to **Target -> Scope settings**.*

## 2. Restricting the Proxy Interceptor
Even after you tell Burp to stop *logging* out-of-scope traffic, the Proxy will still pause and *intercept* everything by default. You must explicitly tell the proxy to ignore background traffic.


**How to filter interception:**
1. Click the **Proxy settings** button (or navigate to Settings and search for Proxy).
2. Scroll to the **Intercept Client Requests** section.
3. Check the box next to the rule that states: `And` `URL` `Is in target scope`.

Enabling this specific rule ensures that the proxy completely ignores any background internet noise, giving you a clean, focused traffic view.

# Practical Example: Bypassing Client-Side Filters
A common task in web application penetration testing is identifying and bypassing client-side filters. Because these filters run in the user's browser (usually via HTML attributes or JavaScript), they are incredibly easy to bypass by intercepting the traffic *after* it leaves the browser but *before* it reaches the server.

## The Scenario: Reflected XSS
**Cross-Site Scripting (XSS)** occurs when an attacker injects malicious client-side scripts (like JavaScript) into a webpage, causing it to execute in a victim's browser.
* **Reflected XSS:** The payload is included in the web request (e.g., in a form submission or URL parameter) and the server immediately "reflects" it back to the user on the resulting page. It only affects the person making the specific request.

## The Obstacle: Client-Side Validation
Imagine a support form with a "Contact Email" field. If you try to directly type an XSS payload like `<script>alert("Succ3ssful XSS")</script>` into the browser, it fails. The browser prevents the submission because the field expects a valid email format, blocking special characters like `<` and `>`.

## Walkthrough: The Proxy Bypass
Because we control the Burp Proxy, we can submit legitimate data to satisfy the browser's filter, and then swap out the data for our malicious payload while it is paused in transit.

**Step-by-Step Execution:**
1.  **Prepare the Proxy:** Ensure Burp Suite Proxy is active and **Intercept is on**.
2.  **Satisfy the Client:** In your web browser, enter legitimate, filter-passing data into the form (e.g., Email: `pentester@example.thm` and Query: `Test Attack`) and click submit. 
3.  **Catch the Request:** The request will be intercepted and held in the Burp Proxy tab. 
4.  **Inject the Payload:** Locate the legitimate email address in the intercepted request body and delete it. Replace it with your malicious payload: 
    `<script>alert("Succ3ssful XSS")</script>`
5.  **URL Encode the Payload:** You cannot send raw `<` and `>` characters over HTTP safely. Highlight your payload in Burp Suite and press **`Ctrl + U`** to URL encode it.
6.  **Forward the Attack:** Click the **Forward** button to send the modified request to the server. 
7.  **Result:** When the browser loads the server's response, an alert box will pop up, proving the XSS payload successfully bypassed the filter and executed!