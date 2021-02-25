# AWS Security, Identity, and Compliance Services

## Identity and Access Management

### AWS IAM

[Fill in later.]

### AWS Single Sign-On

> [AWS Single Sign-On](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) is a cloud-based single sign-on (SSO) service that makes it easy to centrally manage SSO access to all of your AWS accounts and cloud applications. Specifically, it helps you manage SSO access and user permissions across all your AWS accounts in AWS Organizations. AWS SSO also helps you manage access and permissions to commonly used third-party software as a service (SaaS) applications, AWS SSO-integrated applications as well as custom applications that support Security Assertion Markup Language (SAML) 2.0. AWS SSO includes a user portal where your end-users can find and access all their assigned AWS accounts, cloud applications, and custom applications in one place.

* AWS SSO has a built-in identity store.
* It can connect AWS SSO to both [AWS Managed Microsoft AD](https://docs.aws.amazon.com/singlesignon/latest/userguide/connectawsad.html) and [on-premises Microsoft AD](https://docs.aws.amazon.com/singlesignon/latest/userguide/connectonpremad.html) (two way trust needed for latter). Other options include the Okta Universal Directory and Azure Active Directory (Azure AD).
* AWS SSO is best practice over traditional workforce identity federation. It is offered for free.
* Only one directory or identity provider can be connected to SSO at a time, but the identity source can be changed.
* Customer identities **do not use SSO**. They use Cognito. SSO is for **workplace identities**. It does not support AWS IAM users or groups.
* SSO permissions management can be autoamted through API or CloudFormation. Attribute-based access can be enabled as well.

### Amazon Cognito

> [Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) provides authentication, authorization, and user management for your web and mobile apps. Your users can sign in directly with a user name and password, or through a third party such as Facebook, Amazon, Google or Apple.
> 

AWS Cognito provides authentication, authorization, and user management on web and mobile applications. The two main components of Amazon Cognito are user pools and identity pools. 

User pools are user directories that provide sign-up and sign-in options for application users. This include social sign-in for Facebook, Google, Amazon, Apple, and SAML and OIDC identity providers. It includes security functionality such as MFA, checks for compromised credentials, and phone and email verification. It also includes customized workflows and user migration through AWS Lambda triggers.

Identity pools allow the administrator to grant users access to other AWS services. Identity and user pools can be used either separately or together.  These support anonymous guest users for temporary credentials.

Cognito is a highly available service and is free for up to 50,000 monthly active users.


1. Log into Cognito directly.
2. Exchange for AWS credentials.
3. Access resources

Use cases: Data synchronization, identity merging, managed login UI.

* Manages cognito or social identities (e.g. FB, Google). Uses Lambda triggers to return Cognito JSON Web Token (JWT).
* Identity pools are configured to swap user pool tokens for temporary AWS credentials generated via Role assumption.
* AWS Resources accessed using identity pool generated temporary AWS credentials, nothing long-term is stored.

### AWS Directory Service

>[AWS Directory Service](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/what_is.html) provides multiple ways to use Microsoft Active Directory (AD) with other AWS services. Directories store information about users, groups, and devices, and administrators use them to manage access to information and resources. AWS Directory Service provides multiple directory choices for customers who want to use existing Microsoft AD or Lightweight Directory Access Protocol (LDAP)–aware applications in the cloud. It also offers those same choices to developers who need a directory to manage users, groups, devices, and access.
>

AWS Directory Service is a managed service offering that provides directories to contain information about an organization. It can be used to set up and run new directories in AWS or connect with existing on-premises Microsoft AD. It supports group policies and SSO as well as schema extension with MS AD Aware apps. There are two tiers: standard for up to 30,000 objects and enterprise for up to 500,000. It is used for AD authentication and authorization of products and services in AWS.

Directory service is highly available by default and offers monitoring, recovery, replication, snapshot, and maintenace functionality. It also supports one-way, two-way external, and forest trusts. The directory can operate even during a network link failure to any connected on-premises systems. Microsft Active Directory users cna groups can be assigned IAM permissions using AD Connector.

This is the best choice solution for more than 5,000 users in a hybrid infrastructure.

### LDAP

Lightweight Directory Access Protocol (LDAP) is a standard communications protocol used to read and write data to and from Active Directory. You can manage your user identities in an external system outside of AWS and grant users who sign in from those systems access to perform AWS tasks and access your AWS resources. The distinction is where the external system resides—in your data center or an external third party on the web.

Authenticating against LDAP happens prior to IAM or STS interactions.


### AD Connector

>[AD Connector](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_ad_connector.html) is a directory gateway with which you can redirect directory requests to your on-premises Microsoft Active Directory without caching any information in the cloud. AD Connector comes in two sizes, small and large. You can spread application loads across multiple AD Connectors to scale to your performance needs. There are no enforced user or connection limits.
>

Active Directory connector is a gateway that can bridge on-premises and AWS Directory Service. It consists of a pair of directory ENI endpoints running in a VPC. It redirects requests to existing directory services so that no directory data is stored on AWS. Unlike the other options, **it can connect pre-existing on-premises directories to AWS**. It can come in small or large sizes with a default quota of 10 AD Connector directories where multiple connectors can be used to spread load. It requires two subnets in different AZs to maintain HA. Network connectivity can be configured through Direct Connect or a VPN.

This is the best option if no directory information is permitted to be stored in AWS, but it has risks for larger deployments as network connectivity failure can impact functionality.


### AWS Resource Access Manager

> [AWS Resource Access Manager](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html) (AWS RAM) lets you share your resources with any AWS account or through AWS Organizations. If you have multiple AWS accounts, you can create resources centrally and use AWS RAM to share those resources with other accounts.
> 

* RAM shares with Principals (Accounts, OU's, ORG). Shared resources can be accessed natively.
* No charge for RAM, the user is only billed for the services used.
* Because AWS rotate physical facilities for availability zones, coordinating resources between accounts should depend on **Availability Zone IDs**. These are consistent across accounts, unlike AZ names
* Within an organization, sharing is accepted automatically. Outside an organization, the recipient will receive (and must accept) an invite
* RAM is commonly used for shared VPC services:
   * Only the owner can modify or delete network objects. Shared participants have read-only access
   * VPC owners cannot delete or modify resources created by participant VPCs
   * Some resources can be shared with any account, some only with AWS ORG accounts
        

### AWS Organizations

[Fill in later]

## Detection

### AWS Security Hub

> [AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) provides you with a comprehensive view of your security state in AWS and helps you check your environment against security industry standards and best practices.
> 

Security Hub consumes, aggregates, organizes, and prioritizes findings from AWS services that have been enabled including GuardDuty, Inspector, Macie as well as several third party integrations.

The user can create insights in Security Hub. These are a collection of findings that can be grouped when filtering. Security standards can also be used to display a list of findings from a selected control.

### Amazon GuardDuty

> [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html) is a continuous security monitoring service that analyzes and processes the following data sources: VPC Flow Logs, AWS CloudTrail management event logs, Cloudtrail S3 data event logs, and DNS logs. It uses threat intelligence feeds, such as lists of malicious IP addresses and domains, and machine learning to identify unexpected and potentially unauthorized and malicious activity within your AWS environment. This can include issues like escalations of privileges, uses of exposed credentials, or communication with malicious IP addresses, URLs, or domains. For example, GuardDuty can detect compromised EC2 instances serving malware or mining bitcoin. It also monitors AWS account access behavior for signs of compromise, such as unauthorized infrastructure deployments, like instances deployed in a Region that has never been used, or unusual API calls, like a password policy change to reduce password strength.

AWS Guard duty is an automatic threat detection service which reviews data from supported service and attempts to identify unusual events for AWS accounts. Includes continuous monitoring, analyzes supported data source plus AI/ML and threat intelligence feeds. It can identify unexpected and unauthorized activity. It also can notify or provide event-driven protection/remediation. Lastly, it supports multiple accounts in a master/member distinction.

### Amazon Inspector

> [Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_introduction.html) tests the network accessibility of your Amazon EC2 instances and the security state of your applications that run on those instances. Amazon Inspector assesses applications for exposure, vulnerabilities, and deviations from best practices. After performing an assessment, Amazon Inspector produces a detailed list of security findings that is organized by level of severity.

AWS Inspector is an automated security assessment that improves security and compliance of applications deployed on AWS. It scans EC2 instances and their respective OS. It identifies vulnerabilities and deviations from best practice. Intervals can be 15 minutes, 1 hour, 8 hours, 12 hours, or 1 day. It provides a report of findings sorted by priority.

By default it is a network assessment (agentless), but an agent can be added for richer information about the host. Rules packages determine what is checked.

Network reach (no agent) can provide additional OS visibility.
Reach end-to-end:
* EC2
* ALB
* DX
* ELB
* ENI
* IGW
* ACLs
* RTs
* SGs
* Subnets
* VPCs
* VGWs
* VPC peering
* `RecognizedPortwithListener`
* `RecognizedPortNoListener`
* `RecognizedPortNoAgent`
* `UnrecognizedPortWithListener`

Packages (agent required): 
* Common vulnerabilities and exposures (CVE)
* CVE IDs
* Center for Internet security (CIS) benchmarks
* Security best practices


### AWS Config

### AWS CloudTrail

### AWS IoT Device Defender

## Infrastructure Protection

### AWS Network Firewall

### AWS Shield

### AWS Web Application Firewall (WAF)

### AWS Firewall Manager

## Data Protection

### Amazon Macie

### AWS Key Management Service (KMS)

### AWS CloudHSM

### AWS Certificate Manager

### AWS Secrets Manager

## Incidence Response

### Amazon Detective

### CloudEndure Disaster Recovery

## Compliance

### AWS Artifact

### AWS Audit Manager
