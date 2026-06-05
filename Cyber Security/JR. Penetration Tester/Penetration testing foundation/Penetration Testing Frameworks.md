---
---

> [!note] What is a Framework?
> A **penetration testing framework** is a structured methodology that guides security professionals through every stage of an engagement, from initial planning and scoping to exploitation, reporting, and remediation validation. 

> [!warning] The Danger of Unstructured Testing
> Without a framework, testing becomes a disorganized collection of random checks. This leads to missed vulnerabilities, poor documentation, and potentially severe **legal trouble** if out-of-scope systems are targeted.

## Core Benefits

Following a structured methodology provides four primary advantages:

* **Thoroughness:** Ensures critical attack surfaces and structural elements are not overlooked.
* **Consistency:** Guarantees comparable and reliable results across different testers.
* **Compliance:** Aligns the assessment with strict regulatory requirements.
* **Communication:** Builds trust with clients and auditors using a recognized standard.

## Major Testing Frameworks

| Framework | Focus & Philosophy |
| :--- | :--- |
| **OSSTMM** | A scientific, metrics-driven approach to security testing. |
| **OWASP WSTG** | The definitive, go-to framework for **web application** assessments. |
| **NIST SP 800-115** | The U.S. government's technical guide to security testing. |
| **PTES** | A practical, phase-driven standard mirroring real-world engagements. |
| **ISSAF** | A historically influential methodology featuring a detailed 9-step model. |

## Complementary & Specialized Standards

> [!tip] Threat Mapping
> **MITRE ATT&CK** is highly recommended as a complementary knowledge base that maps real-world adversary tactics and techniques.

Depending on the environment, testers may also use specialized frameworks:
* **Web & Mobile:** WASC Threat Classification, OWASP MASTG (Mobile).
* **Cloud:** CSA Cloud Controls Matrix.
* **Financial/Regulatory:** PCI DSS Penetration Testing Guidelines, CBEST Framework.
---
---

# OSSTMM (Open Source Security Testing Methodology Manual)

> [!note] Core Philosophy
> Developed by ISECOM, OSSTMM v3 applies the **scientific method** to security testing. It prioritizes **metrics over opinions**, aiming to produce quantifiable, verifiable, and repeatable results rather than subjective narratives.

> [!important] Risk Assessment Values (RAVs)
> At the heart of OSSTMM is the **RAV**, which measures the balance between total attack surface (exposure) and existing controls. 
> * **Positive RAV:** Indicates residual risk.
> * **Near-Zero RAV:** Controls are well-matched to exposure.

## The Five Security Channels

OSSTMM assumes security goes beyond just networks, organizing assessments into five distinct channels to ensure no attack surface is overlooked:

* **HUMSEC (Human Security):** Social engineering and human-factor vulnerabilities.
* **PHYSSEC (Physical Security):** Physical access controls (e.g., badge readers, tailgating).
* **SPECSEC (Wireless Communications):** Electromagnetic signals like Wi-Fi, Bluetooth, and RFID.
* **COMSEC (Telecommunications):** Phone systems, VoIP, fax, and modems.
* **DATASEC (Data Networks):** Firewalls, network services, and application-layer protocols.

## The 4 Testing Phases

| Phase | Name | Objective | Practical Example |
| :--- | :--- | :--- | :--- |
| **1** | **Induction** | Enumeration & Verification | Querying DNS and confirming discovered assets are live and responsive. |
| **2** | **Interaction** | Qualification & Quantification | Probing assets, fingerprinting technology, and counting exposed services. |
| **3** | **Inquiry** | Privilege & Verification Escalation | Testing if measured exposure can be converted into unauthorized access (e.g., exploiting an IDOR). |
| **4** | **Intervention** | Quarantine, Audit, Enticement | Restricting endpoints, reviewing the access control model, and deploying canary tokens. |
## Reporting & Trade-offs

> [!tip] Standardized Output
> OSSTMM prescribes the **Security Test Audit Report (STAR)** format to enforce reporting consistency and cross-team comparability.

* **Strengths:** Scientific rigor makes results highly auditable, repeatable, and comparable (like engineering measurements).
* **Weaknesses:** Steep learning curve, time-consuming to implement fully, and fewer trained practitioners compared to OWASP or NIST methodologies.
# OWASP Web Security Testing Guide (WSTG)

> [!note] Overview
> The **OWASP WSTG** is a comprehensive, community-driven framework specifically designed for web application assessments. While the OWASP Top Ten catalogs vulnerabilities, the WSTG provides a practical, risk-based roadmap with over 90 discrete test cases.
## Core Structure

