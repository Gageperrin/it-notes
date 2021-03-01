# AWS Certified Database Specialty 

These are notes compiled from multiple resources including:
* Chad Smith's AWS Certified Database Specialty Complete Video Course

## Exam Domains

* Workload-Specific Database Design: 26%
* Deployment and Migration: 20%
* Management and Operations: 18%
* Monitoring and Troubleshooting: 18%
* Database Security: 18%

## Workload-Specific Database Design

Key terms:
* ACID - Atomicity, Consistency, Isolation, and Durability
* OLTP - Online Transaction Processing
* OLAP - Online Analytical Processing

### Database Service Summary

Relational Database Service (RDS) is the most frequently mentioned service on the exam. It is AZ scoped with 1 write endpoint. It is used for OLTP, e-commerce, big data workloads, data lake ingestion, and monolithic DB applications. RDS supports MySQL, Microsoft SQL Server, Oracle DB, Postgres, and MariaDB.

Aurora includes AZ-scoped and region-scoped endpoints with 2 write endpoint options and a serverless option. It also includes a global DB option. It is also used for OLTP workloads, e-commerce, but also complex data sets, big data workloads, and a mobile app back end. MySQL and Postgres are the only DB engines supported by Aurora.

Redshift is AZ scoped with cluster nodes within a single AZ. It has one endpoint, can take distributed queries and can extend queries to S3 or RDS. It is used for OLAP, big data workloads, data lake ingestion, and columnar data. It supports Postgres. 

DynamoDB is region scoped with a managed NoSQL offering. It is designed around key value pairs and is serverless. It also offers global tables and ACID support (unlike most NoSQL offerings). DynamoDB provides instant scaling as well. It is used for click tracks, session data, time series data, and document storage.

Elasticache (with Redis) is AZ scoped, managed in-memory with 1 write endpoint, and a sharding option. It is used for leaderboards, pub/sub messaging, as well as recommendation data.

DocumentDB is AZ scoped with a document store and 1 write endpoint. It is mainly used for MongoDB compatibility.

Neptune is AZ scoped and focuses on graph data with 1 write endpoint and a multi-AZ option. It can be used for knowledge graphs, fraud detection, and machine learning.

KeySpaces is only region scoped and serverless. It is designed for wide column store and used for its Cassandra compatibility as well as user profiles.

Timestream is region scoped time series data serverless service. It does this with extremely high throughput and low latency. It is used for IoT, operational apps, and anything requiring near real time analysis.

QLDB is AWS' only blockchain database service. It is region scoped, serverless, and covers ledger data. It is mainly used for financial ledgers, medical data, and proof of transaction.

Database Migration Service (DMS) is a managed service for heterogeneous migration. It consists of a one-time full load and then a Change Data Capture phase. 

Schema Migration Tool is a locally run application for heterogeneous migration. It can be used on its own or combined with DMS.

Native DB tools can also be run locally for homogeneous migration depending on circumstantial needs.

#### DB Service Decision Tree ####

1. Is there a pre-defined schema? 
Yes: RDBMS
No: NoSQL

If yes to last...
2a. OLAP?
Yes: Redshift
No: OLTP

If no to last...
3a. Immutable data change history?
Yes: QLDB
No: Other RDBMS

If no to last...
4a. Very large database (>32-64 TB)
Yes: Aurora
No: 3rd party RDBMS

If no to last...
5a. OS/Software customization?
Yes: EC2
No: RDS

If no to Q1...
2b. In-memory data?
Yes: Elasticache
No: Others

If no to last...
3b. Document store?
Yes: DocumentDB
No: Others

If no to last...
4b. Graph data?
Yes: Neptune
No: Others

If no to last...
5b. Time-series data?
Yes: Timestream
No: Others

If no to last...
6. Wide column data?
Yes: KeySpaces
No: DynamoDB

### DR and HA Design

#### RDS ####

RDS resilience can be increased through same-region replicas or through cross-region read replicas (cross region is not available for SQL server). If the primary instance fails then queries will failover to the standby. Resizing the instance, patching the OS, and initiate reboot with failover will cause RDS instance failure. Standy promotion takes about 60 seconds.

#### Aurora ####

