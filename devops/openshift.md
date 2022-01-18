# OpenShift Fundamentals

Notes from the [KodeKloud OpenShift course](https://kodekloud.com/courses/openshift-for-the-absolute-beginners/).

## Introduction to OpenShift

OpenShift is a product developed by Red Hat for hosting and developing enterprise-grade applications. OpenShift is Red Hat's PaaS with four different flavors. (1) Origin is the original, open soruce application container platform, (2) Online is the public application and development hosting service, (3) Dedicated is a managed prviate cluster on AWS or GCP, and (4) Enterprise is on-premises, private PaaS.

OpenShift Origin is based on top of Docker containers and the Kubernetes cluster manager with added developer and operational focused tools that enable rapid application development, deployment, and lifecycle management. Docker powers containers using re-usable images. Kubernetes powers deployment and management of images across clusters. 

Openshift builds on both of these by abstracting Kubernetes management tasks. OpenShift tools include source code management integration, pipelines, registries, software defined networking, API libaries, and organization governance.

OpenShift's architecture consists of a Kubernetes infrastructure with deployed containers from images pulled from a registry. OpenShift has a built in Container Registry. Containers are deployed in pods that grouped in nodes and at a higher level cluster deployments. Each cluster contains multiple nodes with one or more master nodes that host the scheduler, ETCD, and the API server. Deployments are networked internally and externally through services. OpenShift has a web console that can be used to manage deployments and upload code through OpenShift's CI/CD pipelines. The ETCD key-value store contains information about the deployments.

## Projects and Users

A project could host hundreds of pods and clusters across multiple teams. OpenSHift projects provide isolation and grouping for resource and namespacing.

There are three kinds of users: regular, system, and service. Regular users are individuals such as developers, system users manage configurations and maintenance, and service users are configured for non-human processes. OpenShift includes an OAuth server for authentication and authorization of users. It can be configured at `/etc/openshift/master/master-config.yaml`.

## Builds and Deployments

OpenShift builds depend on a specified source code repository. When run, the build downloads from the source and builds the image before creating a deployment automatically and pushing it to a Docker registry. This deployment is then deployed. Note that OpenShift `DeploymentConfig` is not the same as a `Deployment`, but their components are similar.

Build configurations are generally built out of YAMl files. The build process creates a Dockerfile from code files that is then used to create a Docker image. One build strategy is to provide instructions for converting the code, another strategy is source-to-image which creates the image without a DockerFile. To build individual packages (`.jar`, `app.tar.gz`, and `app.gem`), use custom builds.

Use Image Streams to automatically update deployments organizing images by registries. They map Docker image to DockerFiles to maintain consistency by abstracting by image stream.

The build config file can be used to manually adjust components of the build.

Build triggers can automate build deployment so that OpenShift automatically triggers a new build whenever the code repository is updated through webhooks.

Deployments are configured to use a particular image and build. By default, deployments are rolling and update replicas one at a time automatically. Deployment history can be viewed within the web console and rollbacks can be initiated there as well.

One deployment strategy is blue/green which deploys the new "green" version alongside the old "blue" version and tests it before shifting traffic over using a load balancer. A/B gradually shifts a small percentage of traffic over to the new instances while the old ones are maintained until the upgrade is complete.

Commands:
* `oc rollout latest [deployment-name]` triggers a build.
* `oc rollout history [deployment-name]` lists a rollout history.
* `oc rollout describe [deployment-name]` describes a rollout.
* `oc rollout undo [deployment-name]` reverts to an earlier deployment.

## Networking

A Kubernetes cluster consists of master and worker nodes which each have their own assigned internal IP address. Pods have their own unique IP addresses within each node. OpenShift provides a virtual network to provide IP addresses and routing for each pod.  This is done through Open vSwitch which includes VLAN tagging, trunking, LACP, and port mirroring. The default IP for the overlay network is 10.128.0.0/14.

OpenShift includes a built-in DNS server for mapping IPs and servers. It also includes SDN plugins to assist with networking such as Nuage, Contiv, and Flannel.

External connectivity is achieved through Services and Routes. Services act as load balancers in this kind of microservices architecture. It groups pods and provides networking configuration for them. Each service gets its own IP address and DNS configuration.

Routes are used to expose the service to external users through the hostname. The source is the default strategy which provides a sticky session that directs the same source IP to the same back-end server. `roundrobin` takes turns routing requests to each back-end regardless of source. `leastconn` routes traffic to the servers with the least number of connections. 

In the console, route security can be configured for policies concerning traffic type, TLS termination, insecure traffic, as well as keys and certificates. Routes can also be used to split traffic proportionally.

The deployment controller manages the replication controller and thus the deployment scale. The OpenShift web console can be used to scale deployments up or down with a single click.

## Storage

Since Docker containers are ephemeral, persistent volumes are needed for long-term storage. OpenShift provisions storage from persistent volumes using a variety of plugins (e.g. iSCsi, NFS, AWS EBS, GCP PV, etc.).

The Catalog is the first page upon logging in that lists the different third-party services that can be integrated into OpenShift. The template contains a pre-configured list of image streams, builds, deployments, secrets, services, routes, and parameters that can be used for an OpenShift deployment. Templates are built out of YAML files to easily consolidate configuration details.
