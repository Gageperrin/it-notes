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

### AWS Organizations

## Detection

### AWS Security Hub

### Amazon GuardDuty

### Amazon Inspector

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
