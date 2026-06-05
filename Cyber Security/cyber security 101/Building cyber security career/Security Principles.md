# Security Fundamentals: CIA Triad & Beyond
> [!info] The CIA Triad
> The foundational framework for evaluating the security of any system. The emphasis on each element varies based on the specific data (e.g., a public university announcement requires high integrity, but no confidentiality).
## The Core Triad
* **[[Confidentiality]]**: Ensures data is accessible *only* to intended and authorized recipients.
	* *Examples*: Protecting credit card numbers during online shopping; maintaining legal privacy of patient medical records.
* **[[Integrity]]**: Ensures data cannot be altered undetected. Guarantees accuracy and trustworthiness.
	* *Examples*: Preventing an intruder from altering a shipping address; ensuring medical treatments aren't maliciously changed.
* **[[Availability]]**: Ensures systems, services, and data are accessible when needed by authorized users.
	* *Examples*: Maintaining e-commerce website uptime; allowing doctors to access medical history during a clinic visit.

---

## Beyond CIA

> [!abstract] Business Critical Concepts
> These requirements are indispensable for various domains, such as shopping, patient diagnosis, and banking.

* **Authenticity**: Ensuring that a document, file, or data is genuinely from the claimed source (not fraudulent or counterfeit).
* **Nonrepudiation**: Ensuring the original source *cannot deny* being the source of a particular action or data. 
	* *Example*: A business needs to confirm a customer placed an order (Authenticity) AND ensure the customer cannot later claim they didn't order it (Nonrepudiation) before shipping 1,000 cars.

---

## The Parkerian Hexad

> [!note] 
> Proposed by Donn Parker in 1998. It expands the traditional security triad into six distinct elements.

1. **[[Confidentiality]]**
2. **[[Integrity]]**
3. **[[Availability]]**
4. **[[Authenticity]]**
5. **Utility**: Focuses on the *usefulness* of the information. 
	* *Example*: If you lose the decryption key to a laptop, the data is still physically "available" on the intact disk, but it is no longer in a useful form (loss of utility).
6. **Possession**: Protecting information from unauthorised taking, copying, or control. 
	* *Example*: A stolen physical backup drive or data locked by ransomware both represent a loss of possession, even if the adversary cannot read the data.
# The DAD Triad

> [!info] The Opposite of CIA
> The **DAD Triad** represents the primary methods used to attack the security of a system. It serves as the direct opposite and threat counterpart to the [[CIA Triad]].

## Core Elements
* **[[Disclosure]]** (Attacks Confidentiality): Exposing secret or confidential data to unauthorized entities.
	* *Example*: Stealing patient medical records and leaking them publicly online.
* **[[Alteration]]** (Attacks Integrity): Modifying or tampering with data.
	* *Example*: Changing a patient's medical history, potentially leading to life-threatening incorrect treatments.
* **[[Destruction]] / Denial** (Attacks Availability): Deleting data or making systems inaccessible.
	* *Example*: Taking down a paperless hospital's database, completely stalling the facility's operations.

> [!warning] The Security Balance
> Protecting against DAD is equivalent to maintaining CIA. However, **balance is essential**:
> * Pushing Confidentiality and Integrity to the extreme can severely restrict Availability.
> * Pushing Availability to the extreme can result in a loss of Confidentiality and Integrity.

# Security Models

> [!info] Purpose
> Security models are frameworks used to create systems that ensure one or more functions of the [[CIA Triad]].

## Bell-LaPadula Model
Focuses on achieving **[[Confidentiality]]**. 
* **Simple Security Property** ("No Read Up"): A lower-security subject cannot read a higher-security object.
* **Star Security Property** ("No Write Down"): A higher-security subject cannot write to a lower-security object (prevents data leaks to lower levels).
* **Discretionary-Security Property**: Uses an access matrix to authorize specific read and write operations.
* **Summary**: "Write Up, Read Down."
* **Limitation**: Not designed to handle file-sharing.

## Biba Integrity Model
Focuses on achieving **[[Integrity]]**. 
* **Simple Integrity Property** ("No Read Down"): A higher-integrity subject should not read from a lower-integrity object.
* **Star Integrity Property** ("No Write Up"): A lower-integrity subject should not write to a higher-integrity object.
* **Summary**: "Read Up, Write Down." (Direct opposite of Bell-LaPadula).
* **Limitation**: Fails to handle internal/insider threats.

## Clark-Wilson Model
Focuses on achieving **[[Integrity]]** through strict data and procedure classification.
* **Constrained Data Item (CDI)**: The specific data whose integrity must be preserved.
* **Unconstrained Data Item (UDI)**: All other data types (e.g., standard user and system input).
* **Transformation Procedures (TPs)**: Authorized programmed operations (like read/write) that safely manipulate and maintain the integrity of CDIs.
* **Integrity Verification Procedures (IVPs)**: Procedures designed to check and ensure the validity of CDIs.

> [!note] Additional Security Models
> Other models for further exploration include the Brewer and Nash model, Goguen-Meseguer model, Sutherland model, Graham-Denning model, and the Harrison-Ruzzo-Ullman model.

# ISO/IEC 19249 Security Principles

> [!info] Standard Overview
> **ISO/IEC 19249:2017** provides an international catalog of principles for building secure products, systems, and applications. It is broken down into Architectural and Design principles.

## Architectural Principles
These focus on how the overall system is structured and organized.

* **[[Domain Separation]]**: Grouping related components (apps, data) into single entities with shared security attributes.
	* *Example*: CPU privilege levels (the OS kernel operates safely in Ring 0, while user apps are restricted to Ring 3).
