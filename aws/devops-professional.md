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



