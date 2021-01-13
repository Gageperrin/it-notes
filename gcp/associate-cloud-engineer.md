# Google Cloud - Associate Cloud Engineer

These are my notes from Antoni Tzavelas' [training course for the Google Cloud Associate Cloud Engineer examination](https://training.antonit.com/). Other resources include the [official GCP documentation](https://cloud.google.com/docs/).

## Google Cloud Fundamentals

### Google Cloud Global Infrastructure

A user request is sent to a Point of Presence edge network which is then passed over a private fiber network to a Google Data Center with the lowest amount of latency possible.

A zone is a deployment area for Google Cloud in a region. It offers optimal latency because of its precise deployment proximity to users. A region is an independent geographic area that contains multiple zones. There is sub 5ms latency between zones in a region which makes it useful for fault tolerance and high availability. Multi-regions contain two or more regions. It serves content to consumers outside of the Google network.

### Compute Service Options

* Virtual Machines are called instances
* Choose the region and zone to deploy to
* Chose the operating system and software to put on it
* Public and private instances can be used to create instances

Google Kubernetes Engine (GKE) is Google's container orchestration system for automating, deploying, scaling, and managing contaienrs. It is built on open source Kubernetes and has the flexibility to integrate with on-premises Kubernetes. It uses compute engine instances as nodes in a cluster. A cluster is a group of nodes or Compute Engine instances. It is considered CaaS.

App Engine is a fully managed, serverless platform for developing and hosting web applications at scale (PaaS). It provisions servers and scales your app instances based on demand. It connects with other Google services seamlessly as well as Web Security Scanner. It is PaaS.

Cloud Functions is a serverless execution environment for building and connecting cloud services. It consists of single, simple-purpose functions that are closed upon completion. It is considered FaaC.

Cloud Run is a fully managed compute platform for deploying and scaling containerized applications quickly and securely. It is built upon an open standard Knative. It abstracts away all infrastructure management. It is distinctively serverless for containers. It is considered FaaC.

### Storage and Database Options

Cloud Storage is consistent, scalable, large-capacity, highly durable **object** storage. It has 11 9's of durability. Unlimited storage with no minimum object size. Use Cloud Storage for content delivery, data lakes, and backup. It is available in different storage classes and availability.

Storage Classes:
* Standard: Maximum availability, no limitations
* Nearline: Low-cost archival storage, accessed less than once a month
* Coldline: Even lower cost archival storage accessed less than once a quarter
* Archive: Lowest cost archival storage for less than once a year

Availability: Region, dual-region, and multi-region

Filestore is a fully managed NFS file server that is NFSv3 compliant to store data from running applications to use with VM instances or Kubernetes clusters.

Persistent disks is durable block storage for instances. Standard versus Solid State (SSD) options as well as zonal and regional options.

Database options include Cloud SQL and Cloud Spanner for relational database services. For NoSQL, there is Bigtable, Datastore, Firestore, and Memorystore.

### Networking Services

VPC is a virtualized private network within Google Cloud, and it is a core, global networking service.

Firewall Rules and Routes govern traffic coming into instances as well as provide advanced networking functions.

There is HTTP(S) Load Balancing and Network Load Balancing. The former distributes traffic across regions based on traffic type. The latter distributes traffic based on IP protocol data within a region.

Google DNS publishes and maintains DNS records by using the same infrastrucutre that Google uses.

Advanced connectivity options include Cloud VPN and Direct Interconnect. Direct Peering exchanges Internet traffic between the business network and Google while Carrier Peering connects the infrastructure to Google's network edge.

### Big Data Services

BigQuery is a fully managed petabyte scale, low cost analytics data warehouse. It can ingest data through streaming and batch as well as through free bulk loading. It provides real time analytics, autoamtic high availability, automatic backup and restore, standard SQL, and a big data ecosystem integration. It includes IAM, VPC managemnet, and data encryption.

Pub/Sub is a real-time fully-managed messaging service. Send and receive messages between independent applications. A publisher creates a messages and sends them to a topic which contains the messages that are passed along to those contianed in a subscription.

Composer is a fully managed orchestration service built on Apache Airflow.

Dataflow is a fully managed processing service for executing Apache Beam pipelines for batch and realtime data streaming. It takes data in from a source and read transforms it into a PCollection, transformed and processed again, before it is write-transformed into a sink. A DataFlow job runs through this pipeline.

Dataproc is a fully managed spark and Hadoop service that can be used to replace on-premise Hadoop infrastructure.

Dataproc is managed, dependencies to Hadoop ecosystem tools while Dataflow is serverless and built on an Apache Beam runtime. 

Cloud Datalab is an easy-to-use interactive tool for data exploration, analysis, visualization, and machine learning. It is built on notebooks.

Cloud Dataprep is a serverless, intelligent data service for visually exploring, cleaning, and preapring structured or unstructured data for analysis, reporting, and machine learning.

### Machine Learning Overview

Vision and Video Intelligence are visually-based machine learning models for image and video classification and processing.

Natural Language and Translation are Google's language-based machine learning options. Their functions include Dialog Flow, Speech-to-Text, and Text-to-Speech. 

AutoML is a fully managed suite of machine learning products.



## Account Management

### Resource Hierarchy

A resource can be service level (databases) or account level (organization, folders). Google Cloud's resource hierarchy structure is a parent child relationship. Policies are controlled by IAM. Access control policies and configuration settings on the parent are by default inherited by the child. Each child object has exactly one parent.

Resources are contained in projects which are contained in folders which are contained in the organization root node that is itself hosted in the domain at the cloud level.

### Controlling Costs and Budget Alerts

