# CISSP

These are my manually compiled notes for the CISSP exam.

- [CISSP](#cissp)
- [Domain 1: Security and Risk Management](#domain-1-security-and-risk-management)
  - [1.1 - Professional ethics](#11---professional-ethics)
  - [1.2 - Understand and apply security concepts](#12---understand-and-apply-security-concepts)
  - [1.3 - Evaluate and apply security governance principles](#13---evaluate-and-apply-security-governance-principles)
  - [1.4 - Determine compliance and other requirements](#14---determine-compliance-and-other-requirements)
  - [1.5 - Understand legal and regulatory issues that pertain to information security in a holistic context](#15---understand-legal-and-regulatory-issues-that-pertain-to-information-security-in-a-holistic-context)
  - [1.6 - Understand requirements for investigation types](#16---understand-requirements-for-investigation-types)
  - [1.7 - Develop, document, and implement security policies, procedures, standards, baselines, and guidelines](#17---develop-document-and-implement-security-policies-procedures-standards-baselines-and-guidelines)
  - [1.8 - Identify, analyze, and prioritize business continuity (BC) requirements](#18---identify-analyze-and-prioritize-business-continuity-bc-requirements)
  - [1.9 - Contribute to and enforce personnel security policies and procedures](#19---contribute-to-and-enforce-personnel-security-policies-and-procedures)
  - [1.10 - Understand and apply risk management concepts](#110---understand-and-apply-risk-management-concepts)
  - [1.11 - Understand and apply threat modeling concepts and methodologies](#111---understand-and-apply-threat-modeling-concepts-and-methodologies)
  - [1.12 - Apply supply chain risk management (SCRM) concepts](#112---apply-supply-chain-risk-management-scrm-concepts)
  - [1.13 - Establish and maintain a security awareness, education, and training programs](#113---establish-and-maintain-a-security-awareness-education-and-training-programs)
- [Domain 2: Asset Security](#domain-2-asset-security)
  - [2.1 - Identify and classify information and assets](#21---identify-and-classify-information-and-assets)
  - [2.2 - Establish information and asset handling requirements](#22---establish-information-and-asset-handling-requirements)
  - [2.3 - Provision resource securely](#23---provision-resource-securely)
  - [2.4 - Manage data life cycle](#24---manage-data-life-cycle)
  - [2.5 - Ensure appropriate asset retention](#25---ensure-appropriate-asset-retention)
  - [2.6 - Determine data security controls and compliance requirements](#26---determine-data-security-controls-and-compliance-requirements)
- [Domain 3: Security Architecture and Engineering](#domain-3-security-architecture-and-engineering)
  - [3.1 - Research, implement, and manage engineering processes using secure design principles](#31---research-implement-and-manage-engineering-processes-using-secure-design-principles)
  - [3.2 - Understand the fundamental concepts of security models](#32---understand-the-fundamental-concepts-of-security-models)
  - [3.3 - Select controls based upon systems security requirements](#33---select-controls-based-upon-systems-security-requirements)
  - [3.4 - Understand security capabilities of information systems (IS)](#34---understand-security-capabilities-of-information-systems-is)
  - [3.5 - Assess and mitigate the vulnerabilities of security architectures, designs, and solution elements](#35---assess-and-mitigate-the-vulnerabilities-of-security-architectures-designs-and-solution-elements)
  - [3.6 - Apply cryptography](#36---apply-cryptography)
  - [3.7 - Understand methods of cryptanalytic attacks](#37---understand-methods-of-cryptanalytic-attacks)
  - [3.8 - Apply security principles to site and facility design](#38---apply-security-principles-to-site-and-facility-design)
  - [3.9 - Design site and facility security controls](#39---design-site-and-facility-security-controls)
- [Domain 4: Communication and Network Security](#domain-4-communication-and-network-security)
  - [4.1 - Implement secure design principles in network architectures](#41---implement-secure-design-principles-in-network-architectures)
  - [4.2 - Securee network components](#42---securee-network-components)
  - [4.3 - Implement Secure Communication Channels According to Design](#43---implement-secure-communication-channels-according-to-design)
    - [Common Tunneling Protocols](#common-tunneling-protocols)
    - [SSL/TLS Handshake Protocol](#ssltls-handshake-protocol)
    - [TLS VPN vs. IPsec VPN](#tls-vpn-vs-ipsec-vpn)
    - [Remote Authentication Protocols](#remote-authentication-protocols)
- [Domain 5: Identity and Access Management (IAM)](#domain-5-identity-and-access-management-iam)
  - [5.1 - Control physical and logical access to assets](#51---control-physical-and-logical-access-to-assets)
  - [5.2 - Manage Identification and Authentication of People, Devices, and Services](#52---manage-identification-and-authentication-of-people-devices-and-services)
    - [Identification](#identification)
    - [Authentication](#authentication)
    - [Single Sign-On (SSO)](#single-sign-on-sso)
    - [CAPTCHA](#captcha)
    - [Session Management](#session-management)
    - [Identity Proofing](#identity-proofing)
    - [NIST SP 800-63B Authentication Levels](#nist-sp-800-63b-authentication-levels)
    - [Federated Identity Management (FIM)](#federated-identity-management-fim)
    - [SAML Workflow](#saml-workflow)
    - [Just-in-Time Access](#just-in-time-access)
  - [5.3 - Federated Identity with a Third-Party Service](#53---federated-identity-with-a-third-party-service)
    - [Risks of IDaaS](#risks-of-idaas)
  - [5.4 - Implement and manage authorization mechanisms](#54---implement-and-manage-authorization-mechanisms)
  - [5.5 - Manage the identity and access provisioning life cycle](#55---manage-the-identity-and-access-provisioning-life-cycle)
- [Domain 6: Security Assessment and Testing](#domain-6-security-assessment-and-testing)
  - [6.1 - Design and validate assessment, test, and audit strategies](#61---design-and-validate-assessment-test-and-audit-strategies)
  - [6.2 - Conduct Security Control Testing](#62---conduct-security-control-testing)
    - [Testing Lifecycle](#testing-lifecycle)
    - [Types of Testing](#types-of-testing)
    - [Application Security Testing](#application-security-testing)
    - [Fuzz Testing](#fuzz-testing)
    - [Testing Strategies](#testing-strategies)
    - [Test Design Techniques](#test-design-techniques)
    - [Vulnerability Assessments and Penetration Testing](#vulnerability-assessments-and-penetration-testing)
      - [Penetration Testing Steps:](#penetration-testing-steps)
    - [Types of Testing Approaches](#types-of-testing-approaches)
    - [Vulnerability Management](#vulnerability-management)
    - [Automated Tools](#automated-tools)
    - [Common Vulnerability Resources](#common-vulnerability-resources)
    - [Log Management and Monitoring](#log-management-and-monitoring)
      - [Log Data Lifecycle:](#log-data-lifecycle)
      - [Log File Management:](#log-file-management)
    - [Monitoring Techniques](#monitoring-techniques)
    - [Regression Testing](#regression-testing)
  - [6.3 - Collect security process data](#63---collect-security-process-data)
  - [6.4 - Analyze test output and generate report](#64---analyze-test-output-and-generate-report)
  - [6.5 - Conduct or facilitate security audits.](#65---conduct-or-facilitate-security-audits)
  - [6.3 - Collect Security Process Data](#63---collect-security-process-data-1)
  - [6.4 - Analyze Test Output and Generate Report](#64---analyze-test-output-and-generate-report-1)
  - [6.5 - Conduct or Facilitate Security Audits](#65---conduct-or-facilitate-security-audits-1)
    - [Audit Planning](#audit-planning)
    - [Audit Standards](#audit-standards)
    - [SOC Reports](#soc-reports)
      - [SOC Report Types:](#soc-report-types)
    - [Audit Roles and Responsibilities](#audit-roles-and-responsibilities)
- [Domain 7: Security Operations](#domain-7-security-operations)
  - [7.1 - Understand and comply with investigations](#71---understand-and-comply-with-investigations)
  - [7.2 - Conduct logging and monitoring activities](#72---conduct-logging-and-monitoring-activities)
  - [7.3 - Perform configuration management (CM)](#73---perform-configuration-management-cm)
  - [7.4 - Apply foundational security operations concepts](#74---apply-foundational-security-operations-concepts)
  - [7.5 - Apply resource protection techniques](#75---apply-resource-protection-techniques)
  - [7.6 - Conduct incident management](#76---conduct-incident-management)


Sources include:
* Witcher, Rob, John Berti, Lou Hablas, and Nick Mitropoulos. Destination CISSP: A Concise Guide. 1st ed. Destination Certification, Inc., 2022.

# Domain 1: Security and Risk Management

## 1.1 - Professional ethics

CISSP holders are subject to the (ISC)^2 Code of Professional Ethics whose preamble holds to the following: "The safety and welfare of society and the common good, duty to our principals, and to each other, requires that we adhere to, and be seen to adhere, to the highest ethical standards of behavior."

The (ISC)^2 Code of Ethics holds the following canons, prioritized in order of importance:
1. Protect society, the common good, necessary public trust and confidence, and the infrastructure
2. Act honorably, honestly, justly, responsibly, and legally
3. Provide diligent and competent service to principals
4. Advance and protect the profession

## 1.2 - Understand and apply security concepts

The CIA triad consists of confidentiality, integrity, and availability:
* Confidentiality: Protects assets using important principles such as need-to-know and least privilege; prevents unauthorized disclosure
* Integrity: Protects and adds value to assets by making them more accurate, more timely, more current, more meaningful; prevents unauthorized or accidental changes to assets such as information
* Availability: Protects critical assets based on value to ensure organizational assets are available when required by stakeholders

These three pillars have been supplemented by two more:
* Authenticity: Proves assets are legitimate and bona fide, and verifies that they are trusted and verified. Proves the source and origin of important valuable assets. Also referred to as "proof of origin".
* Nonrepudiation: Provides assurance that someone cannot dispute the validity of something; the inability to refute accountability or responsibility. Also, inability to deny having done something.

## 1.3 - Evaluate and apply security governance principles

Governance is intended to enhance organizational value based on the goals and objectives of the organization. Security must be managed top down.

Two key parts of this are (1) scoping and (2) tailoring. Scoping determines which potential control elements are in scope. Tailoring then considers in scope security control elements and further refines or enhances them so they are most effective and aligned with the goals of the organization.

When enforcing these policies, only one person, group, or entity should be accountable for security, even if everyone else is also responsible for doing their part. At the highest level, the Board of Directors is ultimately accountable for security.

At a high level:
* Owners/Controllers/Senior Management are *accountable* for (1) ensuring appropriate security controls are implemented, (2) determining the appropriate sensitivity or classification levels, and (3) determining access privileges
* Security Professionals/IT Security Officers are responsible for design, implementation, management, and review of the organization's security policies, standards, baselines, procedures, and guidelines
* IT Officers are responsible for developing and implementing technology solutions, working with the security officers to evaluate security strategies, and working closely with Business Continuity managers to ensure continuity of operations should disruption occur
* IT Function is responsible for implementing and adhering to security policies
* Operators/Administrators are responsible for managing, troubleshooting, and applying hardware and software patches to systems as necessary, managing user permissions per owner specifications, and administering and managing specific applications and services
* Network Administrators are responsible for maintaining computer networks and installing and configuring networking equipment and resolving problems
* Information system auditors are responsible for providing management with independent assurance that the security objectives are appropriate, determining whether the security policy, standards, baselines, procedures, and guidelines are appropriate and effective given the company's security objectives, and determining whether the objectives have been met
* Users are responsible for adherence to security and ToS policies. Preserving the availability, integrity, and confidentiality of assets when accessing and using them

A key distinction lies in custodians versus owners. Custodians are caretakers or users of an asset who may not necessarily own that asset but are entrusted with its protection. However, accountability for the asset will always rest ultimately on the asset owner.

"Due care" includes the accountable protection of assets based on the goals and objectives of the organization while "Due diligence" is the ability to prove due care to stakeholders. For example, due care is requesting a penetration test. Due diligence is providing proof of addressing the vulnerabilities uncovered during that pentest.

## 1.4 - Determine compliance and other requirements

There are a number of core concepts at the root of compliance:
* Laws - These are laws that an organization must comply with based on where their assets are owned or managed given the respective jurisdiction or industry the company operates in (e.g. HIPAA, FERPA, GDPR)
* Regulations - The specific regulations an organization must comply with based on the jurisdiction or industry of their assets (e.g. ITAR, EAR)
* Industry Standards - Specific industry standards to guide organizational activities (e.g. NIST, ISO)
* Import/Export Controls
* Transborder Data Flow Regulations
* Assets
* Personal Data
* Corporate Policies

## 1.5 - Understand legal and regulatory issues that pertain to information security in a holistic context

Cybersecurity must often factor in time and expense, not only for the organization. Many attacks can be effectively mitigated by making their object too time consuming or expensive to reach. At the end of the day, one must ask how the assets are protected, what are the issues pertaining to information security, and what does the current threat landscape look like?

Intellectual property is an intangible product of human intellect protected by law from unauthorized use. There are four primary kinds of IP:
* Trade secret: This is business information that need not be disclosed and is legally protected from misappropriation
* Patent: Functional innovations that are protected for a set period of time if properly disclosed.
* Copyright: The expression of idea in a fixed medium that is also protected for a set period of time if properly disclosed.
* Trademark: Color, sound or symbols used to distinguish products and companies from another. This is primarily rooted in protecting consumers from confusion

In addition to this, there are import and export controls at the national law to manage which products, technologies, and information can move in and out of those countries:
* The Wassenaar Arrangement is an agreement by a number of countries to share cryptographic information with one another while also protecting it from terrorists
* International Traffic in Arms Regulation (ITAR) is a US regulation built to ensure control over any exports of items belonging to the United States Munitions List (USML). It falls under the Department of State's Directorate of Defense Trade Controls (DDTC)
* Export Administration Regulations (EAR) focuses on export controls around commercial use items. It falls under the US Department of Commerce's Bureau of Industry and Security (BIS)

Privacy also falls under legal scope. Privacy is the state or condition of being free from being observed or disturbed by other people. This can include Personally Identifiable Information (PII), Personal Health Information (PHI), Sensitive Personal Information (SPI), and Personal Cardholder Information (PCI). These combined with IP are sensitive data.

Personal data may include direct or indirect identifiers. Direct identifiers are unique identifiers for an individual (phone number, SSN) while indirect identifiers may be used to narrow the scope of identification (age, zip code).

Privacy policies are required to speak to how:
* Data owners have defined accountabilities regarding the collection and protection of customer data
* Data custodians need to have defined responsibilities based on the input of owners
* Data processors need to have clearly defined responsibilities when processing data on behalf of the owner
* Data subjects which are the individuals that the data belongs to

General Data Protection Regulation (GDPR) is a single set of rules that applies to all EU member states. Each state has a Supervisory Authority (SA) to hear and investigate complaints. Data subjects have the right to lodge a complaint with a SA. There are seven principles that describe lawful processing of personal data: lawfulness, purpose limitation, data minimization, accuracy, storage limitation, integrity and confidentiality, and lastly accountability. GDPR also stipulates that privacy breaches must be reported within 72 hours.

The Organization for Economic Cooperation and Development (OECD) also has its own set of standards and policies. These include the following principles:
* Collection Limitation Principle: Limit the collection of personal data to only what is needed to provide a service.
* Data Quality Principle: Personal data should be relevant, accurate, and complete, and it should be kept up to date.
* Purpose Specification Principle: The purposes for which personal data is collected should be clearly specified at the time of collection.
* Use Limitation Principle: Personal data should only be used based on the purposes for which it was collected and with consent of the data subject or by authority of law; in other words, if an organization says it has collected personal data for a specific purpose, they should only use the personal data for that purpose.
* Security Safeguards Principle: Personal data should be protected by reasonable security safeguards against loss, unauthorized access, destruction, use, modification, etc. Essentially, security controls must be put in place, because privacy is unattainable without security.
* Openness Principle: The culture of the organization collecting personal data should be one of openness, transparency, and honesty about how personal data is being used and in what context.
* Individual Participation Principle: When an individual--data subject--provides personal data to an organization, that individual should have the right to obtain their data from the data controller as well as have their data removed. In other words, the individual should have the chance to participate or choose whether to share their personal information or withhold it. The term data subject refers to the individual to whom the personal data pertains.
* Accountability Principle: A data controller should be accountable for complying with the other principles. What this basically means is that an organization collects personal data is now accountable for the protection of that information.

Because of the importance of privacy, Privacy Impact Assessments (PIA) and Data Protection Impact Assessments (DPIA) may be taken to assess an organization's handling of data in accordance with privacy standards. PIA takes the following steps:
1. Identify the need for a DPIA
2. Describe the data processing
3. Assess necessity and proportionality
4. Consult interested parties
5. Identify and assess risks
6. Identify measures to mitigate the risks
7. Sign off and record outcomes
8. Monitor and review

## 1.6 - Understand requirements for investigation types
Cf. 7.1

## 1.7 - Develop, document, and implement security policies, procedures, standards, baselines, and guidelines

The board or CEO often institutes an Overarching Security Policy which is a broader statement of goals and objectives that also grants authority to security activities. Functional policies then extend this to provide more specificity that can be applied to:
* Standards: Specific hardware and software solutions, mechanisms, and products
* Procedures: Step-by-step descriptions on how to perform a task
* Baselines: Defined minimal implementation methods/levels for security mechanisms and products
* Guidelines: Recommended or suggested actions

## 1.8 - Identify, analyze, and prioritize business continuity (BC) requirements
Cf. 7.1

## 1.9 - Contribute to and enforce personnel security policies and procedures

Personnel security includes controls like:
* Personnel security policies implemented through procedures, and regularly reviewed
* Candidate screening and hiring
* Employment agreements and policies through separation of duties and job rotation
* Process for employee duress
* Need-to-know and least privilege
* Nondisclosure agreements (NDA's)
* Noncompete agreements (NCA's)

## 1.10 - Understand and apply risk management concepts

Risk management is the identification, assessment, and prioritization of risks and the cost-efficient application of resources to minimize the probability or impact of these risks.

It consists of the following steps:
1. Value: Quantitative or qualitative value analysis
2. Risk Analysis: Threat, Vulnerability, Impact, Probability/likelihood
3. Treatment: Avoid, Transfer, Mitigate, Accept

Risk = (threat * vulnerability / probability) * impact

Risk management terms include:
* Threat agent: Entity that has the potential to cause damage to an asset
* Threat: Any potential danger
* Attack: Any harmful action that exploits a vulnerability
* Vulnerability: A weakness in an asset that could be exploited by a threat
* Risk: Significant exposure to a threat or vulnerability
* Asset: Anything valued by the organization
* Exposure/Impact: Negative consequences to an asset if the risk is realized
* Countermeasures and Safeguards: Controls implemented to reduce threat agents, threats, and vulnerabilities and reduce the negative impact of a risk being realized
* Residual risk: The risk that remains after countermeasures and safeguards are implemented

One form of quantitative risk analysis is the Annualized Loss Expectancy which expresses the annual cost of the risk materializing.

ALE = SLE * (AV * EF ) * ARO

* Single Loss Expectancy (SLE) - Denotes the cost if the risk occurs once
* Asset Value (AV) - The cost of the asset in a monetary value
* Exposure Factor (EF) - Measured as a percentage and expresses how much of the asset's value stands to be lost in case of a risk materializing
* Annualized Loss Expectancy - Expresses the annual cost of the risk materializing.

Risk can be managed by:
1. *Avoiding* it, which means not doing the thing that exposes the risk
2. *Transferring* it, by moving the cost of the risk elsewhere, such as an insurance company
3. *Mitigate* it, implement controls to reduce the risk to an acceptable level
4. *Accept* it, the asset owner or senior management (the accountable party) accepts the risk.

There are seven major kinds of risk controls:
1. Directive controls - direct, confine, or control the actions of subjects to encourage compliance with security policies.
2. Deterrent controls - discourage violation of security policies
3. Preventive controls - prevent undesired action or events.
4. Detective controls - identify if the risk has occurred.
5. Corrective controls - minimize the negative impact of a risk occurring.
6. Recovery controls - recover the system or process and return to normal operation.
7. Compensating controls - auxiliary support of other controls.

These can be combined in some form of preventive, detective, and corrective controls to create *complete control*. Safeguards are proactive controls that are designed to deter or prevent the risk. Countermeasures are reactive controls that are triggered after the risk has occurred, whether that be administrative, technical, or physical.

Each control should have a functional aspect (it works) and an assurance aspect (it can be proven that it works).

Security controls should be selected or advocated based on the appropriate risk metrics for an organization and presented to the decision-making audience in a way that relates it to a big picture.

Risk management should undergo continuous improvement through the four steps of plan, do, check, and act.

Risk management must be applied to every link in the supply chain, including vendors and service providers.

There are four primary risk management frameworks:
1. NIST SP 800-37 (RMF) - describes the risk management framework (RMF) and provides guidelines for applying it to information systems and organizations.
2. ISO 31000 - a family of standards and best practices relating to risk management.
3. COSO - a definition to essential enterprise risk management components, reviewing ERM principles and concepts
4. ISACA Risk IT Framework - ISACA's Risk IT Framework contains guidelines and practices for risk optimization, security, and business value. It aligns with COBIT.

For the CISSP exam, NIST SP 800-37 Rev. 2 should be the focus.
1. Prepare to execute the RMF
2. Categorize Information Systems
3. Select Security Controls
4. Implement Security Controls
5. Assess Security Controls
6. Authorize Information System
7. Monitor Security Controls

## 1.11 - Understand and apply threat modeling concepts and methodologies

Threat modeling is the systematic identification, enumeration, and prioritization of threats related to an asset. Three major threat modeling methodologies are STRIDE, PASTA, and DREAD.

STRIDE was developed by Microsoft and focuses on applications and operating systems.
S - Spoofing - This violates authentication
T - Tampering - This violates integrity
R - Repudiation - This violates nonrepudiation
I - Information Disclosure - This violates confidentiality
D - Denial of Service - This violates availability
E - Elevation of Privilege - This violates authorization

The Process for Attack Simulation and Threat Analysis (PASTA) is attacker-focused and includes seven stages.
1. Define objectives related to application risk profiles
2. Define technical scope so as to decompose the technology stack into understandable and assessable components.
3. Application decomposition understands the data flows among application components and services in the application threat model
4. Threat analysis reviews threat assertions from within the environment
5. Vulnerability and weakness analysis at the level of application design and code and how those correlate with the findings from threat analysis
6. Attacking modeling emulates attacks that could exploit identified weaknesses/vulnerabilities.
7. Risk and impact analysis centers around remediating vulnerabilities and weaknesses in code or design that can facilitate attack patterns.

DREAD is often used to rank threat severity in conjunction with STRIDE with each key point having an assigned score of 1-3.
D - Damage
R - Reproducibility
E - Exploitability
A - Affected Users
D - Discoverability

Social engineering consists of deception or intimidation to get people to provide sensitive information that should not be accessible to the exploiting party. The most common forms of social engineering include but are not limited to: phishing, spear phishing, whaling, smishing, vishing, pretexting, baiting, or tailgating and piggybacking.

## 1.12 - Apply supply chain risk management (SCRM) concepts

Security must be considered for all acquisitions during the procurement process, and security requirements must be clearly communicated to suppliers/vendors/service providers.

These include:
* Service Level Requirements (SLR's) that outline detailed service descriptions, service level targets, and mutual responsibilities.
* Service Level Agreements (SLA's) that include service levels, governance, security, compliance, and outlines liability and indemnification.
* Service Level Reports issued by the vendor or service provider that includes achievement of metrics defined in the SLA, identification of issues, reporting channels, management, and third party SOC reports.

## 1.13 - Establish and maintain a security awareness, education, and training programs

Everyone in the organization is responsible for security and this consists of promoting awareness, pinpointed training, and general education on fundamental concepts. These trainings should prioritize topics, review concepts regularly, and evaluate program effectiveness.

# Domain 2: Asset Security

## 2.1 - Identify and classify information and assets

Asset classification is assigning assets the level of protection they require, based on their value to the organization.

Classifying information can help identify critical information, its sensitivity to modification as well as promote commitment to both protecting valuable assets and keeping it confidential where applicable.

Data classification ensures that data receives an appropriate level of protection. It consists of the following steps:
1. Asset inventory
2. Determine and assign ownership
3. Classify based on value
4. Protect and handle based on classification
5. Assess and review

Classification orders objects and their classes according to value while categorization sorts objects into defined classifications.

This depends on either labeling for system-readable association of security attributes with subjects and objects represented by internal data structures, based on system-based enforcement, while marking is human-readable association of security attributes with objects for process-based enforcement.

## 2.2 - Establish information and asset handling requirements

Handling requirements are based on the classification of the asset, not the type of media. The asset owner determines who may access media, and the requirements for its storage, retention, and destruction.

## 2.3 - Provision resource securely

The owner is ultimately accountable for an asset including how it is classified and categorized, managing its access, and ensuring appropriate controls are in place based on the asset's classification. Owners can delegate responsibility for an asset, but they always remain accountable for the protection of the asset. In other words, accountability cannot be delegated to anyone else. The owner can delegate the responsibility, but accountability remains with the owner.

The data owner is accountable for the protection of data, the data processor is responsible for processing it on behalf of the owner, the custodian has the technical responsibility for data, the data steward has the business responsibility for data, and the data subject is the entity to which the data pertains.

## 2.4 - Manage data life cycle

Data must be protected in each stage of its lifecycle whether it be creation, collection, its update, storage, use, sharing, archiving, or final disposal and destruction.

When destroying data, defensible destruction means being able to prove there is no possible way for anyone to recover the data.

To destroy data is to physically destroy the media and is most effective. To purge uses logical/physical techniques to sanitize data so that it cannot be reconstructed. Clearing data uses logical techniques but the data may be reconstructed. Various techniques that employ this include incineration, shredding, disintegration, drilling holes, degaussing, crypto shredding, overwriting, wiping, erasing, or formatting.

Object reuse is a security method that overwrites data from media. SSD's present a problem for this with how they use flash memory technology to represent binary data. SSD vendors generally provide their own tools to securely remove data. If all else fails, it can be physically destroyed.

## 2.5 - Ensure appropriate asset retention

Some forms of data may be required to be retained for certain periods of time, sometimes for years. These are determined by laws, regulations, and industry standards. Data archival depends on understanding the media type, the security requirements, the availability requirements, the retention period, and associated costs.

## 2.6 - Determine data security controls and compliance requirements

Security controls often need to be determined for the different states of data:
* At rest - encryption, access control, backup + restoration
* In transit - access control, network encryption
* In use - homomorphic encryption, RBAC, DRP, DLP

Network encryption can take the following forms:
* End-to-end encryption means the data portion of a packet is encrypted when transmitted from the source node and remains encrypted through every node until it reaches its destination.
* Link encryption means the packet header and data are encrypted between each node but decrypted and re-encrypted again with each hop. This is not a very secure option.
* Onion encryption is the most secure of the three as multiple layers of encryption are wrapped at the first node then each subsequent node unwraps one layer and passes it along. This can help provide anonymity and confidentiality to the data.

Beyond this information can also be obfuscated through concealing, pruning, fabricating, trimming, or encrypting data.

Digital Rights Management (DRM) protects intellectual property assets in their use, modification, or sharing, as stipulated by NIST SP 500-241. DRM technologies have been developed to protect mass-produced media, and these same technologies have been adopted for protecting sensitive organization data using Information Rights Management (IRM) tooling.

Data Loss Prevention (DLP) is a system's ability to identify, monitor, and protect data in use, motion and at rest using deep packet content inspection, contextual security analysis of the transaction originator, data object, medium, timing, recipient, destination, etc. within a centralized management framework.

# Domain 3: Security Architecture and Engineering

## 3.1 - Research, implement, and manage engineering processes using secure design principles

Architecture, broadly speaking, implies the cohesion of multiple components inside a unified umbrella framework. Security architecture is the security (policies, knowledge, experience) that is superimposed upon the enterprise's architecture to protect the organization. Engineering consists of designing a solution by walking through a series of steps and phases to put the components together so they can work in harmony as an architecture.

Secure design principles include threat modeling, least privilege, defense in depth, secure defaults, fail securely, separation of duties, keep it simple, zero trust, trust but verify, privacy by design, shared responsibility.

Zero Trust can be broken down into eight principles:
1. Know your architecture
2. Know your identities
3. Know the health of users, devices, and services
4. Use policies to authorize requests
5. Authenticate everywhere
6. Focus your monitoring
7. Don't trust any network
8. Choose services designed for zero trust

## 3.2 - Understand the fundamental concepts of security models

A security model is a representation of what security should look like in an architecture being built. Some of these models include Bell-LaPadula, Biba, Clark-Wilson, and Brewer-Nash (the Chinese Wall model).

Security is only as strong as the weakest link in the chain, every component of the architecture should be fully decomposed and understood.

The three most popular enterprise security architecture models are Zachman, Sherwood Applied Business Security Architecture (SABSA), and The Open Group Architecture Framework (TOGAF).

Lattice-based models like Bell-LaPadula and Biba visualize security as layers. Rule-based models provide specific rules to dictate security (e.g. Information Flow, Clark-Wilson, Brewer-Nash, Graham-Denning, Harrison-Ruzzo-Ullmann).

![Bell-LaPadula Model](https://media.geeksforgeeks.org/wp-content/uploads/20200709152516/BellLaPadula.png)

The Bell-LaPadula model consists of three principles:
1. Simple security property - "no read up" - you cannot access data at a higher security level
2. Star property - "no write down" - data cannot move from more secure to less secure environments.
3. Strong star property - A subject should only be able to read or write at their layer


![Biba Model](https://media.geeksforgeeks.org/wp-content/uploads/20200709152715/Biba.png)

The Biba model focuses on data integrity:
1. Simple integrity property - "no read down" - you cannot depend on data with a lower level of integrity
2. Star property - "no write up" - you cannot write to data that is has a higher integrity level than yours
3. Invocation property - Data cannot be sent to someone with a higher layer of integrity


The Lipner implementation combines these two models.


Rule-based models feature several common rules. Information flow models track the flow of data throughout its lifecycle. Covert channels are unintentional communication paths that may lead to the disclosure of confidential information.

The Clark-Wilson model focuses on integrity by focusing on three goals:
1. Prevent unauthorized subjects from making changes
2. Prevent authorized subjects from making bad changes
3. Maintain system consistency

These can be upheld by three specific rules:
1. Well-formed transactions
2. Separation of Duties
3. Access Triple - Subject, Program, Object

The Brewer-Nash model is concerned with preventing a conflict of interest by separating departments or environments (e.g. development + production).

The Graham-Denning Model provides rules for allowing subjects to access objects.

The Harrison-Ruzzo-Ullman model provides a finite set of rules for editing the access rights of a subject. Generic rights can be added to groups of individuals.

---

Security implementation can be certified and accredited. Certification is the comprehensive technical analysis of a solution to ensure it meets the desired needs, while accreditation is concerned with official management signoff of certification for a set period of time.

Evaluation criteria include TCSEC (the Orange book) and ITSEC.

The Orange book was developed by the Department of Defense in the early 80s and has a number of functional levels:
* A1 - Verified Design
* B3 - Security labels, verification of no covert channels, and must stay secure during start up
* B2 - Security labels and verification of no covert channels
* B1 - Security labels
* C2 - Strict login procedures
* C1 - Weak protection mechanisms
* D1 - Failed or was not tested

The Orange book is only concerned with *confidentiality*.

ITSEC was published in 1991 and gained traction in European security decision making. ITSEC improves upon CTSEC by including coverage for network environments and ways to measure function:
* E6 - Formal end-to-end security tests + source code reviews
* E5 - Semi-formal system + unit tests and source code review
* E4 - Semi-formal system + unit tests
* E3 - Informal system + unit tests
* E2 - Informal system tests
* E1 - System in development
* E0 - Inadequate assurance

Both of these were replaced in 2005 by Common Criteria (ISO 15408) which provides industry wide confidence outside national boundaries.

Its components include:
* Protection Profile - The security capabilities that a type or category of security products should possess.
* Target of Evaluation - The vendor's product to be assessed
* Security Targets - what is covered by the assessment both in terms of functional and assurance capabilities.

Based upon this evaluation, the TOE is provided an Evaluation Assurance Level (EAL) grade:
* EAL7 - Formally verified, designed, and tested
* EAL6 - Semi-formally verified, designed, and tested
* EAL5 - Semi-formally designed and tested
* EAL4 - Methodically designed, tested, and reviewed
* EAL3 - Methodically tested and checked
* EAL2 - Structurally tested
* EAL1 - Functionally tested

However, products that are above EAL4 typically do not sell well because of their strict requirements and overhead to maintain. Once attained though, the EAL level will be maintained for that product throughout its lifecycle even through patches or firmware updates.

## 3.3 - Select controls based upon systems security requirements

Only a particular set of security control frameworks are applicable to any one organization. There are a number of security frameworks with particular sets of focus and scope.

* COBIT - The Control Objectives for Information Technologies is particularly useful for IT assurance in the case of audits or gap assessments. It was created by ISACA
* ITIL - Information Technology Infrastructure Library defines the processes in a well-run IT department.
* NIST SP 800-53 - This is a set of best practices, standards, and recommendations that help an organization improve its cybersecurity controls.
* PCI DSS - The Payment Card Industry Data Security Standard helps handle credit card data.
* ISO-27001 specifies the requirements for establishing, implementing, maintaining, and continually improving an information security management system within the context of an organization. It also includes requirements for the assessment and treatment of information security risks tailored to the needs of an organization. Organizations can be certified against ISO 27001
* ISO 27002 provides guidelines for organizational information security standards and information security management practices.
* COSO - The Committee of Sponsoring Organization of the Treadway Commission (COSO) is a voluntary private sector initiative dedicated to improving organizational performance and governance.
* HIPAA - Health Insurance Portability and Accountability Act (HIPAA) relates to security controls in the healthcare industry for PHI.
* FISMA - Federal Information Security Management (FISMA) Act of 2002 requires US federal agencies to develop, document, and implement agency-wide security programs to information securities for federal operations.
* FedRAMP - Federal Risk and Authorization Management Program (FedRAMP) provides a standardized approach to security asessment, authorization, and continuous monitoring for cloud products and services. Any cloud services that hold US federal government data must be FedRAMP authorized.
* SOX - Sarbanes-Oxley (SOX) Act is a direct result of Enron's fraud and is designed to prevent financial fraud.

## 3.4 - Understand security capabilities of information systems (IS)

A subject is an active entity (person, process, or program) that tries to access an object. An object is a passive entity (file, server, hardware) that is accessed by a subject.

The Reference Monitor Concept (RMC) is the concept of a subject accessing an object through a mediation based on a set of rules. The RMC must mediate all access, be protected from modification, be verifiable as correct, and always be invoked.

The implenentation of the RMC is known as a security kernel. An implemented security kernel should contain three properties:
1. Completeness: Impossible to bypass
2. Isolation: Mediation rules are tamperproof
3. Verifiability: Logging and monitoring and testing to ensure the kernel functions as intended

The Trusted Computing Base (TCB) encompasses all security controls that would be implemented to protect an architecture. This term refers to the protection mechanisms within a system or architecture. The TCB is the totality of protection mechanisms within an architecture. Processors, memory, storage, firmware, OS, and the system kernel are all found in the TCB.

A central processing unit (CPU) is the brain of a computer that processes all of the instructions. It operates through four steps of fetching, decoding, executing, and storing. CPU's operate in one of two processor states: the supervisor or problem state. These states are privilege levels (i.e. restrictions on the operations that can be performed). The supervisor state has a higher privilege level and is typically where the system kernel runs, allowing full access to all the instructions and capabilities of a CPU. The problem state is a lower privilege level with limited access to CPU instructions and is the standard operating mode of a CPU. It is called the problem state because that is the fundamnetal role of the CPU, to solve problems.

Process isolation prevents interactions or conflicts that could have negative consequences. There are two general methods to accomplish this:
1. Time-division multiplexing allows the CPU to determine process isolation by allocating very small slots of time to each process and rapidly switching between them as needed.
2. Memory segmentation relates more to Random Access Memory (RAM) found in computer systems. Memory segmentation assigns memory to one and only one application until it is released.

Storage consists of two types:
1. Primary storage is fast, volatile, ephemeral, and generally small. Examples include caching, CPU registers, and RAM.
2. Secondary storage is slow, durable, and generally larger. Examples include magnetic hard drives, optical media, tapes, and SSD.

Primary sotrage is also referred to as volatile memory as it is temporary. This can often lead to getting "filled up" and causing the sytem to crash. This can be mitigated by virtual memory which moves certain processes out of RAM and into the hard drive temporarily to give specific processes prioritized use of RAM.

The system kernel is the core of the operating system that has complete control overything and relies on privilege levels for smooth and safe operation. The system kernel drives the computing system. (Note this is very distinct from the security kernel which is the implementation of the RMC, not a computing process).

Privilege levels establish operational trust boundaries for software running on a computer.

The hardware is accessed via the kernel in kernel mode. This includes a very small set of processes such as the process manager, the security monitor, the memory manager, the I/O manager, the device drivers, and the hardware abstraction. The user mode is used by virtually everything else and has a lower level of trust.

The ring protection model is a form of conceptual layering that segregates and protects operational domains from each other. Ring 0 is the most trusted and secure ring (e.g. firmware). Everything else runs in the concentric rings outside Ring 0.

Middleware acts as an intermediary between two applications. It is a layer of software that can speak the languages of two disparate applications and facilitate communication between them.

Abstraction refers to the underlying complexity of a system and keeps them hidden/simplified/removed from view.

Virtualization is one mode of abstraction that creates a virtual version of something to abstract away from the true underlying hardware. or software.

Defense-in-depth implements multiple control layers whether physical, logical, operational, etc. to protect data.

Trusted platform modules (TPM) are pieces of hardware that implement an ISO standard, resulting in the ability to establish trust involving security and prviacy. Comptuers that contain a TPM create and encrypt keys using a specific endorsement key so that only the TPM can decrypt them. This is known as binding and serves to prevent the key's disclosure. Keys are sealed as they are associated to the TPM via specific configuration settings and parameters used at the time of the key's creation.

## 3.5 - Assess and mitigate the vulnerabilities of security architectures, designs, and solution elements

There are a number of common vulnerabilities across security architectures.

A single point of failure refers to the minimum necessary but sufficient condition of failure that would cause a failure in teh system. Points of failure can be mitigated through redundancy to support high availability and failover.

Bypass controls are intended methods to bypass and access administration settings but do introduce a risk vector in the security architecture. Strong protections around these bypass contorls can help mitigate or prevent their exploitation. This can be done via segregation of duties, logging and monitoring, and physical security.

Time-of-Check Time-of_use (TOCTOU) is a short window between when something is used and when authorization or access for that use is checked. This window is a risk vector for exploitation and can be referred to as a "race condition". Frequency of access checks can help mitigate this.

Emanations manifest in the forms of unseen things leaking out of systems usch as radio or magnetic waves like Wi-Fi or Bluetooth. Properly equipped listeners can eavesdrop on such emanations. Shielding, white noise, and control zones can protect against this.

Hardening is the process of looking at individual components of a system and then securing each component to reduce overall vulneerabilities. Hardening can include, disabling unnecessary services, installing security patches, closing certain ports, installing anti-malware, installing host-based firewall/IDS, enabling encryption, requiring strong passwords, and taking back-ups.

Mobile systems present their own kinds of risks that can be managed through mobile device management (MDM) and mobile application management (MAM). MDM is concerned with the devices themselves while MAM is concerned with the applications on those devices.

Policies can reduce, mitigate, or acknowledge risk associated with mobile devices. There should be clearly defined processes related to the loss or theft of stolen devices. Remote access security, endpoint security, and application whitelisting are methods of increasing the security of devices.

OWASP has a Mobile Top 10 which identifies the top ten risks with mobile devices:
1. M1 - Improper platform usage
2. M2 - Insecure data storage
3. M3 - Insecure communication
4. M4 - Insecure authentication
5. M5 - Insufficient cryptography
6. M6 - Insecure authorization
7. M7 - Poor client code quality
8. M8 - Code tampering
9. M9 - Reverse engineering
10. M10 - Extraneous functionality

The OWASP foundation has provided a Mobile Security Testing Guide (MSTG) to identify a security baseline for mobile applications.

Distributed systems are spread out across a network and while present a number of advantages also introduce a variety of security concerns. Distributed file systems (DFS) for example allow files to be hosted by multiple hosts and shared across a network. This raises additional overhead for integrity and nonrepudability. Grid computing is like distributed systems but are directly connected via a high-speed network.

Data warehouses perform data analytics from a number of data sets with the hope of identifying interesting bits of information. These have been in use for decades but with the rise of cloud computing raises the question "where is the data island(s) located physically?" Data warehouses because they are large can be a single point of failure.

Big data is a more recent evolution of the data warehouse and includes variety, volume, and depends on velocity. Data mining and analytics read from big data. One must consider the integrity of the data aggregation process in which data is collected, gathered, and combined as well as the inference process where information is deduced (or perhaps accessed without proper authentication or authorization).

Polyinstantiation reduces the risk of unauthorized inference by allowing information to exist in different classification levels for the purpose of preventing unauthorized inference and aggregation.

Industrial control system (ICS) is a general term used to describe control systems related to industrial processes and critical infrastructure. Three primary types include: Supervisory Control and Data Acquisition (SCADA), Distributed Control System (DCS), and Programmable Logic Controller (PLC).

ICS can be vulnerable to attack and it is best to air gap them to protect them from Internet access. Often ICS are not patched as much as possible because patching critical systems may cause unintended failure or downtime. These require dedicated patching strategies to guarantee availability as much as possible. This can be done through logging, monitoring, anomaly detection, conducting vulnerability assessments, and using VLAN's or zoning techniques to prevent an attacker from iterating through ICS systems if an individual one is breached.

SCADA is a system architecture that comprises computers, networking, and proprietary devices as well as UI for the management of an entire system. Manages small and large-scale industrial, infrastructure, and facility processes.

DCS monitors, controls, and gathers data from components like controllers, sensors and other devices. unlike SCADA it include local and remote management capabilities but is usually controlled locally.

PLC is an industrial computer specifically used for the control of manufacturing processes. Key features include high reliability, ease of programming, and diagnosis of process problems. It is often networked with other PLC devices and SCADA systems.

The Internet of Things refers to a large fleet of devices connected, often on a low level network. By their very nature, IoT architectures introduce considerable security risk and for security-centric architectures should be avoided.

Cloud computing is an on-demand self-service set of services and resources that offer on-demand access across a broad network with substantial resource pooling, rapid elasticity and scalability, measured service, and multitenancy to empower organizations through a variety of cloud service models (IaaS, PaaS, SaaS, CaaS, FaaS, etc.) and deployment models (public, private, physically isolated).

* SaaS provides access to an application that is web-based and usually paid via a subscription fee or agreement.
* IaaS is a virtual data center with virtualized, storage, compute, and networking components. It provides virtualization up to but not including the OS layer.
* PaaS is a middleground between IaaS and SaaS which allows customers to build custom application on top of other layers of hardware and software. It provides support but to the runtime layer.

Cloud computing operates under the shared responsibility model for customer and service provider have a distinct set of responsibilities.

Public clouds are available over the public Internet at no charge, private cloud is only accessible to paying customers. A community cloud is shared by a group of organizations or providers and is restricted to a trusted set of users. Hybrid is any combination of the three.

Regardless of the deployment model or service, every organization should be concerned with the protection and privacy of their cloud data.

Cloud compute consists of various layers of service models. 

Virtual machines are run on top of an underlying hypervisor (software that runs directly on hardware to allow multiple operating systems to share a single physical machine). A virtual machine resembles a computer except everything is emulated and each VM can host an operating system and its applications.

Virtual machines can be built off baseline images which present the most dangerous threat vector as a tampered base image provides a route to attack each VM created from that image.

By contrast, containers are a more lightweigh option. These run an application with its bins and libraries on top of a containerization engine which in turn runs on top of the Operating System. Containers are more efficient and portable. but they do run in a shared operating system which can expose them to a common threat vector.

---

Cloud forensics depends on "securing the scene" and can depend on the deployment model. For SaaS, the consumer is entirely dependent upon the service provider. For PaaS, the consumer is only dependent for application layer code they have deployed. For IaaS, consumrs can perform forensic investigations on their VM's and relevant network traffic, memory snapshots, etc.

A cloud service customer often establishes a service level agreement (SLA) with the cloud service provider in which accountability always remains with the asset owner for service. Between these, cloud service brokers provide aggregation services that help with various elements of engaging with cloud services (architecture, developmnet, etc.)

Identity and access management is particularly crucial in the realm of cloud computing. Traditionally organizations have used Active Directory (AD) or Lightweight Directory Access Protocol (LDAP) to manage IAM. Since then, many platforms offer Identity as a Service (IDaaS) to streamline the IAM process.

Federated identity management (FIM) in a similar vein provides protocols , standards, and policies to support identity portability and trust relationships among disparate organizations and resources. These include SPML (deprecated), SAML, OAuth, and OpenID.

---

As web applications continue to proliferate two new common forms of attack are XSS and CSRF. Cross-site scripting XSS is either Type I (stored or persistent) or Type II (reflected or nonpersistent).

Type I XSS is when an attacker identifies a vulnerable website, injects malicious code into a page on the website, the victim then uses the compromised code, and the victim's browser executes the malicious code that may send data to the attacker, disclose files, or install malicious applications.

Type II XSS is when an attacker sends a malicious URL to the victim, the victim clicks on it and the vulnerable website reflects malicious code on to the victim and the victim's browser executes it.

In the first type, malicious code is stored on the web server itself. In the second type, malicious code is reflected back to the victim even if not stored on the site itself.

DOM-based attacks modify the client-side document object model (DOM) to modify it. This can be Type I or Type II XSS. It is much more rare.

Cross-Site Request forgery is when an attacker forges a request, embeds the request in a hyperlink sent to the victim, the victim clicks the link and executes the embedded logic to a service or provider, and the service provider trusting the victim's request executes the logic. CSRF is dependent upon the persistence of cookies in browsers which allows the attacker to operate using the identity of the victim.

In summary, XSS exploits a user's browser to perform an unwanted action. CSRF exploits a web server to perform an unwanted action on a trusted website.

SQL injection is a method of attack that nests or embeds malicious logic into a SQL query that can be executed against the website, if there is no validation or sanitization of SQL input. User input should always be validated. Prepared statements, parameterized queries, or stored procedures also provide tools to mitigate or prevent SQL injections.

Server-side input validation is important in general for any web application and can prevent XSS attacks. Allow list input validation can ensure only accepted characters may be used. Client-side input validation is effectively useless from a security perspective as any bad actor may modify their client side to remove such validation.

## 3.6 - Apply cryptography

Cryptography is the strategic communication of information that can only be understood by specific parties. Key management is one of the most important aspects of cryptography.

Cipher technology has evolved from the manual Caesar cipher of antiquity to mechanical, electro-mechanical, electronic, and even quantum cryptography systems. Cryptography can help achieve confidentiality, integrity, authenticity, nonrepudiation (either of origin or delivery), and access control.

In cryptography, plaintext is the text as it is meant to be understood while ciphertext is the transferred or encrypted information that must be decoded.

A key is used to encrypt plaintext into ciphertext. If it is symmetric encryption, the same key is used to decrypt the ciphertext into plaintext, or in asymmetric a separate key is used for decryption. Key clustering is when two different keys can generate the same ciphertext from the same plaintext, something which must be avoided as this undermine the integrity of encryption. The work factor is the estimated amount of time or effort required by an attacker to brute attack a cryptosystem, given no key information. The initialization vector (IV) or nonce is a random number that is used in conjunction with the key during encryption to prevent patterns or key clustering. However, if the IV is too short then the ciphertext can be vulnerable to deciphering attacks. This happened with WEP.

In cryptography, confusion hides the relationship between the key and the ciphertext to make it more difficult for attackers to identify patterns. Likewise, diffusion is the idea that if a single bit of plaintext is changed then about half the bits in the ciphertext should be changed as well. 

An avalanche effect can look at the degree of confusion and diffusion that an algorithm provides to assess its effectiveness.

The key space refers to the unique number of keys available based on the length of a key. A 2-bit key has four possible unique keys. DES uses a 56-bit key which allows for 2^56 unique keys. Most symmetric keys today use 128 or 256 bit keys while asymmetric keys are often 1024 or 2048 bit.

Encryption involves substitution (replacing one character with another) and transposition (changing the order of characters). A simple form of transposition is called Rail Fence or Zigzag and transposes the text into a table where each row represents a rail and the characters are written diagonally down-and-up each rail to transpose them.

Encryption can be done synchronously where encryption/decryption requests must be performed immediately or asynchronously where they are dictated by some element that requires input (often processed in batches).

Frequency patterns can also lead to vulnerabilities in tying ciphertext back to plaintext, so one solution for this is to polyalphabetic ciphers in substitution where a key has the cipher iterate through multiple alphabet ciphers in sequence to minimize patterns. Another strategy is to use running key ciphers where ciphers are consistently (or constantly) cycled through to make pattern analysis of messages much more difficult. One-time pads are when a key is only used once for encryption and never again. This is the only unbreakable cipher system. Other methods include stream ciphers which encrypt or decrypt one bit at a time or block ciphers which only encrypt or decrypt one block of bit (often 64-bit chunks) at a time. Stream ciphers are much faster and are most suitable for network or hardware encryption. Block ciphers have a higher diffusion rate though and can be very resistant to tampering.

There are five primary modes of block ciphering:
1. Electronic Codebook (ECB) - Least secure but fastest. Has no IV. Only useable with short bits of nonrepeating random text.
2. Cipher Block Chaining (CBC) = Uses an IV, good for email
3. Cipher Feedback (CFB) - Uses an IV, good for email
4. Output Feedback (OFB) - Uses an IV, good for email
5. Counter (CTR) - Uses a counter similar to an IV, almost the most secure and is fast. Good balance of speed and security, most commonly used mode of block cipher

Steganography is hiding information within another kind of information such as hiding a message in an image. For instance, slack space is leftover storage when a file does not all need its allocated space and can be designated for other uses, such as steganography. A null cipher can be used by embedding a plaintext message within another plaintext message.

Symmetric cryptography is extremely fast and can be used for bulk data encryption/decryption. However, anyone with the key can access this system. Out-of-band distribution of keys (using some other medium to transfer keys) can mitigate this. Another problem is scalability as more users are added and each potential communication channel needs its own key. If 1000 people needed to all communicate each with each other, that would require (1000 - 1) / 2 = 499,500 keys.

The Data Encryption Standard (DES) uses an algorithm called Data Encryption Algorithm (DEA) whose key length is 56-bits but susceptible to attack. The newer International Data Encryption Algorithm (IDEA) with key length of 128 bits is stronger but is still symmetric in its algorithm. It never became popular as instead of the Double and Triple DES used compounded DES algorithms to strength the original.

Most common symmetric encryption algorithms:

| Name                        | Strength                  | Key-Length       | Block-Length  |
|-----------------------------|---------------------------|------------------|---------------|
| RC2-40                      | Weak                      | 40 bits          | 64 bits       |
| RC5-64/16/10                | Moderate                  | 64 bits          | 64 bits       |
| RC5-64/12/16                | Moderate                  | 64 bits          | 64 bits       |
| RC5-65/16/17                | Moderate                  | 65 bits          | 64 bits       |
| Skipjack                    | Moderate                  | 80 bits          | 64 bits       |
| RC2-128                     | Strong                    | 128 bits         | 64 bits       |
| IDEA                        | Strong                    | 128 bits         | 64 bits       |
| Blowfish                    | Strong                    | 32-448 bits      | 64 bits       |
| 3DES                        | Strong                    | 112 or 168 bits  | 64 bits       |
| RC5-64/12/32                | Strong                    | 128 bits         | 64 bits       |
| RC6                         | Very Strong               | 128, 192, 256 bits | 128 bits      |
| Rijndael (AES)              | Very Strong               | 128, 192, 256 bits | 128 bits      |

DES, 2-DES, and 3-DES all share the same basis of a 56-bit key with sixteen rounds of substitution and transposition with a 64-bit size. 2-DES doubles DES but is susceptible to man-in-the-middle attacks. 3-DES has an effective key length of 112 bits and is much stronger even if compromised by a man-in-the-middle attack.

By contrast asymmetric cryptography solves the key exchange problem by using public and private key pairs. Diffie and Hellman pioneered this form of public key cryptography in the 1970s. A public key is distributed to all recipients while a private key is the only retained by the owner and never shared. The key pairs are matehmatically linked in such a way as to make their relationship indiscernible and allows for digital signatures, authenticity, confidentailtiy, and access control while also solving for scalability. However it is significantly slower and requires larger key sizes.

Two hard mathematics problems are still primarily used for key generation: factorign and discrete logarithms. Factoring is when two large prime numbers are multiplied together, creating a number which would be very difficult to decompose into its two prime number factors. This is used by RSA. Another math problem is discrete logarithms used by Elliptic Curve (ECC) and Diffie-Hellman. This raises a prime number to the power of another prime number which is also very difficult to trace back to its original source. Diffie-Hellman key exchange uses this to generate symmetric keys to be exchanged between two parties.

One third, deprecated math problem is the Knapsack problem but a vulnerability was identified that could break it with any algorithm it uses.

Hybrid cryptography is a combination of symmetric and assymetric cryptography to achieve the benefits of each kind.

Message integrity checks (MIC) help to ensure the integrity of a message between the time it is created and the time it is read. It creates a representation of the message which is sent alongside it. MIC are based upon solving math problems, and so simple math should be avoided as those are more likely to result in collisions. Hashing is very effective as a MIC and works the same way.

Hashing is effectively because there is a fixed length digest. Any length input always equals the same length output. Additionally they are one way in that it is impossible to determine the input of a hashing algorithm by inspecting the output. They are also deterministic; the same input will always equal the same output. Good hashing algorithms are also collision resistant. They will generate a completely different output even if only a single bit of input is changed. It is very difficult to find two inputs that hash to the same output.

The most popular hashing algorithms include:
* MD5 - 128-bit digest
* SHA-1 - 160-bit digest
* SHA-2 - 224/256/384/512-bit digests (determined by version used)
* SHA-3 - 224/256/384/512-bit digests (determined by version used)

Digital signatures provide integrity, authenticity, and nonrepudiation. The sender hashes their message which produces a fixed length message digest. The sender then encrypts the hash value with their private key. These combined not only provides a way to validate the message is tamper-proof but also that is authentic in its origin. It should be noted that digital signatures do not provide confidentiality as the message itself is not encrypted and can be intercepted. Digital signatures are popularly used in code signing for software builds.

Digital certificates bind an individual to their public key. All certificate authorities conform to the X.509 certificate standard. The root of trust or trust anchor is the foundation of all digital certificates and is represented by a root certificate authority (CA). As public/private key pairs ought to be periodically cycled, so too should digital certificates. If a private key is compromised, a digital certificate should be revoked too. A certificate can be pinned so that trust only needs to establish for the first visit to a web server, and a new copy is not requested on subsequent visits.

The X.509 certificate standard includes fields like certificate version, serial number, encryption algorithm, issuing CA, validity period, and public key value.

Root of trust or the trust anchor is the foundation of digital certificates' integrity. The root CA self-signs its certificates and then signs subordinate certificates (intermeidate CA's). Intermediate CA's can sign further subordinated certificates. These are known as Issuing CA's. Again, certificates should be rotatedas they either expire or are revoked.

There are two ways to confirm if a certificate is revoked: certificate revocation list (CRL) which is an older downloadable list of serial numbers and online certificate status protocol (OCSP) where a client queries CA for revocation status of a specific certificate serial number. A certificate's life cycle consists of the following stages:
1. Enrollment - an entity submit a certificate signing request (CSR) to a CA with proper identifying fields. The requesting entity also generates a public/private key pair, of which the public key is included in the CSR.
2. Issuance - upon receiving the CSR, the CA will proof the identity of the entityt to ensure the information in the CSR is valid. After completing this process, the CSR will be signed by the root of trust private key, following a X.509 standard, at which point the certificate will be issued by an intermediate/issuing CA.
3. Validation - when a certificate is invoked as part of regular web activity, the request will validate the provided certificate with the issuing CA.
4. Revocation - a certificiate may be revoked for a number of reasons if it does not first expire.
5. Renewal - The entity will need to renew the certificate.

Public key infrastructure (PKI) is the entire suite of technology systems that allow keys to be distributed publicly and verified. A Registration Authority (RA) as part of the CA will ask for identifying information when performing identity proof on the requesting entity. Each CA maintains a dedicated DB which includes all certificates that have been issued or revoked. By contrast, a certificate store is a repository of certificates and user's private key on a user's computer.

Once a key is encrypted, it must be effectively managed as a key part of encryption architecture. Kerckhoffs' principle states that a system should be secure even if everything about the system is known except its key. Key lifecycle steps include:
1. Key creation - randomly generating a key from the entrie available key space.
2. Key distribution - secure distributing keys either through out-of-band distribution or key wrapping using key encrypting keys (KEK)
3. Key storage - storing keys in either a Trusted Platform Module (TPM) usually built into an individual machine or a Hardware Security Module (HSM) usually attached to a corporate network.
4. Key rotation - replacing keys as needed
5. Key disposition - crypto shedding or key destruction
6. Key recovery - split knowledge (cut in half), dual control (two keys), or key escrow (loan key to trusted party)

Secure/Multipurpose Internet Mail Extensions (S/MIME) is standard for public key encryption and provides authentication, nonrepudiation of origin, message integrity, and confidentiality. S/MIME offers optional security services in the form of signed receipts, security labels, secure mailing lists, and an extended method of identifying the signer's certificate.

## 3.7 - Understand methods of cryptanalytic attacks

Cryptoanalysis is the science of decryption.

There can be cryptanalytic attacks or cryptographic attacks.

The primary goal of a cryptanalytic attack is to determine the key. A brute force attack tries to guess every possible key until it identifies the correct one. There are four combinations of attacks:
1. Ciphertext only - the attacker only has the ciphertext which is most difficult
2. Known plaintext - the attacker has both ciphertext and plaintext and can try to determine the key via their relationship.
3. Chosen plaintext - the attacker has all elements except the key, and the attacker attacks the plaintext (feeding it into the encryption process)
4. Chosen ciphertext - the attacker has all elements except the key, and the attacker attacks the ciphertext (feeding it into a decryption process)

Other attacks include usign linear and differential cryptanalysis to deduce the key using complex mathematics or factoring cryptanalysis which tries to factor a very large number to determine the private key. This is a common strategy to attack the RSA algorithm.

Cryptographic attacks tend to focus on retrieving information or executing action without necessarily accessing the key directly. 
* Man-in-the-middle attacks is where the attacker pretends to be both parties in communication to elicit information or permissions. 
* A replay attack is similar but where the attacker eavesdrop on information and replays it later to gain access to target system. 
* Pass-the-hash attacks are where the attacker has the password hash and tries to access the target system using the hashed password. 
* Temporary files attacks are where ephemeral keys or processes are hijacked to gain privileged information. 
* Implementation attacks target the weakness of an encryption algorithm's implementation. 
* A side-channel attack does not target the system but monitors it via indirect information such as time, emissions, or power consumption to deduce more about the system. 
* A dictionary attack will use freqeuency analysis to brute force a password. Rainbow tables are similar in that this stores the hashes of the most common passwords. 
* Rainbow tables can be mitigated by using a random salt value that is appended to a password before it is hashed. A pepper value may also be used but these are less secure as this is a shared value that is appended to all passwords in the system. 
* A birthday attack attempts to identify hashing collisions to access a system. 
* Purchase key attacks (bribery) or rubber hose attack (duress) are social engineering forms of cryptographic attacks. 
* Kerberos attacks focus specifically focused on using information like password hashes to generate a valid Kerberos ticket that can be used to bypass authentication. 
* A ransomware attack encrypts or locks down a user's system or data until certain demands are met. 
* A fault injection attack attempts to leverage normally legitimate fault injections to modify hardware or software system behavior.

## 3.8 - Apply security principles to site and facility design

Physical security like information security includes the CIA triad. Its key objectives are deterence, delay, detection, assessment, and response. Physical security must always consider the safety and protection of people first and foremost. The best physical security leverages defense in depth.

## 3.9 - Design site and facility security controls

Crime Prevention through Environmental Design (CPTED) is a specific professional practice that outlines guidelines and best practices regarding the design of building and surrounding structures.

Considerations include threat definition, target identification, facility characteristics, life and property at stake, and operations (BCP/DRP).

Perimeter controls include using landscaping design, grading, or bollards.

Closed-circuit TV (CCTV) primaily serve as detective control. One must consider placement, image quality, and transmission media when adopting this strategy. Passive infrared devices are motion detectors that can pick up on infra-red light. Lighting, doors, mantraps, locks, card access, biometrics, windows, and walls can all be factors in physical security design.

Infrastructure are also threat vectors whether that is through power outages (mitigated through UPS or generators), HVAC disruption, fire, or water.

Fire detection tools can consist of infrared sensors, ionization detectors, photoelectric sensors, dual sensors, or VESDA (most effective and expensive), as well as a system which identifies the rising temperature.

Fire suppression can consist of water-based systems like wet pipe, dry pipe, pre-action, and deluge, or it can be gas-based fire suppression systems like INERGEN, argonite, FM-200, or Aero-K. Halon used to be one of these but is now illegal.

Fire extinguishers can also be used and consist of the following:
* Class A - puts out common combustibles using water, foam, and dry chemicals
* Class B - puts out liquid fires using gas, carbon dioxide, foam, and dry chemicals
* Class C - puts out electrical fires using gas, carbon dioxide, and dry chemicals
* Class D - puts out combustible metal fires using dry powders
* Class E - puts out commercial kitchen fires using wet chemicals.

Carbon dioxide is a very common suppressant agent as it is inexpensive and does not introduce as much corrosion as other suppressants but it can be deadly if used in excess with people present.

# Domain 4: Communication and Network Security

## 4.1 - Implement secure design principles in network architectures

A network is how two or more devices communicates with one another. A protocol is a set of common rules that define network communication. The Open System Interconnection (OSI) Model is one heuristic breakdown of networking layers in seven layers.

![OSI Layers](https://coderepublics.com/blog/wp-content/uploads/2023/09/WHAT-IS-OSI-MODEL-7-LAYERS-EXPLAINED.jpg)

Layer 1: Physical exists as binary data, zeros and ones. Devices can conect via wired or wireless technologies. Wired technologies include twisted pair, coaxial, and fiber optic. Wireless include radio frequency, infrared/optical, and microwave.

Twisted pair cable is when a pair of wires are twisted together and can be shielded (STP) or unshielded (UTP). Most people are familiar with coaxial cable for their home networks which is a single strand of copper wire sheathed in a protective coating. Fiber optic uses light pulses to represent zeroes and ones. Fiber optic is immune to the problem of crosstalk that electrical-based wiring like twisted pairs and coaxial can suffer from.

Wiring can have a number of different topology models:
* Bus topology is the most common and is when devices are connected to a central wire. One node can fail without impacting the rest. However the bus does itself represent a point of failure, and also a large-scale bus introduces the risk of a compromised device connecting to the network.
* Tree topology can isolate transmissions to specific branches on the tree. Outages would be restricted to a branch as well.
* Star topology connects every device to a central device which allows for network segmentation which can each have their own kind of network topology.
* Mesh topology interconnects every device with every other device which is ideal for redundancy but less htan ideal for security.
* Ring topology loops devices in a bus-like topology but mitigates collision issues. It is not as possible anymore.

Three strategies to address collisions:
1. Token-based collision avoidance: A token is passed from device to device, and only the device holding the token can transmit information. This is what token ring networks use.
2. Polling: Interconnected devices poll each other to learn if any information needs to be transmitted. It introduces significant network traffic and is not used prevalently.
3. Carrer Sense Multiple Access (CSMA): This is used by modern networks. Before transmitting information, devices check for any voltage on the wire to ensure there is potential for collisions. 

Two flavors of CSMA. CSMA with Collision Avoidance (CSMA/CA) uses two lanes of communication, one for sending and one for receiving infromation. CSMA with Collision Detection (CSMA/CD) is useful for wired networks which detects collisions after the information has been transmitted.

Alongside this, transmission methods include unicast (one-to-one), multicast (one-to-many), and broadcast (one-to-all).

Several devices operate at Layer 1: hubs (connects multiple devices by repeating all signals), repeaters (signal boosters), and concentrators (combine all signals).

Layer 2: Data Link

The Data Link layer translates between the Physical Layer's bits and the Network Layer's datagrams. Devices operating in a network are physically and uniquely identified at this layer using their Media Access Control (MAC) address. It consists of 48 bits where the first 24 bits are for the organizational unique identifier (OUI) and the second 24 bits for the device, specific to the network interface controller (NIC).

Address Resolution Protocol (ARP) and Reverse Address Resolution Protocol (RARP) are used to translate between IP addresses and MAC addresses. One point of attack is ARP poisoning which can spoof or masquerade device mappings.

Network communications can either be circuit-switched or packet-switched. Circuit-switched networks such as the Public Switched Telephone Network (PSTN) establish connection routes between each party at the end of the communication. Circuits can be switched to establish connections as needed. For a very long time this was done via analog connections. Now VoIP or IP telephony technology allows digital data to be transmitted over analog connections.

Packet-switched networks break up data into smaller packets can transfer it across a network. Switches transfer these packets from one end to the otehr.

Layer 2 protocols include:
* L2F - Layer 2 Forwarding tunneling protocol
* PPTP - Point-to-Point Tunneling Protocol. Three auth types:
  1. Password Authentication Protocol (PAP) - simplest and least secure
  2. Challenge Handshake Authentication Protocol (CHAP) - More secure as password is encrypted before being sent
  3. Extensible Authentication Protocol (EAP) - Most robust and can be combined with other protocols
* L2TP - Layer 2 Tunneling Protocol
* SLIP - Serial Line Internet Protocol (older used for remote access with modem connections)
* ARP - IP to MAC
* RARP - MAC to IP

Layer 2 devices include bridges that connect networks together with no concern for traffic crossing the bridge. Switches also exist at the Layer 2 level for forwarding packets to the intended recipient.

Protected Extensible Authentication Protocol (PEAP) is an improved version of EAP as it encapsulates EAP in a TLS tunnel. EAP-TLS includes both client and seerver certificates and has high security and industry support without being proprietary.

Layer 3 is the network layer where data exists as packets and datagrams. It is responsible for fragmentation and IP addressing.

Layer 3 protocols include:
* ICMP - Internet Control Message Protocol is used for messaging and specifically provides feedback about problems in the network environment. `ping` and `traceroute` both use ICMP.
* IGMP - Internet Group Management Protocol is used to establish and manage group memberships for hosts, routers, and similar devices.
* IPsec - A tunneling protocol that supports authentication of other Layer 3 devices as well as encryption.
* OSPF - Open Shortest Path First is a routing protocol used by routers to manage and direct network traffic properly and efficiently.
* BGP - Border Gateway Protocol

Layer 3 devices include:
* Routers - connect and route network traffic based on IP addressing
* Layer 3 switches - route devices on the same virtual local area network (VLAN)
* Packet filtering firewalls - devices that make decisions based upon the header portion of a datagram

IPv4 addresses are compromised of four 32-bit numbers separated by dots. Each number represents an 8-bit octet. That valid range for them is 0-255. Each packet header contains routing details like source and destination IP addresses. IPv4 is limited to just under 4.3 billion addresses. Network Address Translation (NAT) is leveraged by ISP's to translate private network IP ranges into publicly routable IP addresses. 

The Institute of Electric and Electronic Engineers (IEEE) has ratified a number of standards for Layer 3 networking:
* IEEE 802.3 defines communication standards over physical and wired networks.
* IEEE 802.11 is a collection of communication standards specific to implementation of WLAN.
* IEEE 802.1Q defines the standard for virtual local area networks (VLAN's).

IP is the principal communications protocol for addressing and routing packets of data across networks. Internet Protocol v4 Datagram depicts the IPv4 header which includes several fields including the source and destination IP addresses. IPv6 expands IP addressing to 128 bits and includes backward compatibility with IPv4. Also, IPv6 requires IPsec which is only optional for IPv4.

Private network address spaces include:
* 10.0.0.0 -> 10.255.255.255
* 172.16.0.0 -> 172.31.255.255
* 192.168.0.0 -> 192.168.255.255

If networks were restricted to Class A ranges it could have 2^26 IP addresses, Class B would only have 2^16, and Class C would only have 2^8. Subnetting can be used to logically divide network host environments into discrete architecture models.

Layer 4 Transport: transports data between services and consists of TCP or UDP. TCP is ordered and reliable while UDP is unreliable and unordered but much faster.

TCP consists of a three-way handshake. First, device A sends a synchronization (SYN) request to device B alongside a session ID. evice B responds with an acknowledge (ACK) flag set with its own SYN request and random session ID number. Device A sends a final ACK flag including the incremented session ID number to increment the SYN value it received before.

To close the connection, device A sends an ACK plus FIN request. Device B would respond with n ACK and FIN request. Device A would in turn send a final ACK to end the connection gracefully.

The three-way TCP handshake can be hijacked if the server is spammed with an excessive level of SYN requests and overload the server. Ports equate to services and can provide specific functionality. Ports should only be opened if they are being used. Among the 65,535 available TCP and UDP ports three classes exist: well known (e.g. HTTP), registered (IANA-assigned), and dynamic/private/ephemeral which are easily interchangeable.

The most common well-known ports are:
* FTP - 21
* SSH - 22
* Telnet - 23
* SMTP - 25
* HTTP - 80
* HTTPS - 443

Layer 5 Session: supports interhost communicatoin and maintains a logical connection between two processes on each end host. Ideal layer for identification and authentication:

Conceptaully PAP, CHAP, and EAP can be labeled as Layer 5 in the OSI model even if they functionally run at Layer 2. In addition to these NetBIOS and RPC are other Layer 5 protocols.

The circuit proxy firewall or gateway can be found at Layer 5. These can mask internal network traffic and activity from external parties.

Layer 6: Presentation focuses on visual elements fo data like graphics, character conversions, codecs, etc.

Layer 7: Application is where most functionality resides and security breaches occur. It provides a user interface through which a user gains access to communication services. Ideal place for end-to-end encryption and access control.

Layer 7 protocols include:
* Hypertext Transfer Protocol (HTTP) and Hypertext Transfer Protocol Secure (HTTPS)
* File Transfer Protocol (FTP) and Secure FTP (SFTP)
* DNS (port 53)
* Telnet
* SSH
* SMTP/POP3
* SNMP

Layer 7 devices include gateways and application-gateway firewalls.

---

There are other pertinent details to understand about networking mangaement. Convergence refers to the ability of native IP networks to carry non-IP traffic via converged protocols. These include Fibre Channel over Ethernet (FCoE), Internet Small Computer Systems Interface (iSCI), and Voice over Internet Protocol (VoIP).

VoIP protocols include secure real-time transport protocols (SRTP) and Session Initiation Protocol (SIP). SRTP is the secure version of RTP and supports encryption, authentication, integrity, and replay attack protection. SIP is responsible for initiating, maintaing, and terminating voice and video sessions.

---

Network attacks can take on a variety of forms and can be categorized as network attacks or network assessments (discovery).

The four attack phases are reconnaissance, enumeration, vulnerability analysis, and exploitation. Passive attacks primarily eavesdrop or gather information. Active attacks change or modify their targets. SYN scanning using nmap can be used to identify open ports and then partially open a connection without sending the final ACK. SYN flooding will try to overload the server in the three way handshake process.

Fragment attacks can either be overlapping fragmnets which attempt to use fragment overlap to bypass firewalls and IDS tools to get access to a target system or a teardrop TCP attack which sends packet of various sizes out of order with fake sequence numbers to corrupt a target system's ability to assemble a TCP packet and cause it to crash.

IP spoofing attacks include a smurf attack where the attacker spoofs their IP address to match a victim's and pings a server so the victim's IP address is inundated with the response traffic. A fraggle attack sends a massive amount of UDP packets to a target system which would respond to the spoofed IP address. Neither of these are very common anymore.

A denial of service (DoS) attack is an attack that impedes or denies functionality of a system or network. A distributed denial of service attack involves multiple machines acting in unison.

---

Radio frequency management is concerned with the placement of wireless network devices and the kind of exposure they may have considering where it is replaced with respect to the external perimeter.

802.11 describes the wireless protocol family.

| **802.11 Standard** | **Frequency Band(s)**          | **Top Speed (Theoretical)** |
|---------------------|--------------------------------|-----------------------------|
| 802.11 (original)   | 2.4 GHz                        | 2 Mbps                      |
| 802.11a             | 5 GHz                          | 54 Mbps                     |
| 802.11b             | 2.4 GHz                        | 11 Mbps                     |
| 802.11g             | 2.4 GHz                        | 54 Mbps                     |
| 802.11n (Wi-Fi 4)   | 2.4 GHz / 5 GHz                | Up to 600 Mbps              |
| 802.11ac (Wi-Fi 5)  | 5 GHz                          | Up to ~3.46 Gbps*           |
| 802.11ax (Wi-Fi 6)  | 2.4 GHz / 5 GHz (6 GHz\*)       | Up to 9.6 Gbps              |

\* **Note**: 802.11ax (Wi-Fi 6) can also be extended to the 6 GHz band (Wi-Fi 6E), which offers additional channels but has the same maximum theoretical speeds as standard 802.11ax.

Wireless security solutions have evolved over time and have included the following

| **Protocol** | **Release Year** | **Access Control**            | **Authentication**                        | **Encryption**                  | **Integrity**                       |
|--------------|------------------|--------------------------------|-------------------------------------------|---------------------------------|--------------------------------------|
| **WEP**      | 1997             | Basic passphrase, MAC filtering| Open System or Shared Key                 | RC4 (40/104-bit keys)           | CRC-32 (weak)                       |
| **WPA**      | 2003             | 802.1X (Enterprise) or PSK     | 802.1X/EAP (Enterprise) or Pre-Shared Key | TKIP (Temporal Key Integrity)    | Michael (TKIP-integrated integrity) |
| **WPA2**     | 2004             | 802.1X (Enterprise) or PSK     | 802.1X/EAP (Enterprise) or Pre-Shared Key | AES-CCMP (with TKIP fallback)    | CCMP (robust integrity)             |
| **WPA3**     | 2018             | 802.1X (Enterprise) or SAE     | SAE (Simultaneous Authentication of Equals) or 802.1X/EAP | AES-CCMP or GCMP               | GCMP (enhanced integrity)           |

There are three ways to authenticate to a wireless network:
1. Open authentication (any device can connect)
2. Shared key (a pre-shared network key is used and shared to devices directly)
3. EAP is used (authentication requires an authenticated key exchange mechanism)

EAP can have one-factor authentication like EAP-MD5, LEAP, PEAP-MSCHAP, TTLS-MSCHAP, EAP-SIM or two-factor EAP-TLS, TTLS with OTP, and PEAP-GTC.

The main encryption technologies are temporal key integrity protocol (TKIP) was a quick fix when WEP vulnerabilities were discovered. Counter-Mode-CBC-MAC Protocol (CCMP) uses Advanced Encryption Standard (AES) with 128-bit keys, used in WPA2.

TKIP implements a MIC often referred to as "Michael".

---

Virtual Local Area Networks (VLANs) allow local networks to be created using hardware devices like Layer 3 switches. These can reduce the need for physical wiring. IEEE 802.1Q is the standard that supports VLANs on networks, including VLAN tagging.

Software-Defined Networks are networks created and managed using software via application, control, and data planes. Communication between the application and control planes is facilitated by nortbound APIs while communication between data and control planes is facilitated by southbound APIs.

A VLAN reduces the need for physical rewiring by creating virtual tunnels through physical networks to connect devices.

Wide Area Networks (WAN) connect LANs through technologies such as dedicated leased lines, dial-up phone lines, satellite and other wireless links.

WAN protocols include:
* X.25 - One pioneer of WAN pack-switching protocols with a high error correction capability but not particularly effective today
* Frame Relay - A successor to X.25 with a focus on speed. It uses permanent virtual circuits (PVCs) and switched virtual circuits (SVCs). PVCs support end-to-end links over a physical network, while SVCs are similar to circuit-switched networks.
* Asynchronous Transfer Mode (ATM) - Builds on top of Frame Relay and supports very high-speed transmission needs.
* Multi-Protocol Label Switching (MPLS) - At the forefront of current WAN connectivity solutions. It offers network connectivity that includes built-in security.

## 4.2 - Securee network components

Good network architecture consists of a combination of the following components:
* Defense in depth - multiple layers of protection
* Partitioning - controlling flow of traffic between segments
* Network perimeter controls - Limited choke points for ingress or egress of traffic
* Network segmentation - Dividing into public, private, and isolated networks or subnets
* Bastion host - A demilitarized zone (DMZ) to segregate network access via bastion hosts or a boundary router
* Proxy - An intelligent application that acts as an intermediary and is placed between clients and the server
* NAT and PAT - Translates public and private IP addresses
* Firewall - A logical entity that enfroces security rules between two or more networks.

Firewall technologies can take on a number of forms today;
* Packet filtering - Examines packet headers against access control lists (ACLs) to accept or deny access. Exists at Layeer 3 and is the simplest and fastest.
* Stateful packet filtering - Packet state and context data is stored and updated dynamically for better tracking (RPC, UDP). Layer 3 and 4, somewhat complex but still fast.
* Circuit-level proxy - An agnostic circuit between client or server (e.g. SOCKS). Exists at Layer 5 with more complexity and latency. Filters sessions based on rules.
* Application-level proxy - Inspects packet payload based on the application, but can be a performance bottleneck. Layer 7 with highest latency of the four, filters based on data payload.

Some firewall technologies include context-based access controls (CBAC) to intelligently filter TCP and UDP packets based on application layer protocol session information.

Aside from the general firewall technologies, there are also various architectures that could be implemented:
* A packet filtering architecture simply consists of a router between the client and the server.
* A dual-homed host improves upon the packet filtering router by using a computer with two network cards that can understand all layers of the OSI model
* Screened host firewalls combined packeting filtering with dual-homed host by having the packet filter peform an initial check, and if the packet is approved, pass it on to the dual-homed host.
* Screened subnet architecture contains two firewalls encompassing a central DMZ. Traffic flows between from client and server through the firewall, switch, DMZ, second firewall, then to its destination.
* Three-legged firewall has simply a firewall (without a switch) route to a DMZ.

Intrusion Detection Systems (IDS) and Intrusion Prevention Systems (IPS) provide further defense mechanisms at the network layer.

IDS could include virus scanning at the files level, stateful inspection in the state table, or content inspection. It monitors a network or host for malicious activity and reports events.

IPS by contrast monitors a network or host for malicious activity or policy violations, reports events, and attempts to block them. Network-based IDS/IPS monitors network traffic transiting a network segment. Host-based IDS/IPS is installed only on the host.

These can be placed in a variety of places in the network architecture depending on the layout.

A port on a network device is a mirror, span, or promiscuous when traffic passing through the device is copied to the port and any device connected to it (Wireshark is an example of this).

IDS/IPS can be pattern-based (focusing on known attack pattern types through signature analysis) or anomaly-based (focusing on anomalous patterns in stateful matching, statistical, protocol, or traffic level) to detect potentially malicious traffic. 

Honeypots (hosts) and honeynets (two or more honeypots networked together) are technical detective controls which can detect sophisticated cyberattacks including Advanced Persistent Threats (APTs) and can trace attack paths or distract attackers, but however may unintentionally expose company patterns or systems at a more fundamental layer.

Honeypots should never be used to entrap individuals, they should only entice.

Endpoint security is concerned with the specific protection of client devices on corporate networks. Data leak prevention (DLP) and network access control (NAC) solutions restrict access and can help fortify device endpoints.

## 4.3 - Implement Secure Communication Channels According to Design

Remote access security is concerned with controls for remote access over insecure/public networks. VPN solutions are generally utilized to protect remote access traffic. VPNs are tunneling protocols with encryption; if there is no encryption, it is not a VPN, only a tunnel.

Tunneling encapsulates a datagram within another. It does not provide confidentiality unless encryption is applied.

Generic Routing Encapsulation (GRE) is a tunneling protocol that encapsulates a variety of protocols and routes them over IP networks, including IPv4, multicast, and IPv6. GRE is not secure on its own as it only provides encapsulation.

Split tunneling allows a user to access disparate resources simultaneously without traffic passing through the VPN. This is generally considered a security risk because the end device can bypass organizational security controls by using other networks.

VPN solutions exist at Layer 2 and Layer 3, depending on the protocol used. VPNs rely on symmetric encryption.

### Common Tunneling Protocols

| **Protocol**                     | **Includes Encryption**             | **OSI Layer**         |
|----------------------------------|-------------------------------------|-----------------------|
| **VXLAN (Virtual Extensible LAN)** | No                                 | Layer 2 (Data Link)   |
| **L2TP (Layer 2 Tunneling Protocol)** | No (but can use IPSec for encryption) | Layer 2 (Data Link)   |
| **PPTP (Point-to-Point Tunneling Protocol)** | Yes (uses MPPE encryption)      | Layer 2 (Data Link)   |
| **GRE (Generic Routing Encapsulation)** | No                                 | Layer 3 (Network)     |
| **IPSec (Internet Protocol Security)**  | Yes                                | Layer 3 (Network)     |
| **WireGuard**                   | Yes                                | Layer 3 (Network)     |
| **MPLS (Multiprotocol Label Switching)** | No                                 | Layer 2.5 (between Data Link and Network) |
| **OpenVPN**                     | Yes                                | Layer 4 (Transport)   |
| **SSL/TLS (for tunneling, e.g., stunnel)** | Yes                          | Layer 5 (Session)     |
| **SOCKS**                       | No                                 | Layer 5 (Session)     |
| **SSH (Secure Shell Tunneling)** | Yes                                | Layer 7 (Application) |

IPsec is preferred for VPNs and is embedded in IPv6 by default, offering authentication via Authentication Header (AH) and encryption via Encapsulating Security Payload (ESP).

IPsec can operate in:
- **Transport Mode**: Uses the header of the original packet with the AH or ESP header, protecting the payload.
- **Tunnel Mode**: Encapsulates the entire original IP packet (header and payload) with a new header and the AH or ESP header.

IPsec depends on Internet Key Exchange (IKE) to generate the same session key at each end of the VPN. IPsec tunnels are established through a Security Association (SA), which defines attributes such as authentication and encryption algorithms, encryption keys, transport or tunnel modes, sequence numbers, and expiry.

SSL/TLS provides secure client-to-server connections. SSL is considered obsolete and has been replaced by TLS. The latest SSL revision was 3.0, and TLS 1.3, released in 2018, is the most recent version of TLS. The DROWN attack is a major threat to SSLv2.

### SSL/TLS Handshake Protocol
1. The client sends a request to the server using supported algorithms.
2. The server returns its certificate with selected algorithms.
3. The client decrypts the server's certificate to obtain its public key.
4. The client creates a symmetric session key and encrypts it with the server's public key.
5. The client sends the encrypted session key to the server.
6. The server decrypts the session key using its private key, concluding the handshake.

### TLS VPN vs. IPsec VPN
* TLS VPN operates at the Transport Layer, while IPsec operates at the Network Layer.
* TLS encrypts traffic between processes identified by port numbers, while IPsec encrypts traffic between systems identified by IP addresses.
* TLS encrypts connections by default, while IPsec does not.
* TLS is easier to establish and manage with more granular configuration options, while IPsec can be more complex.
* Attacks on TLS compromise specific applications, while attacks on IPsec compromise the entire network.

### Remote Authentication Protocols
- **Remote Authentication Dial-In User Service (RADIUS)**: An application-layer protocol that allows users to connect to and access network resources. It supports dial-in networking.
- **Terminal Access Controller Access Control System Plus (TACACS+)**: An improvement over RADIUS that encrypts all transmitted packets.
- **Diameter**: A successor to RADIUS that adds support for EAP authentication.

# Domain 5: Identity and Access Management (IAM)

## 5.1 - Control physical and logical access to assets

Access control is the collection of mechanisms that work together to protect the assets of an organization and allow controlled access to authorized subjects They specify which users can access which resources using which operations while maintaining individual accountability.

Access control principles include: need to know, least privilege, and separation of duties

Access control can be administered in several ways:
* Centralized: One central system with SSO and central administration point manages all access control. Represents a single point of failure
* Decentralized: Control granted at the people/resource level by separate entities but introduces lack of standardization and has a greater risk of security holes.
* Hybrid a combination of the two which can have the tradeoffs of each but can add additional complexity because it contains multiple architectural patterns

## 5.2 - Manage Identification and Authentication of People, Devices, and Services

Access control consists of four key elements: **identification**, **authentication**, **authorization**, and **accountability**.

### Identification
User identification should be:
- Unique
- Nondescriptive of role
- Issued securely
- Used securely

### Authentication
Authentication can be based on:
1. **What you know**: Passwords, security questions, etc.  
   - **Least secure** due to ease of compromise.
2. **What you have**: One-time passwords (OTP), smart cards, etc.  
   - **More secure** but still susceptible to theft or loss.
3. **What you are**: Biometrics like fingerprints, facial recognition, voice, or handwriting.  
   - **Most secure** method. Retina scans are the most secure of all biometrics.

**Biometric authentication** must be designed to minimize:
- **Type 2 errors**: False acceptance of unauthorized users.
- **Type 1 errors**: False rejection of valid users.  
The intersection of these two error types is the **Crossover Error Rate (CER)**, a key measure of biometric accuracy.

### Single Sign-On (SSO)
SSO improves user experience and reduces vulnerabilities from weaker authentication systems but introduces a **single point of failure**.

**Kerberos** is a widely used SSO authentication protocol:
- The client sends an initial message to the **Authentication Service (AS)**.
- The AS replies with:
  - Password encrypted as a key.
  - Ticket Granting Ticket (TGT).
  - Ticket Granting Service's (TGS) key.
- The client decrypts the message using their password and generates new tickets.
- The client sends the TGT and new tickets to the TGS.
- The TGS verifies the message and returns an encrypted **Service Ticket** and key.
- The client uses the Service Ticket to authenticate with the service.  
**Note**: The AS and TGS are components of the **Key Distribution Center (KDC)**.

**Disadvantages of Kerberos**:
- Only supports symmetric encryption.

**SESAME** is an alternative to Kerberos:
- Supports asymmetric encryption.
- Less widely adopted due to Kerberos's established presence.

### CAPTCHA
A **Completely Automated Public Turing test to tell Computers and Humans Apart (CAPTCHA)** prevents automated exploitation of systems.

### Session Management
Sessions must be managed to prevent hijacking by attackers. This includes:
- Scheduled or login limitations.
- Session timeouts.
- Device-level protections (e.g., screensavers).

### Identity Proofing
**Identity proofing** (registration) confirms or establishes an individual’s claimed identity.

### NIST SP 800-63B Authentication Levels
1. **AAL1**: Basic assurance. Single-factor authentication with secure protocols.
2. **AAL2**: High assurance. Multi-factor authentication with approved cryptographic techniques.
3. **AAL3**: Very high assurance. Multi-factor authentication with hardware-based cryptographic authenticators.

### Federated Identity Management (FIM)
FIM relies on trust relationships between identity providers and service providers. For example:
- **Microsoft Active Directory** implements FIM.

**Federated Access Standards**:
- **SAML**: Provides authentication and authorization.
- **WS-Federation**: Similar to SAML with both authentication and authorization.
- **OpenID**: Focuses on authentication.
- **OAuth**: Focuses on authorization.

### SAML Workflow
1. The user requests access to the service provider.
2. The service provider requests SAML authentication.
3. The user relays the SAML request to the identity provider.
4. The identity provider authenticates the user and generates a SAML assertion.
5. The user relays the SAML assertion to the service provider.
6. The service provider approves access.

**SAML Terminology**:
- **Assertion**: Covers authentication, authorization, and attributes. Written in XML.
- **Protocol**: Defines how entities request or respond to requests.
- **Bindings**: Maps SAML onto standard communication protocols.
- **Profiles**: Specifies how SAML is used for specific business use cases.

### Just-in-Time Access
Just-in-time access temporarily escalates user privileges for authorized tasks, preventing ongoing or permanent privilege escalation.

---

## 5.3 - Federated Identity with a Third-Party Service

**Identity as a Service (IDaaS)** leverages cloud environments for access control. IDaaS supports:
- Creating and managing identities in the cloud.
- Syncing on-premises identities with the cloud.
- Linking separate accounts.
- Federating identities across systems.

### Risks of IDaaS
- **Availability**: Depends on third-party uptime.
- **Data Protection**: Entrusts sensitive or proprietary data to a third party.

## 5.4 - Implement and manage authorization mechanisms

In the realm of authorization, there are several approaches:
* Discretionary Access Control (DAC) means an asset owner determines who can access the asset.
   - Role-Based Access Control (RBAC) is based on user roles
   - Rule-based Access Control is based on a set of rules (e.g. ACL)
   - Attribute-based Access Control is based on user attribute access
* Mandatory Access Control is a system that determines rules based on labels. It is very rare and typically only seen in government organizations where confidentality is a primary factor
* Risk-Based Access Control assess the riskiness of the user's connection

Non-discretionary access control means someone other than the owner gets to determine access such as an IT owner or admin.

## 5.5 - Manage the identity and access provisioning life cycle

Identity life cycle is composed of provisioning, review, and revocation. Access reviews should be conducted on a periodic basis whose frequency may depend on the nature of the role, the company, the industry, and compliance regulations.


# Domain 6: Security Assessment and Testing

## 6.1 - Design and validate assessment, test, and audit strategies

Validation identifies if the right product is being built, while verification identifies if the product is being built right by assessing completeness, correctness, and consistency.

Security assessment and testing provides assurance around the architecture, application, or system, to validate and verify it meets requirements and best practices.

Testing can be internal to an organization, external in the sense that someone in the organization is auditing something external, external in the sense that someone external to the company is auditing something internal, or a third-party audit where the customer, vendor, and an independent audit firm are all engaged.

Security professionals are obligated to identify risk, advise on testing processes, and provide advice and support to stakeholders.

## 6.2 - Conduct Security Control Testing

### Testing Lifecycle
Tests operate with the following lifecycle stages:
1. **Plan:** Requirements gathering.
2. **Design:** System, architecture, and module design.
3. **Develop:** Test suites for development processes, including unit, interface, integration, and system testing.
4. **Deploy:** Test suites for deployment processes, such as vulnerability assessment, log analysis, performance testing, and usability testing.
5. **Operate:** Configuration management reviews and assessment of vulnerabilities and logs.
6. **Retire:** Ensure the integrity of data transfer and defensible destruction of data.

### Types of Testing
- **Unit Testing:** Examines and tests individual components of an application.
- **Interface Testing:** Verifies that components connect properly.
- **Integration Testing:** Focuses on testing groups of components as a whole.
- **System Testing:** Tests the entire, integrated system.

Testing can be conducted manually or automatically.

### Application Security Testing
- **Static Application Security Testing (SAST):** Assesses source code without executing it (white-box testing).
- **Dynamic Application Security Testing (DAST):** Tests an application while it is running (black-box testing).

### Fuzz Testing
Fuzz testing involves chaotic inputs to identify application vulnerabilities:
- **Dumb Fuzzers:** Randomly modify input without understanding its structure.
- **Intelligent Fuzzers:** Generate structured input based on protocols, formatting, and documentation.

### Testing Strategies
- **Positive Testing:** Focuses on system behavior under normal conditions.
- **Negative Testing:** Focuses on behavior under abnormal or erroneous conditions.
- **Misuse Testing:** Evaluates the system's response to intentional exploitation or misuse.

### Test Design Techniques
- **Boundary Value Analysis:** Identifies boundaries where behavior changes (e.g., minimum password length) and tests conditions near those boundaries.
- **Equivalence Partitioning:** Groups inputs with similar behavior for testing.
- **Decision Table Analysis:** Captures input combinations and their corresponding system behavior in a table.
- **State-Based Analysis:** Defines abstract states for a system and compares its actual state to the expected state.

### Vulnerability Assessments and Penetration Testing
- **Vulnerability Assessments:** Automated, non-intrusive techniques to identify weaknesses, often reporting false positives.
- **Penetration Testing:** Manual, intrusive testing to exploit vulnerabilities and confirm their impact.

#### Penetration Testing Steps:
1. **Reconnaissance:** Passively gather publicly available information.
2. **Enumeration:** Actively discover target IP addresses and ports.
3. **Vulnerability Analysis:** Identify potential vulnerabilities.
4. **Execution:** Attempt to exploit vulnerabilities.
5. **Document Findings:** Report severity and outcomes for the audience.

### Types of Testing Approaches
- **Blind Testing:** Assessor has minimal information about the target.
- **Double-Blind Testing:** The internal team is also unaware of the test.
- **Black-Box Testing:** Assessor has no prior knowledge of the system.
- **Gray-Box Testing:** Assessor has partial knowledge of the system.
- **White-Box Testing:** Assessor has full knowledge of the system.

### Vulnerability Management
Effective vulnerability management requires:
- **Asset Inventory:** Accurate tracking of assets.
- **Asset Value Identification:** Data classification, ownership, and categorization.
- **Vulnerability Identification:** Detailing remediation paths for each asset.
- **Ongoing Management:** Regular review of the above processes.

### Automated Tools
- **Authenticated Scans:** Operate with valid user credentials.
- **Unauthenticated Scans:** Operate without user credentials.
- **Banner Grabbing:** Identifies software and versions of underlying systems.
- **Fingerprinting:** Identifies unique characteristics of a system.

### Common Vulnerability Resources
- **Common Vulnerability and Exposures (CVE):** A dictionary of publicly disclosed vulnerabilities.
- **Common Vulnerability Scoring System (CVSS):** Quantitatively measures vulnerabilities on a scale of 1 to 10.

### Log Management and Monitoring
- Regular log reviews identify errors and anomalies such as unauthorized modification or breaches.
- Synchronize log events with a common **Network Time Protocol (NTP)**.

#### Log Data Lifecycle:
1. **Generation**
2. **Transmission**
3. **Collection**
4. **Normalization**
5. **Analysis**
6. **Retention**
7. **Disposal**

#### Log File Management:
- **Circular Overwrite:** Overwrites older logs to avoid storage issues.
- **Clipping Levels:** Avoid storing irrelevant logs by setting thresholds.

### Monitoring Techniques
- **Real User Monitoring:** Passively observes user interactions with applications.
- **Synthetic Performance Monitoring:** Runs scripted transactions to test functionality, availability, and response times.

### Regression Testing
Regression testing ensures that previously tested and functional software remains operational after updates. Key elements include:
- Objective pass/fail criteria.
- Tailored detail for specific audiences.
- Relevant metrics for each test case.


## 6.3 - Collect security process data

Key risk and performance indicators (KRIs and KPIs) should be used to help inform security decision making. KPI's are backward-looking and historical while KRI's are forward-looking indicators. SMART metrics are specific, measureable, achievable, relevant, and timely.

## 6.4 - Analyze test output and generate report

Security assessments and testing should include steps related to remediation, exception handling, and ehtical disclosure.

## 6.5 - Conduct or facilitate security audits.

Audit plans should:
1. Define the audit objective
2. Define the audit scope
3. Conduct the audit
4. Refine the audit process

Audit standards have matured over the years. SAS70 was replaced by SSAE 16 which in turn was replaced by SSAE 18. ISAE 3402 is the international standard in assurance engagements and is quite similar to SSAE 16/18 standards.

System Organization Controls (SOC) Reports come in three types:
* SOC 1 is basic and focuses on financial rpeorting risks
* SOC 2 is much more involved and focused on the controls relating to the five trust principles: security, availability, confidentiality, processing integrity, and privacy.
* SOC 3 reports are stripped down versions of SOC 2 reports and typically used for marketing purposes.

Type 1 SOC reports focuses on a point in time while Type 2 reports focus on a period of time, covering design, and operational effectiveness. For security professionals, SOC 2 Type 2 is most desirable.

Audit roles and responsibilities include the executive senior management which sets the tone from the top, the audit committee is composed of board/senior stakeholders to provide oversight of the audit program. The security officer advises on security related risks to be evaluated. The compliance manager ensures corporate compliance with applicable laws and regulations, professional standards, and company policy. Internal auditors are company employees who provide assurance that corporate internal controls are operating effectively. External auditors provide an unbiased and indepdent audit report as they are independent of the entity being audited.

## 6.3 - Collect Security Process Data

Key risk and performance indicators (KRIs and KPIs) are essential for informed security decision-making:
- **KPIs:** Backward-looking, historical metrics that measure performance against goals.
- **KRIs:** Forward-looking indicators that identify potential risks before they materialize.

Metrics should adhere to the **SMART** framework:
- **Specific**
- **Measurable**
- **Achievable**
- **Relevant**
- **Timely**

---

## 6.4 - Analyze Test Output and Generate Report

Security assessments and testing must address:
1. **Remediation:** Identify and fix vulnerabilities or issues found during testing.
2. **Exception Handling:** Document and manage scenarios where identified risks cannot be remediated.
3. **Ethical Disclosure:** Report findings responsibly to stakeholders, ensuring compliance with laws and ethical standards.

---

## 6.5 - Conduct or Facilitate Security Audits

### Audit Planning
Audit plans should include the following steps:
1. **Define the Audit Objective:** Clarify the purpose and goals of the audit.
2. **Define the Audit Scope:** Specify the systems, processes, and areas to be audited.
3. **Conduct the Audit:** Execute the plan and gather evidence.
4. **Refine the Audit Process:** Evaluate the audit's effectiveness and improve future audits.

### Audit Standards
- **SAS70:** Replaced by **SSAE 16**, which was later replaced by **SSAE 18**.
- **ISAE 3402:** The international standard for assurance engagements, similar to SSAE 16/18.

### SOC Reports
- **SOC 1:** Focuses on financial reporting risks.
- **SOC 2:** Evaluates controls related to the five trust principles: security, availability, confidentiality, processing integrity, and privacy.
- **SOC 3:** A public-facing, simplified version of SOC 2 for marketing purposes.

#### SOC Report Types:
- **Type 1:** Examines controls at a specific point in time.
- **Type 2:** Evaluates controls over a period, addressing design and operational effectiveness.  
  **Note:** For security professionals, **SOC 2 Type 2** is the most comprehensive and relevant.

### Audit Roles and Responsibilities
- **Executive/Senior Management:** Establishes the audit's importance and tone from the top.
- **Audit Committee:** Composed of senior stakeholders; oversees the audit program.
- **Security Officer:** Advises on security-related risks and controls to be evaluated.
- **Compliance Manager:** Ensures adherence to laws, regulations, standards, and policies.
- **Internal Auditors:** Employees who ensure corporate internal controls are effective.
- **External Auditors:** Independent professionals who provide unbiased audit reports.


# Domain 7: Security Operations

## 7.1 - Understand and comply with investigations

Investigations are a standard practice in security operations. Investigators must conduct reliable investigations that can withstand scrutiny and cross-examination. A key requirement is securing the scene, as contaminated evidence cannot be uncontaminated.

The forensic process includes identifying and securing the crime scene, properly collecting evidence to preserve integrity and chain of custody, examining all evidence, analyzing the most pertinent evidence, and preparing a final report.

There are various types of evidence:
- Real evidence refers to physical objects.
- Direct evidence requires no inference.
- Circumstantial evidence suggests a fact by inference.
- Corroborative evidence supports other facts but is not a fact itself.
- Hearsay evidence is from witnesses who were not present.
- The best evidence rule states original evidence should be entered rather than a copy.
- Secondary evidence is a reproduction or substitute for the original proof.

Motive, Opportunity, and Means (MOM) analysis is often used to guide investigations. Locard's Exchange Principle states that every interaction leaves a trace; something is taken, and something is left behind.

Digital forensics involves the scientific examination and analysis of data from storage media. Live evidence, such as data in RAM, CPU, or cache, changes when examined, creating contamination risks. Hard drives are a significant source of direct evidence, and best practices involve creating two identical bit-for-bit copies of the original drive, sealing the original, and examining the copy. Mobile device forensic analysis is challenging due to variations in firmware, operating systems, and tools, often requiring tailored methods. Forensic artifacts are remnants of a breach or attempted breach.

The chain of custody is crucial for maintaining full control over evidence and ensuring its integrity.

Evidence must meet five rules: authenticity, accuracy (integrity), completeness, convincing reliability, and admissibility.

Investigations may be criminal (involving legal punishment), civil (resolving disputes with financial penalties), regulatory (handled by regulatory bodies), or administrative (internal to an organization).

## 7.2 - Conduct logging and monitoring activities

Security Information and Event Management (SIEM) systems collect logs from multiple sources, compile and analyze log entries, and report relevant information. SIEM correlation engines provide actionable insights to security analysts, enabling them to take corrective action.

SIEM-driven workflows include the following steps:
1. **Aggregation:** Collecting logs and data from various sources.
2. **Normalization:** Converting data into a standard format for analysis.
3. **Correlation:** Identifying relationships and patterns between events.
4. **Secure Storage:** Ensuring collected logs are stored securely and tamper-proof.
5. **Analysis:** Examining data to detect trends, anomalies, and potential threats.
6. **Reporting:** Generating insights and sharing findings with stakeholders.

Event data can originate from security appliances, network devices, DLP systems, data activity monitoring tools, applications, operating systems, servers, and IPS/IDS systems.

Threat intelligence encompasses all research, analysis, and understanding of current threat trends to proactively identify and mitigate risks.

User and Entity Behavior Analytics (UEBA), often integrated into SIEM solutions, analyze patterns of user or entity behavior. By identifying normal patterns, UEBA helps highlight anomalies that may indicate malicious activity.

A SIEM system should be continuously configured and reviewed to align with organizational needs and evolving threat trends.

## 7.3 - Perform configuration management (CM)

Asset management addresses the lifecycle of organizational assets. The steps in the lifecycle include:
1. **Plan:** Define asset requirements and objectives.
2. **Request:** Formally initiate the acquisition process.
3. **Procure:** Acquire the asset through purchase or other means.
4. **Receive:** Take possession and verify the asset.
5. **Manage:** Deploy, maintain, and monitor the asset throughout its use.
6. **Retire:** Decommission and securely dispose of the asset.

Configuration management relies on standardized policies, standards, and baselines to ensure consistent and controlled management of all company assets. Automation is often employed to streamline and enforce configuration controls.

## 7.4 - Apply foundational security operations concepts

There are several key concepts for security operations:
* Privileged account management refers to the access controls surrounding privileged or system administrator accounts.
* Need to know - restriction of knowledge
* Least privilege - restriction of actions/privileges
* Job rotation - Switching job duties to help with fraud detection
* Service level agreements (SLAs) - Legal contracts between a customer and vendor

## 7.5 - Apply resource protection techniques

Data protection requirements may vary depending on the specific data retention policies for individual resources. This may help determine the medium or media upon which thes eshould be stored. The Mean Time Between Failure (MTBF) can help determine the best media to use for a given need. Operational processes must be put in place to constantly move data to new media, updating file formats consistently, the protection of data must be updated to reflect current cryptography standards.

## 7.6 - Conduct incident management

Incident response is the process used to both detect and respond to incidents with these goals:
* Provide an effective and efficient response to reduce impact to the organization
* Maintain or restore business continuuity
* Defend against future attacks

It is important to distinguish between an event (an observable occurrence) and an incident (an adverse event).

Incident response should include the following stages:
1. Preparation
2. Detection - identify and activate incident
3. Response - activate IR team
4. Mitigation - containment
5. Reporting
6. Recovery - return to normal
7. Remediation - prevent
8. Lessons Learned
