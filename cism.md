
* HIPAA
* PCI DSS
* GLBA
* SOX
* GDPR
* FERPA


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
* Risk capacity: The level of risk the organization is *able* to expose itself to and be able to reasonable maintani operations

A business impact analysis (BIA) is a formal process to identify mission-essential functions within an organization. It includes:
1. The mean time between failures (MTBF) is the reliability of a system.
2. The mean time to repair (MTTR) is the average amount of time to restore a system to its normal operating state after a failure.
3. The recovery time objective (RTO) is the amount of time the organization can tolerate a system being down
4. The recovery point objective (RPO) is the amount of data an organization can tolerate losing during an outage