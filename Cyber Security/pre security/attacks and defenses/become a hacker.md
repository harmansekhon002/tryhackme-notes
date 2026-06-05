## offensive security
- the goal is to break into systems to find weaknesses to fix them before real hackers does
##   Core Offensive Security Terms

- **Red Teaming**: A structured, authorised attack methodology that simulates a real adversary to test the effectiveness of defenses and find vulnerabilities within a defined scope
- **Penetration Test**: A structured security assessment where an authorized tester attempts to identify and exploit vulnerabilities within a defined scope to understand real-world risk
- **Vulnerability**: A weakness or flaw in a system, application, or configuration that an attacker could abuse
- **Exploit**: A technique or method used to take advantage of a vulnerability to achieve a specific outcome, such as accessing restricted functionality or data
- **Scope**: The boundaries of what is allowed to be tested during an engagement. Scope defines which systems, applications, and actions are permitted, and what is off-limits

## Using Automated Tools

One tool in an ethical hacker's arsenal is **Gobuster**. This tool runs in the terminal and automates the scanning for web pages. 

`gobuster dir --url http://www.onlineshop.thm/ -w /usr/share/wordlists/dirbuster/directory-list.txt`

The command above is made up of the following parts:

- `gobuster` The command-line tool used to perform the discovery of web content
- `dir` Specifies the directory and file enumeration mode, which attempts to discover hidden directories and files on a web server
- `--url http://www.onlineshop.thm/` Sets the target website that Gobuster will scan
- `-w /usr/share/wordlists/dirbuster/directory-list.txt` Specifies the wordlist Gobuster will use to guess directory and file names

Here are some key points to keep in mind as you continue your ethical hacking journey.

- **Ask questions**: Don't assume a feature works as intended. Instead, ask “What if it doesn't?”
- **Test the unexpected**: Try actions and inputs that the developers didn't consider
- **Chain small weaknesses**: A tiny flaw may be harmless alone, but could be connected to create a bigger impact
- **Think like an adversary**: Think “How would a malicious actor approach this target?”

## A Valuable Target

Attackers are often interested in gaining valid credentials, such as usernames and passwords, because gaining access can unlock private areas of an application and increase their capabilities. Let’s explore what becomes accessible to an attacker once they gain entry to the private areas of an application.

- **Sensitive functionality**: Features that perform essential actions, such as modifying data, viewing restricted content, or triggering processes that should only be available to authorized users
- **User data**: Personal or private information belonging to users, such as names, email addresses, or account details, which attackers may steal, abuse, or sell
- **Administrative features**: High-privilege functionality that allows attackers to manage users, change settings, or gain full control of the application if accessed
- **Further attack opportunities**: Authenticated access can expose other vulnerabilities, allowing attackers to expand their access or move deeper into the application

**Hacking Automation**

In this section, you’ll use **Hydra**, a password‑testing tool that automates login attempts against a target application using a wordlist. Since we already know the username, Hydra will systematically try each password in the wordlist to see if the login is successful. This technique is known as a dictionary attack, as the tool relies on a predefined list of possible passwords.

`hydra -l admin -P passlist.txt www.onlineshop.thm http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V`

The command above is made up of the following parts:

- `hydra` The command-line tool used to perform the dictionary attack
- `-l admin` Attempts to log in using the username `admin`
- `-P passlist.txt` Specifies the password list to try
- `www.onlineshop.thm` Sets the target website
- `http-post-form` Indicates that this is an HTTP POST request form
- `"/login:username=^USER^&password=^PASS^:F=incorrect"` Specifies how the login request is sent and how Hydra determines whether a login attempt has failed
- `-V` Enables verbose output, which displays each username and password attempted