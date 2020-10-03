# Data Analytics

## Table of Contents

1. Kinesis
* [Kinesis Data Streams](#kinesis-data-streams)
* [Kinesis Data Firehose](#kinesis-data-firehose)
* [Kinesis Data Analytics](#kinesis-data-analytics)

2. [MapReduce 101](#mapreduce-101)
3. [Elastic MapReduce EMR Architecture](#elastic-mapreduce-emr-architecture)
4. [Redshift Architecture](#redshift-architecture)
5. [Redshift Resilience and Recovery](#redshift-resilience-and-recovery)
6. [AWS Batch](#aws-batch)
7. [Amazon QuickSight](#amazon-quicksight)


## Kinesis Data Streams

> You can use [Amazon Kinesis Data Streams](https://docs.aws.amazon.com/streams/latest/dev/introduction.html) to collect and process large streams of data records in real time. You can create data-processing applications, known as Kinesis Data Streams applications. A typical Kinesis Data Streams application reads data from a data stream as data records. These applications can use the Kinesis Client Library, and they can run on Amazon EC2 instances. You can send the processed records to dashboards, use them to generate alerts, dynamically change pricing and advertising strategies, or send data to a variety of other AWS services. 

Kinesis is a scalable streaming service where producers send data into a Kinesis stream. Streams can scale from low to near infinite data rates. Public service and HA. Streams store a 24 hour rolling window of data, and multiple consumers can access data at a time. A Kinesis stream is composed of individual shards (1 MB for ingestion, 2 MB for consumption), and it is possible to pay more to increase the 24 hour window up to 7 days.

Kinesis Data Records of 1 MB are stored across shards. Kinesis Data Firehose can move streaming data into another service like EMR.

Intead of Kinesis, use SQS for workables, decoupling, asynchronous communication. SQS has one production group and one consumption group. SQS has no persistence of messages or time window. Kinesis allows for huge scale ingestion of data for multiple consumers across a rolling window. It is also used for analytics, monitoring or app clicks.


## Kinesis Data Firehose

> [Amazon Kinesis Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html) is a fully managed service for delivering real-time streaming data to destinations such as Amazon Simple Storage Service (Amazon S3), Amazon Redshift, Amazon Elasticsearch Service (Amazon ES), Splunk, and any custom HTTP endpoint or HTTP endpoints owned by supported third-party service providers, including Datadog, MongoDB, and New Relic.

Kinesis Data Firehose is a fully managed service to load data for data lakes, data stores and analytics services. It includes automatic scaling, fully serverless environments, and it is fully resilient. Firehose is near real time (~60 seconds) unlike Kinesis which is real-time (~200 ms).

Firehose supports the transformation of data using Lambda, but this can cause latency. Billing is based on volume sent through firehose.

Producers can send records to data streams or directly to Firehose which can also read from a data stream as a consumer. Delivery when buffer size in MB (1) fills or buffer interval of 60 seconds passes. If transforming data, it can store the raw data in backup bucket and send transformed records to Elasticsearch or destination bucket. Redshift has to copy the transformed data from a bucket through firehose.


## Kinesis Data Analytics

Kinesis Data Analytics allows for real time processing of data using SQL and ingests from Kinesis Data Streams or Firehose. Destinations include Firehose, Lambda (real-time), and data-streams (real-time). The input stream must always match the source stream. Reference tables contain static data that can be used to enrich the streaming input.

Application code processes input and produces output streams that feed into Kinesis Stream or Firehose. Errors generated from processing are stored in an in-application error stream.

Use cases: Streaming data that needs real-time SQL processing, time-series analytics (elections or e-sports), real-time dashboard (leaderboards), real-time metrics (security and response teams).

## MapReduce 101

MapReduce is a process for huge scale parallel processing of large data sets with two main phases: map and reduce. Mapreduce can also optionally combine and partition. The data is first separated into splits and each is assigned to a mapper. It performs operations at scale, and it then recombines data into results.

Splits are based on the split size and are assigned to mappers (this amount is only limited by the number of mappers). Mapping outputs are shuffled to consolidate records. Shuffle outputs are aggregated by the Reduce phase. Reduce outputs are brought together for a final output deliver to the user of the system.

Hadoop File System (HDFS) is available in MapReduce products. It is traditionally stored across multiple nodes. HDFS is highly fault tolerant and replicated between nodes. The name node provides the namespace for the file system and controls access to the HDFS. A block segment of data on HDFS is generally 64 MB.


## Elastic MapReduce EMR Architecture

> [Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html) is a managed cluster platform that simplifies running big data frameworks, such as Apache Hadoop and Apache Spark, on AWS to process and analyze vast amounts of data. By using these frameworks and related open-source projects, such as Apache Hive and Apache Pig, you can process data for analytics purposes and business intelligence workloads. Additionally, you can use Amazon EMR to transform and move large amounts of data into and out of other AWS data stores and databases, such as Amazon Simple Storage Service (Amazon S3) and Amazon DynamoDB.

EMR is the AWS implementation of Apache Hadoop, Spark, HBase, Presto, Flink, Hive, and Pig. It can be operated long-term or for ad-hoc use (transient) clusters. It runs in one AZ in a VPC using EC2 for computing its nodes. It automatically scales with spot, instance fleet, reserved, and on-demand options. EMR can load data from and back into S3.

Each cluster has at least 1 node--the master node. The master node manages the cluster and its health. It distributes workloads and acts as the name node within MapReduce. Can only SSH to the cluster through the master node. Clusters can have zero or more core nodes. They act as data nodes for HDFS, and they run task trackers and can run mapping and reduce tasks in the cluster.

HDFS is storage linked ot the lifecycle of the cluster. It is managed by the core nodes and can fail if the core nodes fail. It is not regionally resilient because all nodes are within one AZ. Task nodes are optional and have no HDFS involvement. They do not run task trackers, just the tasks. Task nodes are ideal for spot-based scaling.

EMRFS is a resilient file system and is supported natively within EMR. It is backed by S3 and is regionally resilient. It persists past the life-time of the cluster and is resilient to core node failure. If spot scaling is used for the master, then use spot for everything. Spot should only be used for non-critical workloads. In most cases, on-demand and reserved should be used for master and core nodes.


## Redshift Architecture

> [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/welcome.html) is a fully managed, petabyte-scale data warehouse service in the cloud. You can start with just a few hundred gigabytes of data and scale to a petabyte or more. This enables you to use your data to acquire new insights for your business and customers.
Use cases: big data processing, manipulation, analytics, indexing, transformation and more.

Amazon Redshift is a petabyte-scale data warehouse (long-term data processing) and is OLAP.

(OLAP is column based for complex queries of aggregated, historical data. OLTP is row/transaction and is real time.)

Pay as you use service, similar structure to RDS. Direct Query S3 through Redshift Spectrum. Direct Query other databases using federated query. It integrates with AWS tooling (e.g. Quicksight) and has a SQL-like interface for JDBC/ODBC connections. It is server based, so it would be better to use Athena for ad-hoc processing.

* One AZ in a VPC - network cost/performance
* Leader Node - query input, planning and aggregation
* Compute Node - performing queries of data
* Nodes have slices. Data is replicated to one additional node.

Redshift also uses VPC Security, IAM Permissions, KMS at rest encryption, and CW Monitoring. Redshift Enhanced VPC Routing can make use of VPC Networking. Automatic snapshots to S3 (8 hours, 5 GB) with retention or manual snapshots managed by administrator. Snapshots are configured to copy to another AWS region.

### Redshift Resilience and Recovery

Redshift has automatic incremental backups that occur every 8 hours or every 5 GB of data by default. It has one day retention, but this can this be configured to last up to 35 days. Manual snapshots can be taken at any time and deleted by an admin as required. Redshift backups into S3 to protect against AZ failure. It can configure Redshift to copy data across a region.

## AWS Batch

> [AWS Batch](https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html) enables you to run batch computing workloads on the AWS Cloud. Batch computing is a common way for developers, scientists, and engineers to access large amounts of compute resources, and AWS Batch removes the undifferentiated heavy lifting of configuring and managing the required infrastructure, similar to traditional batch computing software. This service can efficiently provision resources in response to jobs submitted in order to eliminate capacity constraints, reduce compute costs, and deliver results quickly.

AWS Batch is a batch processing compute service for jobs that can run without end user interaction or can be scheduled to run as resources permit (usually resource heavy). It handles underlying compute and orchestration while the user defines the job. It is best used as a replacement for Lambda when compute resources are oto big.

Terms:
* Job: Script, executable, or docker container submitted to Batch. The thing that is run. It can be dependent on other jobs.
* Job Definition: Metadata for a job, including IAM permissions, resource configuration, mount points.
* Job Queue: Jobs are submitted to a queue where they wait for compute environment capacity.
* Compute Environment: Managed or unmanaged compute. User can configure instance type/size, vCPU amount, spot price, or price details on a managed compute environment (ECS).

Batch receives jobs from EventBridge, Lambda, Step Functions, or even S3. Jobs are added to queues. Compute environment loads and uses a container image to complete job. Output data is stored in S3 and metadata in DynamoDB. Further execution steps can be sent to Step Functions or AWS Batch Events can be sent to AWS Batch Event Stream.

Batch is not serverless; it uses docker with any runtime. No time limit or effective resource limit, unlike Lambda. Best for event-driven workloads with flexible compute. AWS Batch manages capacity based on workload needs. On-demand or spot instances with a size or type. Unmanaged compute environment requires much more manual administration and is best for tightly controlling costs with an already built environment.

## Amazon QuickSight

> [Amazon QuickSight](https://docs.aws.amazon.com/quicksight/latest/user/welcome.html) is a business analytics service you can use to build visualizations, perform ad hoc analysis, and get business insights from your data. It can automatically discover AWS data sources and also works with your data sources. Amazon QuickSight enables organizations to scale to hundreds of thousands of users, and delivers responsive performance by using a robust in-memory engine (SPICE).

Amazon QuickSight is a BA/BI visualization, ad-hoc analysis, and dashboard tool that integrates with AWS and external data sources. It supports:
* Athena
* Aurora
* Redshift
* Redshift Spectrum
* S3
* AWS IOT
* JIRA
* GitHub
* Twitter
* SalesForce
* Microsoft MySQL Server
* MySQL
* PostgreSQL
* Apach Spark
* Snowflake
* Presto
* Teradata

