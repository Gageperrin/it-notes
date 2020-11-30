# Certified Kubernetes Administrator Exam Notes

## Core Concepts

### Cluster Architecture

#### Basic Cluster Architecture ####

Kubernetes hosts containers in an automated fashion providing many features for deployment, communication, and maintenance. A Kubernetes cluster consists of a set of nodes. There is a master node that manages the cluster and worker nodes that carry out various tasks. 

Within the master node, the ETCD cluster is a key value store that offers highly available information about the cluster infrastructure. The Node and Replication Controllers handle nodes and exist under the purview of the controller manager. The kube-scheduler handles rolling updates among other time-based maintenance functionality. The kube-apiserver is the chief manager of these various components. 

Within the worker nodes, the container runtime engine (most popular Docker) powers the system. The kubelet is a captain for each node, monitoring and providing reports on their respective nodes. The Kube-proxy service ensures that rules are in place on each worker node.

#### ETCD ####

ETCD is a distributed, reliable key-value store that is simple, secure and fast. To install ETCD, one only has to download the binaries, extract it, and then run the service that will enable listeners on *port 2379*.

To retrieve a key-value pair, run `./etcdctl set [key1] [value1]`.
To retrieve stored data, run `./etcdctl get [key1]`.
For more commands, run `./etcdctl`.

In Kubernetes, ETCD stores the information associated with `kubectl` including nodes, PODs, configs, secrets, accounts, roles, bindings, among other entities. It can be setup manually by downloading the binaries, but it can be installed via `kubeadm` as a pod using `kubectl get pods -n kube system`. This should be run inside the etcd-master pod. In a HA environment, specify the different instances of the ETCD service since they all run on port 2379.

#### `kube-api` Server ####

`kube-api` server is the primary management component. Any `kubectl` command reaches out to `kube-api`.

When creating a pod, the request is authenticated and validated through the `kube-api` which retrieves the data from ETCD and then updates it. The scheduler notices the new pod and places it on the right node, passing the informaiton onto the `kube-apiserver` which passes the information along to the target node's `kubelet`.

View `api-server` options by running `cat /etc/systemd/system/kube-apiserver.service`.
To see the running process, run `ps -aux | grep kube-apiserver`.

### API Primitives

#### Kube Controller Manager ####

The kube controller manager monitors node status and remediates situations. There are many controllers but these are the two most important:

The Node-Controller communicates through the `kube-apiserver`. It checks the nodes every 5 seconds but has a grace period of 40 seconds. After 40 seconds, it gives the pod an eviction time out of 5 minutes upon which it removes the unhealthy pod.

The Replication-Controller creates and provisions new pods as necessary.

View `controller-manager` options by running `cat /etc/systemd/system/kube-controller-manager.service`.
View running processes and effective options at `ps -aux | grep kube-controller-manager`.


#### Kube Scheduler ####

The scheduler looks at each pod and finds the best node for the pod. It filters and then ranks the nodes from a score of 0-10 based on the amount of available resource remaining after attaching the pod.

To install, download the binary, extract, and run.

To view the options, run `cat /etc/kubernetes/manifests/kube-scheduler.yaml
To view running processes and available options, run `ps -aux | grep kube-scheduler`

#### Kubelet ####

Kubelets are the directors of the worker nodes and the only point of contact with other nodes or components.

The kubelet registers the node (i.e. its container runtime engine) with the `kube-apiserver`. It creates pods and monitors them, reporting to the `kube-apiserver`. `Kubeadm` does *not* deploy kubelets. Kubelets must always be manually installed.

View kubelet options by running `ps -aux | grep kubelet`.


#### Kube Proxy ####

A pod network is an internal virtual network that spans all the nodes in the cluster, enabling communication between them. Normally, a service is used to expose databases. However, a service cannot be joined into the pod network directly, so a `kube-proxy` is needed to connect a service to the the pod network.

It can do this through IP-table rules that redirects IP routing properly.

It is installed by downloading the binary, extracting it, and running it as a service.

To view it, run `kubectl get pods -n kube-system`.
To view the deployed daemon set, run `kubectl get daemonset -n kube-system`.

#### PODs ####

[Next lesson]


### Services & Other Network Primitives

## Scheduling

### Labels & Selectors
### Resource Limits
### Manual Scheduling
### Daemon Sets
### Multiple Schedulers
### Scheduler Events
### Configure Kubernetes Scheduler


## Logging & Monitoring

### Monitor Cluster Components
### Monitor Cluster Component Logs
### Monitor Applications
### Application Logs


## Application Lifecycle Management

### Rolling Updates and Rollbacks in Deploy
### Configure Applications
### Scale Applications
### Self-Healing Application


## Cluster Maintenance

### Cluster Upgrade Process
### Operating System Upgrades
### Backup and Restore Methodologies


## Security

### Authentication & Authorization
### Kubernetes Security
### Network Policies
### TLS Certificates for Cluster Components
### Images Securely
### Security Contexts
### Secure Persistent Value Store

## Storage

### Persistent Volumes
### Access Modes for Volumes
### Persistent Volume Claims
### Kubernetes Storage Object
### Configure Applications with Persistent Storage

## Networking

### Pre-Requisites
### Networking Configuration on Cluster Nodes
### Service Networking
### POD Networking Concepts
### Network Loadbalnacer
### Ingress
### Cluster DNS
### CNI


## Design and Install a Kubernetes Cluster

### Install Kubernetes Master and Nodes
### Secure Cluster Communication
### HA Kubernetes Cluster
###
### Provision Infrastructure
### Choose a Network Solution
###
### Run and Analyze an end-to-end test
### Node end-to-end tests


## Install Kubernetes the kubeadm way

## Troubleshooting

### Application Failure
### Worker Node Failure
### Control Plane Failure
### Networking

## Other Topics