Aurora uses 10 GB blocks called protection groups. Data in each protection group replicated six times across three availability zones. Data is continuously backed up to S3 for 11x9s durability. Writes require 4 out of 6 storage nodes to act. Reads find closest storage node, and scaling happens by provisioning more protection groups. 

Aurora can withstand loss of AZ with no availability impact as well as loss of 2 storage nodes. Because of continuous S3 backups, no backup windows are needed. Aurora fails over with existing replica and flips the DNS CNAME, promotes the replicas in 30 seconds. If multiple replicas are present, the customer can set replica priority and proomotion tiers. Failover with Aurora serverless launches replacement in a new AZ. If no replicas are available and not running serverless, Aurora attempts to create new DB isntances in teh same AZ as the primary on a best-effort basis.

Aurora offers a backtrack figure that be configured for between 24 and 72 hours. It is a rewind feature for emergencies such as accidentally dropping a table. However, this can only be enabled on cluster creation, only available on MySQL engines, affects the entire cluster not a table or row, is mutually exclusive with cross-region replication, and mutually exclusive with multi-primary clusters (2 write endpoints). To backtrack, pause the application choose the backtrack time, then Aurora pauses the DB, closes connections, drops uncommitted writes, performs backtrack, and then the user unpauses the application.

With Aurora serverless, the database can have up to 488 GB of aggregate memory. This changes teh endpoint to a proxy for database capacity pool. The capacity pool scales according to load by adding discrete units.

#### DynamoDB ####

DynamoDB SLA with a single region has 99.99% availability. The global tables increases availability to 99.999%. This is one data service where the SLA includes both the service API and the data access. In other services, they are separate SLAs. Table data is spread across multiple AZs, so it can sustain the loss of 1 AZ. Global table replication is not instantaneous but eventually consistent.

#### Redshift ####

Redshift is not very resilient but is high performance within a single AZ. It consists of a leader node and its compute nodes. Redshift has no built-in redundancy or failover if any node fails. The cluster becomes unreachable.

A custom solution consists of a Route 53 entry for one Redshift cluster in one AZ and an identical cluster in another AZ also added to Route 53. This will require custom engineering to ensure synchronous inserts into both clusters.

#### Elasticache ####

If there is a single node and the primary node fails, then there is no protection. With read replicas, the read replica with the least lag is promoted to become the new primary instance. A new read replica is provisioned in the same AZ as the failed primary. A multi-AZ complex failover means that read replicas are launched in the same AZ as failed instances, including the old primary. When a complete failure happens, all nodes are launched in the same AZ they failed from. Data can be restored from backup at this point.

Elasticache global datastore provides more resiliency with at least 2 nodes in each cluster, up to 2 passive regions, and where secondary clusters are read only. If the active cluster fails, then manual failover is required. The original active cluster then becomes passive.

#### DocumentDB ####

DocumentDB replicates each cluster volume into a separate AZ. Failure of an entire AZ will not cause any data loss. If the primary node is the only node and fails then the instance is removed entirely as it has no unique storage. It is replaced with a new primary node within a few minutes. No time is required for data load as the storage tier is separate.

If a primary node fails and there are replicas, the primary node is removed and the replica is promoted. This process will respect promotion tiers for order of succession. The old primary is replaced but it is not guaranteed that it will be placed into the same AZ as the old node.


#### Neptune ####

All Neptune nodes are considered replicas with a copy of the data. Neptune stores data in multiple AZs within a region regardless of replica location. With one replica alongside a cluster endpoint and reader endpoint, the reader endpoint can be used for writes without any conflicts.

With two or more replicas, there will be a cluster endpoint, a reader endpoint, and an endpoint for each instance. The reader endpoint uses round robin to direct requests in this scenario.

With one replicas failure, a new primary is created and endpoints are switched to the new replica. Failover with two or more replicas means a failover to another replica that also updates reader endpoints and then replaces the old primary and updates the reader endpoints.

### Database Service Comparisons and Tradeoffs

#### RDS Design ####

RDS performance can be increased with insteance size, read replicas, storage size, and storage type.

Compliance solutions vary based on backups, multi-AZ, read replicas, and snapshots.

Scability is based on instance size, read replicas, storage size, and type.

Cost design can vary based on each aspect of RDS: backups, instance size, multi-AZ, read replicas, snapshots, storage size, and storage type.

