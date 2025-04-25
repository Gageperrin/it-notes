# CISM

These are my manually compiled notes for the CISM exam. Due to the level of overlap with CISSP, I ended up opting not to include information in this page that was not already included in my CISSP notes.

Sources include:
* Chapple, Mike. *Certified Information Security Manager Study Guide*. Wiley and Sons, 2022.


# Chapter 1 - Today's Information Security Manager

Information security managers are responsible for the ongoing execution of the goals of their business' cybersecurity program.

Cybersecurity operates under the CIA triad:
* Confidentiality: Protects assets using important principles such as need-to-know and least privilege; prevents unauthorized disclosure
* Integrity: Protects and adds value to assets by making them more accurate, more timely, more current, more meaningful; prevents unauthorized or accidental changes to assets such as information
* Availability: Protects critical assets based on value to ensure organizational assets are available when required by stakeholders

Information security managers must be both security experts and business leaders.

The most senior information security leader in the organization is most frequently the CISO, though this may also fall under the VP or director level.

Cybersecurity can often be subjugated to another organizational function such as the CTO (development) or the CIO (IT operations) but this can often introduce a conflict of interest where cybersecurity is bypassed in favor of that department's primary function they are incentivized to uphold.

The CISO often bears ultimate *responsibility* for the cybersecurity program. Depending on the size of the organization, directors may report to the CISO with manager underneath the directors and individual contributors beneath the managers, though this may vary. The span of control represents the number of individuals who directly report to a position.

The RACI matrix is a common tool to specify how roles and responsibilities are shared throughout an organization:
* **R - Responsible**: The person(s) who perform the work or complete the task. There should be exactly one person with this role for each task.
* **A - Accountable**: The person who is ultimately answerable for the task or deliverable. This is typically the person who signs off on the work.
* **C - Consulted**: People whose opinions are sought before the task is completed. These are subject matter experts who provide input.
* **I - Informed**: People who need to be kept up-to-date on progress and outcomes, but don't need to be consulted or make decisions.

Security incidents are when an organization experiences an adverse impact to the confidentiality, integrity, or availability of information or information systems.

The DAD Triad which is an inversion of the CIA triad can ennumerate how this occurs:
* **D - Disclosure**: The unauthorized exposure of information to individuals, entities, or processes that are not authorized to have access to it. This is the opposite of Confidentiality.
* **A - Alteration**: The unauthorized modification of information or systems. This is the opposite of Integrity.
* **D - Destruction**: The unauthorized disruption of access to or use of information or systems. This is the opposite of Availability.

The impact of these incidents can be classified according to different kidns of risk for the organization:
* Financial risk is the risk of monetary damage
* Reputational risk is the risk of negative publicity or loss of goodwill from customers, investors, etc.
* Strategic risk is the risk that could undermine the accomplishment of strategic objectives
* Operational risk is the risk that could inhibit a company's ability to carry out day-to-day functions
* Compliance risk is when a security breach could jeopardize the company's ability to fulfill legal or compliance requirements

---

The information security leader is responsbile to create, implement, and maintain an information security strategy. This can be accomplished through several assessments.

Threat research investigates the threats the organization is suceptible to inlcuding the threat actors who could seek to undermine the security of the organization and the threat vectors which are tactics, tools, or techniques leveraged by those threat actors to achieve their objectives.

A SWOT (Strengths, Weaknesses, Opportunities, Threats) analysis is a strategic planning framework that helps organizations evaluate their current security posture and develop effective strategies:
* Strengths represent internal security capabilities that provide a competitive advantage, such as strong encryption practices or a skilled security team. 
* Weaknesses are internal vulnerabilities that could be exploited, like outdated systems or insufficient access controls. 
* Opportunities are external factors that could enhance security posture, such as emerging security technologies or industry partnerships. 
* Threats are external risks that could compromise security, including new attack vectors or regulatory changes.

Gap analysis starts by defining security requirements through a series of control objectives that describe the plan to manage identified risks. Control objectives are strategic in focus. The current gap between that intended control state can then be identified.

COBIT is one such control objectives framework (covered later).

The Capability Maturity Model Integration (CMMI) is another model with five defined levels:
* **Level 1 - Initial**: Processes are unpredictable, poorly controlled, and reactive. Security practices are often ad-hoc and inconsistent.
* **Level 2 - Managed**: Basic project management processes are established. Security controls are documented and implemented, but may not be consistently applied across the organization.
* **Level 3 - Defined**: Processes are well-characterized, understood, and documented. Security practices are standardized and integrated into the organization's processes.
* **Level 4 - Quantitatively Managed**: Processes are measured and controlled using statistical and quantitative techniques. Security metrics are collected and used to manage performance.
* **Level 5 - Optimizing**: Focus is on continuous process improvement. Security practices are continuously refined based on quantitative feedback and innovative ideas.