Committed Use Discounts are discounts available when the user commits to a minimum level of resources for a specified term of one to three years. These can be spend-based or resource-based, and the commitment fee is billed monthly. Spend-based is a consistent amount of usage while Resource-based is for compute engine resources in a region. It is available for vCPU, Memory, GPU, and Local SSD with up to 57% discounted for most resources. Memory-optimized machine types can have up to 70% off.

Sustained-use discounts are automatic discounts fo running compute engine resources for a significant portion of the billing month. It applies to vCPUs and memory and VMs created by GKE, but it does not apply to App Engine flexible, Dataflow and E2 machine types.

The GCP Pricing Calculator and Cloud Billing Budgets can help monitor and predict overall and specific costs. Pub/sub can be used for programmatic notifications and to automate cost management tasks.

### Billing Export

Billing export enables granular billing data such as usage, cost details and pricing data to be exported automatically to BigQuery. It is not retroactive.

### Cloud APIs

Cloud APIs allow workflow automation using software libraries in a variety of programming languages.


## Identity and Access Management

### Cloud IAM 

Principle of Least Privilege: A user, program, or process should have the bare minimum privileges necessary to perform its function.

Cloud IAM provides configuration options for identities, roles, and resources. A policy is a collection of bindings, metadata, and configuration. Binding configures how members and resources should be matched with permissions.

A member can be a Google account, a service account attached to an application, a Google Group, G Suite Domains, Cloud Identity Domains, `AllAuthenticatedUsers`, or `AllUsers`.

Permissions determine what operations are allowed on a resource and correspond to REST API methods. These permissions are not granted to users directly. Instead they are granted through roles which are a collection of permissions. 

There are three kinds of roles in IAM: primitive, predefined, and custom. Primitive are roles alike Owner, Editor, or Viewer that are legacy options. They are no longer recommended. Predefined provide fine-grained access control. Custom roles are user-defined and managed.

Conditions can be used to specify resource access based on certain identity attributes or other triggers. When a condition exists, the access is only granted if the expression is true.

Policies are inheritted from organizations to folders to projects to resources.

### Policies and Conditions

Example policy statement:
```
bindings:
- members:
  - serviceAccount:service-733661073xxx@compute-system.iam.gserviceaccount.com
  role: roles/compute.serviceAgent
- members:
  - serviceAccount:733661073xxx@cloudservices.gserviceaccount.com
  - serviceAccount:7336610731xxx-compute@developer.gserviceaccount.com
  role: roles/editor
- members:
  - user:example@mail.com
  role: roles/owner
etag: BwW2qo0_TVk=
version: 1
```
Use version 1 if there is no condition statement. If there is a condition statement, it needs a version 3 policy. There is one policy per resource and up to 1500 members or 250 Google groups per policy. It takes up to 7 minutes for policy changes to fully propagate across GCP. Limit of 100 conditional role bindings per policy.

Conditions are either based on the resources itself or details about the request such as timestamp or geolocation. Conditions are limited to specific services and cannot be applied to primitive roles. Members cannot be `allUsers` or `allAuthenticatedUsers`. Limit of 100 conditional role bindings per policy with 20 role bindings for same role and member.

`AuditConfig` logs determine which features have logs generated for them.

### Service Accounts

Service accounts can be user-managed, a default pre-configured account that automatically grants roles, and Google-managed.

Service account keys can either be Google-managed or user-managed. The former is entirely managed by Google while the latter requires the user to manage key storage, distribution, revocation, rotation, protection, and recovery.

Service account permissions can be granted at the project or service account level.

Access scopes are the legacy method for permission authorization before IAM roles. These are used for default service accounts, but IAM roles should be used instead for custom service accounts.

Best practices: audit service accounts and keys using either the `serviceAccount.keys.list()` method or with the Logs Viewer page. Delete service account external keys when not needed. Grant the service account only the minimum set of permissions needed. Create service accounts for each service. Take advantage of the IAM service account API to implement key rotation.

### Cloud Identity

Cloud Identity is Identity as a Service (IDaaS) that centrally manages users and groups. It provides options for user and group management as well as identity federation with active directory. Cloud Identity provides device management options to enforce secure permissions options. Security options include two step verification as well as single sign on (SSO) integration. Cloud Identity also offers audit logs and directory management options through Google Cloud Directory Sync (GCDS).

### Cloud IAM Best Practices

**General**
* Apply only the minimal access level required
* Predefined roles should be chosen over primitive roles
* Grant roles at the smallest scope
* Child resources cannot restrict access granted on its parent
* Restrict who can create and manage service accounts
* Be cautious with owner roles

**Resource Hierarchy**
* Mirror the Google Cloud resource hierarchy structure to the organization structure
* Use projects to group resources that share the same trust boundary
* Set policies at the organization level and at the project level rather than at the resource level
* Use the security principle of least privilege to grant IAM roles
* Grant roles for users or groups at the folder level if it spans multiple projects

**Service Accounts**
* Treat each application as a separate trust boundary
* Do not delete service accounts that are in use by running services
* Rotate user managed service account keys
* Name service account keys to reflect use and permissions
* Restrict service account access
* Do not insert service account keys into source code

**Auditing**
* Use Cloud Audit Logs to regularly audit IAM policy changes
* Audit who can edit IAM policies on projects
* Export audit logs to Cloud Storage for long term retention
* Regularly audit service account key access
* Restrict log access with logging roles

**Policy Management**
* Use an organization-level policy to grant access to all projects
* Grant roles to a Google group instead of individual users wherever possible
* Create a Google group if multiple roles are granted to particular task


## Networking Services

### Networking Refresher

Information on the 7 layer OSI model. Lesson focuses on layers network (3), transport (4), and application (7) layers. Overview of IPv4 versus IPv6 addresses, CIDR ranges and prefixes, IP packets, etc.

### Virtual Private Cloud

