# Databases

## Table of Contents
[RDS Architecture](#rds-architecture)
* [RDS Multi AZ](#rds-multi-az)
* [RDS Backups and Restores](#rds-backups-and-restores)
* [RDS Read Replicas](#rds-read-replicas)
* [RDS Data Security](#rds-data-security)
[Aurora Architecture](#aurora-architecture)
[Aurora Serverless](#aurora-serverless)
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



