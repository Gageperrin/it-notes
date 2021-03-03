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

Example: Automate schema updates to an RDS database.

Have a CodeBuild project that can point to a source code repo containing the buildspec file. The pre-change DB schema is stored as an RDS snapshot. The first step in the CodeBuild process is to use the CLI to restore the RDS snapshot to a new instance. The process pauses while the snapshot restore completes before proceeding.

The next step is to validate the restored data, apply the schema changes, and perform app validation. Then validate restored data.If the validation is successful, complete the build and remove the restored DB instance. Then approve the schema change.

### Migration Strategies

#### Rs of Cloud Migration ####

1. Rehost - "lift and shift"
2. Replatform - move into a managed service
3. Repurchase - purchase new software
4. Refactor - re-architect the solution
5. Retire - get rid of an old application
6. Retain - keep as is



#### AWS Schema Conversion Tool ####

1. SCT is not a managed service but is downloaded and run locally.
2. Configure source and target database connections, must be available from the local instance at the same time.
3. Generate the Database Migration Assessment Report.
4. Convert the source database schema to the target engine.
5. Apply schema, procedure and code to target database.

AWS Schema Conversion Tool can convert OTLP schemas from Microsoft SQL Server, MySQL, Oracle, PostgreSQL, IBM Db2, Apache Cassandre, and Sybase. It can be destined for Amazon RDS, EC2, Aurora, DynamoDB and S3.

For OLAP, it can convert from Microsoft SQL Server, Greenplum Database, Netezza, Oracle, Teradata, and Vertica. It can be destined for Redshift, EC2, and S3.

#### AWS Database Migration Service ####

DMS Workflow:
1. Create a replication server
2. Create source and target endpoints
3. Create migration and tasks for data migration
  a. Full load of existing data
  b. Application of cached changes
  c. Ongoing changes
  
DMS can take the following as sources:
* Oracle
* SQL Server
* Azure SQL
* PostgreSQL
* MySQL
* SAP ASE
* MongoDB
* S3
* IBM Db2 LUW

Targets:
* Oracle
* SQL Server
* PostgreSQL
* MySQL
* Redshift
* SAP ASE
* S3
* DynamoDB
* Kinesis Data Streams
* Apache Kafka
* Amazon Elasticsearch
* DocumentDB 
* Neptune


## Migration Execution and Validation

### DB Migration Cycle

1. Plan
2. Schema conversion
3. Data migration
4. Application update
5. Test
6. DB cutover (if necessary)

### DB Cutover Options

Offline migration:
1. Start write downtime (switch DB to read-only)
2. Migrate and verify data
3. Point application to new DB
4. End downtime

Flash-cut migration:
1. Start DMS in continuous data replication mode
2. When synced, verify the data
3. Start downtime
4. Deploy new application version
5. End downtime

Active/Active Database Configuration:
1. Copy data to target
2. Set up 2-way replication
3. Alternative to 2-way replication
4. Verify data
5. Migrate traffic to target

Incremental migration:
1. Migrate from one DB in small parts with various AWS services
2. Microservice 1
3. Microservice 2 etc.

## Management and Operations

### Maintenance Tasks and Processes

Here are a few scenarios of maintenance tasks for database administration.

#### RDS Parameter Changes ####

For RDS parameter changes, one example is that a DBA needs to configure `lower_case_table_names=1` for and RDS database instance running MySQL 5.6.

1. Check which parameter group is associated with the RDS instance. A default parameter group cannot be edited.
2. If it is default, create a new parameter group and associate the RDS instance with the new group.
3. The RDS instance must be rebooted for the parameters to be updated.

#### Neptune Bulk Data Load ####

1. A Neptune DB can be placed in a private subnet.
2. The S3 bucket must be in the same region as the DB and a role should grant read access to S3.
3. A NAT Gateway and IGW cannot be used for this procedure, only a VPC Gateway Endpoint on the target VPC.
4. Place the data in the bucket.
5. Initiate the CLI using `curl`.

#### RDS Snapshot Cross-Account Copy ####

Copy a RDS database backup to a separate AWS account.

1. Because snapshots and keys are region-scoped resources, it is first necessary to modify the CMK key policy for sharing. The Key Users section must be updated to add the remote account ID.
2. Modify the snapshot itself for sharing by adding the remote account ID in the permissions.
3. Initiate snapshot copy from the target account.
4. Restore from snapshot in the target account and validate the data.