The framework organizes testing into 12 distinct categories that cover the full attack surface of modern web applications:
* Information Gathering & Configuration Management
* Identity, Authentication, & Authorization
* Session Management & Input Validation
* Error Handling & Cryptography
* Business Logic, Client-Side, & API Testing

## Security Across the SDLC

Unlike frameworks that treat testing as a single event, the WSTG embeds security throughout all five phases of the **Software Development Life Cycle (SDLC)**.

| SDLC Phase | Focus Area | Practical Example |
| :--- | :--- | :--- |
| **1. Before Development** | Requirements & Compliance | Defining regulatory obligations (e.g., PCI DSS) and patching criteria. |
| **2. Definition & Design** | Threat Modeling & Architecture | Designing rate-limiting and identifying high-value targets (e.g., APIs). |
| **3. Development** | Code Review | Vetting code against WSTG test cases to catch flaws like non-expiring tokens. |
| **4. Deployment** | Production Verification | Running penetration tests to verify configurations, TLS, and default credentials. |
| **5. Maintenance** | Post-Launch Operations | Re-executing tests after pushing new features to ensure no new vulnerabilities are introduced. |
## Trade-offs

> [!warning] The "Checklist" Trap
> Testers must avoid mechanically executing the WSTG test cases without critically assessing the application's overall risk posture.

* **Strengths:** Exhaustive, highly practical, continuously updated by the community, and covers modern architectures (SPAs, microservices).
* **Weaknesses:** Fully implementing all >90 test cases is resource-intensive, and some tests require highly specialized expertise (e.g., cryptographic analysis).
# NIST SP 800-115

> [!note] Overview
> **NIST Special Publication 800-115** is a comprehensive guide to information security testing and assessment. Highly trusted in U.S. federal and regulated environments, it treats penetration testing as just one of several validation techniques.
## Core Objectives

1. **Identify Vulnerabilities:** Find weaknesses in systems, networks, and applications.
2. **Validate Controls:** Ensure security mechanisms perform as expected under adversarial conditions.
3. **Assess Exploitability:** Simulate real-world attacks to determine if weaknesses can actually be leveraged.

## The 3 Testing Phases

| Phase | Focus Area | Key Activities |
| :--- | :--- | :--- |
| **1. Planning** | Scoping & Rules | Defining objectives, scoping assets, establishing communication protocols, and signing the formal test plan. |
| **2. Execution** | Active Testing | Progresses through four categories: **Review** (policies/configs), **Target ID** (scanning), **Vulnerability Validation** (confirming exploits), and **Penetration Testing** (full exploitation). |
| **3. Post-Testing** | Reporting | Analyzing results, categorizing risk severity, and delivering actionable remediation steps. |
> [!important] Execution Progression
> Phase 2 deliberately progresses from passive, low-impact reviews to active, high-impact exploitation. Not every assessment needs to reach the final penetration testing stage depending on the objectives.
> 
> 
## Trade-offs

* **Strengths:** High institutional credibility, adaptable to diverse infrastructure (Cloud, IoT), and standardizes testing quality across teams.
* **Weaknesses:** It is strictly *guidance* (lacks enforced audit frequencies or penalties like PCI DSS) and demands highly skilled personnel capable of performing the full spectrum of assessment techniques.
---
---

# Penetration Testing Execution Standard (PTES)

> [!note] Overview
> **PTES** is a practical, workflow-driven framework created by experienced security practitioners. Unlike frameworks that focus on *what* to test, PTES focuses on *how* a real-world engagement flows from the first client call to the final report.

## The 7 Phases of PTES

PTES maps directly to the lifecycle of an actual penetration test, answering the question: "I have a signed contract; now what?"

| Phase | Name | Key Activities & Objectives |
| :--- | :--- | :--- |
| **1** | **Pre-Engagement Interactions** | Defining scope, rules of engagement, testing windows, and securing legal authorization to prevent disputes. |
| **2** | **Intelligence Gathering** | Performing passive (OSINT) and active (scanning) reconnaissance to map the target environment. |
| **3** | **Threat Modeling** | Using gathered intel to identify high-value assets and map out the most likely adversarial attack paths. |
| **4** | **Vulnerability Analysis** | Systematically identifying and manually verifying weaknesses to eliminate false positives. |
| **5** | **Exploitation** | Purposefully exploiting confirmed vulnerabilities to demonstrate actual business impact (not just "popping boxes"). |
| **6** | **Post-Exploitation** | Pivoting, lateral movement, and data extraction to determine the real-world severity of the compromise. |
| **7** | **Reporting** | Delivering an Executive Summary (focusing on business risk) and a Technical Report (exact steps, evidence, and remediation). |

