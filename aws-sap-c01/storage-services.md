# Storage Services

## FSx for Windows File Server

> [Amazon FSx for Windows File Server](https://aws.amazon.com/fsx/windows/faqs/?nc=sn&loc=8) provides fully managed, highly reliable, and scalable file storage that is accessible over the industry-standard Service Message Block (SMB) protocol. It is built on Windows Server, delivering a wide range of administrative features such as user quotas, end-user file restore, and Microsoft Active Directory (AD) integration.

FSx for Windows File Server is a fully managed native Windows file server/share. Integrates with Directory Service or Self-Managed AD. 

* Single or Multi-AZ mode within a VPC.
* On-demand and schedule backups available.
* Accessible using VPC Peering, VPN, and Direct Connect.
* Supports managed or self-managed AD (On-premises)
* Supports de-duplication (sub file), Distributed File System (DFS), KMS at-rest encryption and enforce encryption in-transit.
* VSS: User-driven restores.
* Native file system accessible over SMB.
* Windows permission model.
* Managed with no file server admin.


## FSx for Lustre

>[Amazon FSx for Lustre](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html) makes it easy and cost-effective to launch and run the popular, high-performance Lustre file system. You use Lustre for workloads where speed matters, such as machine learning, high performance computing (HPC), video processing, and financial modeling.

FSx for Lustre is a managed Lustre designed for HPC LINUX Clients (POSIX). It is ideal for machine learning, big data, and financial modelling. FSx has 100 GB/s throughput and sub ms latency.

Deployment:
* Scratch: Highly optimized for short term, no replication (pure performance).
* Persistent: Longer term, HA in on AZ, self-healing.

* FSx for Lustre is accessible over VPN or DX.
* Data is lazy loaded from S3 into the file system as needed.
* Data can be exported back to S3 at any point using `hsm_archive`.
* Metadata stored on Metadata targets (MST).
* Objects are stored on called object storage targets (OSTs) (1.17TiB).
* Baseline performance based on size (1.2TiB then increments of 2.4TiB).
* For scratch - 200 MB/s per TiB of storage. Persistent offers 50 MB/s, 100 MB/s, and 200 MB/s per TiB of storage. Burst up to 1,300 MB/s per TiB (Credit system).
* Can back up to S3 with both deployments (Manual or Automatic 0-35 day retention).


## Elastic File Service (EFS)

> [Amazon Elastic File System](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html) (Amazon EFS) provides a simple, scalable, fully managed elastic NFS file system for use with AWS Cloud services and on-premises resources. It is built to scale on demand to petabytes without disrupting applications, growing and shrinking automatically as you add and remove files, eliminating the need to provision and manage capacity to accommodate growth. Amazon EFS has a simple web services interface that allows you to create and configure file systems quickly and easily. The service manages all the file storage infrastructure for you, meaning that you can avoid the complexity of deploying, patching, and maintaining complex file system configurations.

EFS is an implementation of NFSv4. EFS File systems can be mounted in Linux only, and it is shared between many EC2 instances. It is a private service, via mount targets inside a VPC. It can be accessed from on-premises (VPN or DX) via Lambda.

Modes:
* General purpose - default for 99.9% of cases
* Bursting
* Provisioned Throughput

* Standard and Infrequent access classes.
* Lifecycle policies can be used with classes.


## S3 Object Storage Classes

### S3 Standard

At least 3 AZs with 99.999999999% durability for 10,000,000 objects. Content MD5 checksums and cyclic redundancy checks (CRCs). Billed a GB/m fee for data stored. No specific retrieval fee, no minimum duration, no minimum size. Milliseconds first byte latency and objects can be made publicly available. When objects are stored a HTTP/1.1, 200 OK result is provided by API endpoint.

### S3 Standard-IA

A per GB data retrieval fee. Minimum duration charge of 30 days. Standard-IA has a minimum capacity charge of 128KB per object. S3 Standard-IA should be used for long-lived data

### S3 One Zone-IA

Per GB data retrieval fee, minimum duration charge of 30 days. Minimum capacity charge of 128 KB per object. Does not provide the multi—AZ resilience model. Same durability of S3 Standard just not if AZ fails. Should be used for long-lived data which is non-critical & replaceable and where access is infrequent.

### S3 Glacier

1/5 the storage cost of S3 Standard. Objects cannot be made publicly accessible any access of data requires a retrieval process. Speeds: Expedited (1-5 minutes), Standard (3-5 hours), bulk (5-12 hours). First byte latency = minutes or hours. 40 KB minimum size with 90 day minimum duration. Used for archival data where frequent or realtime access is not needed. Minutes-hours retrieval.

### S3 Glacier Deep Archive

¼ the price of Glacier. 40 KB min size, 180 day minimum duration. Objects cannot be made publicly available. Speeds: Standard (12 hours), Bulk (up to 48 hours) Used for archival data that needs to be retained for legal or regulation purposes.

### S3 Intelligent Tiering

Frequent Access and Infrequent Access tiers. Intelligent tiering monitors and automatically moves any objects not accessed for 30 days to a low cost infrequent access tier. As objects are accessed, they are moved back to the frequent access tier where there are no retrieval fees. Should be used for long-lived data with changing or unknown patterns. Monitoring and automation cost per 1,000 objects.


### S3 Lifecyle Configuration

A lifecycle configuration is a set of rules consisting of actions (transition or expiration) on a bucket or group of objects. Lifecycle configuration can only move in a downward direction (to less frequent/longer storage). Smaller objects can cost more upon transition due to minimum size billing. 30 day minimum before transitioning to IA automatically. A single rule cannot transition to Standard-IA, intelligent tiering or One Zone-IA and then to a Glacier type within 30 days. It must first way 30 days in one of those tiers if a single rule is used.

### S3 Replication

Cross-Region Replication (CRR) and Same-Region Replication (SRR) are available. If Cross-account replication is used, then the destination bucket needs a bucket policy to specify that the role in the source account is authorized to write changes. S3 can replicate all objects or a subset. The default storage class maintains, and default ownership is the source account. Replication Time Control (RTC).

Considerations:
* Replication is not retroactive.
* Versioning needs to be enabled for both source and destination buckets.
* One-way replication only.
* Can handle unencrypted SSE-S3 and SSE-KMS if it has extra configuration, but it cannot do SSE-C.
* Source bucket owneer needs permissions to object.
* No system events are replicated; Glacier and Glacier Deep Archive cannot be replicated.
* No deletes in source bucket are replicated in destination bucket.

Use Cases:
* SRR - Log aggregation, sync between PROD and TEST environments, resilience with strict sovereignty.
* CRR - Global resilience improvements, latency reductions


### S3 Object Encryption

Buckets are not encrypted, but the objects inside are. SSE enacts encryption at the S3 Endpoint.

Client-Side Encryption: Client encrypts data, so the data is encrypted during its entire AWS lifecycle.
SSE-C: Customer provided keys.
SSE-S3: Encryption with Amazon S3-Managed Keys.
SSE-KMS: Customer Managed Keys stored in AWS Key Management Service. Great for role separation and CloudTrail tracking. Includes rotation control.
Bucket Default Encryption: AES256.


### S3 Presigned URLs

Pre-signed URLs are a feature of S3 that allows generated URLs to be encoded with access permissions for a specific bucket and object, valid for a certain period of time. The permissions match the identity which generated it, it is technically possible to create a URL for an object the user has no access to. The generated URL could therefore lead to an access denied. URLs do not generate a role.

It is better to use CloudFront presigned URLs for more secure access. Unwanted parties can still connect using an S3 presigned URL. [See more here](https://tutorialsdojo.com/s3-pre-signed-urls-vs-cloudfront-signed-urls-vs-origin-access-identity-oai/)

### S3 Select and Glacier Select

S3 Select and Glacier Select allows the user to avoid retrieving the whole object from S3 to save both time and money. Filtering at the client side does not reduce these costs which is why a Select is necessary as a SQL-like query. It is up to 400% faster and 80% cheaper. It can be used for CSV, JSON, Parquet and BZIP2 compression for CSV and JSON.


## EBS and Instance Store Volume Performance

> [Amazon Elastic Block Store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) (Amazon EBS) provides block level storage volumes for use with EC2 instances. EBS volumes behave like raw, unformatted block devices. You can mount these volumes as devices on your instances. EBS volumes that are attached to an instance are exposed as storage volumes that persist independently from the life of the instance. You can create a file system on top of these volumes, or use them in any way you would use a block device (such as a hard drive). You can dynamically change the configuration of a volume attached to an instance.

EBS is network storage that is separated from EC2. EBS is provisioned in one AZ (so risk of failure), but volumes can be re-sized and snapshotted as well as detached and re-attached. They follow instances between EC2 hosts with a per instance max storage performance. However, EBS is not the best option for a scalable solution.

Instance stores are physical disks in an EC2 host. Data lost if hardware fails or instance moves between hosts. The size cannot be adjusted because the disks are physical. High performance (IOPS and MB/s optimized) since they are locally connected. Temporary and should not be used for persistent data.

Instance can be striped. Raid 0 provides even higher performance, and Raid 1 provides data redundancy. 

* HDD Based (MB/s focused or low cost) with no boot volumes. There is a burst MiB/s pool that replenishes when consuming less than base MiB/s. Anti patterns – boot, high IOPS, low latency.
* SSD Based is IOPS focused and can be boot volumes. It has a burst IOPS pool that replenishes when using less than base IOPS. 

Raid 0 Striped formula: Size x n capacity. IOPS x n = IOPS.
Raid 1 Mirror: No extra capacity or IOPS.

**Instance EBS limit (network limit) is a hard 160k IOPS.**

Types:
* st1 - Frequently accessed, throughput optimized. 40 MiB/s per TiB base, 250 MiB/s per TiB Burst – 500 MiB/s max.
* sc1 - Infrequently accessed, cold. 12 MiB/s per TiB base, 80 MiB/s per TiB Burst – 250 MiB/s max.
* gp2 – 1 GiB-16TiB, Max 16K IOPS (3 per GiB) – linked to size. Max throughput 250 MiB/s.
* io1/io2 – 4GiB – 16TiB, Max 64k IOPS (50 or 500 per GiB) … IOPS controllable independent of size. Max throughput per volume 1000 MiB/s.

Use cases:
* Cheap -> st1 or sc1
* Throughput or streaming -> st1
* Boot -> Not st1 or sc1
* Up to 16,000 IOPS -> gp2
* Up to 64,000 IOPS -> io1/2
* Up to 160,000 IOPS -> RAID0 + EBS
* Over 160,000 IOPS -> Instance Store
