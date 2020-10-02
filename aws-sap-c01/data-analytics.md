# Data Analytics

## Table of Contents

Kinesis
* [Kinesis Data Streams](#kinesis-data-streams)
* [Kinesis Data Firehose](#kinesis-data-firehose)
* [Kinesis Data Analytics](#kinesis-data-analytics)

[MapReduce 101](#mapreduce-101)
[Elastic MapReduce EMR Architecture](#elastic-mapreduce-emr-architecture)
[Redshift Architecture](#redshift-architecture)
[Redshift Resilience and Recovery](#redshift-resilience-and-recovery)
[AWS Batch](#aws-batch)
[Amazon QuickSight](#amazon-quicksight)


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

