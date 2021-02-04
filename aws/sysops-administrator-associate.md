# AWS Certified SysOps Administrator Associate Notes

These are my notes for the AWS SOA-C01 exam. I have taken the content from [Adrian Cantrill's course](https://learn.cantrill.io/p/aws-certified-sysops-administrator-associate) for this exam, Tutorial Dojo's practice tests, and the official AWS documentation. 

These notes assume full and prior knowledge of requisite material for the Solutions Architect Associate and Professional exams and will not mention or cover any topics that are featured in that exam. Consequently, these notes will only cover a few SysOps specific items.

## AWS Service Catalogue

A service catalogue in general is a document or database created by an IT team to describe an organized colleciton of products offered by the IT team. It provides key product information including product owner, cost requirements, and dependencies. It defines approval of provisioning from IT and customer side.

AWS Service Catalogue is a self-service portal for end users. It can control end user permissions defined by admins. It is a regional service. Admins define products and portfolios using CloudFormation templates and Service Catalogue configuration before deploying the portfolio to any service enabled regions. Users can review products and launch them into service-enabled regions depending on access permissions.

## AWS Config

AWS Config has two jobs: (1) record configuration changes over time on resources and (2) checking for compliance of resources to certain standard. AWS Config cannot prevent or create any chagnes on resources. AWS Config is regional but does support cross-region and cross-account configuration.

## AWS X-Ray

AWS X-Ray is a distributed tracing service that uses a tracing header with a unique trace ID that is used to track a request through your distributed application. It includes segments (data blocks), subsegements, and a JSON serbvice graph that details the services and resources that make up the application. The service map displays all this information visually.

Compatibility with other services:
* EC2 - X-Ray agent
* ECS - Agent in tasks
* Lambda - enable the option
* Beanstalk - agent is preinstalled
* API Gateway - per stage option

## AWS Systems Manager

AWS Systems Manager is a product that allows the user to view and control AWS and on-premise infrastructure. It is agent-based so the SSM agent needs to be installed (can be installed on both Windows and Linux AMI's). It manages inventory and patch assets. It can run commands, manage desired state, store parameters, and also hosts Sessions Manager which can be used to securely connect to EC2s.

AWS instances require the agent, connectivity ot the AWS public zone endpoint and a requisite IAM role.

Run Command allows you to run command documents on managed instances without SSH/RDP access. These can be run based on instances, tags, or resource groups. Command documents can be reused and can have parameters. Command documents can be re-used and can have parameters. It also features rate control of concurrency and error threshold.

Concurrency defines how many instances run at once. Error threshold defines how many fails before the job fails.

The Patch Manager contains a patch baseline for which patch should be installed, which groups the patches should be applied to, as well as maintenance windows to determine the time patching can be done. Run Command often executes the patching. There are also settings for concurrency and error threshold. Patch Manager can also determine compliance after the fact.
