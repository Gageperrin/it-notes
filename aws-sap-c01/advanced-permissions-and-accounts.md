# Advanced Permissions and Accounts

## Security Token Service

> [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html) (AWS STS) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for users that you authenticate (federated users)

STS generates temporary credentials through `sts:AssumeRole`. These credentials do not belong to the identity and expire. An external or AWS identity requests the token, and STS uses the Permissions Policy to generate temporary credentials (`AccessKeyId`, `Expiration`, `SecretAccessKey`, `SessionToken`). Credentials are returned to the identity requesting them. A trust policy determines who can assume a role.

## Revoking IAM Role Temporary Security Credentials

### [How to revoke temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_revoke-sessions.html)

Roles have many identities, same permissions, and credentials cannot be canceled.

Not solutions: 
* Changing the trust policy does not impact existing credentials. 
* Changing the permissions policy impacts all credentials.

Solution: 
1. Update permissions policy with a `AWSRevokeOlderSessions` with an in-line DENY for any sessions older than NOW.
2. Revoke the sessions with a conditional DENY ALL policy so bad actor cannot directly perform `sts:AssumeRole`

## Policy Interpretation Deep Dive

### How to exegete IAM policies:
1. Identify number of statements
2. Identify what each statement does
3. Identify negations

* A policy with only DENY effect will have no effect unless paired with an overlapping ALLOW. DENY is the default.
* Best practice to start with ALLOW statements at the top.

## Permissions Boundaries and Policy Evaluation

Boundaries can be applied to IAM Users or IAM Roles. Permissions boundaries define maximums an identity can receive.

Policy evaluation sequence:
1. Explicit Deny
2. [Service Control Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_scp) (SCP)
3. [Resource Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_resource-based)
4. [Permissions Boundaries](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_bound)
5. [Session Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_session)
6. [Identity Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_id-based)

[Access Control Lists](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_acl)

* Multi-account access requires Allows from both requester and receiver

## AWS Resource Access Manager

> [AWS Resource Access Manager](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html) (AWS RAM) lets you share your resources with any AWS account or through AWS Organizations. If you have multiple AWS accounts, you can create resources centrally and use AWS RAM to share those resources with other accounts.

* RAM shares with Principals (Accounts, OU's, ORG). Shared resources can be accessed natively.
* No charge for RAM just the services
* Because AWS rotate physical facilities for availability zones, coordinating resources between accounts should depend on **Availability Zone IDs**. These are consistent across accounts, unlike AZ names
* Within an organization, sharing is accepted automatically. Outside an organization, recipient will receive (and must accept) an invite
* RAM is commonly used for shared VPC services:
        * Only owner can modify or delete network objects. Shared participants have “read-only” access
        * VPC owners cannot delete or modify resources created by participant VPCs
        * Some resources can be shared with any account, some only with AWS ORG accounts
        
## AWS Service Quotas

> [Service Quotas](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) enables you to view and manage your quotas for AWS services from a central location. Quotas, also referred to as limits in AWS, are the maximum values for the resources, actions, and items in your AWS account. Each AWS service defines its quotas and establishes default values for those quotas. Depending on your business needs, you might need to increase your service quota values. Service Quotas makes it easy to look up your service quotas and to request increases.

* Each service has a default per-region quota, but some services can be per account.
* Most services can be incrased as needed, but others such as IAM user count cannot
* Quota request templates can be used to reduce admin overhead.
* Can create a CloudWatch alarm based on applied service quota value.
* Can request quota increase through [command line](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/service-quotas/request-service-quota-increase.html) as well.