VPCs are virtualized networks within Google Cloud as global resources. They are encapsulated within a project. While they do not have any IP address ranges, they are associated with them. Firewall rules control traffic flowing in and out of the VPC.

Resources within a VPC can communicate with one another using internal private IPv4 addresses. VPCs only support IPv4 addresses internally. Each VPC contains a default network. There are auto and custom mode VPCs. Automatically created VPCs have a subnet in each region.

### Networks and Subnets

A subnet is a subnetwork of a VPC. Each VPC network consists of one or more subnets and each subnet is associated with a region. The name or region of a subnet cannot be changed after you have created it. Primary and secondary ranges for subnets cannot overlap with any allocated range.

GCP has a special feature to increase subnet IP space without considerable maintenance on the condition that the new subnet does not overlap with other subnets, that it is inside the RFC 1918 address space, and that the network range is larger than the original. A subnet expansion cannot be undone.

There are always reserved IP addresses in a GCP subnet. The network is reserved in the first address. The default gateway is reserved in the second address. The penultimate address is for GCP future use, and the broadcast is the final address.

### Routing and Private Google Access

Routes define the network traffic path from one destination to the other. In a VPC, routes consist of a single destination (CIDR) and a single next hop. All routes are stored in the routing table for the VPC. Each packet leaving a VM is delivered to the next hop of an applicable route based on a routing order (lowest number means higher priority).

There are two types of routing on GCP: system-generated and custom routes. System-generated routes can be default or for subnet routes while custom routes support static and dynamic routes. 

The default route provides a path to the Internet and to private Google access. It can only be deleted by replacing it with a custom route. It is the lowest priority in routing order. A subnet route is a route that defines paths to each subnet in the VPC. Each subnet has at least one subnet route of whose destination matches the primary IP range of the subnet. When a subnet is created, a corresponding subnet route is created for both primary and secondary IP range. One cannot delete a subnet route unless you modify or delete the subnet.

Static routes can use the next hop feature and be created manually. Static routes for the remote traffic selectors are created automatically when creating Cloud VPN tunnels. Dynamic routes are managed by one or more cloud routers. These dynamically exchange routes between a VPC and on-premises networks. Destination IP ranges are outside the VPC network. They are used with dynamically routed VPNs and Interconnect.

Google VPC networks have special return paths that cannot be seen or configured in the route table. Private Google Access can be used to create secure routes to Google APIs and services.

### IP Addressing

The internal IP address is used only within a network and is not publicly advertized. Alias IP ranges can be configured to have multiple internal IP addresses for containers or VM applicatoins without a separate network interface. IP ranges can be assigned from the subnet's primary or secondary ranges. Alias IPs can be assigned from primary or secondary VPC network ranges.

VPCs can have static or ephemeral IP addresses. Ephemeral IPs persist as long as the life of the application, and they are automatically assigned. Ephemeral IP addresses can be promoted to static as well. Static IPs remain attached until they are explicitly released.

External IP addresses are needed to communicate with the Internet with resources in another network or a public GCP service. Sources from outside a Google Cloud VPC network can address a specific resource by the external IP address. Only resources with an external IP address can send and receive traffic directly to and from outside the network.

Internal IP address reservations can reserve a specific address and then associate it with a specific resource. Specify an ephemeral internal IP address for a resoruce and then promote the address. External IP address reservations can be created by reserving a new static external IP and assigning it to a resource or by specifying an ephemeral external IP for a resource and then promoting it.

### Firewall and Firewall Rules

GCP VPC Firewall rules are either incoming or outgoing but cannot be both. A firewall rule has a specified protocol, ports, sources, and destinations. Implied and pre-populated rules are blocking outgoing TCP traffic on port 25 and blocking TCP, UDP, ICMP, and GRE. The Metadata server (169.254.169.254) is always kept accessible.

The implied rules are allow egress and deny ingress. Firewall rules can only be applied to IPv4 address in CIDR format. A firewall rule must be associated with a VPC and cannot be shared between VPCs. Firewall rules are stateful, they cannot be configured to apply inconsistent rules to associated traffic.

Firewall rules have several components: the associated VPC, numerical priority, direction of the connection, action on match, defining the instance, source IP, source tags, source service account, protocols, and ports.

### VPC Network Peering

VPC Peering allows private connectivity across two VPC networks whether they are in the same or different projects and organizations. Peering reduces network latency, security, and costs. CIDR ranges cannot overlap between peering configurations. To allow ingress traffic from VM instances in a peer network, you must have ingress allow firewall rules. By default, there is an implied deny rule to ingress VM traffic. Transitive peering and internal DNS are not supported.

### Shared VPC

Shared VPCs span multiple projects while maintaining segmented billing and account management as needed. The Shared VPC admin has project-level and subnet-level permissions. Service projects connect to the host project through internal ephemeral or static IP addresses. Shared VPCs can be used for multiple host environments (production versus development), hybrid environments, and two tier web services.

### VPC Flow Logs

VPC Flow Logs monitor and analyze traffic coming in and out of VPCs from VM instances. Flow logs are exported in real-time for up to thirty days. They can be placed in Cloud Storage for long-term storage. One of every ten packets are captured, interpellating missing data from captured packets.

VPC Flow Logs are used for network monitoring with real-time visibility into network throughput and performance. It can analyze network usage and optimize network traffic expenses. It can be used for network forensics when incidents occur. It can provide real-time security anlaysis and stream to pub/sub and integrate with SIEM.

Flow Logs are recorded in necessary core fields while additional fields of metadata are presented in a multi-field format. These can be annotated with GKE annotations.

### Cloud DNS

Cloud DNS acts as an authoritative DNS server that allows authoritative DNS lookups. It is globally resilient. Zones are hosted through managed name servers either as public zones or private zones visible only within the network.

## Compute Engine

### Compute Engine Overview

