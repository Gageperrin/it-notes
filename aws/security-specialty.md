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

Evidence gathering and handling:
* CloudTrail
* CloudWatch Logs
* VPC Flow Logs
* IAM Access Advisor

Identify source of attack:
* DNS lookup
* GuardDuty

Eradicate compromised resources:
* EC2 instance termination
* Disable compromised keys
* Segregate compromised data for analysis

Recovery steps:
* Know the RTO/RPO
* Resources to repair or replace
* What can be automated?

Clean up:
* Remove temporary resources
* KMS key audit
* Full IAM audit
* Review further findings

### IR Post-Incident Activity

Learning for next incident:
* Evidence retention in S3, AMI, or snapshots
* Proposals for improvement in terms of least privilege implementation, access control, and network permissions
* Monitoring through better dashboards and active responses

## Security Monitoring

### Infrastructure Security Monitoring

The best solutions for security monitoring:
* VPC Flow Logs within a VPC or Subnet
* GuardDuty
* OS Logs
* Config Rules
* Amazon Inspector

### Application Security Monitoring

Application-level security: CloudWatch Logs receives logs from
* Lambda Execution Logs
* EC2 Application Logs
* ECS/EKS Container Logs

CloudTrail receives logs from:
* Cognito User Authentication Logs
* Step Functions Logs
* Deployments via CodeDeploy

S3 receives logs from:
* ALB Access Logs
* CloudFront Access Logs
* Redshift Audit Logs

### Account Security Monitoring

Account security logging can be done through:
* CloudWatch Events pulled from GuardDuty Findings, CloudTrail Events, and AWS Org Events
* Config Rules from CloudTrail, IAM User Key rotation, and root user MFA enabled.

### Troubleshooting Security Monitoring

## Logging Solutions





## Account Security Monitoring



## Infrastructure Security

## Permissions and Roles

## Federation and Resource-based Access Control

## Key Management

## Data Encryption at Rest and in Transit

## Next Steps