When setting security objectives, they should follow the SMART criteria:
* **S - Specific**: Goals should be clearly defined with no ambiguity about what needs to be accomplished.
* **M - Measurable**: Goals should include quantifiable metrics to track progress and determine when they've been achieved.
* **A - Achievable**: Goals should be realistic and attainable with available resources and constraints.
* **R - Relevant**: Goals should align with the organization's overall mission and security strategy.
* **T - Time-bound**: Goals should have specific deadlines or timeframes for completion.

Cybersecurity roles and responsibilities around data can be divided into these categories:
* Data Owners are senior-level individual who is ultimately responsible for classifying, defining, and protecting data within an organization including the policies and baselines to do so.
* Asset owners manage hardware, software, or IT infrastructure, enforcing the policies defined by Data Owner.
* Data Stewards handle the implementation of high-level policies through day-to-day decisions of implementing the policy (often a director).
* Data Custodians implement technical security controls and operational management of data per the policies of the Data Owner.
* Data Processors process data on behalf of the Data Owner. They do not own or control the data or make decisions on it but stick to the rules.
* Data subjects are the individuals whose personal data is collected.

Security controls can be categorized based on their mechanism of action:
* Technical controls enforce confidentiality, integrity, and availability in the digital space.
* Operational controls include processes to manaeg technology in a secure manner.
* Managerial controls are procedural mechanisms focused on the implementation of risk management.

There are seven major kinds of risk controls:
1. Directive controls - direct, confine, or control the actions of subjects to encourage compliance with security policies.
2. Deterrent controls - discourage violation of security policies
3. Preventive controls - prevent undesired action or events.
4. Detective controls - identify if the risk has occurred.
5. Corrective controls - minimize the negative impact of a risk occurring.
6. Recovery controls - recover the system or process and return to normal operation.
7. Compensating controls - auxiliary support of other controls. If one control is bypassed, what is the secondary strategy.

# Chapter 2 - Information Security Governance and Compliance

Governance programs are the sets of procedures and controls put in place to allow an organization to effectively direct its work.

Corporate governance is directed by the board of directors who have ultimate authority over the organization. It is considered best practice (and in some cases required) for board members to be independent directors who do not have a significant relationship with the company.

The chief executive officer (CEO) manages the company's operations and is approved and reviewed by the board.

Organizations carry out governance through governance, risk, and compliance (GRC) programs.

The CISO must work with senior management peers to design and implement an informaiton security governance framework to guide the activity of the information security governance framework that guides the activity of the information security function and ensures alignment with the information security strategy.

Cybersecurity buy-in is often dependent upon developed business cases. These generally include:
* A scope statement to concisely describe the proposed initiative
* A strategic context that demonstrates the need for the initiative and how investment aligns with broader strategic goals
* A cost analysis that outlines the financial and human resource costs of the initiative as a one-time and recurring basis
* An evaluation of alternatives for accomplishing the project goals and why the current proposal is the best available option
* A project plan that describes the detailed implementation plan for the initiative
* A management plan that describes how the organization will oversee the processes created by this initiative on an ongoing basis in a manner that integrates with broader frameworks

Third-party relationships between companies, and sometimes between departments or teams can be managed through the following agreements:
* **Master Service Agreement (MSA)**: A comprehensive contract that establishes the terms and conditions for all future transactions between parties. It typically covers pricing, service levels, intellectual property rights, and dispute resolution. MSAs are particularly important for establishing security requirements and data handling practices for long-term vendor relationships.
* **Service Level Agreement (SLA)**: A specific commitment between a service provider and a client that defines the level of service expected, including metrics like uptime, response times, and performance standards. SLAs are crucial for ensuring vendors maintain appropriate security controls and incident response capabilities.
* **Memorandum of Understanding (MOU)**: A letter written to document aspects of the relationship as an informal mechanism that is not legally binding but can help prevent misunderstanding.
* **Blanket Purchase Agreement (BPA)**: A simplified procurement method that establishes terms for future purchases of goods or services. BPAs often include security requirements and compliance standards that vendors must meet when providing products or services.
* **Non-Disclosure Agreement (NDA)**: A legal contract that establishes confidentiality between parties, preventing the sharing of sensitive information. NDAs are essential for protecting intellectual property, trade secrets, and sensitive security information when working with third parties.

