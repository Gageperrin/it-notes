# AWS Certified Developer Associate Notes

These are my notes for the AWS DVA-C01 exam. I have taken the content from [Adrian Cantrill's course](https://learn.cantrill.io/p/aws-certified-developer-associate) for this exam, Tutorial Dojo's practice tests, and the official AWS documentation. 

These notes assume full and prior knowledge of requisite material for the Solutions Architect Associate and Professional exams and will not mention or cover any topics that are featured in those exams. Consequently, only a few topics will be treated here.

## CloudFormation

CloudFormation templates are in either YAML or JSON and contain logical resources used to create stacks of physical resources.

Template parameters accept input when a stack is created or update, and these can be referenced from within logical resoruces. They influence physical resources and/or configuration. They can create defaults, `AllowedValues`, min and max, `AllowedPatterns`, `NoEcho`, and have a type. Pseudo Parameters are injected by AWS into the CloudFormation Stack (e.g. `AWS::Region`).


### Intrinsic Functions

Intrinsic Functions can be used to assign values to properties that are not available until runtime. For example:
* `Ref` and `Fn::GetAtt` can be used to reference resources in another template or stack. 
* `Fn:Join` and `Fn::Split` joins and splits strings. 
* `Fn:GetAZs` and `Fn::Select` are commonly used together to pick an AZ out of a list of available AZs. 
* Conditional operators
* `Fn::Base64` and `Fn::Sub` alter the inputted string.
* `Fn::Cidr` can configure CIDR ranges automatically.

`Ref` can be used to reference the main value of a parameter or logical resource. Using `!Ref` on template or pseudo parameters returns their value. When used with logical resources, the physical ID is usually returned.

`!GetAtt LogicalResource.Attribute` can be used to retrieve any attribute associated with the resource. This includes detailed configuration of the physical resource.

`Fn::GetAZs` returns the list of AZs in a region where subnets exist for that VPC. This is every AZ by default but a badly configured VPC may not have all AZs. These can be selected with `Fn::Select`.

`Fn::Base64` accepts plaintext and outputs Base64 encoded text. It substitutes variables in the input.

`Fn::Cidr` automates subnet range deployment and can specify how many subnets to generates and how many bits for each range. This can be combined with `Fn::Select` to assign to AZs.

### Mappings, Outputs, Conditions, DependsOn, and Wait Conditions

Templates can contain a Mappings object which can contain many mappings that map keys to values. These can be looked up from a key or from a top or second level (e.g. a top key for an AMI would be region). Mappings use the `!FindInMap` intrinsic function. They are commonly used for retrieving AMI data.

Syntax: `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]`
Example: `!FindInMap [ "RegionMap", !Ref 'AWS::Region', "HVM64" ]` finds HVM64 AMI architecture.

Templates can have an optional outputs section that can be visible from the CLI, the console UI, can be accessible from the parent stack when nesting, and can be exported for cross-stack references.

Conditions are created in the optional `Conditionals` section of a template, and they are processed before resources are created. Use other intrinsic functions associated with logical resources to control if they are created or not.

`DependsOn` helps organize a dependency order for CloudFormation because CF does things in parallel by formally defining dependencies. This is mainly useful for making CF templates more readable. 

Note that an elastic IP requires an IGW attached to a VPC in order to work, but there is no dependency in the template. Without an explicit dependency, CF will delete resources in a random order, and this can cause errors depending on the sequence it creates IGW and the EIP. An explicit depends on can prevent this.

With simple provisioning, once the logical resource reaches a `CREATE_COMPLETE` state then no other information from the resource is used in the CF provisioning stage, even if this is long before the bootstrapping is complete.

However with CF Signal, it is possible to hold, wait for a certain number of success signals, or wait for a timeout for these signals of up to 12 hours. If there is a failure signal, then creation fails. If timeout is reached, then creation fails. `CreationPolicy` or `WaitCondition` can help with this process.

`CreationPolicy` applies signal or timeout requirements for particular resources before a `CREATE_COMPLETE` state is reached. `WaitCondition` is a specific logical resource of its own. It can have dependencies or be depended upon. It has a `WaitHandle` that can pause provisioning for certain resources while waiting for other resources to return JSON data. It can also query this returned data in other resources using `!GetAtt WaitCondition.Data`.

### Nested Stacks, Cross-stack, and StackSets

A single stack has a hard limit of 500 resources, and resources in a single stack share a lifecycle. Stacks can't easily resuse resources or reference other stacks.

Nested stacks have an ultimate root stack (manually created) and a parent stack for each child stack. Child stacks can have a sequence of `DependsOn` to provide order when provisioning, providing outputs to one another. Whole templates can be reused in other stacks. Use nested stacks when they form part of one solution and have a linked lifecycle.

Cross-stack references are designed to be isolated and self-contained so they are not lifecycle linked. They are helpful because outputs are not normally visible from other stacks. Outputs can be exported to make them visible from other stacks. Exports must have a unique name in the region. `Fn::ImportValue` can be used instead of `Ref`. Every region in an account has its own list of exports. `ImportValue` can be used instead of `Ref` to reference region exported values. Cross-stack references are service oriented, feature stack reuse, and permit different lifecycles.

StackSets allow for deploying CFN stacks across many accounts and regions. StackSets are containers in an admin account. StackSets contain stack instances which reference stacks (they are not just stacks, stack instances remain even if a stack fails). Stack instances and stacks are in target accounts, and they can be self-managed or service-managed. StackSets can specify concurrent accounts, failure tolerance, and retention policies. They can be used to enable AWS Config, Config rules, or create IAM roles.

### Deletion Policy

If a logical resource is deleted from a template, then the physical resource is deleted by default which may accidentally cause data loss. A deletion can be set to `Delete`, `Retain`, or sometimes `Snapshot`. This determines what happens when a logical resource is deleted. The deletion policy does not apply to a replace action where resources are recreated.

### Stack Roles

When a stack is created, CFN creates physical resources. CFN uses the permissions of the logged in identity which requires AWS permissions. CFN can assume a role which does not need resource permissions only `PassRole`.

### `cfn-init` and `cfn-hup`

`cfn-init` is a simple configuration management system. Its configruation directives are stored in a template. `AWS::CloudFormation::Init` is part of a logical resource and is an imperative approach. By contrast, `cfn-init` is declarative and provides the desired state.

`cfn-init` helper script installed on the EC2 OS makes it possible to access `cfn-init`. It communicates directly with the provisioned resources rather than only indirectly via the stack. `ConfigKeys` provide creation meta-data for packages, groups, users, sources, files, commands, and services to be included in the launched infrastructure. `ConfigKeys` can be bundled into a `ConfigSet`.

[`cfn-hup` later]

### ChangeSets

Change sets can be used to preview how changing configured resources in a stack may affect the environment. It is possible to review many different versions of a change set and execute a single one.

### Custom Resources

Custom resources allow the user to write custom provisioning logic that runs anytime a stack is created, updated, or deleted. For example, a Lambda function can be invoked to receive events and upload them to a S3 bucket.


## CI/CD using AWS Code

CI/CD in AWS consists of four stages: Code, Build, Test, and Deploy. These stages often overlap but constitute the best practice development pipeline. Legacy solutions include Github, Bitbucket, Jenkins, etc.

Code is supported by CodeCommit. Build and Test are supported by CodeBuild. Deploy is supported by CodeDeploy. A pipeline is linked to only one branch in a repository. A team could therefore have a Development pipeline and a Master pipeline that deploy to test and production environments respectively.

`buildspec.yml` and `appspec.yml/json` are important for this process. The first is a series of commands and specifications used in CodeBuild to shape the build process. The second provides specification for application deployment in CodeDeploy. They could also refer to CodePipeline. 

CodeDeploy can be used to deploy an application onto EC2, into AWS Elastic Beanstalk (environment name needed, AWS OpsWorks (stack needed), AWS CloudFormation, Amazon ECS (and ECS Blue/Green), AWS Service Catalog, Alexa Skills Kit, and Amazon S3.


## AWS Lambda

Lambda is a function-as-a-service. A function is a piece of code that Lambda runs. Functions use a runtime and they are loaded and run in a runtime environment. The environment has a direct memory (indirect CPU) allocation, and they are only billed for the duration that the function runs for.

Python, Ruby, Java, Go, C# supported but Docker is not supported (anti-pattern). Custom runtimes such as Rust are possible using layers. Can directly control the memory allocated for Lambda functions whereas vCPU is allocated indirectly. 512MB storage avialable at /tmp. Functions can only run up to 900 seconds (15 minutes).

Common uses: Serverless applications (S3, API Gateway, Lambda), file processing (S3, S3 Events, Lambda), database triggers (DynamoDB streams, Lambda), serverless CRON (EventBridge/CWEvents, Lambda), real-time stream data processing (Kinesis, Lambda).

By default, Lambda functions are given public networking. They can access public AWS services and the public Internet. Public networking offers the best performance because no customer specific VPC networking is required, but Lambda functions will have no access to VPN-based services unless public IPs are provided and security controls allow external access. Lambda functions running in a VPC have to obey all VPC networking rules. VPC Endpoints can provide access to public AWS service. NAT gateway and IGW are required for VPC Lambdas to access Internet resoruces. Lambda will also need EC2 networkign permissions.

VPC-based Lambda functions do not run in the VPC, but they are like Fargate. AWS automatically sets up an ENI for every unique combination of security groups and subnets for Lambda functions. This ENI has 90 second initial setup with no invocatoin delay. The new design makes VPC-baesd Lambdas a viable solution.

Lambda execution roles are IAM roles attached to functions which control the permissions the function receives. Lambda resource policy controls which services and accoutns can invoke Lambda functions. Lambda uses CloudWatch, CloudWatch Logs & X-Ray. Logs from Lambda executions are CloudWatch Logs. Metrics are stored in CloudWatch. Lambda can be integrated with X-Ray for distributed tracing of user/application pathing. CloudWatch Logs requires permisssions via exceution role for Lambda function.

For synchronous invocation, the CLI/API invokes a Lambda function, passing in data and wait for a response. Lambda function responds with data or fails. Clients communicate with API gateway, proxied to Lambda function. The function responds or fails and the response is sent back to the client. With a synchronous invocation, the result is returned during the request. Errors or retries ahve to be handled within the client.

For asynchronous invocation, the event is generated and the requester stops tracking it. Lambda is responsible for re-processing (between zero and two times).

Lambda function needs to be idempotent re-processing a result should have the same end state. Events can be sent to dead letter queues after repeated failing processing. Lambda supports destination like SQS, SNS, and EventBridge where successful or failed events can be sent.

Event source mappings are typicallly used on streams or queues which do not support event generation to invoke Lambda (Kinesis, DynamoDB streams, SQS). Event soruce mapping read/poll from the steam or queue and deliver event batches to Lambda. Event batches are processsed OK or FAIL as a batch. Permissions from the Lambda execution role are used by the event source mapping to interact wiht the event source.

SQS queues or SNS topics can be used for any discarded failed event batches. Lambda functions have versions, but they are immutable once published with their own ARN. $Latest points at the latest. Aliases (DEV, STAGE, PROD) point at a version that can be changed.

An execution context is the environment a Lambda function runs in. A cold start is a full creation and configuration including code download (~100 ms). A warm start already has the execution context and can re-use it. A new event is passed in but the event context creation can be skipped (~1-2 ms). A Lambda invocation can re-use an execution context but has to assume it cannot. If used, infrequently contexts will be removed. Concurrent executions will use multiple context. Provisioned concurrency can be used. AWS will create and keep x context warm and ready to use which improves starting speeds.


## API Gateway

API Gateway is a fully managed service that creates and manages APIs, including endpoint/entry-point for applications. Sits between the applications and the integrated services. It is HA, scalable, handles authorization, offers throttling, caching, CORS, transformations, OpenAPI spec, and direct integration. It can connect to service/endpoints in AWS or on-premises. Uses HTTP, REST, and WebSocket APIs.

Steps:

Request phase: authorize, validate, transform.
Integrations
Response: Transform, prepare, and return.
CloudWatch logs can store and manage full stage request and response logs. CloudWatch can store metrics for client and integration sides. API Gateway Cache can be used to reduce the number of calls made to backend integrations and improve client performance. Authentication with Cognito and authorization through Lambda authorizer.

Endpoint types include edge-optimized routed to the nearest CloudFront POP (point of presence). It is regionally routed to clients in the same region, and it is private, acccessibly only wihtin a VPC via an interface endpoints.

APIs are deployed to stages that have one deployment each. Stages can be enabled for canary deployments where deployments are made to the canary, not the stage. Stages enabled for the canary deployments can be configured so do a certain percentage of traffic is sent to the canary. This can be adjusted over time or the canary can be promoted to make it the new base stage.

Errors: 400 series are client errors: 400 - generic bad request 403 - access denied error or filtered 429 - API gateway throttling, exceeded amount 502 - bad gateway exception (bad output returned by Lambda) 503 - service unavailable 504 - integration failure/timeout 29 second limit

Caching is defined per stage. The cache TTL default is 300 seconds, configurable to be between 0 and 3600 seconds. It can be encrypted, and the cache size is between 500 MB to 237 GB. Calls are only made to backend integratoins if request is a cache miss.


## Elastic Beanstalk

Elastic Beanstlak is a platform as a service vendor that handles infrastructure, while the user handles the code. It is developer focused and provides managed applicatoin environmnets. User provides the code and Elastic Beanstalk handles the environment. Fully customizable and uses AWS products under the hood. Requires application changes so it is not free.

Built-in languages:

Go
Java SE
Tomcat
.NET Core (Linux)
.NET (Windows)
Node.js
PHP
Python
Ruby
Docker: Single container docker option, multi-container docker, or pre-configured docker (e.g. supports Glassfish Java 8). Custom platforms via packer.

EB application is not code but a collection of things relating to an application. Application versions are a specific labeled version of deployable code for an application. The source bundle is stored on S3. Environments are containers of infrastructure and configuration for a specific application version. Each environment is either a web server tier or a worker tier--it controls the structure and function of the environment. App versions can be deployed to environmnets. Web and worker tiers communciate via SQS queues. Each environment has its own CNAME which can be swapped to exchange two environment DNS.

Blue/green deployment is where a green environment is created for testing. When it is ready, the CNAME is swapped from blue to green. The blue environment is only deleted when green is fully functional.

Elastic Beanstalk is not free but is great for small development teams. Use Docker for anything unsupported. Databases should be outside Elastic Beanstalk. Databases in an environment are lost if the environment is deleted.
