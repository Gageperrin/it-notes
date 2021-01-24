# OpenShift Fundamentals

Notes from the [KodeKloud OpenShift course](https://kodekloud.com/courses/openshift-for-the-absolute-beginners/).

## Introduction to OpenShift

OpenShift is a product developed by Red Hat for hosting and developing enterprise-grade applications. OpenShift is Red Hat's PaaS with four different flavors. (1) Origin is the original, open soruce application container platform, (2) Online is the public application and development hosting service, (3) Dedicated is a managed prviate cluster on AWS or GCP, and (4) Enterprise is on-premises, private PaaS.

OpenShift Origin is based on top of Docker containers and the Kubernetes cluster manager with added developer and operational focused tools that enable rapid application development, deployment, and lifecycle management. Docker powers containers using re-usable images. Kubernetes powers deployment and management of images across clusters. 

Openshift builds on both of these by abstracting Kubernetes management tasks. OpenShift tools include source code management integration, pipelines, registries, software defined networking, API libaries, and organization governance.

OpenShift's architecture consists of a Kubernetes infrastructure with deployed containers from images pulled from a registry. OpenShift has a built in Container Registry. Containers are deployed in pods that grouped in nodes and at a higher level cluster deployments. Each cluster contains multiple nodes with one or more master nodes that host the scheduler, ETCD, and the API server. Deployments are networked internally and externally through services. OpenShift has a web console that can be used to manage deployments and upload code through OpenShift's CI/CD pipelines. The ETCD key-value store contains information about the deployments.

## OpenShift Concepts - Projects and Users