End of Life (EOL), End of Service Life (EOSL), and End of Support (EOS) are critical concepts in asset lifecycle management. EOL indicates when a product is no longer manufactured or sold, while EOSL marks when a vendor stops providing maintenance services and security updates. EOS represents the final date when technical support and security patches are discontinued, which is particularly important for security planning as systems without support may become vulnerable to new threats and should be replaced or isolated from critical networks.

Information Security Policies are fundamental documents that establish the framework for an organization's security program. Policies are broad statements of management intent. Compliance with policies is mandatory for all personnel and systems.

Supporting documentation includes:
* **Standards**: Mandatory requirements for hardware, software, and security mechanisms that support policy implementation
* **Procedures**: Detailed, step-by-step instructions for performing specific security tasks
* **Baselines**: Minimum security configurations required for systems and applications
* **Guidelines**: Recommended practices and approaches that support policy implementation

Procedures can be subdivided into monitoring procedures, evidence production procedures, and patching procedures (change management).

Organizations have a variety of control standard frameworks to choose from.

ISACA defines three principles for IT Governance:
1. A governance framework should be based upon a conceptual model, identifying the key componetns and relationships among components, to maximize consistency and allow automation.
2. A governance framework should be open and flexible. It should allow the addition of new content and the ability to address new issues in the most flexible way, while maintaining integrity and consistency.
3. A governance framework should align to relevant major related stadnards, frameworks, and regulations.

ISACA promotes the Control Objectives for Information Technology (COBIT) which has the following six principles:
1. Each enterprise needs a governance system to satisfy stakeholder needs and to generate value from the use of information and technology.
2. A governance system for enterprise information and technology is built from a number of components that can be of different types and work together in a holistic way.
3. A governance system should be dynamic.
4. A governance system should clearly distinguish between governance and management activities and structures.
5. A governance system should be tailored to the enterprise's needs.
6. A governance system should cover the enterprise end-to-end, focusing not only on the function but on all technology and information processing the enterprise puts in place to achieve its goals

COBIT divides these principles into five domains:
1. Evaluate, Direct, and Monitor (EDM): IT governance and the selection and monitoring of strategic goals
2. Align, Plan, and Organize (APO): How the IT function should be organized and structure its work
3. Build, Acquire, and Implement (BAI): How the IT organization should create and acquire new information systems to integrate within the business
4. Deliver, Service, and Support (DSS): How the organization should manage the operational tasks of information technology.
5. Monitor, Evaluate, and Assess (MEA): How the organization should measure its effectiveness against performance targets, control objectives, and any external requirements it faces.

The components that can meet these objectives include:
* Processes
* Services, infrastructure, and applications
* People, skills, and competencies
* Culture, ethics, and behavior
* Information
* Principles, policies, and procedures
* Organizational structures

Aside from ISACA, NIST has provided its own frameworks.

In 2018, NIST released v1.1 of its Cybersecurity Framework (CSF) to meet five objectives:
1. Describe their current cybersecurity posture.
2. Describe their target state for cybersecurity.
3. Identify and prioritize opportunities for improvement within the context of a continuous and repeatable process.
4. Assess progress toward the target state.
5. Communicate among internal and external stakeholders about cybersecurity risk.

The NIST framework consists of three components:
1. The Framework Core: Identify, protect, detect, respond, and recover as documented in *Framework for Improving Critical Infrastructure Cybersecurity* 
2. The Framework Implmentation: A maturity model that assesses how an organization is positioned to meet its cybersecurity objectives. The tiers are: (1) partial, (2) risk informed, (3) repeatable, (4) adapative.
3. Framework Profiles: How a specific organization might approach the security functions described in the Core.

Aside from the CSF, NIST publishes a Risk Management Framework under (NIST SP 800-37). This is a mandatory standard for federal agencies and is a formal process for implementing security controls.

The seven stages of the RMF are:
1. Prepare
2. Categorize
3. Select
4. Implement
5. Assess
6. Authorize
7. Monitor

ISO also publsihes a series of standards around cybersecurity and privacy best practices:
* ISO 27001 was once a very common standard covering 14 categories of control objectives
* ISO 27002 describes the actual controls that an organization may implement to meet cybersecurity objectives. This is more a guideline than a standard
* ISO 27004 helps organizations consistently monitor, measure, analyze, and evalutae its information security management function
* ISO 27701 offers guidance on privacy
* ISO 31000 provides guidelines for risk management

