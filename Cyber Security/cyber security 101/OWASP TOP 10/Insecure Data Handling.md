# Cryptographic Failures
Cryptographic failures occur when sensitive data is inadequately protected due to a lack of encryption, faulty implementation, or insufficient security measures. This leaves data highly vulnerable both at rest (when stored) and in transit (when transmitted over a network).



## Common Vulnerabilities
* **Weak or Outdated Algorithms:** Utilizing deprecated, easily cracked encryption or hashing algorithms (such as MD5, SHA1, or DES).
* **"Rolling Your Own Crypto":** Attempting to create custom, proprietary encryption algorithms instead of using well-established, vetted, and mathematically proven standards.
* **Cleartext Storage:** Storing highly sensitive information, especially user passwords, without applying any cryptographic hashing.
* **Exposed Keys:** Hardcoding encryption keys, API tokens, or access credentials directly into source code, configuration files, or public code repositories.

---

## Prevention & Mitigation Strategies
Preventing cryptographic failures requires strict adherence to industry standards, utilizing modern algorithms, and properly isolating cryptographic keys.

| Vulnerable Practice       | Secure Prevention Strategy                                                                                                                                                                               |
| :------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Storing Passwords**     | Never store passwords in cleartext. Always use robust, slow hashing functions designed specifically to resist brute-force attacks, such as **bcrypt**, **scrypt**, or **Argon2**.                        |
| **Encrypting Data**       | Avoid creating your own encryption. Rely exclusively on trusted, industry-standard cryptographic libraries and modern algorithms (like AES-256).                                                         |
| **Managing Keys/Secrets** | Never embed access credentials or encryption keys in source code. Utilize secure Key Management Systems (KMS) or secure environment vaults specifically designed to store and inject secrets at runtime. |
# Software & Data Integrity Failures
Software and Data Integrity Failures occur when an application relies on code, updates, or data that it falsely assumes are safe. If the application does not explicitly verify the authenticity, integrity, or origin of these elements, attackers can inject malicious code or manipulate critical system data.



## Common Vulnerabilities
* **Unverified Updates:** Automatically trusting and installing software updates or patches without verifying their source or integrity.
* **Untrusted Sources:** Loading scripts, configuration files, or plugins from unverified or third-party sources.
* **Unvalidated Artifacts:** Accepting serialized data, binaries, templates, or JSON files without confirming if they have been tampered with in transit or storage.

---

## Prevention & Mitigation Strategies
Preventing these failures requires establishing strict trust boundaries. You must operate under the assumption that no code, update, or data payload is inherently safe until explicitly verified.

| Vulnerable Practice          | Secure Prevention Strategy                                                                                                                                                                       |
| :--------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Blindly Trusting Updates** | Implement cryptographic checks (such as hash checksums or digital signatures) to verify the integrity and origin of all update packages before installation.                                     |
| **Accepting Untrusted Data** | Ensure all incoming data, binaries, and files are validated for authenticity and have not been altered before the application processes them.                                                    |
| **Insecure Build Pipelines** | Enforce strict integrity and trust boundaries within your CI/CD (Continuous Integration/Continuous Deployment) processes. Ensure only trusted, authorized sources can modify critical artifacts. |