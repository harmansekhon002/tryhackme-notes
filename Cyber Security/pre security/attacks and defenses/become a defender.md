## defensive security
- these are often known as the blue teams 
- their goal is to intercept and predict any upcoming attack and prepare and block it from the red team
## What Can You Do as a Defender?

Once you understand what systems exist and how they can be protected, defenders typically organize their work around a set of foundational security concepts. These concepts apply across nearly all environments and will appear repeatedly as you continue learning defensive security.

- **Prevention:** Putting security controls in place to stop attacks before they happen, such as firewalls, antivirus software, and regular patching.
- **Detection:** Monitoring systems and networks to identify suspicious or malicious activity through logs, alerts, and security tools.
- **Mitigation:** Taking action during an incident to limit damage, such as blocking traffic, isolating affected systems, or disabling compromised accounts.
- **Analysis:** Investigating what happened, how it happened, and which systems were affected by reviewing logs and other evidence.
- **Response and Improvement:** Recovering from the incident and improving defenses to reduce the risk of similar attacks in the future.
## chain reaction

a malicious email attachment might affect an employee's workstation. From there, an attacker could steal usernames and passwords, access a mail server, and eventually reach sensitive data stored on a database server. Each step builds on the last and is an opportunity for a defender to apply security measures to protect systems.

**Key Defender Principles**

- **Threat anticipation**: Review the systems you aim to protect and ask, "What if?" Imagine realistic paths an attacker may take to achieve their goal.
- **Attack awareness**: Attacks typically follow recognizable stages. Studying common attack chains and frameworks is incredibly useful for defenders.
- **Risk prioritization**: Not every part of your system carries equal risk. Defenders should identify high-value systems and targets.
- **Continuous adaptation**: Defense is not a one-time set up. Threats and attackers evolve, techniques change, and vulnerabilities emerge.

|System or Infrastructure Component|What Could Go Wrong|Defenses You'll Use|
|---|---|---|
|Employee Devices|Someone clicks a bad link or downloads unsafe software|Antivirus to detect bad programs  <br>Regular software updates|
|Web Server|Attackers try to break into the website|Only allow safe traffic  <br>Use secure communication|
|Mail Server|Malicious or deceptive emails|Spam filters  <br>Scan attachments|
|Firewall|Strangers from the internet try to break in|Firewall rules that control access  <br>Block known troublemakers|
|The Outside Internet|External threats come from here|Restrict inbound traffic  <br>Monitor for suspicious activity|
