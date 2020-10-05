# Advanced Security and Config Management

## Table of Contents

* [AWS Guard Duty](#aws-guard-duty)
* [AWS Config](#aws-config)
* [AWS Inspector](#aws-inspector)
* [Key Management Service](#key-management-service)
* [CloudHSM](#cloudhsm)
* [AWS Certificate Manager ACM](#aws-certificate-manager-acm)
* [AWS Parameter Store](#aws-parameter-store)
* [AWS Secrets Manager](#aws-secrets-manager)
* [VPC Flow Logs vs Packet Sniffing](#vpc-flow-logs-vs-packet-sniffing)
* [AWS WAF](#aws-waf)
* [AWS Shield](#aws-shield)

## AWS Guard Duty

> [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html) is a continuous security monitoring service that analyzes and processes the following Data sources: VPC Flow Logs, AWS CloudTrail management event logs, Cloudtrail S3 data event logs, and DNS logs. It uses threat intelligence feeds, such as lists of malicious IP addresses and domains, and machine learning to identify unexpected and potentially unauthorized and malicious activity within your AWS environment. This can include issues like escalations of privileges, uses of exposed credentials, or communication with malicious IP addresses, URLs, or domains. For example, GuardDuty can detect compromised EC2 instances serving malware or mining bitcoin. It also monitors AWS account access behavior for signs of compromise, such as unauthorized infrastructure deployments, like instances deployed in a Region that has never been used, or unusual API calls, like a password policy change to reduce password strength.

AWS Guard duty is an automatic threat detection service which reviews data from supported service and attempts to identify unusual events for AWS accounts. Includes continuous monitoring, analyzes supported data source plus AI/ML and threat intelligence feeds. It can identify unexpected and unauthorized activity. It also can notify or provide event-driven protection/remediation. Lastly, it supports multiple accounts (master and member).


## AWS Config

> [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html) provides a detailed view of the configuration of AWS resources in your AWS account. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time.

AWS Config is a service which records the historical configuration of resources. It can audit chances and check for compliance with standards. It does not prevent changes from happening, it only identifies if they are non-compliant. It is a regional service which supports cross-region and account aggregation. Changes can generate SNS notifications and near real-time events via EventBridge and Lambda.

Every time a change occurs to a resource, a configuration item (CI) is created for that resource. A CI represents the configuration of a resource at a point in time and its relationships. All CI's for a resource is a configuration history and is stored in S3. Resources are evaluated against Config Rules (AWS Managed or custom through Lambda). It can integrate with EventBridge to trigger Lambda remediation.


## AWS Inspector

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


## Key Management Service

Key Management Service is a regional and public service where a user can create, store and manage both symmetric and asymmetric keys through cryptographic operations. Keys never leave KMS. It provides FIPS 140-2 (L2).

Customer Master Keys (CMKs) are logical with ID, date, policy, description and state. They are backed by physical key material. They are generated or imported. CMKs can be used for up to 4 KB of data.

Data Encryption Keys (DEKs) `GenerateDataKey` works on > 4 KB and creates either a plaintext version or ciphertext version. It encrypts data using plaintext dey, discards it, then stores the encrypted key with the data.

CMKs are isolated to a region and never leave. CMKs can be AWS managed or customer managed. Customer managed keys, as usual, offer more customization. CMKs support rotation (AWS Managed rotates once every 3 years, cannot be changed while customer managed once every year). It also has a backing key and aliases. Every CMK has key policies (resources) that is needed to enable trust for an account that uses it.


## CloudHSM

> [AWS CloudHSM](https://docs.aws.amazon.com/cloudhsm/latest/userguide/introduction.html) provides hardware security modules in the AWS Cloud. A hardware security module (HSM) is a computing device that processes cryptographic operations and provides secure storage for cryptographic keys.

CloudHSM (Hardware secruity module) is a computing device with KMS, AWS Managed, and shared but separated options for additional security configuration. It is a true "single tenant" HSM which is AWS provisioned but fully customer managed. It is fully FIPS 140-2 Level 3 (by contrast KMS is L2 with some L3 features).

Industry standard APIs:
* PKCS#11
* Java Cryptography Extensions (JCE)
* Microsoft CryptoNG (CNG) libraries

KMS can use CloudHSM as a custom key store, CloudHSM integration with KMS. HSMs operate in an AWS managed HSM VPC. Interfaces are added to customer VPC. HSMs keep keys and policies in sync when nodes are added or removed (ENIs). AWS has no access to secure area where key material is held.

No native AWS integratoin (e.g. no S3 SSE). Use it to offload the SSL/TLS processing for web servers. Enable transparent data encryption (TDE) for Oracle Databases. Protect the private keys for an issuing certificate authority (CA).


## AWS Certificate Manager ACM

> [AWS Certificate Manager](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) (ACM) handles the complexity of creating, storing, and renewing public and private SSL/TLS X.509 certificates and keys that protect your AWS websites and applications. You can provide certificates for your integrated AWS services either by issuing them directly with ACM or by importing third-party certificates into the ACM management system. ACM certificates can secure singular domain names, multiple specific domain names, wildcard domains, or combinations of these. ACM wildcard certificates can protect an unlimited number of subdomains. You can also export ACM certificates signed by ACM Private CA for use anywhere in your internal PKI.

Certificate provide identity via a signature from a trusted authority when they encrypt data in transit through HTTPS.

ACM lets you run a public or private certificate authority (CA). A private CA for applications need to trust your private CA. A public CA when browsers trust a list of providers which can trust other providers. ACM can generate or import certificates. Generated certificates can automatically renew. If imported, the user is responsible for renewal. Certificates can be deployed out to supported services (CloudFront and ALBs, not EC2). 

ACM is a regional service, so certificates cannot leave the region they are generated or imported in. To use a certificate with an ALB in a region, a certificate in ACM is needed in that region. Global services such as CloudFront operate as though within `us-east-1`.


## AWS Parameter Store

AWS Parameter Store is a SSM parameter store for strings, stringlists, or secure strings. It includes storage for configuration, secrets, license codes, database strings, full configs & passwords. It includes hiearchies and versioning as well as plaintext and ciphertext. There are public parameters for the latest AMIs per region.


## AWS Secrets Manager

[Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) enables you to replace hardcoded credentials in your code, including passwords, with an API call to Secrets Manager to retrieve the secret programmatically. This helps ensure the secret can't be compromised by someone examining your code, because the secret no longer exists in the code. Also, you can configure Secrets Manager to automatically rotate the secret for you according to a specified schedule. This enables you to replace long-term secrets with short-term ones, significantly reducing the risk of compromise.

AWS Secrets Manager shares functionality with Parameter Store, but it is designed for secrets (passwords, API keys). It is usable via the console, the CLI, API, or SDK's (integration). It supports **automatic rotation** through Lambda, and it directly integrates with other AWS products such as **RDS**.

Architecture:
1. Application uses the Secrets Manager SDK to retrieve database credentials. 
2. SDK uses IAM credential authorization.
3. Application uses secrets obtained from Secrets Manager to securely access the database.
4. Lambda is invoked periodically to rotate secrets, and products such as RDS are kept in sync.
5. Lambda permissions via IAM roles.


## VPC Flow Logs vs Packet Sniffing

VPC Flow Logs capture packet metadata, not content. Applied at the VPC level, the subnet level, or directly on the interface. VPC Flow Logs are not real-time. The destination can be S3 or CloudWatch Logs. It uses packet sniffing for packet content.

Flow Log Syntax: `[Source ID][Destination ID][Source IP][Destination IP][Source Port number][Destination Port number][Protocol Number: ICMP = 1, TCP = 6, UDP = 17][ACCEPT/REJECT]`

Information to and from 169.254.169.254, 169.254.169.123, DHCP, Amazon DNS sever & Amazon Windows license not recorded.


## AWS WAF

> [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/what-is-aws-waf.html) is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to an Amazon CloudFront distribution, an Amazon API Gateway REST API, an Application Load Balancer, or an AWS AppSync GraphQL API. AWS WAF also lets you control access to your content. Based on conditions that you specify, such as the IP addresses that requests originate from or the values of query strings, Amazon CloudFront, Amazon API Gateway, Application Load Balancer, or AWS AppSync responds to requests either with the requested content or with an HTTP 403 status code (Forbidden). You also can configure CloudFront to return a custom error page when a request is blocked.

AWS WAF is a layer 7 (HTTP/S) firewall that protects against SQL injections, cross-site scripting, geo blocks, rate awareness, web access control lists (WEBACL) integrated with ALB, API Gateway and CloudFront. Rules are added to a web ACL and evaluated when traffic arrives. WAF Rules are defined and included in a WEBACL which is associated to a CloudFront distribution and deployed to the edge. The WEBACL filters traffic at the edge.


## AWS Shield

> For additional protection against DDoS attacks, AWS also provides AWS Shield Standard and AWS Shield Advanced. AWS Shield Standard is automatically included at no extra cost beyond what you already pay for AWS WAF and your other AWS services. AWS Shield Advanced provides expanded DDoS attack protection for your Amazon EC2 instances, Elastic Load Balancing load balancers, CloudFront distributions, Route 53 hosted zones, and AWS Global Accelerator accelerators. AWS Shield Advanced incurs additional charges.

Shield standard is free with Route 53 and CloudFront which offers protection against layer 3 and layer 4 DDoS. Shield advanced costs $3,000 per month and covers EC2, ELB, CloudFront, Global Accelerator and Route 53. Advanced also provides access to a DDoS response team and financial insurance in event of an attack.
