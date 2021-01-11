# Monitoring, Logging and Cost Management

## CloudWatch

>[Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) monitors your Amazon Web Services (AWS) resources and the applications you run on AWS in real time. You can use CloudWatch to collect and track metrics, which are variables you can measure for your resources and applications.

CloudWatch focuses on the ingestion, storage and management of metrics. It is a public service with public space endpoints. It includes AWS Service and agent integration. On-premises or application integration is available via Agent/API. Can view data via console UI, the CLI, API, or dashboards. Alarms react to metrics and can either notify or perform actions.

Terms:
* Namespace: Container for metrics (AWS/EC2 & AWS/Lambda)
* Datapoint: Timestamp, value, unit of measure
* Metric: Time-ordered set of data points (EC2: CPUUtilization, NetworkIn, DiskWriteBytes)

Every metric has a MetricName and a NameSpace. Dimension is a name/value pair.

Syntax: `ASGName - ImageID - InstanceID - InstanceType`

CloudWatch data has a resolution with a standard 60 second granularity that be adjusted down to 1 second.

Retention:
* Sub 60 second -> For 3 hours
* 60 seconds -> For 15 days
* 300 seconds -> For 63 days
* 3600 seconds -> For 455 days

Statistic aggregation over a period is also available including minimum, maximum, sum, avergae, and percentiles. Namespaces are conatiners and provide isolation for metrics. Every datapoint has a timestamp, value, and can have a unit of measure.

## CloudWatch Logs

>[CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) enables you to centralize the logs from all of your systems, applications, and AWS services that you use, in a single, highly scalable service.

There two sides to CloudWatch Logs: ingestion and subscription. It is a public service that stores and monitors access logging data across a fleet. Supports AWS Services, on-premises, IOT, or any applications. Install CWAgent for system or custom applicatoin logging.

CW Logs can ingest VPC Flow Logs, CloudTrail (Account events and AWS API Calls), Elastic Beanstalk, ECS Container Logs, API GW, Lambda execution logs. Route 53 logs DNS requests.

Log Events (timestamp and message) are collected into Log Streams (each stream shares one source). Streams are collected into Log Groups that store data indefinitely but can have their retention, permissions, and encryption configured. Can manually export data to an S3 bucket that can be encrypted with S3 encryption, but it will not be real time.

CloudWatch Logs subscription only helpful if real-time data is not important (Kinesis Data Firehose provides better real-time reporting). Alternatively, it is possible to use AWS Lambda functions to deliver CW Logs subscription to Elastisearch in real time, or one can use a custom Lambda function to export data to any other destionation in real time. Another alternative is to export to Kinesis data streams and integrate applications iwth that.

* Subscription filters can be used to aggregate data into a single Kinesis data stream.
* CloudWatch Logs should be default for any log management scenarios both on AWS and on-premises.
* Export to S3 with `CreateExportTask`, takes 12 hours.
* Metric filter can scan log data or generate a CloudWatch metric


## CloudTrail

> [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail. Events include actions taken in the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs.

CloudTrail logs API calls as a CloudTrail Event which is by default stored for 90 days in Event History. CloudTrail is enabled by default. The user can create trails to customize the service, management events, and data events. IAM, STS, CloudFront log their events to Global Service Events in `us-east-1`. Not real-time, provides updates multiple times an hour.


## AWS X-Ray

> [AWS X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) is a service that collects data about requests that your application serves, and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization. For any traced request to your application, you can see detailed information not only about the request and response, but also about calls that your application makes to downstream AWS resources, microservices, databases and HTTP web APIs.

AWS X-Ray can be used to analyze and debug production, distributed applications espeically those built with a microservices architecture.

Terms:
* Tracing header: First service generates its unique trace ID which tracks a request through the distributed application.
* Segments: Data blocks - Host/IP, Request, Response, Work Done (time), Issues.
* Subsegements: More granular version of segments, calls to other services.
* Service graph: JSON document detailing services and resources which make up the application.
* Service map: Visual version of service graph. Shows the flow through a distributed app alongside response time, requests and any errors or issues.

EC2 has an X-Ray Agent installed. ECS has an Agent in tasks. Lambda needs the option enabled. Beanstalk comes with the agent pre-installed. API Gateway has a per stage option. SNS & SQS. X-Ray requires IAM permissions.


## Cost Allocation Tags

Cost allocation tags are a key/value label assigned to an AWS resource that can be used to organize and track AWS costs on a detailed level. Cost allocation tags have to be enabled individually and can be either AWS-Generated or User-defined. They are added to resources after they are enabled by AWS. Both are visible in Cost Reports and can be used as a filter. They can take up to 24 hours to be visible, and they are not retroactive.

## AWS Trusted Advisor

> [AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/trusted-advisor.html) draws upon best practices learned from serving hundreds of thousands of AWS customers. Trusted Advisor inspects your AWS environment, and then makes recommendations when opportunities exist to save money, improve system availability and performance, or help close security gaps. All AWS customers have access to five Trusted Advisor checks. Customers with a Business or Enterprise support plan can view all Trusted Advisor checks.

Trusted Advisor provides real-time guidance to provision resources using AWS best practices. It operate at the account level and does not need any agents installed. It provides cost optimization, performance, security, fault tolerance, and service limits.

Seven core checks with Basic/Developer plans:
1. S3 Bucket Permissions
2. Security Groups
3. IAM Use
4. MFA on Root Account
5. EBS Public Snapshots
6. RDS Public Snapshots
7. 50 service limit checks

Business or enterprise plans add:
* 115 further checks (14 cost, 17 security, 24 fault tolerant, 10 performance, and 50 service limit)
* Access via the AWS Support API. Can get the names and identifiers for Trusted Avisor. Programmatic summaries, status checks, and more are available. Can code applications to use Trusted Advisor.

Trusted Advisor can be integrated with CloudWatch to react to changes.
