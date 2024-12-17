# CISSP

These are my collated notes for the CISSP exam.

Sources include:
* Witcher, Rob, John Berti, Lou Hablas, and Nick Mitropoulos. Destination CISSP: A Concise Guide. 1st ed. Destination Certification, Inc., 2022.

# Domain 1: Security and Risk Management

## - 1.1 Professional ethics

CISSP holders are subject to the (ISC)^2 Code of Professional Ethics whose preamble holds to the fhollwoing: "The safety and welfare of society and the common good, duty to our principals, and to each other, requires that we adhere to, and be seen to adhere, to the highest ethical standards of behavior."

The (ISC)^2 Code of Ethics holds the following canons, prioritized in order of importance:
1. Protect society, the common good, necessary public trust and confidence, and the infrastructure
2. Act honorably, honestly, justly, responsibly, and legally
3. Provide diligent and competent service to principals
4. Advance and protect the profession

## - 1.2 Understand and apply security concepts

The CIA triad consists of confidentiality, integrity, and availability:
* Confidentiality: Protects assets using important principlpes such as need-to-know and least privilege; prevents unauthorized disclosure
* Integrity: Protects and adds value to assets by making them more accurate, more timely, more current, more meaningful; prevents unauthorized or accidental changes to assets such as information
* Availability: Protects critical assets based on value to ensure organizational assets are available when required by stekholders

These three pillars have been supplemented by two more:
* Authenticity: Proves assets are legitimate and bona fide, and verifies that htey are trusted and verified. Proves the source and origin of important valuable assets. Also referred to as "proof of origin".
* Nonrepudiation: Provides assurance that someone cannot dispute the validity of something; the inability to refute accountability or responsibility. Also, inability to deny having done something.

## - 1.3 Evaluate and apply security governance principles

Governance is intended to enhance organizational value based on the goals and objectives of the organizaiton. Security must be managed top donw.

Two key parts of this are (1) scoping and (2) tailoring. Scoping determines which potential control elements are in scope. Tailoring then considers in scope security control elements and further refines or enhances them so they are most effective and aligned with the goals of the organization.

When enforcing these policies, only one person, group, or entity should be accountable for security, even if everyone else is also responsible for doing their part. At the highest level, the Board of Directors is ultimately accountable for security.

At a high level:
* Owners/Controllers/Senior Management are *accountable* for (1) ensuring appropriate security controls are implemented, (2) determining the appropriate sensitivity or classification levels, and (3) determining access privileges
* Security Professionals/IT Security Officers are responsible for design, implementation, management, and review of the organization's security policies, standards, baselines, procedurers, and guidelines
* IT Officers are responsible for developing and implementing technology solutions, working with the security officers to evaluate security strategies, andworking closely with Business Continuity managers to ensure continuity of operations should disruption occur
* IT Function is responsible for implementing and adhering to security policies
* Operators/Administrators are responsible for managing, troubleshooting, and applying hardware and software patches to systems as necessary, managing user permissions per owner specifications, and administrering and managing specific applications and services
* Network Administrators are responsible for maintaining computer networks and installing and configuring networking equipment and resolving problems
* Information system auditors are responsible for providing management with independent assurance that the security objectives are appropriate, determining whether the security policy, standards, baselines, procedures, and guidelines are appropriate and effective given the company's security objectives, and determining whether the objectives have been met
* Users are responsible for adherence to security and ToS policies. Preserving the availabilty, integrity, and confidentiality of assets when accessing and using them

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
* The Wassenar Arrangement is an agreement by a number of countries to share cryptographic information with one another while also protecting it from terrorists
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

Because of the importance of privacy, Privacy Impact Assessments (PIA) and Data Protection Impact Assessments (DPIA) may be taken to assess an organization's handling of data in accordance of privacy standards. PIA takes the following steps:
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

The board or CEO often institutes as Overarching Security Policy which is a broader statement of goals and objectives that also grants authority to security activities. Functional policies then extend this to provide more specificity that can be applied to:
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
* Vulnerability: A weakness in an asset tha could be exploited by a threat
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

Risk management must be applied to very link in the supply chain, including vendors and service providers.

There are four primary risk management frameworks:
1. NIST SP 800-37 (RMF) - describes the risk management framework (RMF) and provides guidelines for applying it to information systems and organizations.
2. ISO 31000 - a family of standards and best practices relating to risk management.
3. COSO - a definition to essential enterprise risk management components, reviewing ERM principles and concepts
4. ISACA Risk IT Framework - ISACA's Risk IT Framework contains guidelines and practices fo risk optimization, security, and business value. It aligns with COBIT.

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

The Process for Attack Simulation and Threat Analysis (PASTA) is attacker-focused and include seven stages.
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

Classifying information can help identify critical information, its sensitivity to modification as well as promote commitment to both protecting valuable assets and keep it confidential where applicable.

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

When destroying data, defensible destruction means being able tgo prove there is no possible way for anyone to recover the data.

To destroy data is to physically destroy the media and is most effective. To purge uses logical/physical techniques to sanitize data so that it cannot be reconstructed. Clearing data uses logical techniques but the data may be reconstructed. Various techniques that employ this include incineration, shredding, disintegration, drilling holdes, degaussing, crypto shredding, overwriting, wiping, erasing, or formatting.

Object reuse is a security method that overwrites data from media. SSD's present a problem for this with how they use flash memory technology to represent binary data. SSD vendors generally provide their own tols to securely remove data. If all else fails, it can be physically destroyed.

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