# Migrations and Extensions

## The 6 R's of Cloud Migrations

1. Rehosting: lift and shift. Migrate the system as is. Little flexibility on the architecture. Reduced admin overhead (IaaS). Potentially easier to optimize in AWS. Cost savings. But negative sides are not taking full advantage of the Cloud, potentially only postponing migration. Do this with VM Import/Export.

2. Replatforming: Lift and shift with optimization. Preferable to Rehosting.  Might use RDS, ELB, S3 instead of self-managed services. No real negatives or positives. Admin overhead reductions, performance benefits, more effective backups, or improved HA/FT.

3. Repurchasing: Move to something new. Use XaaS products unless there is a strong reason to self-manage. MS Exchange -> Microsoft 365. CRM -> Salesforce. Payroll -> Xero.

4. Refactoring/Re-architecting: Take advantage of cloud. Reviewing architecture to adopt cloud-native architectures and products (service-orientated or microservices). APIs, Event-Driven, Serverless. Initially very expensive and time consuming but has best long-term benefits. Cheaper, scalable, better HA/FT, costs aligned with usage.

5. Retire – Get rid of application. When systems are running for no reason. Auditing usage is more work than leaving running. Switching off can save 10-20% costs.

6. Retain – Not worth the time or effort to migrate. Old, complex, or critical applications. Complete the macro-mirgation then return to later.

Steps:
1. Discover, assess and prioritize applications.
2. Retain and retire as needed.
3. Choose migration method.
4. Validate, transition, production.


## Virtual Machine Migrations (VM Migrations)

