# AWS Certified Security Specialty

These are notes compiled from Chad Smith's AWS Certified Security Specialty Complete Video Course.

"It takes twenty years to build a reputation and a few minutes of cyber incident to ruin it." - Stephane Nappo

## Incident Response

### Abuse Notice Strategies

GuardDuty can be used to evaluate the suspected compromised instance. VPC Flow Logs can view incoming and outgoing traffic, security groups can isolate resources from the network, and a replacement can be launched from an AMI.

If an access key has been exposed, Access Advisor reports, GuardDuty, and CloudTrail logs can be used to view activity. The key itself can be disabled and replaced.

### Incident Response Basics

The [Computer Security Incident Handling Guide](https://www.nist.gov/publications/computer-security-incident-handling-guide?pub_id=911736) provides a comprehensive strategy for incident response.

### IR Preparation

The first phase of the Incident Response lifecycle is preparation and anticipation.

Resources for best practices include:
* AWS Organizations
* Networks deployed in VPCs
* AWS Config
* AWS CloudFormation
* AWS SSM

Procedures and run books should be stored both in cloud and physically.

It is also important to identify a normal behavior baseline for one's infrastructure. This includes CloudWatch and GuardDuty.

Clean images for recovery are also important. These can be provisioned through EC2 AMI, EBS snapshots, backups and configuration files in S3, as well as licenses and keys in SSM.

Risk assessment can be provided by Amazon Inspector, GuardDuty, and Macie.

Network security can be improved through VPC NACLs, VPC SGs, VPC Flow Logs, and AWS WAF.

Event incident log storage can be done through CloudWatch Logs, AWS Config streams, CloudWatch Events, CloudTrail Logs, and Access Logs stored in S3

### IR Detection and Analysis

Recognizing signs of an intrusion attempt:
* CloudWatch
* CloudTrail
* GuardDuty
* VPC Flow Logs

Performance baseline:
* CW Dashboards
* SSM Insights Dashboard
* GuardDuty Dashboard

Incident analysis:
* CloudWatch
* S3 Lifecycle policies
* Glacier Vault lock policy
* ElasticSearch and Kibana
* CloudWatch Logs Insights

Notifications:
* SNS
* SES
* Trusted Advisor

### IR Containment Eradication and Recovery

Containment strategy:
* Security Group rules
* Revoke IAM sessions
* WAF ACL rules
* IAM policies
* Access Key rotation
* KMS CMK rotation

Evidence Gathering and Handling:
* CloudTrail
* CloudWatch Logs
* VPC Flow Logs
* IAM Access Advisor

Identify Source of Attack:
* DNS lookup
* GuardDuty


### IR Post-Incident Activity

## Security Monitoring

## Logging Solutions

## Infrastructure Security

## Permissions and Roles

## Federation and Resource-based Access Control

## Key Management

## Data Encryption at Rest and in Transit

## Next Steps
