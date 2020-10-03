# Disaster Recovery and Business Continuity in AWS

## Types of Disaster Recovery

### Backup and Restore:

Data is constantly backed up at the primary site. The only costs are backup media and management, so there are no ongoing spare infrastructure costs. However restores require new hardware or a lengthy restore process.

### Pilot Light:

A backup site is provisioned in advance with the absolute minimum functionality running. Critical components such as databases are constantly syncing so they are ready to be used. This process is much faster but more expensive than backup and restore.

### Warm Standby:

A full copy of the infrastructure is duplicated at a smaller level (smaller instance size, etc.). It is fully functional and ready to be scaled up when failover is required. Faster and more expensive than Pilot Light.

### Active/Active:

Backup site is identical to primary site. Data is constantly replicated from the primary site to the backup. Costs are generally 200% because two full copies are running simultaneously. Can load balance across environments, improving HA and performance. Most expensive of these options.

## Disaster Recovery for Storage

* Instance store volumes are attached to a host in an AZ, failure of either means failure of the volume.
* EBS volumes replicate within an AZ. Failure of the AZ means failure of the volume.
* S3 Snapshots provide regional resilience.
* EFS replicates across AZs in a region. An entire region has to fail before EFS fails. Snapshot copies can provide global resilience.

## Disaster Recovery for Compute

* EC2 fails if an instance or AZ fails.
* EBS fails if the AZ fails.
* Auto-scaling groups can provision another instance in another AZ with its own storage.
* Clusters can be in EC2 mode or Fargate mode. In EC2 mode the DR characteristics are the same as an EC2 instance.
* ECS Container hosts fail if the AZ fails. In fargate mode, containers use ENI's in a VPC. Services can be used to achieve a similar architecture to auto scaling groups.
* VPC Lambda functions are allocated an ENI in each subnet. Failure of an AZ will result in another subnet being used instead. Public Lambda functions operate from all AZs in a region. The entire region would have to fail to impact service.

## Disaster Recovery for Databases

* Database of EC2, dependent on EC2 host and AZ.
* DynamoDB replicates between multiple nodes in multiple AZs, region failure needed to impact service.
* Non-Aurora RDS replicates between primary and standby instances. Failure of both would impact service.
* Aurora cluster storage is replicated 6+ times across 3 AZs at the storage level. Only region failure would significantly impact service. Aurora can have replicas in all AZs in a region. Storage is handled by the shared cluster storage.
* DynamoDB global tables have multi master replication between regions.
* Aurora global databases have storage level replication (lag is 1 second or less).
* RDS Cross-Region read replica has asynchronous replication.

## Disaster Recovery for Networking

* VPCs are regionally resilient. VPC routes and IGWs are also regionally resilient. 
* Subnets are in one AZ. 
* Load balancers have LB nodes in 2+ AZs as long as one node is functional.
* Public services run from all AZs only region failure would cause service failure.
* Route 53 is a global service that can withstand multiple region failures.


