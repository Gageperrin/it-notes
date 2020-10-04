# App Services, Containers and Severless

## Table of Contents

1. [Introduction to Containers](#introduction-to-containers)
2. [Elastic Container Service ECS](#elastic-container-service-ecs)
3. [Simple Notification Service SNS](#simple-notification-service-sns)
4. [Simple Queue Service SQS](#simple-queue-service-sqs)
5. [Amazon MQ](#amazon-mq)
6. [AWS Lambda](#aws-lambda)
7. [CloudWatchEvents and EventBridge](#cloudwatchevents-and-eventbridge)
8. [Advanced API Gateway](#advanced-api-gateway)
9. [AWS Step Functions](#aws-step-functions)
10. [Simple Workflow Service](#simple-workflow-service-swf)
11. [Amazon Mechanical Turk](#amazon-mechanical-turk)
12. [Elastic Transcoder and AWS Elemental MediaConvert](#elastic-transcoder-and-aws-elemental-mediaconvert)
13. [AWS IOT](#aws-iot)
14. [AWS Greengrass](#aws-greengrass)
15. [AWS Serverless Application Model SAM](#aws-serverless-application-model-sam)


## Introduction to Containers

Docker containers are built from a docker image template that is identical in every way except for the Read/Write layer. A container registry is a hub of containers. Dockerfile uploaads the container image to a registry. These can be deployed to docker hosts. Docker hosts can be run based on one or many images. Dockerfiles are used to build images. They are portable, self-contained, lightweight, and always run as expected. The parent OS used and fs layers are shared.

Containers only run the application and environment needed. It provides much of the isolation virtual machines do. Ports are exposed to the host and beyond. Application stacks can be multi-container.


## Elastic Container Service ECS

> [Amazon Elastic Container Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) (Amazon ECS) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage containers on a cluster. Your containers are defined in a task definition which you use to run individual tasks or as a service. You can run your tasks and services on a serverless infrastructure that is managed by AWS Fargate or, for more control over your infrastructure, you can run your tasks and services on a cluster of Amazon EC2 instances that you manage.

ECS clusters are where containers run from. Task definitions store resources are used by task (CPU, memory), networking, and memoery as well as the task role (security through IAM role). A task can contain one or more containers. A service definition is concerned with HA, scalability, how many tasks should be running. The container definition contains image and ports. ECS can run in EC2 mode or Fargate mode.

In EC2 mode, a ECS cluster is created in an AWS account. EC2 instances are used to run containers through an auto-scaling group. User needs to manage capacity and availability. Fargate depends on a shared infrastructure.

Use cases: If using containers, EC2 for large workloads and price conscious, and use Fargate for large loads and if overhead conscious, as well as for small or burst workloads, and for batch or periodic workloads.


## Simple Notification Service SNS

[Amazon Simple Notification Service](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) (Amazon SNS) is a managed service that provides message delivery from publishers to subscribers (also known as producers and consumers). Publishers communicate asynchronously with subscribers by sending messages to a topic, which is a logical access point and communication channel. Clients can subscribe to the SNS topic and receive published messages using a supported protocol, such as Amazon SQS, AWS Lambda, HTTP, email, mobile push notifications, and mobile text messages (SMS).

Amazon SNS is a pub/sub system for reliable delivery of notification messages (up to 256 KB payloads). SNS topics are the base entity of SNS which require permissions and configuration. A publisher sends messages to a topic, and topics have subscribers which receive messages. SNS is used across AWS for notifications. It offers delivery status, delivery retries, HA, scalability, server-side encryption (SSE) and cross-account (via topic policy).


## Simple Queue Service SQS

> [Amazon Simple Queue Service](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) (Amazon SQS) offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components. Amazon SQS offers common constructs such as dead-letter queues and cost allocation tags. It provides a generic web services API and it can be accessed by any programming language that the AWS SDK supports.

Amazon SQS is a public, fully managed, highly-available service that provides standard or FIFO queues with messages up to 256 KB, but can link to larger messages in S3. Received messages are hidden (VisibilityTimeout), then either re-appear or are explicitly deleted. Dead-letter queues can be used for problem messages that re-appear multiple times. Auto scaling groups can scale and lambda can be invoked using queue length as a trigger.

Auto scaling group web pool -> Queue -> Auto scaling group worker pool

Auto scaling group web pool is scaled out when CPU is high, and scaled in when CPU is low. The auto scaling group worker pool is scaled out when queue length is long and scaled in when queue length is short. To create multiple events out of one SNS topic, the topic is fanned out into multiple SQS queues. FIFO 3,000 messages per second with batching, without batching up to 300 messages per second.

Billing: Based on reqeust, 1 request = 1-10 messages of up to 64 KB total. More frequently polled is less cost effective.

Short polling is immediate versus long polling (`waitTimeSeconds`). SQS offers encryption at rest (KMS) and in-transit. Queue policy allows access from external accounts.


## Amazon MQ

> [Amazon MQ](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/welcome.html) is a managed message broker service for Apache ActiveMQ that makes it easy to migrate to a message broker in the cloud. A message broker allows software applications and components to communicate using various programming languages, operating systems, and formal messaging protocols.

Amazon MQ is an AWS implementation of Apache ActiveMQ. SNS and SQS are AWS services that use AWS APIs and are highly scalable and AWS integrated. However, many organizations already use topics and queues and want to migrate to AWS. SNS and SQS do not work out of the box, but Amazon MQ is an open-source message broker that supports JMS API protocols such as AMQP, MQTT, OpenWire, and STOMP.

MQ provivdes queues and topics. One-to-one and one-to-many. Single instance (test, dev, cheap) or HA Pair (active and standby). MQ is VPC based which means that **private networking required**. No AWS native integration as it delivers an ActiveMQ product. On-premises message producer interacting with on-premises ActiveMQ. It delvier to ActiveMQ broker. Private network connection needed between on-premises and VPC (Direct Connect or Site-to-Site VPN). Use SNS or SQS for most new implementations or if AWS integration is required.

Use cases: To migrate from an existing system with little to no application change or if APIs such as JMS or protocols such as AMQP, MQTT, OpenWire, and STOMP are needed.

## AWS Lambda

> [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) is a compute service that lets you run code without provisioning or managing servers. AWS Lambda executes your code only when needed and scales automatically, from a few requests per day to thousands per second. You pay only for the compute time you consume - there is no charge when your code is not running.

Lambda is a function-as-a-service. A function is a piece of code that Lambda runs. Functions use a runtime and they are loaded and run in a runtime environment. The environment has a direct memory (indirect CPU) allocation, and they are only billed for the duration that the function runs for.

Python, Ruby, Java, Go, C# supported but docker is not support (anti-pattern). Custom runtimes such as Rust are possible using layers. Can directly control the memory allocated for Lambda functions whereas vCPU is allocated indirectly. 512MB storage avialable at /tmp. Functions can only run up to 900 seconds (15 minutes).

Common uses: Serverless applications (S3, API Gateway, Lambda), file processing (S3, S3 Events, Lambda), database triggers (DynamoDB streams, Lambda), serverless CRON (EventBridge/CWEvents, Lambda), real-time stream data processing (Kinesis, Lambda).

By default, Lambda functions are given public networking. They can access public AWS services and the public Internet. Public networking offers the best performance because no customer specific VPC networking is required, but Lambda functions will have no access to VPN-based services unless public IPs are provided and security controls allow external access. Lambda functions running in a VPC have to obey all VPC networking rules. VPC Endpoints can provide access to public AWS service. NAT gateway and IGW are required for VPC Lambdas to access Internet resoruces. Lambda will also need EC2 networkign permissions.

VPC-based Lambda functions do not run in the VPC, but they are like Fargate. AWS sets up on ENI for every unique combination of security groups and subnets for Lambda functions. This ENI has 90 second initial setup with no invocatoin delay. The new design makes VPC-baesd Lambdas a viable solution.

Lambda execution roles are IAM roles attached to functions which control the permissions the function receives. Lambda resource policy controls which services and accoutns can invoke Lambda functions. Lambda uses CloudWatch, CloudWatch Logs & X-Ray. Logs from Lambda executions are CloudWatch Logs. Metrics are stored in CloudWatch. Lambda can be integrated with X-Ray for distributed tracing of user/application pathing. CloudWatch Logs requires permisssions via exceution role for Lambda function.

For synchronous invocation, the CLI/API invokes a Lambda function, passing in data and wait for a response. Lambda function responds with data or fails. Clients communicate with API gateway, proxied to Lambda function. The function responds or fails and the response is sent back to the client. With a synchronous invocation, the result is returned during the request. Errors or retries ahve to be handled within the client.

For asynchronous invocation, the event is generated and the requester stops tracking it. Lambda is responsible for re-processing (between zero and two times).

Lambda function needs to be idempotent re-processing a result should have the same end state. Events can be sent to dead letter queues after repeated failing processing. Lambda supports destination like SQS, SNS, and EventBridge where successful or failed events can be sent.

Event source mappings are typicallly used on streams or queues which do not support event generation to invoke Lambda (Kinesis, DynamoDB streams, SQS). Event soruce mapping read/poll from the steam or queue and deliver event batches to Lambda. Event batches are processsed OK or FAIL as a batch. Permissions from the Lambda execution role are used by the event source mapping to interact wiht the event source.

SQS queues or SNS topics can be used for any discarded failed event batches. Lambda functions have versions, but they are immutable once published with their own ARN. `$Latest` points at the latest. Aliases (DEV, STAGE, PROD) point at a version that can be changed.

An exceution context is the environment a Lambda function runs in. A cold start is a full creation and configuration including code download (~100 ms). A warm start already has the execution context and can re-use it. A new event is passed in but the event context creation can be skipped (~1-2 ms). A Lambda invocation can re-use an execution context but has to assume it cannot. If used, infrequently contexts will be removed. Concurrent executions will use multiple context. Provisioned concurrency can be used. AWS will create and keep x context warm and ready to use which improves starting speeds.

## CloudWatchEvents and EventBridge

> [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html) is a serverless event bus service that makes it easy to connect your applications with data from a variety of sources. EventBridge delivers a stream of real-time data from your own applications, Software-as-a-Service (SaaS) applications, and AWS services and routes that data to targets such as AWS Lambda. You can set up routing rules to determine where to send your data to build application architectures that react in real time to all of your data sources. EventBridge allows you to build event driven architectures, which are loosely coupled and distributed.

EventBridge is version 2 of CloudWatch Events. Use EventBridge by default. A default event bus for the account. In CW Events, this is the only bus (implicit). EventBridge can have additional event busses. Rules match incoming events (or schedules) that route the events to 1+ targets e.g. Lambda. State change sends the event into the event bus. EventBridge applies the event pattern rule or schedule rule ot target through lambda. Events are JSON structures.

## Advanced API Gateway

> [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale. API developers can create APIs that access AWS or other web services, as well as data stored in the AWS Cloud. As an API Gateway API developer, you can create APIs for use in your own client applications.

API Gateway is a fully managed service that creates and manages APIs, including endpoint/entry-point for applications. Sits between the applications and the integrated services. It is HA, scalable, handles authorization, offers throttling, caching, CORS, transformations, OpenAPI spec, direct integration. It can connect to service/endpoints in AWS or on-premises. Uses HTTP, REST, and WebSocket APIs.

Steps: 
1. Request phase: authorize, validate, transform.
2. Integrations
3. Response: Transform, prepare, and return.

CloudWatch logs can store and manage full stage request and response logs. CloudWatch can store metrics for client and integration sides. API Gateway Cache can be used to reduce the number of calls made to backend integrations and improve client performance. Authentication with Cognito and authorization through Lambda authorizer.

Endpoint types include edge-optimized routed to the nearest CloudFront POP. It is regionally routed to clients in the same region, and it is private, acccessibly only wihtin a VPC via an interface endpoints.

APIs are deployed to stages that each have one deployment. Stages can be enabled for canary deployments where deployments are made to the canary, not the stage. Stages enabled for the canary deployments can be configured so do a certain percentage of traffic is sent to the canary. This can be adjusted over time or the canary can be promoted to make it the new base stage.

Errors:
`400` series are client errors:
`400` - generic bad request
`403` - access denied error or filtered
`429` - API gateway throttling, exceeded amount
`502` - bad gateway exception (bad output returned by Lambda)
`503` - service unavailable
`504` - integration failure/timeout 29 second limit

Caching is defined per stage. The cache TTL default is 300 seconds, configurable to be between 0 and 3600 seconds. It can be encrypted, and the cache size is between 500 MB to 237 GB. Calls are only made to backend integratoins if request is a cache miss.


## AWS Step Functions

>[Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) is a serverless orchestration service that lets you combine AWS Lambda functions and other AWS services to build business-critical applications. Through Step Functions' graphical console, you see your application’s workflow as a series of event-driven steps.

It is designed to compensate for Lambda's limitations. A serverless workflow that occurs through states. Maximum duration is one year. There are standard and express workflows. Standard is default. Express is used for high-volume data processing and other kinds of workloads for up to five minutes. State machines can be started by API Gateway, IOT rules, EventBridge, Lambda. State machines coordinate the work occurring. Amazon State Language (ASL) is a JSON template.

IAM roles are used for permissions.

States:
* Succeed
* Fail
* Wait
* Choice
* Parallel
* Map (list)
* Task (Lambda, Batch DynamoDB, ECS, SNS, SQS, Glue, SageMaker, EMR, Step Function)


## Simple Workflow Service SWF

> The [Amazon Simple Workflow Service](https://docs.aws.amazon.com/amazonswf/latest/developerguide/swf-welcome.html) (Amazon SWF) makes it easy to build applications that coordinate work across distributed components. In Amazon SWF, a task represents a logical unit of work that is performed by a component of your application. Coordinating tasks across the application involves managing intertask dependencies, scheduling, and concurrency in accordance with the logical flow of the application. 

Simple Workflow Service helps developers build and scale background jobs that have parallel or sequential steps. It is a fully managed state tracker and task coordinator. It builds workflows and coordination over distributed components.

It was a predecessor to Step Functions using instances and servers. Same patterns and anti-patterns. Step Functions should be used by default.

* Activity task is what is done.
* Activity worker is the program that performs the task.
* Deciders schedule activity tasks, provides input data, and ends workflows.
* One year maximum run-time
* Step functions are serverless and have a lower admin overhead.
* SWF: AWS flow framework, external signals needed to intervene in processses that launch child flows that return to the parent. Bespoke and complex decisions using custom decider application that interact with Mechanical Turk. A child workflow can be initiated as part of a single activity of the parent. It runs, re-runs, and its results are used to decide flows in the parent workflow.


## Amazon Mechanical Turk

Amazon Mechanical Turk is a crowd-sourcing marketplace for outsourcing processes and jobs. Managed human task outsourcing where requesters post human intelligence tasks (HITs). Pay per task basis. Great for data collection, manual processing, and image classification.


## Elastic Transcoder and AWS Elemental MediaConvert

> [AWS Elemental MediaConvert](https://docs.aws.amazon.com/mediaconvert/latest/ug/what-is.html) is a file-based video processing service that provides scalable video processing for content owners and distributors with media libraries of any size. 

A file-based video transcoding service with broadcast-grade features. MediaConvert is a new product designed to replace Elastic Transcoder. It is serverless, so one pays for resources used. Add jobs to pipelines (with ET) or queues (MC). File load from S3, processed, then stored on S3. MC supports EventBridge (CWEvents v2) for job signalling.

Architecture:
1. Upload to InputBucket which is configured to execute a Transcode Lambda function into MediaConvert which loads the product into OutputBucket Origin.
2. MediaConvert registers an event through EventBridge which adds video metadata to DynamoDB using Lambda.
3. Output bucket can have its media distributed privately through CloudFront.
4. Users load HTML/JS from the website, connect to the API Gateway backed by a Lambda function.
5. If authorized, a signedURL is generated from the DynamoDB metadata and provided to the user to give access to the CloudFront private distribution.

Elastic Transcoder is legacy, default to Elemental MediaConvert. MediaConvert supports more codecs and is designed for larger volume and parallel processing, MediaConvert supports reserved pricing. Use Elastic Transcoder for WebM (VP8/VP9), animated GIF, MP3, FLAC, Vorbis, and WAV.


## AWS IOT

> [AWS IoT](https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html) provides the cloud services that connect your IoT devices to other devices and AWS cloud services. AWS IoT provides device software that can help you integrate your IoT devices into AWS IoT-based solutions. If your devices can connect to AWS IoT, AWS IoT can connect them to the cloud services that AWS provides.

The AWS IoT core is a suite of products for the Internet of Things devices. Used for managing millions of devices. It provides provisioning, updates, and control. Communication with devices may have unreliable links, so IoT provides device shadows. These are reliable and consistent copies of a real device which can be read from or written to. Any writes will be sent to the device when it next connects. Rules and event-driven integration with AWS services.

Device messages are in JSON using MQTT topics. IoT rules match events and can be used to invoke Lambda functions which check DynamoDB for customer set water level alert value. It uses SNS to send a push notification. Kinesis Data Firehose could be used to persist the data from IoT into S3 and/or send it on for near real-time processing using Kinesis Data Analytics.


## AWS Greengrass

> [AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/latest/developerguide/what-is-gg.html) is software that extends cloud capabilities to local devices. This enables devices to collect and analyze data closer to the source of information, react autonomously to local events, and communicate securely with each other on local networks. Local devices can also communicate securely with AWS IoT Core and export IoT data to the AWS Cloud. AWS IoT Greengrass developers can use AWS Lambda functions and prebuilt connectors to create serverless applications that are deployed to devices for local execution.

AWS IoT Greengrass extends AWS to edge devices with some compute, messaging, data management, sync and ML capabilities. Locally run Lambda and containers and shadow IoT devices locally.

Device reactions can occur in real-time locally with no cloud communications or latency. Greengrass allows for operation to continue even with intermittent connectivity. AWS based services can be used via AWS IoT core and directly from local Lambda functions.


## AWS Serverless Application Model SAM

The AWS Serverless Application Model (SAM) is an open-source framework for building serverless applications. It provides shorthand syntax to express functions, APIs, databases, and event source mappings.

Functions and services:
* Front end code and assets - SE and CloudFront
* API Endpoint - API Gateway
* Compute - Lambda
* Database - DynamoDB

AWS SAM template specification is an extension of CloudFormation with transform, globals, and resources (CFN plus SAM). AWS SAM CLI to build SAM apps, local testing, and deployment into AWS. SAM apps have a defined structure with a defined template format defining application resoruces. SAM-package packages a SAM application. It takes local assets and build a ZIP file that is uploaded to S3. `Sam-deploy.yaml` template is returned which references the uploaded assets. Sam-deploy deploys infrastructure using packaged resources. CloudFormation Stack deploys SAM resources.