* **[[Layering]]**:  Structuring systems into abstract levels to apply specific security policies at each step. This strongly relates to Defence in Depth.
	* *Example*: The 7-layer OSI networking model, where each layer provides specific services to the one above it.
* **[[Encapsulation]]**: Hiding low-level implementations and preventing direct manipulation of data by requiring specific, controlled access methods.
	* *Example*: In Object-Oriented Programming (OOP), using an `increment()` method on a clock rather than letting a user directly edit the time variable; using APIs for database access.
* **[[Redundancy]]**:  Utilizing backup components or data to guarantee [[Availability]] and [[Integrity]].
	* *Example*: Hardware servers with dual power supplies; RAID 5 drive configurations that can survive a drive failure and use parity to detect data alterations.
* **[[Virtualization]]**: Sharing a single set of physical hardware across multiple operating systems. 
	* *Benefit*: Provides sandboxing capabilities, creating secure boundaries for observing and detonating malicious programs.

## Design Principles
These focus on the practical rules for how systems should operate safely.

* **[[Least Privilege]]**: Providing the absolute minimum permissions necessary for a user or system to perform its task (the "need-to-know" basis).
	* *Example*: Granting "read-only" access to a document when editing isn't required.
	
* **[[Attack Surface Minimisation]]**: Actively reducing potential vulnerabilities and risks to limit how an attacker can interact with the system.
	* *Example*: Disabling unused ports or services to harden a Linux server.
	
* **Centralized Parameter Validation**: Processing and validating all system inputs (especially from users) through a single, centralized library. This prevents malicious inputs from causing DoS or Remote Code Execution (RCE).

* **Centralized General Security Services**: Centralizing core security functions (e.g., using a dedicated authentication server) to maintain consistency, while carefully ensuring it does not become a single point of failure.

* **Error and Exception Handling**: Designing systems to *fail safely* and handle errors without compromising the system or its data.
	* *Examples*: If a firewall crashes, it should default to blocking *all* traffic rather than allowing it. Furthermore, error messages must never leak memory dumps or confidential customer data.
# Trust in Security

> [!info] The Role of Trust
> Trust is essential for systems and businesses to function, but it introduces complexity and risk. To manage this, cybersecurity relies on two primary guiding principles.

## [[Trust but Verify]]
* **Core Concept**: Always verify the actions of an entity (user or system), even if they are fundamentally trusted.
* **Implementation**: Requires robust, continuous logging mechanisms to monitor system and user behavior.
* **Automation**: Because manual verification (e.g., reviewing every single webpage a user visits) is unfeasible, this principle relies heavily on automated security mechanisms like proxies, Intrusion Detection Systems (IDS), and Intrusion Prevention Systems (IPS).

## [[Zero Trust]]
[attachment_0](attachment)
* **Core Concept**: Treats "trust" itself as a vulnerability. The guiding philosophy is strictly *"never trust, always verify."*
* **Default Stance**: Every entity is considered adversarial until proven otherwise, making it highly effective against insider threats.
* **No Inherent Trust**: Unlike older security models, it does *not* grant trust based on a device's physical location (e.g., being on the internal corporate network) or ownership.
* **Strict Access**: Requires strict authentication and authorization before accessing *any* resource. If a breach occurs, this architecture significantly contains the damage.
* **Balance**: Must be applied as much as feasible without negatively stalling core business operations.

### [[Microsegmentation]]
> [!note] A Practical Implementation of Zero Trust
* Shrinks network segments down so small that a segment can be a single host.
* Any communication between these micro-segments strictly requires authentication, Access Control List (ACL) checks, and other rigid security validations.

# Vulnerability, Threat, and Risk

> [!info] Core Definitions
> Understanding the distinction between these three terms is critical in information security to accurately assess and respond to dangers. 

* **[[Vulnerability]]**: A weakness in a system that makes it susceptible to attack or damage.
* **[[Threat]]**: A potential danger that is associated with or could exploit that specific weakness. 
* **[[Risk]]**: The *likelihood* of a threat actor successfully exploiting a vulnerability, combined with the consequent *impact* on the business or system.

## Practical Examples
* **Physical Example**: A showroom built with standard glass has a weakness (**Vulnerability**). The potential that someone could break the glass is the danger (**Threat**). The probability of a break-in actually happening and the resulting financial loss to the showroom owners is the **Risk**.
* **InfoSec Example**: A hospital uses a database with a known security flaw (**Vulnerability**). A working exploit code is released online, proving the danger is real and executable (**Threat**). The likelihood of an attacker using that code to compromise the hospital, and the resulting damage to patient data and operations, is the **Risk**.

# The Shared Responsibility Model

> [!info] Cloud Security Framework
> A framework used in cloud computing to define the security obligations of both the Cloud Service Provider (CSP) and the customer, ensuring all aspects of security are managed.
> 
> 
> 
> 
> 
>  
>  
>  
>  
>  
## Core Concept
* Security in a cloud environment is a joint effort.
* The specific responsibilities (e.g., hardware, network infrastructure, OS, applications) shift dynamically depending on the access level and type of cloud service being utilised.

## Division by Service Type
* **[[IaaS]] (Infrastructure as a Service)**: The user has high access and complete control (and responsibility) over the operating system, applications, and data. The provider only secures the underlying physical hardware and infrastructure.
* **[[SaaS]] (Software as a Service)**: The user has no direct access to the underlying OS or infrastructure. The provider handles the infrastructure, OS, and the application itself, while the user is primarily responsible only for their specific data and user access management.