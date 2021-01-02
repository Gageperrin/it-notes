# Google Cloud - Associate Cloud Engineer

## Google Cloud Fundamentals

### Google Cloud Global Infrastructure

A user request is sent to a Point of Presence edge network which is then passed over a private fiber network to a Google Data Center with the lowest amount of latency possible.

A zone is a deployment area for Google Cloud in a region. It offers optimal latency because of its precise deployment proximity to users. A region is an independent geographic area that contains multiple zones. There is sub 5ms latency between zones in ar region which makes it useful for fault tolerance and high availability. Multi-regions contain two or more regions. It serves content to consumers outside of the Google network.

### Compute Service Options

* Virtual Machines are called instances
* Choose the region and zone to deploy to
* Chose the operating system and software to put on it
* Public and private instances can be used to create instances

Google Kubernetes Engine (GKE) is Google's container orchestration system for automating, deploying, scaling, and managing contaienrs. It is built on open source Kubernetes and has the flexibility to integrate with on-premises Kubernetes. It uses compute engine instances as nodes in a cluster. A cluster is a group of nodes or Compute Engine instances. It is CaaS.

App Engine is a fully managed, serverless platform for developing and hosting web applications at scale (PaaS). It provisions servers and scales your app instances based on demand. It connects with other Google services seamlessly as well as Web Security Scanner. It is PaaS.

Cloud Functions is a serverless execution environment for building and connecting cloud services. It consists of single, simple-purpose functions that are closed upon completion. It is FaaC.

Cloud Run is a fully managed compute platform for deploying and scaling containerized applications quickly and securely. It is built upon an open standard Knative. It abstracts away all infrastructure management. It is known as serverless for containers. It is FaaC.

### Storage and Database Options

Cloud Storage is consistent, scalable, large-capacity, highly durable **objcet** storage. It has 11 9's of durability. Unlimited storage with no minimum object size. Use Cloud Storage for content delivery, data lakes, and backup. It is available in different storage classes and availability.

Storage Classes:
* Standard: Maximum availability, no limitations
* Nearline: Low-cost archival storage, accessed less than once a month
* Coldline: Even lower cost archival storage accessed less than once a quarter
* Archive: Lowest cost archival storage for less than once a year

Availability: Region, Dual-region, and multi-region

Filestore is a fully managed NFS file server that is NFSv3 compliant to store data from running applications to use with VM instances or Kubernetes clusters.

Persistent disks is durable block storage for instances. Standard versus Solid State (SSD) options as well as zonal and regional options.

Database options include Cloud SQL and Cloud Spanner for relational database services. For NoSQL, there is Bigtable, Datastore, Firestore, and Memorystore.

### Networking Services

VPC is virtualized network within Google Cloud, and it is a core networking service. It is a global resource.

Firewall Rules and Routes govern traffic coming into instances as well as provide advanced networking functions.

There is HTTP(S) Load Balancing and Network Load Balancing. The former distributes traffic across regions based on traffic type. The latter distributes traffic based on IP protocol data within a region.

Google DNS publishes and maintains DNS records by using the same infrastrucutre that Google uses.

Advanced connectivity options include Clodu VPN and Direct Interconnect. Direct Peering exchanges Internet traffic between the business network and Google while Carrier Peering connects the infrastructure to Google's network edge.

### Big Data Services

BigQuery is a fully managed petabyte scale, low cost analytics data warehouse. It can ingest data through streaming and batch as well as through free bulk loading. It provides real time analytics, autoamtic high availability, automatic backup and restore, standard SQL, and a big data ecosystem integration. It includes IAM, VPC managemnet, and data encryption.

Pub/Sub is a real-time fully-managed messaging service. Send and receive messages between independent applications. A publisher creates a messages and sends them to a topic which contains the messages that are passed along to those contianed in a subscription.

Composer is a fully managed orchestration service built on Apache Airflow.

Dataflow is a fully managed processing service for executing Apache Beam pipelines for batch and realtime data streaming. It takes data in from a source and read transforms it into a PCollection, transformed and processed again, before it is write-transformed into a sink. A DataFlow job runs through this pipeline.

Dataproc is a fully managed spark and hadoop service that can be used to replace on-premise Hadoop infrastructure.

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

Principle of Least Privilege: A user, program or process should have the bare minimum privileges necessary to perform its function.

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

