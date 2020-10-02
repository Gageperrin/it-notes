# Databases

## Table of Contents
[RDS Architecture](#rds-architecture)
* [RDS Multi AZ](#rds-multi-az)
* [RDS Backups and Restores](#rds-backups-and-restores)
* [RDS Read Replicas](#rds-read-replicas)
* [RDS Data Security](#rds-data-security)

[Aurora Architecture](#aurora-architecture)
* [Aurora Serverless](#aurora-serverless)

[DynamoDB Architecture](#dynamodb-architecture)
* [DynamoDB Operations, Consistency and Performance](#dynamodb-operations,-consistency-and-performance)
* [DynamoDB Indexes LSI and GSI](#dynamodb-indexes-lsi-and-gsi)
* [DynamoDB Streams and Triggers](#dynamodb-streams-and-triggers)
* [DynamoDB Accelerator DAX](#dynamodb-accelerator-dax)
* [DynamoDB Global Tables](#dynamo-global-tables)

[Elasticsearch](#elasticsearch)
[Athena](#athena)
[Amazon Neptune](#amazon-neptune)
[Amazon Quantum Ledger Database QLDB](#amazon-quantum-ledger-database-qldb)

## RDS Architecture

>[Amazon Relational Database Service](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) (Amazon RDS) is a web service that makes it easier to set up, operate, and scale a relational database in the AWS Cloud. It provides cost-efficient, resizable capacity for an industry-standard relational database and manages common database administration tasks.

RDS is a DatabaseServer-as-a-Service, a managed database instance. It supports MySQL, MariaDB, PostgreSQL, Oracle, Microsoft SQL Server and Amazon Aurora. RDS instances are the building blocks and can contain multiple user created databases. Can access the database using the CNAME record.

Types:
* `db.m5` is best for general purpose
* `db.e5` is best for memory
* `db.t3` is best for burst

Block storage is allocated to the instance as io1, gp2, or magnetic (for compatibility). Billing is based on allocated GB/m.

### RDS Multi AZ

RDS Access can only be accessed via CNAME and only points to the primary instance. The standby cannot be directly used. **Synchronous replication** to the replica.

Stages:
1. Database write via CNAME.
2. Disk write primary to local storage.
3. Replicated to standby.
4. Disk write secondary.

There is no free-tier RDS MultiAZ. 60-120 second failover, so not very fault tolerant. **Same region only.** Backups are taken from standby in case of AZ outage, primary failure, manual failover, instance type change, and software patching.

### RDS Backups and Restores

RPO is time between last back up and incident (before).
RTO is time between incident and full recovery (after).

The first RDS snapshot is full, every subsequent snapshot is incremental. RDS restore creates a new RDS instance with a new address. Snapshots restore to a single point in time (RPO suboptimal) while restores are not very fast (RTO suboptimal). Snapshots backup is automated for any 5 minute point in time. Backup is restored and transaction logs are replayed to bring DB to desired point in time.

### RDS Read Replicas

**Asynchronous** replication with performance improvements. 5x direct read-replicas per DB instances. Each replica provides an additional instance of read performance. Read-replicas can have second-order read-replicas but lag starts to cause problems. Read-replicas provide global performance and resilience improvements. 

Snapshots and backups improve RPO but still have problematic RTO. Read-Replicas offer near zero RPO and because they can be promoted quickly, they have low RTO. However it is failure only and can have data corruption. Replicas are read only until they are promoted.

### RDS Data Security

SSL/TLS in transit is available for RDS and can be made mandatory. By default, encryption at rest is provided by EBS volume encryption with KMS. This is handled by HOST/EBS and AWS or Customer Managed CMK generated data keys. Storage, logs, snapshots, and replicas are encryped. This encryption cannot be removed.

RDS supports MSSQL and Oracle Support TDE (Transparent Data Encryption). Encryption is handled within the DB engine. RDS Oracle supports integration with CloudHSM, removing AWS from the chain of trust and boosting security strength. Encryption is handled within the DB engine. TDE is native DB engine encryption.

KMS provides AWS CMKs used to generate DEKs for RDS. DEKs are loaded into hosts as required. Data is encrypted and decrypted by the host. RDS Local DB Account is configured to use AWS Authentication Tokens. The policy attached to the Users or Roles maps that IAM identity onto the local RDS user. Authorization is controlled by the DB Engine. IAM only provides authentication.

## Aurora Architecture

> [Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html) (Aurora) is a fully managed relational database engine that's compatible with MySQL and PostgreSQL. You already know how MySQL and PostgreSQL combine the speed and reliability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases. The code, tools, and applications you use today with your existing MySQL and PostgreSQL databases can be used with Aurora. With some workloads, Aurora can deliver up to five times the throughput of MySQL and up to three times the throughput of PostgreSQL without requiring changes to most of your existing applications.

Aurora Provisioned architecture is very different and uses a cluster. There is a single primary instance with zero or more replicas. It has no local storage and uses cluster volumes. Replication does not cause performance issues. It is all SSD based (high IOPS, low latency). Storage is billed based on what is used. The high water mark means it is billed for what is most used. Storage which is freed up can be re-used.

Aurora uses endpoints. At minimum, there is the cluster endpoints (for primary Read and Write operations) and reader endpoitns for replicas that can do read operations.

Billing: No free-tier option since Aurora does not support micro RDS instances. If moving beyond RDS single AZ micro instances, Aurora offers better value. Computer hourly charge per second, storage GB/month sonumed, I/O cost per request. 100% DB Size in backups are included in the cost.

Aurora backups work the same way as RDS. Restores create a new cluster. Backtrack can be used which allow in-place rewinds to a previous point in time. Fast clones make a new database much faster than copying all the data, copy-on-write.

### Aurora Serverless

>With [Aurora Serverless](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.how-it-works.html), you can create a database endpoint without specifying the DB instance class size. You set the minimum and maximum capacity. With Aurora Serverless, the database endpoint connects to a proxy fleet that routes the workload to a fleet of resources that are automatically scaled. Because of the proxy fleet, connections are continuous as Aurora Serverless scales the resources automatically based on the minimum and maximum capacity specifications. Database client applications don't need to change to use the proxy fleet. Aurora Serverless manages the connections automatically. Scaling is rapid because it uses a pool of "warm" resources that are always ready to service requests. Storage and processing are separate, so you can scale down to zero processing and pay only for storage.

Aurora Serverless is scalable based on ACUs (Aurora Capacity Units). A cluster has a minimum and maximum ACU capacity, and it adjusts based on the load. Can go to zero ACUs and be paused. Consumption billing per-second basis. Serverless has same resilience as Aurora.

A proxy fleet of instances brokers the relationship between the application and the ACUs, so the user is only billed for ACUs used at any given point in time.

Use cases: Infrequently used applications, new applications, variable workloads, development and test databases, multi-tenant applications


## Amazon DynamoDB Architecture

> [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. DynamoDB lets you offload the administrative burdens of operating and scaling a distributed database so that you don't have to worry about hardware provisioning, setup and configuration, replication, software patching, or cluster scaling. DynamoDB also offers encryption at rest, which eliminates the operational burden and complexity involved in protecting sensitive data.

DynamoDB is a NoSQL Public DBaaS with key/value & document structure. No self-managed servers or infrastrucutre. Manual/automatic provisioned performance in/out or in-demand. It is highly resilient across AZs and potentially globally. Very fast since it is SSD based (single-digit millisecond speed). Offers backups, point-in-time recovery, and encryption at rest. It also includes event-driven integration.

DynamoDB tables are the building block of this service. A table is a grouping of items with the same priamry key and can either have a Simple (Partition) or Composite (Partition and Sort) Primary Key. Each item must have a unique value for Primary Key and Secondary Keys. No rigid attribute schema with DynamoDB.

Capacity speed is set on a table. Each item can have a max of 400 KB:
* 1 WCU = 1 KB/s
* 1 RCU = 4 KB/s

On-demand backups include a full copy of the table that is retained until the backup is removed. Backup maintenance and removal is manual. Point-in-time recovery is not enabled by default. It offers a 35 day recovery window with 1 second granularity.

DynamoDB can be accessed via the UI console, CLI and API `NO SQL`.

Billing: Based on RCUs, WCUs, storage, and features used.

### DynamoDB Operations, Consistency and Performance

On-Demand DynamoDB is best for unknown or unpredictable workloads with low admin overhead. Price is et per million RCUs or WCUs. Provisioned DynamoDB is where RCU and WCU is set on a per table basis. Every operation consumes at least 1 RCU or WCU (one 4KB read operation /s and one 1KB write operation /s respectively). Every table has a RCU and WCU burst pool of 300 seconds.

Query operation: query for a single partition key value where the capacity consumed is the size of all returned items. Most efficient operation in most cases.
Scan operation: A scan moves trhougha  table, consuming the capacity of every item. Complete control on what data is selected, any attributes and filters can be used.

When a change is made, the write is made to the leader storage node. The leader node then replicates the change to other nodes within milliseconds. Eventually consistent reads check 1/3 nodes. However, stale data is possible with this method, but it is 50% of the cost of a strongly consistent configuration. Strongly consistent reads read directly from the leader node. It should be noted that note every application or access type tolerates eventual consistency.

### DynamoDB Indexes (LSI and GSI)

LSI and GSI are Local Secondary Indexes and Global Secondary Indexes respectively.

The query operation is very efficient usually, but it can only work on 1 PK value at a time. Indexes are an alternative view on the table database. It can use a different sort key (LSI) or different partittion key and different sort key (GSI). Some or all attributes are proejcted onto them.

LSI must be created with a table with a maximum of 5 LSIs per base table. Has an alternative PK. It shares the RCU and WCU with the table. Attributes: `ALL`, `KEYS_ONLY`, and `INCLUDE`.

GSI can be created at any time with a default limit of 20 per base table. Alternative PK and SK. GSIs ahve their own RCU and WCU allocation. GSIs are sparse, so only items which have values in the new PK are added.

It is important to be careful with projections; queries on attributes which are not projected are expensive.

Use cases: GSI by default, LSI only when strong consistency is required. Use indexes for alternative access patterns.

### DynamoDB Streams and Triggers

DynamoDB streams are a time-ordered list of **item changes** in a table with a 24 hour rolling window. It is enabled on a per table basis and records inserts, updates, and deletes.

View options:
* `KEYS_ONLY`
* `NEW_IMAGE`
* `OLD_IMAGE`
* `NEW_AND_OLD_IMAGES`

Item changes generate an event containing the data which was changed. An action is taken using that data.

* DynamoDB streams is often paired with Lambda
* Good for reporting and analytics.
* Can be used for aggregation, messaging, and notifications

### DynamoDB Accelerator (DAX)

DynamoDB Accelerator (DAX) is a cluster-based feature running in a VPC with multiple nodes to accelerate queries with enhanced caching.

A traditional cache is based on a cache hit or miss. If an application can't find its data in the cache, it is a miss. The data is then loaded from the database with a separation operation and SDK. The cache is then updated with the retrieved data, and subsequent queries will load that data from the cache when queried.

With DAX, the application uses the DAX SDK to make a single call for the data. DAX will return data either from the cache or, failing that, the database with just one call. DAX is accessed via an endpoint. A cache hit is returned in microseconds, a cache miss in microseconds.

Write-through is supported, so data is written to DynamoDB then DAX. If a cache miss occurs, the data is also written to the primary node fo the cluster. Primary Node (Wries) and Replicas (Read). Nodes are HA. Much faster reads at reduced costs. Can scale up and out. Most caches do not support write-through like DAX. DAX is not a public service as it must be dpeloyed in a VPC.

Use cases: Do not use for strongly consistent reads or write-heavy operations.

### DynamoDB Global Tables

DynamoDB global tables provide multi-master cross-region replication. For conflict resolution, the last writer wins. Reads and Writes can occur in any region, and there is sub-second replication between regions. Strongly consistent reads only in the same region. Replication is sub-second. Global eventual consistency, HA, and DR/BC.


## Elasticsearch

> [Amazon Elasticsearch Service](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/what-is-amazon-elasticsearch-service.html) (Amazon ES) is a managed service that makes it easy to deploy, operate, and scale Elasticsearch clusters in the AWS Cloud. Elasticsearch is a popular open-source search and analytics engine for use cases such as log analytics, real-time application monitoring, and clickstream analysis.

Elasticsearch is an open source search product used for ELK Stack, ES, Kibana, and Logstash. It is not serverless and runs in a VPC using compute. It is an alternative if already using ES or ELK.

* ELK Stack: ES - Search and indexing
* Kibana: Visualization and dashboard
* Logstash: Similar to CloudWatch Logs but needs a Logstash agent

Can be integrated with Kinesis Data Streams, DynamoDB Streams, and S3 Buckets through Lambda events.


## Athena

> [Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/what-is.html) is an interactive [serverless] query service that makes it easy to analyze data directly in Amazon Simple Storage Service (Amazon S3) using standard SQL. With a few actions in the AWS Management Console, you can point Athena at your data stored in Amazon S3 and begin using standard SQL to run ad-hoc queries and get results in seconds.

Athena runs ad-hoc queries on data that only charges for consumed data. Schema-on-read with table-like translation. Original data is never changed and remains on S3. The schema translates the data with a relational-like read. The output can be sent to other services. It supports standard format of structured, semi-structured, and unstructured data. The source data is also stored on S3. **Data is fixed, not transformed.**

Athena can directly read from many AWS data formats such as CloudTrail, ELB Logs, and Flow Logs. Tables are defined in advance in a data catalog, and the data is project through when read. It allows SQL-like queries on data without transforming the source data.

Use cases: Queries that do not need loading or transformation. Occasional ad-hoc queries on data stored in S3. Serverless querying scenarios that are cost-conscious. Querying AWS logs. AWS Glue Data Catalog and Web Server Logs.

With Athena Federated Query, the data source connector is read to other data sources.


## Amazon Neptune

> [Amazon Neptune](https://docs.aws.amazon.com/neptune/latest/userguide/intro.html) is a fast, reliable, fully managed graph database service that makes it easy to build and run applications that work with highly connected datasets. The core of Neptune is a purpose-built, high-performance graph database engine. This engine is optimized for storing billions of relationships and querying the graph with milliseconds latency. Neptune supports the popular graph query languages Apache TinkerPop Gremlin and W3C’s SPARQL, enabling you to build queries that efficiently navigate highly connected datasets. Neptune powers graph use cases such as recommendation engines, fraud detection, knowledge graphs, drug discovery, and network security.

Amazon Neptune is a managed graph database service for databases where relationships are as important as the data. Neptune runs in a VPC and is private by default. It is multi-AZ and scales via read replicas. Continuous backups to S3 with a good point-in-time recovery. It is like RDS but for **graph style data**.

Nodes are entities within a GraphDB that can have attributes and tags. Relationships are directional and named.

Use cases: social media (fluid relationships), fraud prevention, recommendation engines, network and IT operations, life sciences, etc.


## Amazon Quantum Ledger Database QLDB

> [Amazon Quantum Ledger Database](https://docs.aws.amazon.com/qldb/latest/developerguide/what-is.html) (Amazon QLDB) is a fully managed ledger database that provides a transparent, immutable, and cryptographically verifiable transaction log owned by a central trusted authority. You can use Amazon QLDB to track all application data changes, and maintain a complete and verifiable history of changes over time.

Amazon Quantum Ledger Database is an immutable append-only ledger based database with a cryptographically verifiable transaction log. It is transparent with a full history and is always accessible. It is perfect for audit-heavy workloads. It is also serverless.

* QLDB has 3 AZ resilience and replication within each AZ.
* Can stream data to Amazon Kinesis
* Has a document DB model
* ACID compliant, either both or none of the two transactions are executed
* QLDB offers a current data state and history of all past data states. It can batch export to S3 or near real-time streaming to Kinesis.
* Blocks are chained together in sequence using a hash of the current data and the previous hash. Every chagne in data is cryptographically verifiable through a block chain.

Use cases: Finance, medical, logistics, or legal work