Virtual machines launched in a VPC network. Host is available in a zone. Multi tenant host or solo tenant node. Per second billing. Each vCPU is implemented on a single hardware hyper thread on CPU. 2 GB/s per CPU.

Public images of Linux or Windows can be used or custom, private images. A third option is a marketplace solution that combines an OS with software pre-configured. 

Storage configured can be Standard HDD, Balanced SSD, or SSD. Local SSD is physically attached swap disk and is the fastest. 

Network configuration can be automatic default, or custom. They include ingress and egress firewall rules and network load balancing with regional load balancing. 


### Compute Engine Machine Types

There are general-purpose, compute-optimized, and memory-optized machine type families. Machine types have names like `e2-standard-32`. The first part is the series and generation. The second part is the type. The third is the number of vCPU's offered.

General-purpose is used for basic computing at a low cost such as web serving, microservices, etc. They can support up to 32 vCPUs and 128 GB of memory. They have the lowest on-demand pricing. 

* N1 machine types were the first general purpose machine type with up to 96 vCPUs and 624 GB of memory. Only machine type with GPU and TPU support. Larger sustained use discount than N2.
* N2 machine types are the second generation with support up to 80 vCPUs and 640 GB of memory. Their workloads can take advantage of higher clock frequency with higher per-thread performance.
* The N2D is the largest general purpose machine type with up to 224 vCPUs and 896 GB of memory. It also has a higher memory-to-core ratio than the other machine types.

Compute-optimized machine types offer ultra-high performance for compute-intensive workloads such as HPC, gaming, etc. These have the highest performance per core but cannot use regional persistent disks. These consist of the C2 machine type.

Memory-optimized machine types are best for ultra high-memory workloads that require large in-memory databases like SAP HANA. It has two generations: M1 and M2. M1 offers up to 160 vCPUs with 3844 GB while M2 offers up to 160 vCPUs with 11,776 GB of memory. These also cannot use regional persistent disks.

The shared core machine type makes uses of burstable workloads as well as cost-effective and non-resource intensive applications. E2 has a physical core available for short periods of time for CPU bursting.

Custom machine types can also be used with a user-specified number of vCPUs and memory load, but these are more expensive.

### Managing Instances

Instance lifecycle stages: provisioning, staging, running (reset), suspend -> resume, and stopping -> terminated.

During provisioning, disks are mounted and the vCPUs and memory are loaded in. During staging, internal and external IPs are attached and the system image is used to boot the instance. When the instance is running, a startup script is run and can take in metadata. Access can be set up through SSH or RDP.

Shielded VM's offer verifiable integrity of the OS to ensure there was no root or kernel sabotage. The boot process is run through Secure Boot. Virtual Trusted Platform Module (vTPM) loads in the policy to authenticate the bootup policy with a measured boot. Integrity Monitoring takes in measurements to ensure boots are secure.

SSH and RDP can be used to access VMs. SSH requires the firewall rule allow tcp:22. RDP requires the firewall rule allow tcp:3389. 

An instance can be modified, repaired, or migrated once logged in without shutting down the instance.

### Compute Engine Billing

Each vCPU and GB is billed separately based on the resource. All vCPUs, GPUs, and GB of memory are charged by the second with a one minute minimum threshold. Instance uptime is the number of seconds between when an instance is started and when it is terminated. Instances are not charged in the terminated state.

Reservations ensure that resources are available when needed. These can serve future increases in demand, planned or unplanned spikes, backup and disaster recovery, or as a buffer. Reservations include sustained use and committed use discounts. They apply only to Compute Engine, Dataproc, and GKE VMs.

Discounts can be sustained use, committed use, or pre-emptible VMs. Sustained use are automatic discounts applied to vCPU, GPU and memory usage. N2 and N2D can get up to 20% off while N1 can get up to 30% off. They are applied on incremental use over time. 

Committed used discounts are 1 or 3 year contracts for predictable or steady workloads for up to 57% off on most resources (70% discount for memory-optimized machine types). Committed use discounts are applied at the project level and can be shared across multiple projects. 

Pre-emptible VMs are up to 80% cheaper with fixed pricing. These are instances that are start or stopped depending on availability in Compute Engine, and they are always stopped within 24 hours. They have no charge if shut down in less than 10 minutes. Pre-emptible VMs have no access to live migration or auto restart, so they are useful fault-tolerant or serverless applications.

### Storage Fundamentals

The basic kinds of storage are block, file, and object.

Block storage is the fastest, most-efficient, and reliable storage type. It consists of uniquely identifiable evenly sized blocks that are usually delivered on mountable or bootable physical media such as spinning hard drive or solid state drives.

File storage is the traditional network file system. Data is received through directory trees. This structure is mountable but not bootable. Cloud File Store is the GCP solution for this kind of storage following NFSv3.

Object storage is unstructured key-value data and metadata with a globally unique identifier. It is infinitely scalable because it does not matter where an object is stored. The GCP solution for this is Cloud Storage buckets. It is neither mountable nor bootable.

Storage performance can be described in terms of I/O block size, I/O queue depth, IOPS, throughput and latency. Performance can also depend on if data is accessed sequentially or randomly.

### Persistent Disks and Local SSDs

Persistent disks and local SSDs are two kinds of block storage with different use cases. 

Persistent disks can be standard, balanced, or SSD with either zonal or regional varieties. These are like physical disks in a computer. They provide network storage and are instance-independent and can be resized while they are running. They are encrypted by default and can have up to 64 TB in disk space. Shared core machine types are limited to 16 persistent disks and 3 TB. Zonal disks are available in one zone in one region. Regional disks are stored in two zones in the same region. Regional persistent disks cannot be used with compute or memory-optimized machine types.

