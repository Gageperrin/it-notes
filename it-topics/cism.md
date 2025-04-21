# CISM

These are my manually compiled notes for the CISM exam.

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

Controls can be prevenThere are seven major kinds of risk controls:
1. Directive controls - direct, confine, or control the actions of subjects to encourage compliance with security policies.
2. Deterrent controls - discourage violation of security policies
3. Preventive controls - prevent undesired action or events.
4. Detective controls - identify if the risk has occurred.
5. Corrective controls - minimize the negative impact of a risk occurring.
6. Recovery controls - recover the system or process and return to normal operation.
7. Compensating controls - auxiliary support of other controls. If one control is bypassed, what is the secondary strategy.