#### Aurora Design ####

Aurora design considerations:
* Performance: Instance size, read replicas, serverless (not storage size)
* Compliance: Backups, RRs, and snapshots
* Scalability: Instance size, read replicas, and serverless
* Cost: Backups, instance size, RRs, serverless, snapshots, and storage size (all features)

#### DynamoDB Design ####

DynamoDB design considerations:
* Performance: Auto scaling, DAX, global table, instant scaling, and provisioned capacity
* Compliance: Backups, global table, and streams
* Scalability: Auto scaling, DAX, global table, instant scaling, provisioned capacity, and table size
* Cost: Auto scaling, backups, DAX, provisioned capacity, restore, streams, table size

#### Redshift Design ####

Redshift design considerations:
* Performance: Number of nodes, concurrency scaling, DB encryption, distribution keys, federated query, node type, and Spectrum
* Compliance: DB encryption and snapshots
* Scalability: Number of nodes, concurrency scaling, federated query, Spectrum
* Cost: Number of nodes, federated query, node type, snapshots, and Spectrum

#### Elasticache Design ####

Elasticache (Redis) design considerations:
* Performance: Cluster mode, global datastore, node size, read replicas, and write endpoints
* Compliance: Backups, global datastore, multi-AZ, and RRs
* Scalability: Cluster mode, global datastore, node size, RRs, and write endpoints.
* Cost: Backups, global datastore, node size, RRs, and write endpoints

#### DocumentDB Design ####

DocumentDB design considerations:
* Performance: Instance size and RRs
* Compliance: Read replicas and snapshots
* Scalability: Instance size and RRs
* Cost: Instance size, RRs, snapshots, and storage size

#### Neptune Design ####

Neptune design considerations:
* Performance: Instance size, RRs
* Compliance: Multi-AZ, RRs, and snapshots
* Scalability: Instance size and RRs
* Cost: Instance size, RRs, snapshots, and storage size

#### EC2 Design ####

EC2 design considerations:
* Performance: Instance size, storage size, and storage type
* Compliance: Backups, inspector, and replicas
* Scalability: Instance size, storage size, and storage type
* Cost: Backups, instance size, replicas, storage size, and storage type

#### Service Term Associations ####

Key words for each service:
* RDS: Oracle, SQL Server, monolithic
* Aurora: Multi-primary, serverless, global database
* DynamoDB: Key-value, partition key, global table
* Redshift: Data warehouse, Spectrum, columnar
* Elasticache: In-memory, cluster mode, and shards
* DocumentDB: MongoDB, replicaset, and collection
* Neptune: SPARQL, Gremlin, and Apache TinkerPop

## Deployment and Migration

### Database Deployment Automation

#### Automation using CloudFormation ####

Each CloudFormation template follows a sequence for provisioning resources in the correct order and proper segregation.

Example: Neptune in three stacks

1. Deploy VPC, Subnets, IGW, NAT Gateways, route tables, and a Neptune subnet group
3. Deploy Neptune cluster with replicas, instance, profiles, security group and S3 load config.
4. Deploy SageMaker notebook with security group, IAM role, and connection configuration.

#### Automation using DMS ####

A DMS automation starts with the source stage. For example, an archive is uploaded to a S3 bucket which contains all of the depenency files and DMS task definitions. The S3 upload triggers a CodeBuild job that manages the rest of the individual tasks for a full data load via DMS. This is an optional step that will create the schema in the destination RDS instance. Once the schema is in place, create the DMS tasks to provision the DMS replication instance. Then it creates an event notification stream with SMS so that outcomes can be monitored. Then the full load task begins and sends any updates to the SNS topic.

The `ApprovalForDMS` stage is a manual approval stage for the process that can still utliize some automation. The startup message is sent via SMS with an approavl token parsed by Lambda. The DMS task sends a notification on completion. 

The PreCDC CodeBuild stage is entirely optional for tasks that require ongoing Change Data capture after the full load. This task creates the object for CDC after the full load. This task creates the objects for CDC and triggers the DMS task. The DMS replication task is resumed and the user is notified via SNS.

#### Automation using CodePipeline ####

### Migration Strategies

#### Rs of Cloud Migration ####

#### AWS Schema Conversion Tool ####

#### AWS Database Migration Service ####