`pd-standard`are backed by standard hard disk drives HDD and have large data processing workloads that primarily use sequential I/Os. It is the lowest priced persistent disk but has a relatively high latency rat at the regional level. `pd-balanced` is the alternative to SSD persistent disks that balance performance and cost. It has the same maximum IOPS as ssd-pd with lower IOPS per GB. It is general purpose with a price in between standard and SSD persistent disks. `pd-ssd` is for enterprise applciations and high-performance databases that demand lower latency and more IOPS. they have single digit millisecond latency but are the highest priced persistent disk.

Local SSDs are physically attached to the server as either SCI or NVME. It has a higher throughput and IOPS and lower latency. Data persists until instance is stopped or deleted. Each local SSD is 375 GB but partitions can be inserted to raise size to 9 TB per instance.

SCSI is a classic solution but only has one queue for commands. NVME is a newer protocol designed for flash memory with 64,000 queues that can each have up to 64,000 concurrent processes. CPUs need to be scale with disk memory performance. This is because performance scales until it reaches either the limits of the disk or the limits of the VM instance to which the disk is attached.

### Snapshots

Snapshots backup and restore persistent disks and are global resources. They provide support for zonal and regional PDs, and they have incremental and automatically compressed options. Snapshots are stored in Cloud Storage. It is stored in regional or multi-regional location.

The first successful snapshot of a persistent disk is a full snapshot. Subsequent snapshots only contain data missing from previous snapshots. If a snapshot is deleted, subsequent snapshots will draw reference from the most recent available snapshot.

Snapshot schedules are the best practice for backups and must be in the same region as `pd`. Snapshot schedules can be created and then attached to a disk or they can be created attached to the disk. Schedules can be set with a retention policy and a source disk deletion rule (what happens if a source disk is deleted). A snapshot schedule cannot be edited after creation; a new one must be created.

Other things to note:
* At most there can be one snapshot every 10 minutes. 
* It is possible to eliminate excessive snapshots by creating an image instead.
* Set schedule to off-peak hours whenever possible
* Can create VSS snapshots for Windows

### Deployment Manager

A configuration is written in YAML syntax and defines the structure of a deployment. It must contain a resources section with a list of resources to create. The three required components are name, type, and properties. A name is a user-defined string. Type can be a base or composite type. A base type lists the `API.VERSION.RESOURCE` while a composite type is written as `gcp-types/PROVIDER:RESOURCE`. Properties take certain parameters for the resource type such as the `zone`, `machineType`, `boot`, and `sourceImage`.

Templates are individual building blocks of a configuration and can be written in Jinja and Python. They can be programmatically generated through abstracted template properties.

A deployment is a collection of resources that are deployed, updated, managed and deleted together. The manifest file provides instructions for the deployment and cannot be edited after the deployment has been created.

Best practices:
* Break up configurations into logical units
* Use references to enforce order when resources are created
* Preview deployments using `--preview flag`.
* Automate the creation of resources through IaaC
* Use version control

## High Availability and Autoscaling

### Cloud Load Balancers

The cloud load balancer distributes user traffic across multiple instances and provides a single point of entry with multiple backends. It is fully distributed and software defined. Load balancers can be global or regional and are configured to serve content as close as possible to users. They provde autoscaling with health checks.

Global load balancing is ideal for applications distributed across multiple regions. It provides external HTTP(S) load balancing with SSL and TCP Proxies as well. Regional load balancing handles Internal HTTP(S) and TCP/UDP traffic.

Backend services include health checks, session affinity, service time out, traffic distribution, and backends (endpoints and service groups).

The HTTP(S) load balancer distributes traffic using (1) rules based on cross-region load balancing through IP address requests or (2) content-based load balancing using URL maps. This kind of load balancer is global, proxy-based, and **Layer 7** behind a single external IP address. It is implemented on Google Front Ends (GFE) and provides HTTPS and SSL encryption in transit for both IPv4 and IPv6 traffic. IPv6 traffic terminates at the load balancer and is server as IPv4 to the back-end. Forwarding rules are in place to distribute defined targets to target pools. URL maps direct requests based on rules. SSL certificates (Google or self-managed) are required for HTTPS traffic. The ports used are 80, 8080 for HTTP and 443 for HTTPS.

SSL Proxy is a rever proxy load balancer that distributes SSL traffic coming from the Internet to VMs. It is global and external and distributes traffic by location only. It is a single Unicast IP address and operates at Layer 4. It provides support for **TCP with SSL offload** and supports IPv4 and IPv6 in the same way as the HTTP(S) load balancer. It is used for other protocols that use SSL.

TCP Proxy load balancer is a reverse proxy load balancer that distributes TCP traffic coming frmo the Internet to VMs. Client TCP sessions are terminated at the load balancer. It forwards traffic as SSL or TCP. It provides intelligent routing and routes to locations that have capacity. It has a single Unicast IP address, operates at Layer 4, is global and external, and it distributes traffic by location only. It is intended for non HTTP traffic. Supports IPv4 and IPv6 traffic in the same way as other load balancers. It supports many well-known TCP ports.

The Network Load Balancer is a regional pass-through load balancer that distributes TCP and UDP traffic to VMs. It is not a proxy and responses from backend go directly to client. It is regional and external. It supports either TCP or UDP but not both. It supports traffic on ports that are not supported by TCP proxy and SSL proxy. SSL is decrypted by backends, not by the load baalancer. Traffic is distributed by protocol, scheme and scope. No TLS offloading or proxying. Multiple forwarding rules reference one target pool. Other protocols use target instances. It can only handle self-maanged SSL certificates (**not Google-managed**).

The Internal Load Balancer is a pass-through load balancer that distributes TCP and UDP traffic to VMs. It is layer 4, regional internal, supports TCP or UDP (not both), balances internal traffic between instances, does **not balance Internet traffic** and only sends traffic to the backend directly. When using forwarding rules, it is necessary to specify at least one and up to five ports by number. It is necessary to specify `ALL` to forward traffic to all ports.