## Trade-offs

> [!warning] Outdated Technical Guidance
> While the core methodology and phase structure remain excellent, PTES has not been formally updated in years. Testers must supplement its phases with modern tool documentation.

* **Strengths:** Exceptional end-to-end playbook, highly practical for building workflow instincts, and deeply emphasizes crucial legal/scoping steps.
* **Weaknesses:** References outdated tools and lacks strict quantitative metrics (unlike OSSTMM), relying heavily on individual tester judgment.

# ISSAF (Information Systems Security Assessment Framework)

> [!note] Overview
> Developed by OISSG (last updated ~2006), **ISSAF** is a vintage but highly instructive open-source framework. While its specific tool guidance is obsolete, its underlying methodology perfectly mirrors an Advanced Persistent Threat (APT) lifecycle and the Cyber Kill Chain.
## The 3 Testing Phases

### Phase 1: Planning and Preparation
Establishing the engagement boundaries, scope, escalation protocols, and constraints before any testing begins.

### Phase 2: Assessment (The 9-Step Model)
> [!tip] The Attacker Progression
> This phase is ISSAF's lasting contribution, detailing how an adversary systematically deepens their network foothold.
1. **Information Gathering:** Reconnaissance using OSINT (DNS, WHOIS, job postings).
2. **Network Mapping:** Discovering live topologies, IP ranges, and services.
3. **Vulnerability Identification:** Scanning mapped assets for exploitable flaws.
4. **Penetration:** Initial exploitation of discovered vulnerabilities to gain a foothold.
5. **Privilege Escalation:** Elevating from initial access to administrative/system rights.
6. **Enumerating Further:** Using elevated access to map newly reachable internal assets.
7. **Lateral Movement:** Pivoting to compromise additional internal systems.
8. **Maintaining Access:** Establishing persistence (e.g., backdoors) to survive reboots.
9. **Covering Tracks:** Evading detection by altering or erasing logs and evidence.

### Phase 3: Reporting and Cleanup
Delivering a prioritized report based on business impact, followed by the strict removal of all test artifacts and temporary accounts from the target environment.

## Trade-offs

> [!warning] Unmaintained Status
> ISSAF should be studied strictly for its **methodology and adversarial logic**. Do not rely on it for technical procedures, as its tool references are over a decade out of date.
---
---

# MITRE ATT&CK Framework

> [!note] Overview
> **MITRE ATT&CK** (Adversarial Tactics, Techniques, and Common Knowledge) is not a traditional penetration testing framework. It is a globally accessible **knowledge base of adversary behavior** based on real-world observations. 

## The ATT&CK Matrix

The framework is structured as a large matrix that categorizes how threat actors operate:



* **Tactics (The "Why"):** The columns represent high-level adversary objectives (e.g., Initial Access, Privilege Escalation, Lateral Movement).
* **Techniques (The "How"):** The rows list specific methods used to achieve a tactic (e.g., Phishing `T1566`, Valid Accounts `T1078`).
* **Sub-Techniques:** More granular, specific variations of a technique (e.g., Spearphishing Attachment `T1566.001`).

Each entry includes descriptions, real-world examples, detection recommendations, and mitigations.

## A Complement, Not a Replacement

> [!important] The Universal Translator
> ATT&CK does not tell you *how* to run a test (like PTES or OSSTMM do). Instead, it provides a standardized vocabulary to describe *what* you found.
> 
> * **PTES** = The diagnostic procedure (steps to follow).
> * **ATT&CK** = The medical dictionary (standardized terminology).

## Mapping Findings to ATT&CK

By annotating a penetration test report with ATT&CK technique IDs, findings are elevated from simple vulnerability lists to narratives grounded in real-world threat behavior.

| Engagement Finding                              | ATT&CK Tactic     | ATT&CK Technique                                 |
| :---------------------------------------------- | :---------------- | :----------------------------------------------- |
| Phishing email delivered payload to workstation | Initial Access    | Phishing: Spearphishing Attachment (`T1566.001`) |
| Exploited Tomcat deserialization flaw           | Initial Access    | Exploit Public-Facing Application (`T1190`)      |
| Extracted cached domain credentials             | Credential Access | OS Credential Dumping (`T1003`)                  |
| Moved to file server using stolen credentials   | Lateral Movement  | Use Alternate Authentication Material (`T1550`)  |
| Accessed database from compromised server       | Collection        | Data from Information Repositories (`T1213`)     |
# Specialized Security Frameworks