Cloud Identity is Identity as a Service (IDaaS) that centrally manages users and groups. It provides options for user and group management as well as identity federation with active directory. Cloud Identity provides device management options to enforce secure permissions options. Security options include two step verification as well as single sign on (SSO) integration. Cloud Identity also offers auditing log and directory management options through Google Cloud Directory Sync (GCDS).

### Cloud IAM Best Practices

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

Resources within a VPC can communicate with one another using internal private IPv4 addresses. VPCs only support IPv4 address internally. Each VPC contains a default network. There are auto and custom mode VPCs. Automatically created VPCs have a subnet in each region.

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

GCP VPC Firewall rules are incoming or outgoing, not both. A firewall rule has a specified protocol, ports, sources, and destinations. Implied and pre-populated rules are blocking outgoing TCP traffic on port 25 and blocking TCP, UDP, ICMP, and GRE. The Metadata server (169.254.169.254) is always kept accessible

The implied rules are allow egress and deny ingress. Firewall rules can only be applied to IPv4 address in CIDR format. A firewall rule must be associated with a VPC and cannot be shared between VPCs. Firewall rules are stateful, they cannot be configured to apply inconsistent rules to associated traffic.

Firewall rules have several components: associated VPC, numerical priority, direction of the connection, action on match, defining the instance, source IP, source tags, source service account, protocols, and ports.

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

## Kubernetes Engine and Containers

### GKE and Kubernetes Concepts

Kubernetes is an orchestration platform for containers that automates, schedules, and runs applications on clusters.

Google Kubernetes Engine (GKE) is Google's public cloud environment for running Kubernetes. It offers cloud load balancing, node pools, automatic scaling, automatic upgrades, node auto-repair, as well as logging and monitoring. 

Each cluster has one or more control planes, one or more nodes, and the control plane manages scheduling and maintenance while the nodes run containerized apps. The nodes are also responsible for Docker runtime. The API server manages interaction with the cluster. The kube scheduler discovers and assigns newly created pods. Kube controller manager runs all controller processes. The cloud controller manager runs controllers specific to the cloud provider. Lastly, ETCD stores the state of the cluster as a HA key value store database.

### Cluster and Node Management

Groups of node in a cluster have the same configuration. Custom node pools are useful for pods that require more resources. Node pools can be manually or automatically upgraded. Zonal clusters have a single control plane in a single zone or a control plane in multiple zones. Regional clusters have a control plane in each zone for HA. Private clusters can be blocked off from the Internet for safety.

Cluster versions can be automated to be upgraded through the release channels (rapid, regular or stable). A specific version can also be specified when creating a cluster. Control planes and nodes do not always run the same version at all times. A control pane is always upgraded before its nodes. Auto-upgrade is the best practice. Manual upgrades have more features but requires only one functionality to be upgraded at a time.

Surge upgrades control the number of nodes that can be upgraded at a time. Use surge upgrade parameters to specify how many additional nodes can be added to the pool during the upgrade (`max-surge-upgrade` and how many 
nodes can be available at one time `max-unavailable-upgrade`.

### Pods and Object Management

A Kubernetes object is a persistent entity that represents the state of the cluster. Each object has a name with a unique ID, and its configuration is specified through a manifest file (YAML or JSON). The manifest file should have the Kubernetes API version, the kind of obejct to be created, metadata, and the desired state `spec:` of the object.

Shared storage volumes for containers in a pod with an auto-assigned unique IP. It is recommended to use a controller to manage pods. A pod remains on the node until its process is complete. Namespaces can be used to categorize groups of objects in discrete spaces. 

Pods have a lifecycle of pending, then running, and end either in succeeded or failed states.

A deployment can run multiple replicas of pods created from a template to abstract pod configuration. Deployments are recommended over replica sets.

Workloads allow you to define the rules of replicas. Deployments run multiple replicas, StatefulSets are useful to persistent storage applications. DaemonSets ensure that every node in the cluster runs a copy of a pod. Jobs are used to run a finite task, and CronJobs run until completion on a schedule. ConfigMaps provide configuration information for any workload to reference.

### Kubernetes Services

Kubernetes networking is designed to outlast ephemeral pods and their assigned IPs. A service is an abstraction that provides a persistent IP address for internal and external traffic, load balancing, and scaling. Service components help determine endpoints through labels and selectors.

ClusterIP is the default service that is used to access nodes in a cluster through ports and target-ports. NodePort is a static port that bridges two nodes. Other Kubernetes services include Ingress, Multi-port, ExternalName, and Headless.