### Instance Groups and Instance Templates

An instance group is a collection of VM instances that you can manage as a single entitty. There are Managed Instance Groups (MIGs) and Unmanaged Instance Groups.

MIGs are stateless (good for website front-end, servers, batch workloads) but can be useful for stateful workloads. It is HA by offering auto-healing and can be regional or zonal. It offers scalability through load balancing and auto-scaling. It also provides auto-update functionality. MIGs include pre-emptible instances, containers, and networking.

UIGs offer load balancing for mixed types of instances but features no auto-healing, no multi-zone distribution, no autoscaling, and no auto-updating.

Instance templates are a resource used to create VM instances and MIGs. They specify machine type, boot disk image, container image, labels, and can create a MIG or VM. An instance template is required to create a group of identical instances. Instance templates cannot be changed after creation, a new one must be implemented.

## Kubernetes Engine and Containers

### GKE and Kubernetes Concepts

Kubernetes is an orchestration platform for containers that automates, schedules, and runs applications on clusters.

Google Kubernetes Engine (GKE) is Google's public cloud environment for running Kubernetes. It offers cloud load balancing, node pools, automatic scaling, automatic upgrades, node auto-repair, as well as logging and monitoring. 

Each cluster has one or more control planes, one or more nodes, and the control plane manages scheduling and maintenance while the nodes run containerized apps. The nodes are also responsible for Docker runtime. The API server manages interaction with the cluster. The kube scheduler discovers and assigns newly created pods. Kube controller manager runs all controller processes. The cloud controller manager runs controllers specific to the cloud provider. Lastly, ETCD stores the state of the cluster as a HA key value store database.

### Cluster and Node Management

Groups of node in a cluster have the same configuration. Custom node pools are useful for pods that require more resources. Node pools can be manually or automatically upgraded. Zonal clusters have a single control plane in a single zone or a control plane in multiple zones. Regional clusters have a control plane in each zone for HA. Private clusters can be blocked off from the Internet for safety.

Cluster versions can be automated to be upgraded through the release channels (rapid, regular or stable). A specific version can also be specified when creating a cluster. Control planes and nodes do not always run the same version at all times. A control pane is always upgraded before its nodes. Auto-upgrade is the best practice. Manual upgrades have more features but requires only one functionality to be upgraded at a time.

Surge upgrades control the number of nodes that can be upgraded at a time. Use surge upgrade parameters to specify how many additional nodes can be added to the pool during the upgrade (`max-surge-upgrade` and how many nodes can be available at one time `max-unavailable-upgrade`).

### Pods and Object Management

A Kubernetes object is a persistent entity that represents the state of the cluster. Each object has a name with a unique ID, and its configuration is specified through a manifest file (YAML or JSON). The manifest file should have the Kubernetes API version, the kind of obejct to be created, metadata, and the desired state `spec:` of the object.

Shared storage volumes for containers in a pod with an auto-assigned unique IP. It is recommended to use a controller to manage pods. A pod remains on the node until its process is complete. Namespaces can be used to categorize groups of objects in discrete spaces. 

Pods have a lifecycle of pending, then running, and end either in succeeded or failed states.

A deployment can run multiple replicas of pods created from a template to abstract pod configuration. Deployments are recommended over replica sets.

Workloads allow you to define the rules of replicas. Deployments run multiple replicas, StatefulSets are useful to persistent storage applications. DaemonSets ensure that every node in the cluster runs a copy of a pod. Jobs are used to run a finite task, and CronJobs run until completion on a schedule. ConfigMaps provide configuration information for any workload to reference.

### Kubernetes Services

Kubernetes networking is designed to outlast ephemeral pods and their assigned IPs. A service is an abstraction that provides a persistent IP address for internal and external traffic, load balancing, and scaling. Service components help determine endpoints through labels and selectors.

ClusterIP is the default service that is used to access nodes in a cluster through ports and target-ports. NodePort is a static port that bridges two nodes. Other Kubernetes services include Ingress, Multi-port, ExternalName, and Headless.


## Hybrid Connectivity

### Cloud VPN

Cloud VPN connects a peer network to a VPC through an IPsec VPN connection. It provides a IPsec tunnel over the public Internet. Traffic is encrypted by one VPN gateway and decrypted by the other. It is a **regional** service. It is site-to-site VPN only. No site to client option. It allows private Google Access for on-premises hosts.

It supports up to 3 GBps per tunnel and offers dynamic and static routing. Cloud VPN supports IKEv1 and IKEv2 using Shared Secret.

There are two types of Cloud VPN: Classic VPN and HA VPN.

Classic VPN:
* 99.9% SLA
* Static and dynamic routing
* One external IP address for a single interface
* Functionality depreciates in October 2021.

HA VPN:
* 99.99% SLA
* Dynamic routing only
* Two external IPs to be configured for two instances. (Better HA)
* New default VPN

Use Cloud VPN if public Internet access is needed, if peering location is not available, when there are budget constraints, when high speeds / low latency are not necessary, and when it is egress traffic from GCP.

### Cloud Interconnect

Cloud Interconnect is a low-latency HA connection that does not travel over the public Internet. It is a low latency, highly available connection between an on-premises environment and Google Cloud VPN networks. It is directly accessible to internal IP addresses through Prviate Google Access. It travels through a dedicated connection from an on-premises network. It is **not encrypted**. It is also a relatively expensive service.

Dedicated Interconnect provides a physical connection between an on-premises environment and GCP. There can be up to eight connections of 10 GBps or up to two 100 GBps connections.

Partner Interconnect provides a connection through a service provider (via Peering Edges) with VLAN attachments between 50 MBps or 50 GBps. This is not a direct connection but is useful if a Google facility is out of reach or for workloads that do not require the Dedicated Interconnect setup.

