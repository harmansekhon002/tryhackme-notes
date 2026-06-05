# Access Control: The IAAA Framework
IAAA is a foundational security model used to understand how users and their actions are verified within applications. 



> **The Sequential Rule:** IAAA is strictly hierarchical. You cannot skip a level; if a preceding step is not successfully performed, the subsequent steps cannot occur. 

## The Four Pillars of IAAA
| Component | Description | Examples |
| :--- | :--- | :--- |
| **1. Identity** | The unique account or identifier that represents a specific person or service. *(Who are you claiming to be?)* | Username, Email Address, User ID. |
| **2. Authentication** | The process of mathematically or logically proving that you actually own the claimed identity. *(Prove you are who you say you are).* | Passwords, One-Time Passwords (OTP), Biometrics, Passkeys. |
| **3. Authorisation** | Determining exactly what that authenticated identity is allowed to do or access within the system. *(What permissions do you have?)* | Standard user permissions, Admin rights, Read/Write access. |
| **4. Accountability** | Recording, logging, and alerting on actions taken by the identity (tracking who did what, when, and from where). *(What did you do?)* | Audit logs, session tracking, security alerts. |
## Security Implications (OWASP Top 10)
Failures in how a system implements IAAA are responsible for several major categories in the **OWASP Top 10**. Weaknesses in this framework are incredibly detrimental, allowing threat actors to bypass security to:
* Access, steal, or modify the private data of other legitimate users.
* Gain unauthorized, elevated privileges they are not supposed to have (Privilege Escalation).
# Authentication Failures
Authentication failures occur when an application cannot reliably verify or securely bind a user's identity. If these vulnerabilities are present, an attacker can often log in as another user or hijack an active session.

## Common Authentication Vulnerabilities
* **Username Enumeration:** Allowing an attacker to systematically guess or confirm whether a specific username exists in the system.
* **Weak Credentials & Lack of Protection:** Allowing easily guessable passwords, or failing to implement account lockouts and rate-limiting against brute-force attacks.
* **Insecure Session/Cookie Handling:** Improperly managing session tokens (e.g., predictable tokens, failing to expire sessions, or lacking secure flags).
* **Logic Flaws in Login/Registration Flows:** Errors in how the application processes identity during the onboarding or authentication phase.

## Practical Scenario: Account Hijacking via Logic Flaw
A common logic flaw involves how an application handles text casing during user registration versus login. 

* **The Target:** We want to compromise the `admin` account.
* **The Exploit:** We attempt to register a new account using a mixed-case variation, such as `aDmiN`.
* **The Flaw:** If the database/registration system is case-sensitive (allowing `aDmiN` to be created as a uniquely new user), but the login or session-handling system normalizes all text to lowercase (treating `aDmiN` as the original `admin`), we can successfully authenticate and gain full access to the actual `admin` account!

# Accountability: Security Logging & Monitoring Failures
When applications fail to adequately record, retain, or alert on security-relevant events, defenders are left blind. Good logging is the absolute foundation of **Accountability** (the ability to prove who did what, when, and from where). Without it, organizations cannot detect active breaches or investigate past incidents.

## Common Logging & Monitoring Failures
If an application exhibits any of the following weaknesses, it fails the Accountability pillar of IAAA and leaves the system vulnerable:

| Failure Type | Description |
| :--- | :--- |
| **Missing Events** | Failing to log critical security actions, such as successful/failed logins, password changes, or high-value transactions. |
| **Vague Logs** | Generating logs that lack actionable context (e.g., missing timestamps, source IP addresses, or specific user IDs). |
| **Lack of Alerting** | Collecting logs but failing to trigger automated alerts for highly suspicious activities (like brute-force attacks or sudden privilege changes). |
| **Short Retention** | Deleting logs too quickly, which destroys vital forensic evidence before a breach is even discovered. |
| **Insecure Storage** | Storing logs locally or without proper access controls, allowing an attacker to easily modify or delete them to cover their tracks. |