#### Operational Overhead ####

EC2 operational overhead:
* Hardware failure recovery
* Disk space management
* OS management
* DB software install
* DB software update
* DB replication
* DB failover
* DB backups and restores

RDS operational overhead:
* DB software updates
* DB replication
* DB failover
* DB backups/restore
* DB security

DynamoDB operational overhead:
* DB replication
* DB backups/restore
* DB security


### Backup and Restore Strategies

#### RDS Backup ####

Automated backups can have a retention of up to 35 days, be retained for the retention period after the DB delete, and backups contain databases and transaction logs. Restores are restored to any second in the retention period. It creates a new DB instance.

Manual snapshots are kept until they are deleted, they are retained after DB delete, and shared with a remote account. It restores to snapshot time (no flexibility), creates a new DB instance, and restores encrypted DB snapshots cross-account directly. However, an encrypted DB cluster snapshot cross-account requires an initial copy first.

Snapshot exports to S3 consist of a back up on MariaDB, MySQL, or PostgreSQL and rstore to data export time. A new DB instance is not required because it is stored in S3.

#### Aurora Backups ####

Continuous backup has up to 35 day retention, but retention cannot be turned off. It is deleted immediately after the database is deleted. It restores to any second in the retention period. It creates a new DB instance and includes an earliest restorable time and lastest restorable time option.

Manual snapshots are kept until deleted, retained after DB delete, and shared with remote account. It restores to a snapshot time, creates a new DB instance, restores encrypted DB snapshot cross-account directly, and the encrypted snapshot cross-account requires copy first.

DB backtrack can be used as a recovery option. It uses continuous backup, transaction logs, covers a 24-72 hour window, and must be enabled at cluster provisioning. It restores through a rewind to previous timestamp, loses all uncommitted writes and transactions after backtrack point, but no new endpoint is needed.

Snapshot export to S3 from MySQL and PostgreSQL are possible. It restores to a data export time and a new database instance is not required.

DB cloning uses continuous backup to create a clone from current data copy with either 0 or 1 database instance. It uses copy-on-write for cloned DB changes.

#### DynamoDB Backups ####

PITR (Point in Time recovery) has a backup of 35 day retention, set statically. It has zero performance impact and can restore to any second in 35 days while creating a new table. It includes cross region restores.

On-demand backup is kept until deleted, has zero performance impact, restores to a snapshot time, creates a new table, and allows cross-region restores.

Global tables propagates deletes as well as additions. It cannot be used for restores but rather for disaster recovery.

#### Redshift Backups ####

Automated snapshots are taken every 8 hours or if there is 5 GB of change per node. Each snapshot can be copied cross-region. Encrypted snapshots require KMS grants for cross-region. Restores to snapshot time, to a new cluster, and can restore table to existing cluster.

Manual snapshots include cross-region copy, cross-account sharing, and is retained until deleted. It restores to snapshot time, restores to a new cluster, can restore to an existing cluster, and can share snapshot.

Unloading to S3 is a manual operation and can have cross-account sharing. It is retained until it is deleted. It can be used to unencrypt the cluster, restore to a new cluster, can restore to an existing cluster, and is accessible from other services.

#### Elasticache Backups ####

Automated backup retained for up to 35 days. It can copy to a manual backup, is stored in S3. It restores to a backup time, restores to a new cluster, and changes parameters.

Manual backups are kept until deleted, exported within the region, stored in S3. It restores to the backup time, restores to the new cluster, and changes parameters.

Snapshot export to S3 allows user to use data outside Redis, creates an RDF file, and is stored durably in S3. To restore, import the RDF file into an existing ElastiCache cluster (cannot be done for a new cluster). Can also import the RDF file into an unmanaged Redis cluster.

#### DocumentDB Backups ####

Continuous backup and automated snapshots are similar to Aurora with up to 35 day retention, no cross-account sharing, cross-region copy, it restores to any second within the retention period, creates a new cluster.

Manual snapshots are retained until deleted, can be shared with other accounts, restores to snapshot time, restores to a new cluster, and can cross account restore from encrypted snapshot requires copy first.

MongoDB native tools can also be used. It allows for migrations and exports to unmanaged replsets. Restores have full flexibility cross-cloud and even for hybrid architectures.