Direct Peering estabilshed a direct connection between an on-premises network and Google's edge network. It is available at 100 locations in 33 different countries. Direct egress pricing is available. Direct Peering connection is free as long as the technical requirements are met.

CDN Interconnect enables select third-party CDN providers to establish direct peering links with Google's edge network. Direct traffic from VPC networks to the provider's network. It provides reduced pricing on egress costs.

Use Cloud Interconnect to prevent traffic from traversing the public Internet. It offers a dedicated physical connection as an extension of a VPC network. It offers high speed and low latency and is suitable for heavy egress traffic from GCP. Private Google Access keeps the process secure.

## Serverless Services

### App Engine Overview

App Engine is a PaaS full managed, serverless platform to develop and host web apps through code or containers using (Python, Java, Node.js, Go, Ruby, PHP, or .NET). It can autoscale based on loads, offer versioning, and support external storage.

App Engine comes in Standard and Flexible environments. 

In standard environments, apps run in a sandbox environment where only specific versions of runtimes can be used. They run for free or at very low cost, they are designed for sudden and extreme spikes of traffic. Their pricing is based on instance hours.

In flexible environments, apps are run in docker containers, any version of runtimes can be used, no free quota is available, it is designed for consistent traffic, pricing is based on VM resources, and it hosts managed VMs.

The topological hierarchy starts with the application at the top with one or more services beneath. These services have individual versions from which each instance is run. 

App Engine automatically creates and shuts down instances. The YAML configuration file can specify a number of instances to run and a scaling type. Automatic scaling is based on request rate and response latencies. Basic scaling creates instances when the application receives requests. Manual scaling specifies the number of instances that continuously run. App Engine can migrate traffic between targeted versions (also supporting gradual "warm-up" traffic transitioning). It can also split traffic based on specified percentage loads.

### Introduction to Cloud Functions

Cloud Functions is a serverless FaaS environment that can run Python, Java, Node.js, Go, .NET core. It is event-driven with triggers such as HTTP, Pub/Sub, Cloud Storage, Firestore, and Firebase. The functions themselves are stateless.

Billing is based on time and resources. There is no free tier.

Only one trigger can be bound to a function at a time. The code is stored in a storage bucket which is accessed by CodeBuild to deploy the container registry which is in turn accessed by Cloud Functions alongside the event data. Only one concurrent request can be run at a time. The results are passed along to a VPC or the public Internet.


## Storage Services

### Cloud Storage and Storage Types

Cloud Storage is a consistent, scalable, large-capacity, highly durable object storage. It has worldwide accessibility and worldwide storage location. Use it for data files, text files, pictures, and videos. Excels for content delivery, big data sets, and backups.

Cloud Storage buckets are the basic container unit that holds data and provides data organization and access control. Buckets cannot be nested. Upon creation, they are given a globally unique name and geographic location. The name cannot be changed after creation. They can be stored in one region, dual regions, or multi-regions for geo-redundancy. A bucket cannot be changed between dual and multi regions after creation.

There is no limit to objects that can be stored in a bucket. An object contains both object data and metadata properties. Buckets can use file structure nomenclature to provide a sense of hierarchy when searching objects even though there is no explicit hierarchy.

Storage classes can be standard, nearline, coldline, archive. Standard is best for hot data as it has maximum availability with no storage duration requirement and is good for analytical workloads and transcoding. Nearline is low-cost for infrequently accessed data. It has a 30 day minimum, provides data backup and data archiving. Coldline is very low-cost for infrequently accessed data of a 90 day minimum storage duration. It offers data backup and data archiving. Archive is the lowest cost archival storage with a 365 day minimum and provides data storage disaster recovery. Data retrieval becomes more expensive the more cold the storage class.

Storage class costs:
* Standard: $0.02 per GB per month
* Nearline: $0.01 per GB per month
* Coldine: $0.004 per GB per month
* Archive: $0.0012 per GB per month

Access can be uniform or fine-grained (IAM + ACLs). IAM offers standard permissions that can be inherited hierarchically. The Access Control List defines who has access to your buckets and objects as well as their level of access. Signed URLs offer time-limited read and write access for a specified duration of time. Signed Policy Documents specify what can be uploaded to a bucket.

IAMs are recommended over ACLs and offer two levels of granularity at the project or bucket level. The roles available can be primitive, standard, or legacy. Legacy roles are equivalent to ACLs. By contrast, ACLs offer entry through a permissions with a specified scope. However, they must be used carefully as ACLs overlap IAM roles. Cloud Storage will grant the broader permission.

Signed URLs allow users without credentials to perform specific actions on a resource. Action are taken as a user or service account. No account needed, just the URL.


### Object Lifecycle Management and Versioning

Objects are immutable and cannot be changed within its lifetime. Objects are instead replaced with a new version. Object versioning retains a non-current object version when a replaced object is created. 

Because versioning can become expensive quickly, object lifecycle management can be used to maintain a manageable storage ecosystem. Objects can have a set TTL (time to live). It can delete or archive non-current versions or downgrade storage class. These features can be automated based on absolute and relative time conditions. Rules are used to create certain actions upon particular conditions.

Changes are in accordance ot object creation date. A deleted object cannot be undeleted. Lifecycle rules can take up to 24 hours to take effect. It is important to test lifecycle ruels in development first.

### Cloud SQL

Cloud SQL is a fully managed relational database service RDBMS. It is a DBaaS. It is low latency, transactional, and suited for relational DB workloads. It is compatible with MySQL, PostgreSQL, and SQL Server. It offers replciation through Read Replicas.

Cloud SQL offers on-demand and automatic backups as well as point in time recovery. It has 30TB storage capacity by default and can automatically increase storage. Data is encrypted at rest and in transit. It is billed for instance, persistent disk and egress traffic.

