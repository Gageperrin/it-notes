# Advanced Identities and Federation


## SAML 2.0 Identity Federation

[Security Assertion Markup Language](https://en.wikipedia.org/wiki/SAML_2.0) (SAML 2.0) is an open standard used by many IDP's such as MS ADFS to exchange authentication and authorization between different security domains.

SAML 2.0 allows on-premises identities to use AWS.

AWS use cases:
1. Enterprise Identity Provider (SAML 2.0 Compatible)
2. Existing identity management team
3. Single source of truth for more than 5,000 users

[SAML 2.0 Identity Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html) uses IAM Roles & AWS Temporary Credentials (12 hour validity).


## AWS Single Sign-On

> [AWS Single Sign-On](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) is a cloud-based single sign-on (SSO) service that makes it easy to centrally manage SSO access to all of your AWS accounts and cloud applications. Specifically, it helps you manage SSO access and user permissions across all your AWS accounts in AWS Organizations. AWS SSO also helps you manage access and permissions to commonly used third-party software as a service (SaaS) applications, AWS SSO-integrated applications as well as custom applications that support Security Assertion Markup Language (SAML) 2.0. AWS SSO includes a user portal where your end-users can find and access all their assigned AWS accounts, cloud applications, and custom applications in one place.

* AWS SSO has a built-in identity store.
* Can connect AWS SSO to both [AWS Managed Microsoft AD](https://docs.aws.amazon.com/singlesignon/latest/userguide/connectawsad.html) and [on-premises Microsoft AD](https://docs.aws.amazon.com/singlesignon/latest/userguide/connectonpremad.html) (two way trust needed for latter).
* AWS SSO is best practice over traditional workforce identity federation.
* Customer identities **do not use SSO**. They use Cognito. SSO is for **workplace identities**.


## AWS Cognito

> [Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) provides authentication, authorization, and user management for your web and mobile apps. Your users can sign in directly with a user name and password, or through a third party such as Facebook, Amazon, Google or Apple.

The two main components of Amazon Cognito are user pools and identity pools. User pools are user directories that provide sign-up and sign-in options for your app users. Identity pools enable you to grant your users access to other AWS services. You can use identity pools and user pools separately or together.

AWS Cognito provides authentication, authorization, and user management on web and mobile applications.

1. Log into Cognito directly.
2. Exchange for AWS credentials.
3. Access resources

Use cases: Data synchronization, identity merging, managed login UI.

* Manages cognito or social identities (e.g. FB, Google). Uses Lambda triggers to return Cognito JSON Web Token (JWT).
* Identity pools are configured to swap user pool tokens for temporary AWS credentials generated via Role assumption.
* AWS Resources accessed using identity pool generated temporary AWS credentials, nothing long-term is stored.


## Amazon Workspaces

> [Amazon WorkSpaces](https://docs.aws.amazon.com/workspaces/latest/adminguide/amazon-workspaces.html) enables you to provision virtual, cloud-based Microsoft Windows or Amazon Linux desktops for your users, known as WorkSpaces. Amazon WorkSpaces eliminates the need to procure and deploy hardware or install complex software. You can quickly add or remove users as your needs change. Users can access their virtual desktops from multiple devices or web browsers.

Amazon Workspaces is a Desktop-As-A-Service similar to Citrix or Remote Desktop-A. Consistent desktop from anywhere with Windows and Linux OS's of various sizes.

* Billing: Monthly or hourly pricing + base infrastructure cost.
* Uses Directory Service (Simple AD, AD Connector).
* Workspaces use an ENI injected into customer managed VPCs.
* Windows can access FSx and EC2 Windows resources.
* At-rest encryption (EBS + KMS).
* Workspaces and directory services (DS) run in AWS managed VPCs.
* Workspaces are not Highly Available by default.


## Directory Service (Microsoft AD)

>[AWS Directory Service](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/what_is.html) provides multiple ways to use Microsoft Active Directory (AD) with other AWS services. Directories store information about users, groups, and devices, and administrators use them to manage access to information and resources. AWS Directory Service provides multiple directory choices for customers who want to use existing Microsoft AD or Lightweight Directory Access Protocol (LDAP)–aware applications in the cloud. It also offers those same choices to developers who need a directory to manage users, groups, devices, and access.

* Uses Microsoft Active Directory 2012 R2.
* Managed using Standard Active Directory tools.
* Supports Group Policy and SSO.
* Supports Schema extension with MS AD Aware apps.
* Sharepoint, SQL, Distributed File System.
* Standard (30,000 objects) and Enterprise (500,000).
* Used for AD Authentication/Authorization of products and services within AWS.
* Highly Available by default in 2 AZs.
* Includes monitoring, recovery, replication, snapshots, and maintenance.
* Supports one-way, two-way external, and forest trusts.
* Directory in AWS can operate through a network link failure to any connected on-premises systems.
* Supports RADIUS-based MFA.
* TRUSTS & SCHEMA Extensions and Actual Microsoft AD.
* Best choice for more than 5,000 users if you need trust relationships between AWS and on-premise infrastructure.
* 2 Domain Controllers which are HA.
* Automatic Patching and maintenance.
* AWS Services which support authentication and authorization via Directory Service can be used. Delivers MS Active Directory natively as a service.
* Microsoft AD if RADIUS/MFA (possibly), TRUSTS/SCHEMAS, support SCHEMA extensions. Microsoft AD for larger user numbers. If no Directory info in AWS then AD Connector. Microsoft AD mode if it needs to work with failed network link.

### LDAP

Lightweight Directory Access Protocol (LDAP) is a standard communications protocol used to read and write data to and from Active Directory. You can manage your user identities in an external system outside of AWS and grant users who sign in from those systems access to perform AWS tasks and access your AWS resources. The distinction is where the external system resides—in your data center or an external third party on the web.

Authenticating against LDAP happens prior to IAM or STS interactions.


## AD Connector

>[AD Connector](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_ad_connector.html) is a directory gateway with which you can redirect directory requests to your on-premises Microsoft Active Directory without caching any information in the cloud. AD Connector comes in two sizes, small and large. You can spread application loads across multiple AD Connectors to scale to your performance needs. There are no enforced user or connection limits.

* A pair of directory endpoints running in AWS (ENIs in a VPC).
* Redirects requests to existing directory servers.
* No directory data stored in AWS, all redirected.
* Unlike the others, it can connect **pre-existing on-premises directory to AWS**.
* Proof of concepts, where you need existing identities.
* Two sizes small and large.
* Multiple AD Connectors can be used to spread load.
* Requires two subnets within a VPC, different AZs so HA.
* Connector requires one or more directory server to be configured.
* Requires a working network connection.
* Network connectivity via Direct Connect or VPN.
* If no Directory info is permitted in AWS this is a good option, as well as for use of on-premises identities.
* Risks for larger deployments, failure of network connectivity will impact functionality.
