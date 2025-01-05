# CISSP

These are my collated notes for the CISSP exam.

Sources include:
* Witcher, Rob, John Berti, Lou Hablas, and Nick Mitropoulos. Destination CISSP: A Concise Guide. 1st ed. Destination Certification, Inc., 2022.

# Domain 1: Security and Risk Management

## - 1.1 Professional ethics

CISSP holders are subject to the (ISC)^2 Code of Professional Ethics whose preamble holds to the following: "The safety and welfare of society and the common good, duty to our principals, and to each other, requires that we adhere to, and be seen to adhere, to the highest ethical standards of behavior."

The (ISC)^2 Code of Ethics holds the following canons, prioritized in order of importance:
1. Protect society, the common good, necessary public trust and confidence, and the infrastructure
2. Act honorably, honestly, justly, responsibly, and legally
3. Provide diligent and competent service to principals
4. Advance and protect the profession

## - 1.2 Understand and apply security concepts

The CIA triad consists of confidentiality, integrity, and availability:
* Confidentiality: Protects assets using important principles such as need-to-know and least privilege; prevents unauthorized disclosure
* Integrity: Protects and adds value to assets by making them more accurate, more timely, more current, more meaningful; prevents unauthorized or accidental changes to assets such as information
* Availability: Protects critical assets based on value to ensure organizational assets are available when required by stakeholders

These three pillars have been supplemented by two more:
* Authenticity: Proves assets are legitimate and bona fide, and verifies that they are trusted and verified. Proves the source and origin of important valuable assets. Also referred to as "proof of origin".
* Nonrepudiation: Provides assurance that someone cannot dispute the validity of something; the inability to refute accountability or responsibility. Also, inability to deny having done something.

## - 1.3 Evaluate and apply security governance principles

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