> [AWS Application Discovery Service](https://docs.aws.amazon.com/application-discovery/latest/userguide/what-is-appdiscovery.html) helps you plan your migration to the AWS cloud by collecting usage and configuration data about your on-premises servers. Application Discovery Service is integrated with AWS Migration Hub, which simplifies your migration tracking as it aggregates your migration status information into a single console. You can view the discovered servers, group them into applications, and then track the migration status of each application from the Migration Hub console in your home region.

AWS Application Discovery Service (discover virtual machines) helps enterprise customers plan migration projects. Agentless mode (application discovery agentless connector) is an OVA appliance that integrates with VMWARE and measures performance and resource usage (similar to CloudWatch agent). Can see dependencies between servers (network activity). Integration with AWS Migration Hub and Amazon Athena.

Server Migration Service migrates whole VMs. Agentless, using a connector which runs on-premises. Integrates with Vmware, Hyper-V, and AzureVM. Incremental replication of live volumes. Orchestrate multi-server migrations. Creates AMIs and integrates with AWS Migration Hub.


## Database Migration Service (DMS)

> [AWS Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html) (AWS DMS) is a cloud service that makes it easy to migrate relational databases, data warehouses, NoSQL databases, and other types of data stores. You can use AWS DMS to migrate your data into the AWS Cloud, between on-premises instances (through an AWS Cloud setup), or between combinations of cloud and on-premises setups.

Database Migration Service is, as its name indicates, a managed database migration service that runs using a replication instance. The source and destination endpoints point toward the source and target databases respectively. One endpoint must be in AWS. The replication instance performs the migration between the source and destination endpoitns which store connection information for source and target databases. Jobs can be full load, full load + CDC (change data capture - ongoing replication) or CDC only in conjunction wiht an alternative bulk transfer such as native tooling.

The Schema Conversion Tool (SCT) is used when converting one database engine to another including DB -> S3. **SCT is not used when migrating between databases of the same type.** It works with OLTP database types (MySQL, MSSQL, Oracle) and OLAP (Teradata, Oracle, Vertica, Greenplum). Larger migrations might be multi-TB in size, moving data over networks takes time and consumes capacity.

DMS can utilize Snowball:
1. Use SCT to extract data locally and move to a Snowball device.
2. Ship the device back to AWS. They load the data onto an S3 bucket.
3. DMS migrates from S3 into the target store.
4. CDC can capture changes and write them to target database via S3.


## Storage Gateway

> [AWS Storage Gateway](https://docs.aws.amazon.com/storagegateway/latest/userguide/WhatIsStorageGateway.html) connects an on-premises software appliance with cloud-based storage to provide seamless integration with data security features between your on-premises IT environment and the AWS storage infrastructure. You can use the service to store data in the AWS Cloud for scalable and cost-effective storage that helps maintain data security.

### Storage Gateway - Volume Gateway

Volume gateway is a virtual machine (or hardware appliance) that present storage using iSCSI, NFS, or SMB. It integrates with EBS, S3 and Glacier and offers migrations, extensions, storage, tiering, D$ and replacement of backup systems. Storage gateway VM with Network Attached Storage (NAS) is one option where servers have local disks that will connect to NAS through iSCI protocol with RAW Block Devices.

Volume gateway has stored and cached modes.

**Stored Mode**: Storage gateway has local storage. **Everything is stored locally.** There is an upload buffer and data is copied asynchronously to a storage gateway endpoint. It is then copied to S3 through EBS snapshots. It is great for "full disk" backups of servers with good RTO/RPO values. It is great also for DR by restoring from EBS snapshots into EBS volumes. It does not improve datameter capacity. Main copy of data is stored on the gateway. There are 32 volumes per gateway, 16 TB per volume, allowing for up to 512 TB per gateway.

**Cached Mode**: Primary data is stored in AWS S3, not local storage. It is in an AWS Managed portion of S3 and cannot be access from the bucket console. It is great for **data center extension** since data is stored on AWS and cached on-premises. 32 TB per volume, 32 volumes per gateway, 1 PB per gateway.

### Storage Gateway Tape - VTL Mode

TAPE is made for large backups (LTO-9 can hold up to 24 TB) with up to 60 TB compressed. One tape drive can use one tape at a time. A library is one or more drives, one or more loaders, and slots. Collectively, there is a drive, a library and a shelf (which is anywhere but the library).

Traditional: Equipment costs (tapes, software, library) and CAPEX and OPEX costs. Offsite tape storage is managed by a 3rd party. Only tapes physically in the library can be used for backups and restores. Tapes are moved offsite when not in active use. Transport between locations takes time and has costs.

Storage Gateway Tape (VTL): The backup server sees VTL as a regular tape loader through iSCI. There is an upload buffer and local cache. The backup is loaded into VTL in S3. Tape Shelf (VTS) is stored in Glacier, **so its data cannot be retrieved in real-time.** Virtual tape can go from 100 GB to 5 TB with 1 PB across 1500 virtual tapes. Unlimited VTS (archive) storage. Virtual Tape Library can retrieve data from Tape Shelf in Glacier. **It pretends to be an iSCI tape library, changer and drive.**


### Storage Gateway - File Gateways

File gateway bridges local storage over NFS and SMB mount points (shares) with S3 stroage. Each mount point maps directly onto an S3 bucket. Files stored into a mount point are visible as objects in an S3 bucket. Read and Write caching ensures LAN-like performance. A bucket share is an S3 bucket and an on-premises file share. S3 objects are visible as on-premises files. 10 bucket shares per file gateway. Primary data is held in S3, only caching stored on-premises. AWS service integrations such as events or Lambda.

Multiple contributors use `NotifyWhenUploaded` API to notify other gateways when objects are changed. File Gateway does not support object locking. Use read-only mode on other shares or tightly control file access.

Replication can use cross-region replication and lifecycle management.


## Snowball, Snowball Edge, and Snowmobile

> The [AWS Snowball](https://docs.aws.amazon.com/snowball/latest/ug/whatissnowball.html) service uses physical storage devices to transfer large amounts of data between Amazon Simple Storage Service (Amazon S3) and your onsite data storage location at faster-than-internet speeds.

Snowball, Snowball Edge, and Snowmobile move large amounts of data in and out of AWS.

Snowball is ordered from AWS as a physical device. It has data encryption at rest through KMS. It has a capacity of either 50 TB or 80 TB. It can connect with 1 GB/s or 10 GB/s with physical networking. It is economical to use for data migrations between 10 TB to 10 PB (multiple devices to multiple premises). Only storage, no compute.

Snowball Edge is storage and compute focused. 10 GB/s (RJ45), 10/25 (SFP), 45/50/100 GB/s (QSFP+). Ideal for remote sites where data processing during ingestion is needed.

Storage optimized with EC2: 80 TB, 24 vCPU, 32 Gib RAM, 1 TB SSD.
Compute optimized - 100 TB + 7.68 NVME, 52 vCPU and 208 GiB RAM
Compute with GPU

Snowmobile is a portable data-center within a shipping container on a physical truck. It is ideal for single location migration with over 10 PB. Up to 100 PB per snowmobile. It is not economical for multi-site or under 10 PB.


## AWS DataSync

> [AWS DataSync](https://docs.aws.amazon.com/datasync/latest/userguide/what-is-datasync.html) is an online data transfer service that simplifies, automates, and accelerates copying large amounts of data to and from AWS storage services over the internet or AWS Direct Connect. DataSync can copy data between Network File System (NFS), Server Message Block (SMB) file servers, self-managed object storage, or AWS Snowcone, and Amazon Simple Storage Service (Amazon S3) buckets, Amazon EFS file systems, and Amazon FSx for Windows File Server file systems.

DataSync provides data transfer services to and from AWS whether it is for migrations, data processing transferrs, archival purposes, cost effective storage, or DR/BC. It is designed to work at a huge scale and keeps metadata (e.g. permissions and timestamps). It has built in data validation.

It is scalable with 10 GB/s per agent and has bandwidth limiters as well as incremental and scheduled transfer options. Compression and encryption are available. Automatic recovery from transit errors. AWS service integration with S3, EFS, FsX. It is a pay as you use service with a per GB cost.

The DataSync Agent runs on a virtualization platform such as VMWare and communicates with the AWS DataSync endpoints. Locations define the source or destination for the sync of data to and from AWS. Schedules can be set to ensure the transfer of data occurring during (or avoiding) specific time periods. Customer impact can be minimized by setting a bandwidth limit. It provides encryption in transit through TLS.

Terms:
* Task: A job within DataSync, what is synced, how quickly, source and target.
* Agent: Software used to read or write using NFS or SMB.
* Location: Every task has two locations the "From" and "To".
