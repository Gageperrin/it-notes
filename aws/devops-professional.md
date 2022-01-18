# AWS Certified DevOps Professional

These are my notes for the AWS DevOps (DOP-C01) exam. I have taken the content from [Stephane Maarek's course](www.udemy.com/course/aws-certified-devops-engineer-professional-hands-on) for this exam, Tutorial Dojo's practice tests, and the official AWS documentation. 

These notes assume full and prior knowledge of requisite material for the Solutions Architect Associate and Professional exams and will not mention or cover any topics that are featured in those exams. Consequently, only a few topics will be treated here.

# Domain 1 - SDLC Automation

## CI/CD Overview

Continuous Integration: Developers push the code to a code repository. A testing/build server checks the code as soon as it is pushed. The developer receives feedback about the tests and checks that have passed or failed.

Continuous Delivery: Deployments are rapid and frequent through automated procedures and often manual approval steps.

Continuous Deployment: Full automation, every code change is deployed all the way to production

## CodeCommit

Version control is the ability to understand various changes that happened to the code over time. A Git repository can live on a machine but usually lives in a central online repository.

CodeCommit is a private Git repostiory solution that is fully managed, highly available and offers increased security and compliance.

IAM users are provided with authorized to use CodeCommit either through SSH keys or HTTPS Git credentials.

Git branches provide isolation between different versions of code and are used for security and insularity of code development. The child branch is merged into the parent branch using the pull request feature.

Customizing the IAM policy can create security to prevent junior developers from making code change or approving pull requests, permissions that should be reserved for senior devs.

Notification rules can be used to create event-driven notifications that target SNS topics. SNS can be further integrated with AWS Chatbot to push notifications to Slack for example. Notification rules are listed and logged in CloudWatch.

Triggers are another form of event-driven functionality that can trigger SNS topics and/or Lambdas.

## CodeBuild

CodeBuild is a fully managed build service offered as an alternative to Jenkins that leverages Docker under the hood for continuous builds. It leverages continuous scaling, pay for usage, the possibility to extend capabilities leveraging our own base Docker images, and secures integration with KMS for encryption of build artifacts.

Build instructions can be defined in code `buildspec.yml` and outputs logs to S3 and CloudWatch Logs. CloudWatch Events can detect failed builds and trigger notifications while CloudWatch Alarms can notify if thresholds are needed for failures.

`buildspec.yml` can include environment variables, parameters, a Git credential helper, phases with runtime versions, commands, and a finally block for each phase. It also include configuration options for the generated artifact.

CodeBuild can log into ECR and build Docker images as well.

CodeBuild environments carry a lot of AWS-set environment variables by default including region and various webhooks. They can be identified with the `printenv` command.

The build output is an artifact, and CodeBuild by default stores artifacts in S3. Artifacts can be packaged as Zip files and also be encrypted for security purposes (AES-256 or AWS KMS).

CloudWatch can be used to monitor CodeBuild processes and can also set up event-driven triggers for CodeBuild.

CodeBuild can validate CodeCommit pull requests via Lambda functions.

## CodeDeploy

CodeDeploy is an AWS managed service to deploying code to EC2 machines or on-premise servers. The machine must be running the CodeDeploy agent, and the agent must be continuously polling CodeDeploy for work to do. CodeDeploy sends the `appspec.yml` file to the server. The CodeDeploy agent will report success or failure of the deployment.

EC2 instances are grouped by deployment group. CodeDeploy can reuse existing setup tools with any application.

In-place deployments can be launched one at a time, half at a time, or all at once. Blue/green deployments are the alternative.

`appspec.yml` specifies the artifact source, environment variables, and hooks for various deployment stages including before the installation, after the installation, when the application starts, when it stops, before traffic os allowed, etc.

CloudWatch Events, Alarms, Logs, and Triggers can be integrated with CodeDeploy to initiate deployment processes. A CodeDeploy state change can be used to target a Kinesis stream and trigger other AWS services.

Rollbacks can be set for either when a deployment fails or when alarm thresholds are met.

Only CodeDeploy can tag on-premise instances. IAM users and roles can be used to register on-premise instances. The former can be done through the `register-on-premises-instance` API command.

On-premise instances need a CodeDeploy configuration file. For Ubuntu this is `/etc/codedeploy-agent/conf/codedeploy.onpremises.yml`.

CodeDeploy can also deploy to Lambda through all-at-once, canary, or linear deployments. Lambda only has the before allow, traffic, and after allow traffic hooks.

## CodePipeline

CodePipeline offers continuous delivery of source, build, test, and deploy stages using AWS-native service or third-party services. Each pipeline stage can create its own artifact.

CodePipeline can be set up with manual approval steps, integrated with CloudWatch events to receive and send triggers with other processes, and include integrations for a wide variety of pipeline processes including ECR and ECS, S3 static websites, EC2 hosting, Lambda infrastructures, Device Farm application testing, and more.

Stage Actions are written in JSON with a "State Change' action specified. Parallel stages can be set up by specifying the same `runOrder` value. For example, a `runOrder` of 1 will be run first, and multiple actions with `runOrder` value 2 will be run concurrently after the first stage.

## CodeStar

CodeStar is a UI that manages and combines all of the above features with AWS's Cloud9 IDE environment to create a more streamlined experience.

## Jenkins

Jenkins is an open source CI/CD tool that can be used instead of CodeBuild, CodePipeline, and CodeDeploy or be integrated alongside any combination of these. Jenkins is deployed in a master/worker configuration via EC2 in AWS. All projects must have a Jenkinsfile like CodeBuild has a `buildspec.yml`. The master and workers can be hosted on the same instance or separated between multiple instances for higher availability. A Jenkins worker is also known as an agent.

Jenkins plugins for AWS include:
* AWS Lambda
* AWS EC2
* AWS Device Farm
* AWS CodeBuild
* AWS ECS
* AWS EC2 Container Service
* AWS SQS Build Trigger
* Artifact Manager on S3

# Domain 2 - Configuration Management and Infrastructure as Code

## Introduction to CloudFormation

Skipped.

## Intermediate CloudFormation

CloudFormation includes several helper scripts such as:

* `cfn-init` can be used to fetch and parse metadata, install packages, write files to disk, and start/stop services.
* `cfn-signal` is typically run after `cfn-init` to indicate if CloudFormation should continue or fail. It is based on a `WaitCondition` typically in conjunction with a `CreationPolicy` for a resource.

`WaitCondition` signalling can fail when the AMI does not have the CloudFormation helper scripts installed, the helper scripts were not successfully run, or the instance is not connected to the public Internet.

General note: Rollback on failure should be disabled for CFN troubleshooting.

Nested stacks are stacks within stacks. These are considered best practice to layer infrastructure and isolate repeated patterns and make them replicable. They consist of parent, child, and root stacks.

ChangeSets are files that can be used to update stacks with explicitly marked changes and additions fo the current stack.

Deletion policies can be used at a resource-level to specify what happens upon resource deletion. Options include retaining resources, resource snapshots, or a default delete. The last is helpful for RDS which has a default delete policy of snapshot.

Termination protections can also be set in place to prevent accidental termination of CFN stacks.

## DevOps-Level CloudFormation

CloudFormation can grab parameters or values from SSM Parameter Store or Secrets Manager. AWS also hosts public SSM parameters like latest Amazon Linux version ID. `DependsOn` can be used to explicitly specify the sequence of provisioning CFN resources in a template.

Lambdas can be deployed entirely in-line through CFN by deploying code from an S3 artifact.

CFN custom resources can be used to cover AWS resources not yet specified by official CFN templates, on-premises resources, emptying S3 buckets before deletion, or fetching an AMI ID.

CloudFormation automatically detects drift from manual changes in comparison to the original stack.

Additional helper scripts:
* `cfn-hup` is a daemon that detects changes in resource metadata and runs specified actions when a change is detected. This can make configuration updates on running EC2 instances through the API action `UpdateStack`. This is stored in `cfn-hup.conf`.
* `cfn-get-metadata` to fetch a metadata block from CloudFormation and print to std out. Subtrees of metadata can also be printed by specifying the key.

Stack policies can be used to specify permissions for creating, updating, or deleting stacks and/or particular stack resources via their logical resource ID.

## Elastic Beanstalk

Elastic Beanstalk is the best lift-and-shift option for hosting applications on AWS through EC2 under the hood. EBS automatically creates EC2 instances with autoscaling groups, CloudWatch alarms, and other configuration pieces as specified. It includes a health dashboard for monitoring an application's health.

EB's config is saved as a `cfg.yml` file in the `.elasticbeanstalk` directory and includes av ariety of option settings including batch size, load balancing, service role, etc. Configurations are saved and versioned. Configurations can be imported and exported across environments as well.

`.ebextensions` can update configurations and persist changes. Configuration files are run in alphabetical order. `.ebextensions` can also include CloudFormation resources in-line to provision resources out of EB. Their output values can then be passed along as environment properties (variables) to an EB deployment. `.ebextensions` can also include commands that are executed before the application and web server are set up and the application version file is extracted. This is distinct from `container_commands` which are run after the application and web server are set up and the version archive has been extracted but still before the application version is deployed.

Note that it is better to decouple databases from EB deployments because databases would be deleted alongside the environment.

Environments can be cloned, have their CNAME records swapped, have the environment rebuilt, managed schedule updates, or be rolled back to a previous version.

EB deployment strategies include:
* All-at-once: fastest, lowest cost, but has downtime
* Rolling: Update a few at a time in a bucket group then move onto the next bucket when complete. Bucket size can be specified.
* Rolling with additional batch: Like rolling but spins up new instances to move the batch to avoid performance reduction in production environment.
* Immutable: Spins up new instances in a new auto-scaling group then moves them to the original group when all instances are healthy. Fastest rollback but highest cost.

## Lambda

(Lambda basics are skipped here.)

Lambda can be triggered by a variety of sources including CloudWatch Events and DynamoDB.

Lambda versioning, aliases, and canary deployments can be used to create stable and secure development and testing ecosystems.

Amazon's Serverless Application Model is a form of CloudFormation templates that can be used to create serverless applications by deploying Lambdas in conjunction with API Gateway. SAM comes with built-in support for CodeDeploy.

Step Functions can be used to coordinate long, multi-step workflows whose duration would exceed the fifteen minute limit of an individual Lambda.

## API Gateway

API Gateway can create REST, HTTP, or Websocket APIs from scratch, cloning an existing API, or importing from Swagger. API Gateway can provide public and/or private endpoints (deployed in a VPC) as well as require authorizers through tokens, Cognito, or other means. API is most often integrated with Lambda but can be connected to other AWS services or a HTTP endpoint as well.

API Gateway can interact directly with Lambda aliases for testing Lambdas and do canary weighting, but it is also possible to point explicitly to a Lambda version. API Gateway can also transform request and response bodies.

API Gateway changes need to be deployed before the changes are live. Deployments are made to individual stages, and each stage can have its own configuration parameters, variables, metrics, and logs. This makes it easy to roll back to previous stages.

API Gateway has a limit of 10,000 requests per second per account, but throttling usage plans can be set up for individual stages of various APIs. Lambda can also throttle requests through its concurrency limits.

## ECS

Elastic Container Service is Amazon's containerization solution through deploying containers either onto EC2 or through a serverless Fargate solution. Fargate runs off the task definition without further infrastructure management.

ECS runs task definitions which include the Docker image to use with each container, the allocated resources, the launch type to use, the Docker networking role, the command the container should run, as well as any data volumes that should be used with the containers in the task. A task definition is run via a service which can be auto-restarted as needed. A load balancer can distribute traffic between services.

Images can be stored publicly or privately in ECR.

ECS tasks require an IAM execution role to deploy the task and retrieve the image. If running on EC2, a separate role is required for the EC2 instance. If running on Fargate, only the task role is required.

Autoscaling can be based on target metrics such as average CPU utilization with a specified maximum and minimum instances as well as scaling cooldown periods. Both EC2 and Fargate offer autoscaling at the service level.

CloudWatch Logs can receive individual container logs by default using `awslogs` as the log driver. EC2 instances can also send OS logs to CloudWatch.

Log groups include:
* `/var/log/dmesg`
* `/var/log/docker`
* `/var/log/ecs/ecs-agent.log`
* `/var/log/ecs/ecs-init.log`
* `/var/log/messages`


### Elastic Beanstalk + ECS

Elastic Beanstalk can be run in single and multi Docker container mode. Multi-Docker helps run multiple contianers per EC2 instance in EB. This creates an ECS cluster, EC2 instances for the ECS cluster, a load balancer, and executed task definitions. It requires a config file `Dockerrun.aws.json` at the root of source code. The Docker image must be pre-built and stored in ECR. In other words, the containers are deployed through EB on top of an ECS cluster.


### ECS + CI/CD Pipeline

CodeBuild can build Docker images from Docker files found in a specified repository and store them in ECR. ECS can then pull the latest or otherwise specified image from ECR and deploy it automatically. CodePipeline provides a variety of configuration options to automate this process.


## OpsWorks

OpsWorks is a configuration management service that consists of OpsWorks Stacks, OpsWorks for Chef Automate, and OpsWorks for Puppet Enterprise.

An OpsWorks Stack consists primarily of three layers:
1. Elastic Load Balancing Layer
2. Application Server Layer
3. Amazon RDS Layer

The first and third layers are AWS-manged while the customer supplies the application for the second layer.

Stacks can be deployed into particular regions, VPCs, subnets, with particular SSH keys, and alongisde custom Chef cookbooks in a specified repository (with a repository SSH key if needed). Advanced options include default root device type (EBS vs. Instance store), IAM role, host name theme, etc.

The layers section of the OpsWorks console UI shows the application blueprint for a set of EC2 instances to be deployed. The layer shows Chef recipe status, EBS volumes, networking, security, CloudWatch Logs, etc. The instance size can be specified alongside scaling conditions such as time or load. Manual deploys can also be triggered by the user via CLI or the OpsWorks UI. This includes specifying custom run commands for cookbooks.

### Lifecycle

Each layer has a set of five lifecycle events:
1. Setup
2. Configure
3. Deploy
4. Undeploy
5. Shutdown

Setup occurs after an instance has booted. Configure occurs when the instance enters or leaves the online state, an Elastic IP is associated or disassociated with the instance, or an ELB is attached to the layer. Deploy occurs alongside an application deployment on an instance. Undeploy occurs when an app is deleted or removed from a set of instances. Shutdown shuts down associated EC2 instances but before the EC2 instance is ultimately terminated.

OpsWorks by default has auto-healing enabled. An instance is considered unhealthy by default if the OpsWorks Stacks agent does not report healthy in five minutes. CloudWatch Events rules can be integrated with OpsWorks to specify healing and termination for specific errors or status codes.


# Domain 3: Monitoring and Logging

## CloudTrail

CloudTrail by default records all actions in an account. Records can be sent to an S3 bucket and can be based on read, write, or both kinds of management events. CloudTrail logs can be configured with a file prefix. Logs can also be encrypted using SSE-KMS. Log files can be validated and log file notifications can be integrated with SNS.

CloudTrail Log File Integrity is command `aws cloudtrail validate-logs` via the CLI to check if any logs have been modified using a start time and a trail ARN as filter parameters.

CloudTrail logs can be retrieved from multiple AWS accounts and multiple region into one S3 bucket in a single account by changing the target bucket policy to allow write access.


## Kinesis

Kinesis is an AWS-managed alternative to Apache Kafka. It is designed for application logs, metrics, IoT, clickstreams, real-time big data, and is great for streaming processing frameworks. Data is automatically replicated across three availability zones.

Kinesis Streams offers low latency streaming ingest at scale, Kinesis Analytics performs real-time SQL analysis on streams, and Kinesis Firehose loads streams into S3, Redshift, and ElasticSearch.

Kinesis streams are divided into shards and partitions that lie between producers and consumers. Data retention is one day by default but can be set for up to seven days. It can re-process and re-play data. Multiple applications can consume the same stream as well. It includes real-time processing with scalable throughput. Data inserted into Kinesis is immutable. Billing is based on each shard provisioned, batching is available (in contrast to per message calls), and the number of shards can evolve over time through resharding or merging. Records are ordered per shard. Latency is real-time (~ 200 ms)

Kinesis Data Streams are limited on the producer side to 1 MB/s or 1000 messages/s to write per shard. If this is exceeded, it throws a `"ProvisionedThroughputException"`. Consumer classic is limited to 2 MB/s to read per shard across all consumers, 5 API calls/s per shard, and throttling if three different applications are consuming. Again, data retention by default is set to 24 hours.

Kinesis producers include Kinesis SDK, Kinesis Producer Library, Kinesis Agent, CloudWatch Logs, and third party libraries. Kinesis consumers include Kinesis SDK, Kinesis Client Library, Kinesis Connector Library, Kinesis Firehose, Lambda, and third party libraries. Kinesis KCL uses DynamoDB to checkpoint offsets. KCL uses DynamoDB to track other workers and share the work amongst shards. It is designed for reading in a distributed manner.

Kinesis Data Firehose is fully managed and near real time (~60s latency). It loads data into Redshift, S3, ElasticSearch, and Splunk. It features automatic scaling, data transformation through Lambda, compression (if the target if S3), and billing is based on the amount of data going through Firehose.

Streams is for custom code, real time latency, un-managed scaling, real-time ElasticSearch insertion, and data storage. Firehose is fully managed and offers serverless data transformations through Lambda, near real-time latency (~60 seconds), and no data storage.

Data Analytics performs real-time analysis on Kinesis Streams using SQL. It is fully-managed and continuous in real-time. Billing is based on actual consumption rate.


## CloudWatch

CloudWatch Metrics oversees various metrics for other services such as EC2 by default every 5 minutes with basic monitoring. Detailed monitoring offers metrics every 60 seconds. Metrics are deleted over time, increasing timestamp gaps in older metrics. Metrics are available for up to 15 months (at which point only one data point per hour is maintained.)

Important CloudWatch EC2 metrics include:
* CPU Utilization (aggregated)
* Disk Reads/Writes
* Network Packets

Custom metrics can also be created with a standard or high resolution of either one minute or one second granularity respectively. Metrics can be exported using various filters including metric name, dimensions, start and end time, etc.

CloudWatch Alarms are alerts based on certain conditions that can trigger SNS notifications or cause auto scaling or EC2 actions. It cannot directly cause any other actions. SNS topics must be used to trigger any other automated responses. Billing alarms can also be set in the `us-east-1` region for AWS accounts.

The Unified CloudWatch Agent can be installed on EC2 instances to provide more detailed monitoring and logs from within an EC2 instance such as a breakdown of CPU utilization. AWS SSM can manage parameters to insert into CloudWatch Agent installations.

EventBridge is an expanded version of CloudWatch Events which supports rules and other triggers that can be integrated with various AWS services and third-party monitoring services such as Datadog.

S3 Events and unrelated to CloudWatch Events. S3 Events are concerned with specified changes in S3 buckets and can be sent to an S3 bucket, an SNS Topic, or a Lambda function. CloudWatch Events can be used to catch events not specified by S3 Events, such as API call events with CloudTrail.

CloudWatch dashbaords can visualize and aggregate logs, metrics, and alarms via widgets.


## X-Ray

AWS X-Ray collects data about requests distributed across servers and services. Distributed calls are tracked in terms of traces which tracks services invoked, operations invoked, metadata, and length of time for call. CloudWatch and SNS can be integrated with X-Ray to trigger Lambdas based on X-Ray high latencies.

## Amazon ElasticSearch

AWS ElasticSearch is an AWS-mnaaged version of ElasticSearch. It is not serverless and needs to be run on a server. It is used for log analytics, real-time application monitoring, security analytics, full text search, clickstream analytics, and indexing.

The ELK stack (ElasticSearch + Kibana + Logstash ) includes ElasticSearch for providing search and indexing capability. Kibana provides real-time dashboard on top of ES data, an alternative to CloudWatch. Logstash is a log ingestion mechanism (an alternative to CloudWatch Logs and Agent). 

AWS ElasticSearch patterns uses CloudWatch Logs subscription filters to trigger a Lambda which passes log data to ES in real-time.


# Domain 4: Policies and Standards Automation

## AWS Systems Manager

AWS Systems Manager helps manage EC2 and on-premise servers at scale with operationals insights, monitoring, patching automation, as well as CloudWatch and AWS Config integrations. It includes resource groupings, insight dashboards, parameter stores, and actions.

SSM Actions include:
* Automation
* Run Command
* Session Manager
* Inventory
* Patch Manager
* Maintenance Windows
* State Manager

SSM agent must be installed on servers whether EC2 isntance or on-premise VMs. It is installed by default on Amazon Linux AMI and some Ubuntu AMIs. EC2 instances require an IAM role to use SSM actions. On-premise instances can be tagged and grouped.

Run Command consists of a Command Documents. These are JSON documents that specify the command to be run at run-time. `AWS-UpdateSSMAgent` updates SSM Agents on instances to the latest version. `AWS-RunShellScript` runs a shell script on all instances.

Patch Manager consists of Patch Baselines with specified operating system and if it is the default baseline for AWS. Patch baselines containe a name, approval rules, exceptions, and patch sources. Patch Manager will then report to SSM Compliance if individual instances are compliant with the most recent patching standards.

SSM Automation simplifies administrative and other custom workflows and send notifications based on the results. This can be used to create golden AMIs, i.e. AMIs standardized through configuration, consistent security patching, logging, etc.


## AWS Config

AWS Config provides rules and compliance monitoring for various services. One Config rule costs $1.00. Configuration history is stored in a S3 bucket. Configuration changes can be streamed to SNS to trigger notifications and alarms. SNS can then automate response patterns via CloudWatch Events and/or Lambda. Config rules can be triggered based on configuration changes or periodic for when they review the specified resources.

Config can be integrated with AWS SSM to create automatic remediation actions based on compliance violations.

Config aggregators can be used to centralize AWS Config data across AWS accounts and regions.


## AWS Service Catalog

Service Catalog can be used by IT administrators to create a menu of organization-compliant virtual machines, databases, storage options, etc. for AWS users to create from using CloudFormation templates.


## AWS Inspector

AWS Inspector checks for security exposures or vulnerabilities in EC2 instances. Network assessments do not require the Inspector agent but Host assessments do. Assessments can be set to certain periodic intervals such as every 7 days. Inspector can trigger SNS topics that can automate remediation. Inspector cannot launch EC2s or Golden AMIs on its own.

## AWS Service Health Dashboard

This dashboard provides a global view of all AWS services across all regions to check for any outages or AWS-internal errors. The Personal Health Dashboard by contrast provides information on services you use and can be integrated with CloudWatch Alarms.

## AWS Trusted Advisor

Trusted Advisor provides a set of best practices to ensure your AWS account follows best practices according to the Well-Architected Framework. AWS hsots an official Trusted Advisor GitHub repo which provides a number of tools to follow Trusted Advisor recommendations. This can be integrated with SSM to automate Trusted Advisor recommendations. By default, Trusted Advisor does not refresh on its own but must be manually refreshed every five minutes. However, the AWS Support API can be used to refresh all recommendations through the `aws support refresh-trusted-advisor-check` 

## GuardDuty

GuardDuty is an intelligent threat discovery process to protect your AWS account. It uses machine learning, anomaly detection, and third party data. It is one click to enable for a 30 day free trial. Input data includes CloudTrail, VPC Flow Logs, and DNS Logs. It notifies user of findings and can be integrated with CloudWatch Event rules to trigger automatic remediation.
## Macie

Amazon Macie analyzes S3 data for detecting and classifying data. It will mark out any potentially sensitive data for the user.



# Domain 5: Incident and Event Response + Domain 6: HA, FT, and DR

## Auto Scaling Groups

EC2 Auto Scaling Groups can be set from Launch Configurations or from Launch Templates. Launch Templates are newer and the preferred method of ASG deployment. Launch Configurations provide a specified IAM role monitoring, user data, storage, VPC, subnet, and security group with a particular AMI. Launch Templates are more advanced. Templates include versioning, template inheritance, AMI ID, instance type, and many other options. 

ASGs can have scheduled events to scale instances based on periodic or cron triggers. Scaling can also be based on policies that use metrics such as CPU Utilization to scale the ASG. This includes a warm-up period which means the number of seconds required for the condition to be met before the scaling action actually occurs. Scaling policies can also be prevented from terminating instances. ASGs can also be integrated with CloudWatch Alarms.

ASGs can be integrated with ALB with full scale-in and scale-out support. Route 53 can target a DNS record toward the ALB which then in turn targets the ASGs. ASG processes can be suspended for a variety of reasons such as for errors that require troubleshooting. `HealthCheck` is used to determine the health of target instances and can be used to terminate and/or replace instances with `ReplaceUnhealthy`. `AddtoLoadBalancer` will not add the suspended instance to the ALB Target Group. Scale-in protections can prevent scaling from passing certain minimum and maximum thresholds.

ASGs include lifecycle hooks which are stages in the scaling process. When scaling out, there are several stages: `Pending:Wait`, `Pending:Proceed`, and `InService`. Following this, scaling in includes `Terminating:Wait`, `Terminating:Proceed`, and `Terminated`. SNS, SQS, and Lambda via CloudWatch Events can be used to interact with Lifecycle Hooks. SNS and SQS are both legacy options, Lambda via CloudWatch Events is the recommended way to interact with Lifecycle Hooks. Termination Policies determine the sequence in which instances are scaled in and terminated. These include `OldestInstance`, `OldestLaunchConfiguration`, etc. 

The default termination policy follows the following steps:
1. It determines which availability zone has the most instances and finds an instance not protected from termination.
2. It looks at the allocation strategy to determine which instance is best to terminate to maintain that strategy. If there is an applicable instance, that is terminated.
3. If (2) is not met, then it looks at the instance using the oldest launch template or launch configuration and terminates that
4. If there is no applicable instance from the above, then it terminates the unprotected instance which is closest to the next billing hour. (Billing is now per second, so this now applies to the instance closest to the next billing second.)

SQS can target ASG instances with messages and scale the instances accordingly. Workers in an auto-scaled pool are protected from auto scaling termiantion to ensure SQS processes are not interfered with. ASG notifications can be sent to an SNS topic.

CodeDeploy and CloudFormation can be integrated with ASGs to automate deployments, including deployment behavior such as in case of failure. CodeDeploy will deploy new revisions of the application in-place on top of the ASG. If autoscaling occurs while a new application revision is being deployed, only the application version that has most recently completed a full deployment will be used for the new scaled-up instance. This can be circumvented by suspending autoscaling while CodeDeploy is executing a new deployment.

There are several deployment strategies for ASGs and ALBs:
* In-place (one LB, one TG, one ASG). The application is stopped and the new one deployed over it.
* Rolling (one LB, one TG, one ASG). New instances are added before the old are removed. 
* Replace (one LB, one TG, two ASGs). A new ASG is created with accompanying instances. The LB will target the new ASG when it is ready.
* Blue/Green (two LB, two TG, two ASG, R53). The Route 53 record will switch from targeting the old ALB to the new ALB. This requires having parallel environments of the old and new application revision.

## Multi-AZ

Multi-AZ is enabled manually in these services:
* EFS
* ELB
* ASG
* EB (assign AZ)
* RDS
* ElasticCache
* ElasticSearch (AWS-managed)
* Jenkins (self-deployed)

Automatically enabled:
* S3 (except OneZone access)
* DynamoDB
* All AWS proprietary managed servies

EBS is single AZ by design. It can be replicated to another AZ through EBS snapshots. This can be integrated with lifecycle hooks to automate multi-AZ EBS deployments. PIOPS volumes (io1) should read the entire volume once during the pre-warming of I/O blocks.


## Multi-region

Some services are natively multi-region:
* DynamoDB Global Tables
* AWS Config Aggregators
* RDS Cross Region Read Replicas
* Aurora Global Database
* EBS volume snapshots, AMI, RDS snapshots
* VPC Peering
* Route53 and CloudFront
* S3 Cross Region Replication
* Lambda@Edge for global Lambda functions at Edge locations (good for A/B testing)

Route 53 health checks can automatically remediate DNS failovers.

CloudFormation StackSets are a useful way to replicate resources stacks across multiple regions and accounts via a single operation. Trusted accounts can create, delete, or update StackSets. When a StackSet is updated, all associated stack instances are updated throughout all accounts and regions.

CodePipeline can also execute multi-region deployments by storing CodePipeline Artifacts across regions, setting up individual CodeDeploy EC2 instances in each region to retrieve their respective artifacts.


## Disaster Recovery

A disaster is any event that negatively impacts business continuity. DR is concerned with pre-emptive preparation and post facto recovery. RPO is concerned with the latest point of time in recovery. How far back does data have to rewind. RTO is the amount of time it takes to recover systems. How long after the incident until everything is operational again.

Disaster recovery strategies:
* Backup and restore
* Pilot light
* Warm standby
* Hot site / multi-site approach

Multi-region DR checklist:
* Is the AMI and accompanying parameters stored in multiple regions?
* Is the CloudFormation StackSet tested for other regions?
* What is the RPO and RTO?
* Are Route 53 health checks working correctly?
* Are CloudWatch Events and associated Lambdas designed to automate failover?
* Is data backed up across regions?