#### Neptune Backups ####

Continuous backups have up to 35 day retention. They are deleted immediately after DB delete. It restores to any second in the retention period, creates a new cluster, and also allows for earliest and latest restorable time settings.

Manual snapshots are kept until deleted and can be shared with other accounts. It restores to the snapshot time and creates a new cluster.


## Monitoring and Troubleshooting

### Monitoring and Alerting Strategies

#### RDS Monitoring ####

All basic metrics are gathered from the hypervisor perspective through CloudWatch, and these include CPU & Memory, disk metrics, network traffic, and DB connections. This can be used to create a CloduWatch dashboard and SNS topic.

CloudTrail logging can also be used in conjunction with IAM users and roles to invoke RDS actions. All RDS actions are logged to CloudTrail even if they are not successful. RDS log entries are delivered to S3 by default, optional to CloudWatch Logs. CloudWatch Logs metric filters can be used to generate metrics, graphs, and CloudWatch alarms.

Enhanced monitoring offers fine-grained, real-time metrics that are gathered from the OS perspective and stored in CloudWatch Logs. OS metrics are different for SQL server instances than for the other engines. 

Performance insights are enabled upon creation or by modifying an existing database instance and published to CloudWatch as metrics. DB load is the active number of sessions on the database, wait events that are unique to each database engine, top SQL shows which queries contribute to the database load, and max CPU by the number of vCPU on the instance.

Database Logs can be used to view the various database logs. Use the AWS CLI to export the various database logs. Use the AWS CLI to export logs for local viewing. Use the AWS SDKs to export logs for local viewing. Stream the logs to CloudWatch Logs for easier viewing and integration with CloudWatch. Each database engine has its own logs with slightly different formats, and some CloudWatch Logs streams are enabled by default.

RDS recommendations provides suggestions on optimizing RDS including instance configuration, engine version, and performance data.

Event notifications can use SNS topics as destination for RDS events. Use SNS topics as destination for RDS events.

Trusted Advisor can also be used to provide advice based on the Well-Architected Framework.

DB instance status can provide availability and durability metrics.

#### Aurora Monitoring ####

Aurora has the same options available as RDS but also Aurora Advanced Auditing.

This requires setting the parameter `server_audit_logging=ON`, `server_audit_events` with events such as `CONNECT`, `QUERY`, `QUERY_DCL`, `QUERY_DDL`, `QUERY_DML`, and `TABLE`. For access controls, set the parameters `server_audit_excl_users` and `server_audit_incl_users`.

View the logs in CloudWatch Logs, the Aurora dashboard, the CLI, or with SDK.

#### DynamoDB Monitoring ####

DynamoDB can mainly only use CloudWatch alarms, CloudTrail Logs, and contributor insights.

DynamoDB streams can act as a transaction log for data changes since it is the mechanism for global table replication. Consuming from DynamoDB Streams does not impact table read/write capacity.

#### Redshift Monitoring ####

Redshift can make use of CloudWatch alarms, CloudTrail Logs, database logs, event notifications, Trusted Advisor, and instance/cluster status.

These metrics are only available in the Redshift dashboard (not CloudWatch). It includes cluster CPU, cluster network/storage, query plans, and query execution and stages.

Audit logging can be enabled via the CLI, SDK, or the dashboard. Connection and user logs are enabled at the same time with delivery to S3. User activity logs require a DB parameter to be set `enable_user_activity_logging=true` (and perhaps a DB reboot). 

#### DocumentDB Monitoring ####

DocumentDB can use CloudWatch alarms, CloudTrail Logs, database events, and instance/cluster status.

DocumentDB profiler can create a new cluster parameter group with custom parameters like `profiler=enabled`, `profiler_threshold_ms=XX`, and `profiler_sampling_rate=YY`. This cluster parameter group is assigned to the existing cluster and the profiler captures execution time and details of operations before delivering them to CloudWatch Logs. 

Audit logging modifies the non-default cluster parameter group to enable logging. The cluster must itself bne updated to enable CloudWatch Logs export with `--enable cloudwatch_logs_exports audit`. Event details are delivered to CloudWatch Logs.

### Troubleshooting Common Issues

#### RDS Troubleshooting ####

Example: "incompatible parameters" status

