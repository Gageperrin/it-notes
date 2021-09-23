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