The user can connect to Cloud SQL through a public or private IP. It is recommended to use a Cloud SQL proxy. It is possible authorize a network or external applications to access the data.

Replication occurs from the primary instance from one zone to another. The Read Replica is read-only. Replicas can be generated in another region or in an external, on-premises environment. To create a Read Replica, the primary instance must have automated backups and binary logging enabled. One backup must have been created after logging was enabled. Replicas can be promoted through planned regional migration or for disaster recovery.

Cloud SQL offers high availability through a multi-zonal configuration with synchronous replciation between zones. If the primary instance fails, failover is initiated to the secondary instance after 60 seconds. When the primary instance is restored, a failback is initiated to redirect traffic to the primary instance. The secondary instance charges twice the rate for usage and does not have read capabilities.

Backups are stored both in the same region and another. Cloud SQL offers on-demand and automated backups. On-demand backups persist until deleted. Automated backups have a 4 hour window, occur everyday, and the seven most recent backups are retained by default. Point in time recovery allows the user to recover an instance to a specific point in time. This is done through creating a new instance.

### Cloud Spanner

Cloud Spanner is a fully managed relational database service that is both strongly consistent and horizontally scalable. It is a DBaaS that supports schemas, ACID transactions, and SQL queries. It is globally distributed and handles replicas and sharding. It provides synchronous data replication. Cloud Spanner provides automatic scaling and node redundancy. It has up to 99.999% availability.

Cloud Spanner offers data layer encryption, audit logging, and IAM integration. It is designed for financial services, global supply chain, and gaming. Pricing consists of $0.90 per node per hour plus $0.30 per GB per month.

Cloud Spanner consists of instances that are configured with a geographical location and a node count. A full copy of database is stored in each zone's node. A three node minimum is recommended. Cloud Spanner can create sharding to improve performance.

Cloud Spanner can provide up to 10,000 queries per second of reads or QPS of writes. 2 TB of storage per node. It adds nodes to increase data throughput and QPS. It scales nodes automatically using Cloud Monitorign metrics triggered by Cloud Functions.

### NoSQL Databases

GCP has four options for NoSQL: Cloud Bigtable, Cloud Datastore (legacy), Firestore for Firebase, and Memorystore

Cloud Bigtable is a fully managed, wide-column NoSQL database designed for terabyte to petabyte-scale workloads that offers low latency and high throughput. It is built for real time app serving and large scale analytical workloads. It is a regional service with automated replication that strores large amounts of single-keyed data. It adds nodes when you need them. It also offers cluster resizing and MapReduce operations. It is a relatively expensive service. Use cases include time-series data, marketing data, financial data, IoT data, and graph data. 

Cloud Datastore is a fully managed, highly scalable NoSQL document database built for automatic scaling, high performance, and ease of application deployment. It offers HA of reads and writes, has atomic transactions, automatic scaling, and SQL-like query language (GQL). It has strong and eventual consistency. There is encryption at rest. Cloud Datastore can provide local emulation of the production environment for developers. **Datastore is being retired in 2021 in favor of Cloud Firestore.** Can be used for product catalogs, user profiles, and transactions based on ACID properties.

Firestore for Firebase is a flexible, scalable NoSQL cloud database to store and sync data for client and server-sdie deployment. It stores data in documents organized by collections. It is serverless and offers multi-region replication. Queries can be retrieved at the document level without having to go through the whole collection. It provides real-time updates and offline support and is a secure option. It is a realtime database. (Firebase is a mobile app development platform that provides tools and cloud services to help enable developers to complete their work more efficiently.)

Memorystore is a fully managed service for either Redis or Memcached in-memory data store to build application caches. It is fully managed and serverless, highly available, can be scaled as needed, is secure, and it is always up to date. Use this for caching, gaming, and stream processing.

## Operations Suite

Operations suite is a suite of tools for logging, monitoring, error reporting, debugging, tracing, and profiling. It is available for GCP and AWS. It consists of VM monitoring with agents. It is available for on-premises environment and offers GCP native integration. It can be integrated with a wide ranging library of application performance technologies.

Monitoring collects measurements or metrics to demonstrate how system services are performing. It can create dashboards and charts. Workspaces are needed to use cloud monitoring. Agents are recommended to monitor VMs. Cloud Monitoring is available for GKE. Workspaces can include up to 500 policies. Alerts can be sent through email, SMS, and other third-party tools.

Cloud Logging is a central repository for log data from multiple sources. It provides real-time log management and analysis. It has tight integration with monitoring. It includes platform, system and application logs. It can export logs to other soruces. Logs Viewer only shows logs from one project. Log Entry records a status or event. Logs are a named collection of log entries within a GCP resource. Retention period how long are logs are kept. Types of logs include audito logs, access transparency, and agent logs.

Error reporting counts, analyzes, and aggregates all the errors in the GCP environment. It alerts when a new applicatoin error occurs. It is integrated into Cloud Function and GAE Standard. It includes tracking issue integration.

Debugger inspects the state of a running application in real time without stopping or slowing it down. It debugs a running application with no latency. It "snapshots" the call stack in the application. Logpoints allow you to inject logging into running services. It can be hooked into rmeote Git repos such as Github, GitLab, and Bitbucket. It can be installed on non-GCP environments.

Trace collects latency data from App Engine, HTTPS load balancers and various application APIs. It helps to understand how long it takes your application to handle incoming requests (latency). It collects latency data from cloud resources and apps. It can be integrated with GAE Standard and installed on GCE, GKE, and GAE.

Profiles continuously gathers CPU usage and memory allocation from applications without consuming a lot of memory. It needs profiling agent instead. Can be installed on GCE, GKE, and GAE. It can also be installed on non-GCP environments.