Most commonly this stems from the incorrect memory parameter for the instance type. To mitigate this, revert the parameter to the default value and restart the instance.

Example: replica lag

Possible root causes:
* High query load on primary
* Transient network issue
* Failed transaction

Mitigation:
* Resize replica
* Reduce load on primary
* Resolve network issue

Example: connectivity failure

Possible root causes:
* VPC network misconfiguration
* Firewall issue

Mitigation:
* Resolve network issue (NACL, security group, etc.) Use VPC flow logs

Examples: DB parameter changes not taking effect

Possible root causes:
* Change made in wrong parameter group
* Parameter change requires reboot

Mitigation:
* Update correct parameter group
* Reboot database instance

#### Aurora Troubleshooting ####

Example: Aurora OOM

Possible root causes:
* Instance undersized
* Too many queries
* Query too large

Possible mitigation:
* Kill other queries
* Reconfigure cache
* Disable new queries

Example: read replica failure

Possible root causes:
* `max_allowed_packet` misconfiguration
* Writing to tables on replica
* Using MyISAM engine

Possible mitigation:
* Enforce read only queries
* Migrate tables to InnoDB

Example: No space left on device

Possible root causes:
* Poorly designed queries
* Undersized instance

Possible mitigation:
* Optimize queries
* Upsize instance

#### DynamoDB Troubleshooting ####

Example: read or write throttling

Possible root causes:
* Poorly chosen partition key
* Traffic spike on specific tiem
* Static provisioning too low

Possible mitigation:
* Recreate table with different partition key
* Enable instant scaling
* Configure DAX

Example: high latency

Possible root causes:
* Hot partition on table
* Throttling in effect

Possible mitigation:
* Configure DAX
* Increase read/write provisioning

Example: 400 error code

Possible root causes:
* Throttling
* `AccessDeniedException`
* `IncompleteSignature`
* `MissingAction`

Possible Mitigation:
* Increase read/writes
* Check permissions
* Verify signing code
* Validate request syntax

#### DocumentDB Troubleshooting ####

Example: blocked queries

Possible root causes:
* Too many write operations

Possible mitigation:
* Use mongo CLI to investigate with `currentOp` and `killOp`
* Increase number of cluster nodes

Example: long running queries

Possible root causes:
* Undersized cluster
* Suboptimal queries
* Query requires full scan

Possible mitigation:
* Create indexes
* Optimize query
* Use explain command

Example: high CPU usage

Possible root causes:
* Resource contention
* Too many concurrent queries
* Garbage collection

Possible mitigation:
* Add replicas
* Increase replica size
* Optimize database structure

### Performance Optimization

#### Performance Optimization Types ####

* Vertical scaling resizes the compute resource to increase CPU and memory
* Vertical scaling the storage resource to improve throughput and IOPS
* Horizontal scaling adds same-region read replicas
* With Aurora Serverless, horizontal scaling can be accomplished by adding Aurora Compute Units
* Add a Neptune Replica for read throughput horizontal scaling.

#### EC2 Optimization ####

Vertical scaling:
* Resize instance
* Upsize storage
* Change volume type

Horizontal scaling:
* Add read replicas
* Configure multiple primaries

Re-architecture:
* Shard the DB
* Move tables to NoSQL
* Split the DB
* Query tuning


#### RDS/Aurora Optimization ####

Vertical scaling:
* Resize instance
* Upsize storage
* Change volume type

Horizontal scaling:
* Add read replicas
* Configure multiple primaries
* Aurora serverless
* Parallel queries

Re-architecture:
* Shard the DB
* Move tables to NoSQL
* Split the DB
* Query tuning


#### DynamoDB Optimization ####

Vertical scaling:
* Burst capacity
* Add indexes

Horizontal scaling:
* Increase read ops
* Increase write ops

Re-architecture:
* Change partition key
* Query tuning
* DAX


#### Elasticache Optimization ####

Vertical scaling:
* Resize node type

Horizontal scaling:
* Add shards
* Add read replicas

Re-architecture:
* Implement hashing


## Database Security

### Data Encryption

#### DB Encryption At-Rest ####

   | Default | AWS keys | KMS keys | Customer keys | CloudHSM keys