> [!note] Core Principle
> Framework selection is driven by **context**. A penetration tester must choose the appropriate framework based on the client's industry, regulatory obligations, technology platform, and geographic jurisdiction.

## Key Frameworks

* **WASC Threat Classification:** An older web application vulnerability taxonomy. It is largely superseded by the OWASP Top Ten and WSTG, but still appears in legacy documentation.
* **CSA Cloud Controls Matrix (CCM):** A governance and compliance framework for cloud environments. It maps controls across 17 domains and is used for cloud security posture assessments rather than traditional pentesting.
* **OWASP MASTG:** The definitive testing guide for **Android and iOS** mobile applications. It is the mobile counterpart to the WSTG and tests against MASVS security requirements.
* **PCI DSS Penetration Testing Guidelines:** A strict regulatory mandate for organizations handling payment card data. It requires annual external and internal testing, plus validation of network segmentation.
* **CBEST:** A highly specialized, threat-intelligence-led penetration testing framework developed by the Bank of England for **UK financial institutions**.

## Framework Comparison

| Framework | Primary Domain | Type | Maintained | When to Use |
| :--- | :--- | :--- | :--- | :--- |
| **WASC** | Web applications | Threat taxonomy | No | Reviewing legacy references and historical context. |
| **CSA CCM** | Cloud environments | Governance controls | Yes | Conducting cloud security posture assessments. |
| **OWASP MASTG**| Mobile apps | Testing guide | Yes | Penetration testing Android/iOS client applications. |
| **PCI DSS** | Payment systems | Regulatory mandate | Yes | Any engagement involving cardholder data processing. |
| **CBEST** | UK Finance | Threat-intel testing | Yes | Engagements specifically with UK financial entities. |

> [!important] Regulatory Impact
> Frameworks like **PCI DSS** and **CBEST** are not optional suggestions; they are strict mandates that dictate both the scope and the required reporting format of your assessment.

# Choosing the Right Penetration Testing Framework

> [!note] Core Concept
> Selecting a framework is like a mechanic choosing tools. The "right" choice depends entirely on the client's industry, the target systems, regulatory requirements, and the engagement's objectives.

## 4 Key Selection Criteria

| Factor | Description & Impact |
| :--- | :--- |
| **1. Scope & Target Type** | Dictates the primary methodology (e.g., **WSTG** for web, **MASTG** for mobile, **PTES/OSSTMM** for broad networks). |
| **2. Regulation & Compliance** | Overrides personal preference. Card data mandates **PCI DSS**; UK banking mandates **CBEST**; U.S. Federal targets favor **NIST SP 800-115**. |
| **3. Quantifiable Results** | If a client needs objective year-over-year or cross-team comparisons, **OSSTMM** (via RAV metrics) is required. |
| **4. Team Expertise** | Frameworks like OSSTMM or CBEST require specialized skills. **PTES** offers a practical workflow for standard network assessments with smaller teams. |

## Practical Application Scenarios

> [!tip] Combining Frameworks
> Engagements rarely rely on a single framework. Typically, a primary framework structures the overall methodology, while supplementary frameworks address specific platforms, threats, or regulations.

| Scenario                     | Client Context                                                                                    | Recommended Framework(s) & Why                                                                            |
| :--------------------------- | :------------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------------------------------- |
| **1. Regional Hospital**     | Web portal & internal network; needs an end-to-end structure and a clear executive summary.       | **PTES** (overall structure/reporting) + **WSTG** (web portal tests) + **MITRE ATT&CK** (threat mapping). |
| **2. UK Multinational Bank** | UK-based; requires a bespoke, threat-intelligence-led assessment.                                 | **CBEST** (Specifically mandated for UK financial institutions).                                          |
| **3. SaaS Startup**          | Web platform being tested by multiple distinct firms over a two-year period to track improvement. | **OSSTMM** (provides RAV metrics for objective, cross-team comparison) + **WSTG** (web testing guide).    |
| **4. Fintech Mobile App**    | Android/iOS banking applications that process and handle credit card transactions.                | **OWASP MASTG** (mobile app testing cases) + **PCI DSS** (mandatory regulatory compliance for card data). |