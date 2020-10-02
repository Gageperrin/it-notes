# Miscellaneous Services

## Table of Contents

[Amazon Lex](#amazon-lex)

[Amazon Connect](#amazon-connect)

[AWS Rekognition](#aws-rekognition)

[Kinesis Video Streams](#kinesis-video-streams)

[AWS Glue](#aws-glue)

[AWS Device Farm](#aws-device-farm)


## Amazon Lex

> [Amazon Lex](https://docs.aws.amazon.com/lex/latest/dg/what-is.html) is an AWS service for building conversational interfaces for applications using voice and text. With Amazon Lex, the same conversational engine that powers Amazon Alexa is now available to any developer, enabling you to build sophisticated, natural language chatbots into your new and existing applications.

Amazon Lex provides text or voice conversation interfaces. It provides automatic speech recognition (ASR) based on natural language understanding (NLU) technologies. It scales, integrates with other services, is quick to deploy, and has pay as you go pricing. It is great for event-driven or serverless architectures.

Use cases: Chatbots, voice assistants, Q&A bots, info/enterprise bots


## Amazon Connect

> [Amazon Connect](https://docs.aws.amazon.com/connect/latest/adminguide/what-is-amazon-connect.html) is an omnichannel cloud contact center. You can set up a contact center in a few steps, add agents who are located anywhere, and start engaging with your customers. You can create personalized experiences for your customers using omnichannel communications.

Amazon Connect is an accessible contact center as a service that can handle both incoming and outgoing voice and chat. It integrates with PSTN networks for traditional voice telephones without needing the physical infrastructure.

Agents can connect to it from anywhere using the Internet. It integrates with other AWS services such as Lambda and Lex for additional intelligence and features.


## AWS Rekognition

> [Amazon Rekognition](https://docs.aws.amazon.com/rekognition/latest/dg/what-is.html) makes it easy to add image and video analysis to your applications. You just provide an image or video to the Amazon Rekognition API, and the service can identify objects, people, text, scenes, and activities. It can detect any inappropriate content as well. Amazon Rekognition also provides highly accurate facial analysis, face comparison, and face search capabilities. You can detect, analyze, and compare faces for a wide variety of use cases, including user verification, cataloging, people counting, and public safety.

Amazon Rekognition makes it easy to add image and video analysis to applications using deep learning (sub-set of Machine Learning). It identifies objects, people, text, activities, inappropriate content, and faces. It can analyze and compare faces, trace pathing for objects, and more.

Billing: Per image or per minute pricing.

Rekognition integrates with applications and is event-driven. It can also analyze live video streams when integrated with Kinesis video streams.


## Kinesis Video Streams

> [Amazon Kinesis Video Streams](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/what-is-kinesis-video.html) is a fully managed AWS service that you can use to stream live video from devices to the AWS Cloud, or build applications for real-time video processing or batch-oriented video analytics.

Amazon Kinesis Video Streams ingests live video data from producers (e.g. security cameras, smartphones, cars, drones, audio, thermal, depth and RADAR data). Consumers can access data frame-by-frame or as needed. It can persist and encrypt both in transit and at rest. It cannot access data directly via storage, only via APIs.

It is often integrated with Rekognition and Connect.


## AWS Glue

> [AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html) is a fully managed ETL (extract, transform, and load) service that makes it simple and cost-effective to categorize your data, clean it, enrich it, and move it reliably between various data stores and data streams. AWS Glue consists of a central metadata repository known as the AWS Glue Data Catalog, an ETL engine that automatically generates Python or Scala code, and a flexible scheduler that handles dependency resolution, job monitoring, and retries. AWS Glue is serverless, so there’s no infrastructure to set up or manage.

AWS Glue is a fully managed **serverless* ETL (extract, transform, load) service. AWS Data Pipeline can do ETL but has to use servers through EMR. Glue moves and transforms data between source and destination. It crawsl data sources and generates the AWS Glue Data catalog.

Supports:
Data source stores: S3, RDS, JDBC Compatible, DynamoDB
Data source streams: Kinesis Data Stream and Apache Kafka
Data target: S3, RDS, JDBC Databases

A data catalog is a collection of persistent metadata about data sources in a region. One catalog per region per account is permitted in order to avoid data silos. Amazon Athena, Redshift Spectrum, EMR, and AWS Lake Formation all use Data Catalog. They configure crawlers for data sources.

Glue job sequence:
1. Data is extracted from a source.
2. The job is executed via a script using an AWS managed warm resource pool.
3. It is loaded into supported data tragets.

Glue jobs can be initiated manually or triggered via EventBridge.

Use cases: Better than Data Pipeline for serverless, ad-hoc, cost-effective ETL jobs.


## AWS Device Farm

> [Device Farm](https://docs.aws.amazon.com/devicefarm/latest/developerguide/welcome.html)is an app testing service that you can use to test and interact with your Android, iOS, and web apps on real, physical phones and tablets that are hosted by Amazon Web Services (AWS).

AWS Device Fram is an application testing service that uses an extensive range of desktop browsers and mobile devices. The tests are conducted on **real** browsers and devices with a wide range of phones, tablets, languages, sizes, and operating systems. It uses built in or supported automated testing frameworks.

The user receives reports detailing testing output. It supports remote connection to devices for issue reproduction and testing.

Device Farm sequence:
1. Create a run.
2. Define test to run (e.g. Explorer for Android, Fuzz for Android and iOS), Web app tests, Appium, or Calabash).
3. Configure which devices tests should be run against.
4. Configure device state, additional apps, radio states, device location and device language. Device remote connection is available as required.


