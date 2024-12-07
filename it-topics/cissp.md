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
* Internatioanl Traffic in Arms Regulation (ITAR) is a US regulation built to ensure control over any exports of items belonging to the United States Munitions List (USML). It falls under the Department of State's Directorate of Defense Trade Controls (DDTC)
* Export Administration Regulations (EAR) focuses on export controls around commercial use items. It falls under the US Department of Commerce's Bureau of Industry and Security (BIS)

Privacy also falls under legal scope. Privacy is the state or condiiton of being free from being observed or disturbed by other people. This can include Personally Identifiable Information (PII), Personal Health Information (PHI), Sensitive Personal Information (SPI), and Personal Cardholder Information (PCI). These combined with IP are sensitive data.

Personal data may include direct or indirect identfiers. Direct identifiers are unique identifiers for an individual (phone number, SSN) while indirect identifiers may be used to narrow the scope of identification (age, zip code).

Privacy policies are required to speak to how:
* Data owners have defined accountabilities regarding the collection and protection of customer data
* Data custodians need to have defined responsibilities based on the input of owners
* Data prcoessors need to have clearly defined responsibilities when processing data on behalf of the owner
* Data subjects which are the individuals that the data belongs to

General Data Protection Regulation (GDPR) principles: