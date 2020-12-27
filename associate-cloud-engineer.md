# Google Cloud - Associate Cloud Engineer

## Account Management

### Resource Hierarchy

A resource can be service level (databases) or account level (organization, folders). Google Cloud's resource hierarchy structure is a parent child relationship. Policies are controlled by IAM. Access control policies and configuration settings on the parent are by default inherited by the child. Each child object has exactly one parent.

Resources are contained in projects which are contained in folders which are contained in the organization root node that is itself hosted in the domain at the cloud level.

## Controlling Costs and Budget Alerts

Committed Use Discounts are discounts available when the user commits to a minimum level of resources for a specified term of one to three years. These can be spend-based or resource-based, and the commitment fee is billed monthly. Spend-based is a consistent amount of usage while Resource-based is for compute engine resources in a region. It is available for vCPU, Memory, GPU, and Local SSD with up to 57% discounted for most resources. Memory-optimized machine types can have up to 70% off.

Sustained-use discounts are automatic discounts fo running compute engine resources for a significant portion of the billing month. It applies to vCPUs and memory and VMs created by GKE, but it does not apply to App Engine flexible, Dataflow and E2 machine types.

The GCP Pricing Calculator and Cloud Billing Budgets can help monitor and predict overall and specific costs. Pub/sub can be used for programmatic notifications and to automate cost management tasks.

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


