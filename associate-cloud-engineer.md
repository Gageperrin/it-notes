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