EC2|    x    |          |    x     |      x        |       x
RDS|         |          |    x     |               |            
Aurora| x    |          |    x     |               |
DynamoDB|x   |    x     |    x     |      x        |
Redshift|    |          |    x     |               |       x
ElastiCache| |    x     |    x     |               |
DocumentDB|  |          |    x     |               | 
Neptune|     |          |    x     |               |

Redshift is the only service that can enabled encryption on an existing cluster.

#### DB Encryption In-Transit ####

   | Default | AWS managed TLS | Customer managed TLS | Unencrypted allowed
EC2|         |                 |    x                 |        x
RDS|    x    |        x        |                      |        x 
Aurora| x    |        x        |                      |        x
DynamoDB|x   |        x        |                      |
Redshift|    |                 |    x                 |        x
ElastiCache| |        x        |                      |        x
DocumentDB|  |        x        |                      |        x
Neptune|     |        x        |                      |        x



#### Enabling At-Rest Encryption ####

To enable at-rest encyrption on the CLI for RDS and Aurora use the `--storage-encrypted` argument with `--kms-key-id [ID]`. For Redshift, create a cluster with `--encrypted` and attach the KMS key id or the HSM client certificate identifier with the configuration identifier.

For DynamoDB, create the table with `--sse-specification \ Enabled = TRUE` alongside `SSEType=KMS` and `KMSMasterKeyID`. If set to false, then AWS will manage the entire chain of trust instead. Encryption cannot be disabled for DynamoDB.

For Elasticache, create a replication group with `--at-rest-encryption-enabled true` and `--kms-key-id [ID]`. For DocumentDB, create a cluster and include `--storage-encrypted true` with `--kms-key-id [ID]`. For Neptune, it is the same as DocumentDB.

Note that ACM can manage TLS certificates for all storage solutions except RDS.

### Auditing for Vulnerabilities

#### DB Service Logs ####

EC2:
* View DB logs on the local filesystem
* Use the CloudWatch agent to stream logs to CloudWatch Logs

RDS:
* View DB logs in the RDS console
* Enable audit logging for MySQL and MariaDB
* View DB logs in CloudWatch Logs
* View RDS operations in CloudTrail

Aurora:
* View DB logs in the Aurora console
* Enabled advanced auditing
* View DB logs in CloudWatch logs
* View Aurora operations in CloudTrail

DynamoDB: (limited because it is serverless and only accessed via an API endpoint)
* View operations in CloudTrail

Redshift: 
* View query/load performance data in the Redshift console
* Enabled auditing
* View Redshift operations in CloudTrail

DocumentDB:
* View profiler logs in CloudWatch Logs
* View operations in CloudTrail

Neptune:
* View audit logs in CloudWatch Logs
* View Neptune operations in CloudTrail

#### AWS Config Rules ####

AWS Config Streams can capture changes to various resources within a timeline, with dependency visualization, and viewed against configuration rules. It supports EC2/EBS, RDS, DMS, DynamoDB, ElastiCache, and Redshift. The Config Stream captures all changes through the AWS service's API endpoint.

Create config rules to evaluate compliance with security controls across many services. These include `dms-replication-not-public`, `db-instance-backup-enabled`, `rds-instance-public-access-check`, `redshift-require-tls-ssl`,`rds-snapshot-encrypted`, `rds-logging-enabled`, `rds-storage-encrypted`, and `rds-snapshots-public-prohibited`.

#### Trusted Advisor ####

The Trusted Advisor dashboard is built on the five pillars of the Well-Architected Framework. With business or Enterprise support, service limits can be queried via the CLI (although this is not available for all services).

Trusted Advisor checks RDS security group access risk, RDS public snapshots, and exposed access keys.

#### Security Hub ####

Firewall Manager can be used to develop guardrails for network access, IAM access analyzer can determine if IAM permissions are too permissive, Systems Manager: Patch Manager checks if patches are in compliance. Detective can look at root causes of security incidents. GuardDuty looks at various logs to build a model of known, legitimate behavior in the account. Inspector is installed on EC2 and can perform OS security audits. Macie can profile data to detect improperly accessible sensitive data and who has accessed it.

Security Hub Standards:
* CIS AWS Foundations Benchmark: No specific DB controls
* PCI DSS: PCI.DMS.* , PCI.RDS.* , PCI.Redshift.* 
* AWS Foundational Best Security Practices


### Access Control and Authorization ###
