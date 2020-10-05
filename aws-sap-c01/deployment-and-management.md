# Deployment and Management

## Table of Contents
* [CloudFormation](#cloudformation)
* [AWS Service Catalog](#aws-service-catalog)
* [CI/CD using AWS Code](#ci/cd-using-aws-code)
* [Elastic Beanstalk](#elastic-beanstalk)
* [AWS OpsWorks](#aws-opsworks)
* [AWS Systems Manager](#aws-systems-manager)

## [CloudFormation]

> [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) is a service that helps you model and set up your Amazon Web Services resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS DB instances), and AWS CloudFormation takes care of provisioning and configuring those resources for you. 

### CloudFormation Custom Resources

CloudFormation uses logical resources to create, update, and delete physical resources. It does not support everything though. This is where custom resources come into play. Custom resources let CloudFormation integrate with anything it does not natively support.

### CloudFormation Nested Stacks

Nested stacks allow for a hierarchy of related templates to be combined into a single product. Resource in a single stack share a lifecycle. Stack resource limit of 200 resources. It is not easy to re-use or reference resources across stacks, but nested stacks can circumvent this with a root stack and nested stacks.

The root stack is created first (parameters and outputs). One can only reference outputs with nested stacks, not resources. Child stacks depend on parent stacks and receive parameters, and they can return outputs to parent stacks. Whole templates can be re-used in other stacks. Because it is **lifecycle linked**, use nested stacks when stacks form part of one single solution.

### CloudFormation Cross-Stack References

Cross stack references allow outputs of one stack to be exported into any other stack in the same region (e.g. shared VPC for multiple application stacks). Outputs are normally not visible from other stacks. Nested stacks can reference outputs. Outputs can be exported, making them visible to other stacks. Exports must have a unique name in the region. `Fn::ImportValue` can be used instead of a reference.

Use for service-oriented architectures for different lifecycles, and for **stack re-use**.

### CloudFormation StackSets

StackSets allow infrastructure to be deployed across multiple regions and accoutns also allowing a dynamic architecture for automatic operations based on accounts being added or removed from the scope of a StackSet. StackSets are containers in an admin account that contain stack instances which reference stacks. Stack instances and stacks are created in target accounts. Each stack runs in one region and one account. Security is self managed or service-managed.

Permissions granted via self-managed IAM Roles or service-managed within an organization. StackSets gain access to target accounts and create stack instances and stacks.

Terms:
* Concurrent accounts
* Failure tolerance
* Retain stacks

Scenarios:
* Enabled AWS Config -> AWS Config Rules: MFA, EIPS, EBS Encryption
* Create IAM Roles for cross-account access.


## AWS Service Catalog

> [AWS Service Catalog](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html) enables organizations to create and manage catalogs of IT services that are approved for use on AWS. These IT services can include everything from virtual machine images, servers, software, and databases to complete multi-tier application architectures. AWS Service Catalog allows organizations to centrally manage commonly deployed IT services, and helps organizations achieve consistent governance and meet compliance requirements. End users can quickly deploy only the approved IT services they need, following the constraints set by your organization.

AWS Service Catalog is a document or database created by an IT team, an organized collection of products offered by the IT team. It provides an end-user portal where products and portfolios can be deployed in a self-service way.

Standard Service Catalog:
* Key product information
* Product owner
* Cost
* Requirements
* Support information
* Dependencies

The service catalog defines approval of provisioning from IT and customer side. It manages costs and scale service delivery. It launch pre-defined products, end user permissions can be controlled, admins can define those products and the permissions required to launch them. It builds products into portfolios.

Architecture: Admins define products and portfolios using CF Templates and Service Catalog configuration, then deploy portfolio to any service enabled regions. Service Catalog users review portfolios they have permissions on and launch products into service enabled regions. Service Catalog launches the infrastructure using defined templates. Service catalog users only need launch permissions, not infrastructure permissions.

Key words:
* end-users
* customers deploying infrastructure in self-service way


## CI/CD using AWS Code

Typical code development pipeline is:
1. Code
2. Build
3. Test
4. Deploy

Traditional:
* Code -> GitHub/Bitbucket
* Build -> Jenkins
* Test -> Jenkins
* Deploy -> Jenkins or other tooling

AWS CodePipeline:
* Code -> CodeCommit
* Build -> CodeBuild
* Test -> CodeBuild
* Deploy -> CodeDeploy

A pipeline is tied to only one branch in a repository. Buildspec.yml, appspec.[ymljson] (Former for built, latter for deployment). CodeDeploy can deploy onto EC2 into AWS Elastic Beanstalk and AWS OpsWorks, CloudFormation, ECS or ECS (blue/green), Service Catalog, Alexa Skills Kit, or S3.


## Elastic Beanstalk

> With [Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html), you can quickly deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications. Elastic Beanstalk reduces management complexity without restricting choice or control. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring.

Elastic Beanstlak is a platform as a service vendor that handles infrastructure, while the user handles the code. It is developer focused and provides managed applicatoin environmnets. User provides code and Elastic Beanstalk handles the environment. Fully customizable and uses AWS products under the hood. Requires application changes so it is not free.

Built-in languages:
* Go
* Java SE
* Tomcat
* .NET Core (Linux)
* .NET (Windows)
* Node.js
* PHP
* Python
* Ruby

Docker: Single container docker option, multi-container docker, or pre-configured docker (e.g. supports Glassfish Java 8). Custom platforms via packer.

EB application is not code but a collection of things relating to an application. Application versions are a specific labeled version of deployable code for an application. The source bundle is stored on S3. Environments are containers of infrastructure and configuration for a specific application version. Each environment is either a web server tier or a worker tier--it controls the structure and function of the environment. App versions can be deployed to environmnets. Web and worker tiers communciate via SQS queues. Each environment has its own CNAME which can be swapped to exchange two environment DNS.

Blue/green deployment is where a green environment is created for testing. When it is ready, the CNAME is swapped from blue to green. The blue environment is only deleted when green is fully functional.

Elastic Beanstalk is not free but is great for small development teams. Use docker for anything unsupported. Databases should be outside Elastic Beanstalk. Databases in an environment are lost if the environment is deleted.


## AWS OpsWorks

> [AWS OpsWorks](https://docs.aws.amazon.com/opsworks/latest/userguide/welcome.html) is a configuration management service that helps you configure and operate applications in a cloud enterprise by using Puppet or Chef. AWS OpsWorks Stacks and AWS OpsWorks for Chef Automate let you use Chef cookbooks and solutions for configuration management, while OpsWorks for Puppet Enterprise lets you configure a Puppet Enterprise master server in AWS. Puppet offers a set of tools for enforcing the desired state of your infrastructure, and automating on-demand tasks.

OpsWorks provides managed implementations of Puppet and Chef in a product which integrates with other AWS products and services.

Most control/high admin over head <------------------------> Least control/low admin overhead
Manual implementation --- CF as Code --- OpsWorks Recipes and Cookbooks --- Elastic Beanstalk

Three modes:
1. Puppet enterprise: AWS Managed Puppet Master Server
2. Chef automate: AWS managed chef servers
3. OpsWorks: AWS Integrated chef and no servers

Use OpsWorks when you already have Puppet or Chef or a requirement to operate.

Key terms: Recipes, Cookbook or Manifests
* Stack: Core component, container of resources.
* Layer: Each layer is a specific function within a stack.
* Recipes and Cookbooks: Applied to layers.

Life cycle events:
* Setup
* Configure
* Deploy
* Undeploy
* Shutdown

Instances:
* 27/7
* Time-based
* Load-based (EC2 or on-premises)

* For apps, OpsWorks triggers a deploy event which runs the deploy recipes on applicable instances.

Architecture: 
* Full stack permissions via IAM
* SSH key management via OpsWorks
* Stack metrics into CloudWatch
* RDS instance created outside of OpsWorks and added to a database layer
* Apps added to layers via a deploy recipe. Apps can be downloaded from S3 or via http


## AWS Systems Manager

> [AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) is an AWS service that you can use to view and control your infrastructure on AWS. Using the Systems Manager console, you can view operational data from multiple AWS services and automate operational tasks across your AWS resources. Systems Manager helps you maintain security and compliance by scanning your managed instances and reporting on (or taking corrective action on) any policy violations it detects.

AWS Systems Manager (formerly Simple Systems Manager) allows the user to view and control AWS and on-premise infrastructure. It is agent based and installed on Windows and Linux AMIs. Systems Manager can manage inventroy, patch assets, run commands, and manage desired state, parameter store (configuration and secrets), and securely connect to EC2 even in private VPCs.

Architecture:
* Agent pre-installed or a manual installation.
* Needs EC2 instance role or IGW or VPCE ot connect ot a Systems Manager Endpoint in AWS Public Zone.
* Managed instance activation needs an IAM role with an activation code and ID that can be used to set up a secure communication channel with on-premises servers over the public Internet.
* Activations securely join on-premises server to Systems Manager and configure the IAM Role to use.

### Run Command

The Run command documents on managed instances. No SSH/RDP access required. The command docs can be run on instances, tags, our resource group. Additionally, they can be reused and have their own parameters. They can have a rate control with a concurrency or error threshold. Output options can be S3, SNS, or EventBridge (CloudWatch Events) Target.

Architecture:
1. Run from SSM, EventBridge, ConsoleUI or CLI. Sends command documents with their targets and parameters to the Systems Manager endpoint.
2. The command document is sent to the instances.
3. Concurrency defines how many instances run at once, and the error threshold defines how many can fail before the job fails as a whole.

### Patch Manager

Patch manager allows for the patching of Windows or Linux managed instances running in AWS or on-premises. The patch baselines defines what should be installed. The patch groups are groups of resources that will be patched. Maintenance windows are the times when patching takes place. The run command is the base-level functionality.

Pre-defined patch baselines:
* Linux: `AWS-[OS]DefaultPatchBaseline`, explicitly define patches. OS: `AmazonLinux2` or `UbuntuDefault`
* Windows: `AWS-DefaultPatchBaseline`, `AWS-WindowsPredefinedPatchBaseline-OS` (same as bove but adds critical and security updates), and `AWS-WindowsPredefinedPatchBaseline-OS-Applications` (same as the previous but adds Microsoft app updates).

Architecture:
1. Define one or more patch baselines to define what gets installed.
2. Create patch groups which act as targets for patch tasks.
3. Create a maintenance window (schedule, duration, targets, tasks).
4. `AWS-RunPatchBaseline` **runs* with a baseline and targets, then lastly a compliance component through Systems Manager inventory.