Lastly, the American Institute for Certified Public Accountants (AICPA) provides service organization controls (SOC) audits to review alignment with the Statement on Standards for Attestations Engagements (SSAE 18).

SOC Reports come in three types:
* SOC 1 is basic and focuses on financial rpeorting risks
* SOC 2 is much more involved and focused on the controls relating to the five trust principles: security, availability, confidentiality, processing integrity, and privacy.
* SOC 3 reports are stripped down versions of SOC 2 reports and typically used for marketing purposes.

Type 1 SOC reports focuses on a point in time while Type 2 reports focus on a period of time, covering design, and operational effectiveness. For security professionals, SOC 2 Type 2 is most desirable.

# Chapter 3 - Information Risk Management

* Threats are any possible events that may have an adverse impact on the confidentiality, integrity, and/or availability of information or information systems
* Vulnerabilities are weaknesses in our systems or controls that could be exploited by a threat.
* Risks are the intersection of a vulnerability and a threat.

One form of quantitative risk analysis is the Annualized Loss Expectancy which expresses the annual cost of the risk materializing.

SLE = AV * EF
ALE = SLE * ARO

* Single Loss Expectancy (SLE) - Denotes the cost if the risk occurs once
* Asset Value (AV) - The cost of the asset in a monetary value
* Exposure Factor (EF) - Measured as a percentage and expresses how much of the asset's value stands to be lost in case of a risk materializing
* Annualized Loss Expectancy - Expresses the annual cost of the risk materializing.

Risk can be managed by:
1. *Avoiding* it, which means not doing the thing that exposes the risk
2. *Transferring* it, by moving the cost of the risk elsewhere, such as an insurance company
3. *Mitigate* it, implement controls to reduce the risk to an acceptable level
4. *Accept* it, the asset owner or senior management (the accountable party) accepts the risk. If a risk is accepted, the reasons why must be documented alongside contingencies which may impact this decision during future review.

Key risk indicators can facilitate risk monitoring. Other important concepts for risk maangement include:
* Risk treatement: Systematic response to organizational risks
* Risk appetite: The level of risk the organization is *willing* to accept as a cost of doing business
* Risk capacity: The level of risk the organization is *able* to expose itself to and be able to reasonably maintain operations

A business impact analysis (BIA) is a formal process to identify mission-essential functions within an organization. It includes:
1. The mean time between failures (MTBF) is the reliability of a system.
2. The mean time to repair (MTTR) is the average amount of time to restore a system to its normal operating state after a failure.
3. The recovery time objective (RTO) is the amount of time the organization can tolerate a system being down
4. The recovery point objective (RPO) is the amount of data an organization can tolerate losing during an outage

# Chapter 4 - Cybersecurity Threats

Threats can vary widely in nature and can be uncovered by "hat" hackers:
* White-hat hackers are authorized attackers who act with authorization to discover security vulnerabilities with the intent of correcting them.
* Black-hat hackers are unauthorized attackers with malicious intent.
* Gray-hat hackers act without proper authorization but intend to inform targets of security vulnerabilities without maliciously exploiting them

Advanced persistent threats (APTs) are sophisticated attacks over a period of time and can often be tied to nation-state actors.

Threat intelligence sharing between organizations is facilitated by standardized protocols:
* **Structured Threat Information Expression (STIX)**: A standardized XML language for representing threat information, including indicators of compromise (IOCs), threat actors, and attack patterns. STIX uses JSON format to describe cyber threat information in a consistent, machine-readable format.
* **Trusted Automated Exchange of Intelligence Information (TAXII)**: A protocol for exchanging cyber threat intelligence over HTTPS. TAXII defines a set of services and message exchanges that enable organizations to share threat information in a secure, automated way using STIX-formatted data.
* **OpenIOC**: An extensible XML schema developed by Mandiant (now part of Google) for describing technical indicators of compromise. It provides a standardized format for sharing forensic data and threat intelligence, particularly useful for incident response and malware analysis.

# Chapter 5 - Information Security Program Development and Management

The CISO's primary responsibility is to develop, implement, and maintain the organization's information security program. This program is a collection of policies, standards, processes, activities, and projects that work together to achieve the goals set forth in the organization's information security strategy.

First, a scope needs to be identified for the information security program. Two elements are included in the scope:
1. The type of the security objectives included in the program.
2. The portion of the organization covered by the information security program.

Second, an information security program charter will be the organizing document for the cybersecurity program, building off the scope. This charter should include:
* A scope statement
* A business purpose clearly linking the information security program to business objectives
* A statement of authority for the program
* Roles and responsiblities for other stakeholders to carry out the activities of the information security program
* Governance structure and processes that will continue to ensure the organization's security program remains aligned with organization business goals
* Program documentation procedures that formalize how the organization will establish, communicate, and maintain information security standards, guidelines, procedures, etc.
* Enforcement mechanisms to establish how the organization will guide and enforce compliance with information security policies and provide consequences for individuals who choose not to comply
* A review process conducted on a periodic basis to ensure that hte informaiton security program continues to achieve the information security objectives
* An approval statement that describes the authority under which the progam is enacted.

It is one thing to set up an information security program but another to maintain it. Metrics are critical to ensure the efficiency and effectiveness of security controls on an ongoing basis.
* Key performance indicators (KPIs) are metrics that demonstrate the success of the security program in achieving its objectives. They are historical in focus.
* Key goal indicators (KGIs) measure progress toward defined goals.
* Key risk indicators (KRIs) quantify the security risk facing an organization, selected based on potential impact, effort, reliability, and sensitivity.

Security budgets can have two approaches:
1. Incremental budgeting starts with the prior year's budget and then adjusts by either raising or lowering the budget.
2. Zero-based budgeting starts with zero each year and managers are asked ot justify their entire budget.

Fiscal years may or may not align to the calendar year and this is important for understanding budgets.

Capital expenses (CapEx) are costs an orgnaization incurs as part of building or maintaining large assets. Operational expenses (OpEx) are costs for running the business day-to-day.

# Chapter 6 - Security Assessment and Testing

Asset inventory and asset criticality should determine testing and asssessment needs.

Security scanners can cover networks or web applications. Vulnerability scanners should be kept up-to-date with vulnerability feeds.

Other data sources for cybersecurity includes log reviews, SIEM, and configuration management systems.

# Chapter 7 - Cybersecurity Technology

(This is largely duplicated information from the CISSP exam content.)

Fagan inspection (SDLC):
1. Planning - Preparation of materials, attendees, and location
2. Overview - Prepares the team by reviewing materials and assigning roles
3. Preparation - Reviewing code and documenting issues/questions
4. Meeting to identify defects based on notes from preparation phase
5. Rework to resolve issues
6. Follow-up by moderator to ensure resolution

# Chapter 8 - Incident Response

A security event is any observable occurrency in a system or network that relates to a security function.

An adverse event is an event with negative consequences.

A security incident is a violation or imminent threat of violation of security policies or practices, particularly violating the CIA triad.

Computer security incident response teams (CSIRTs) are responsible for responding to computer security incidents by following standardized repsonse procedures.

NIST 800-61 describes four categories of security event indicators: alrts, logs, publicly available information, and people.

The incident repsonse process should follow four cyclical phases:
1. Preparation - Cybersecurity best practices for environment management
2. Detection & Analysis - Follow best practices to build a systematic process for detecting and triggering the CSIRT
3. Containment, Eradication and Recovery - (1) Select containment strategy, (2) Implement selected containment strategy, (3) Gather additional evidence to support repsonse, (4) Identify attacker, (5) Eradicate effects of the incident and recover normal business operations
4. Post-Incident Activity - Lessons Learned review, Evidence Retention, final report

The Incident response policy should tbe the cornerstone of the organization's incident response program.

The policy should include:
* Statement of management commitment
* Purpose and objectives of the policy
* Scope of the policy
* Definition of cybersecurity incidents and related terms
* Organizational structure and definition of roles, responsibilities, and level of authority
* Prioritization or severity rating scheme for incidents
* Performance measures for the CSIRT
* Reporting and contact forms

Procedures provide the detailed, tactical information for teh CSIRT to respond to an incident.

CSIRTs should develop playbooks that describe the specific procedures they will follow in the event of a specific type of cybersecurity incident.

CSIRT scope can vary organization by organization and depend on these questions:
* What triggers the activation of the CSIRT? Who is authorized to activate the CSIRT?
* Does the CSIRT cover the entire organization or is it responsible only for certain business units?
* Is the CSIRT authorized to communicate with law enforcement, regulatory bodies, or other external parties?
* Does the CSIRT have internal communication and/or escalation responsibilities?

NIST 800-61 includes specifications around the functional, economic, and recoverability impact of various incidents.