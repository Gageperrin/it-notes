# Certified Kubernetes Administrator Exam Notes

## Core Concepts


### Basic Cluster Architecture

Kubernetes hosts containers in an automated fashion providing many features for deployment, communication, and maintenance. A Kubernetes cluster consists of a set of nodes. There is a master node that manages the cluster and worker nodes that carry out various tasks. 

Within the master node, the ETCD cluster is a key value store that offers highly available information about the cluster infrastructure. The Node and Replication Controllers handle nodes and exist under the purview of the controller manager. The kube-scheduler handles rolling updates among other time-based maintenance functionality. The kube-apiserver is the chief manager of these various components. 

Within the worker nodes, the container runtime engine (most popular Docker) powers the system. The kubelet is a captain for each node, monitoring and providing reports on their respective nodes. The Kube-proxy service ensures that rules are in place on each worker node.

### ETCD

ETCD is a distributed, reliable key-value store that is simple, secure and fast. To install ETCD, one only has to download the binaries, extract it, and then run the service that will enable listeners on *port 2379*.

To retrieve a key-value pair, run `./etcdctl set [key1] [value1]`.
To retrieve stored data, run `./etcdctl get [key1]`.
For more commands, run `./etcdctl`.

In Kubernetes, ETCD stores the information associated with `kubectl` including nodes, PODs, configs, secrets, accounts, roles, bindings, among other entities. It can be setup manually by downloading the binaries, but it can be installed via `kubeadm` as a pod using `kubectl get pods -n kube system`. This should be run inside the etcd-master pod. In a HA environment, specify the different instances of the ETCD service since they all run on port 2379.

### `kube-api` Server

`kube-api` server is the primary management component. Any `kubectl` command reaches out to `kube-api`.

When creating a pod, the request is authenticated and validated through the `kube-api` which retrieves the data from ETCD and then updates it. The scheduler notices the new pod and places it on the right node, passing the informaiton onto the `kube-apiserver` which passes the information along to the target node's `kubelet`.

View `api-server` options by running `cat /etc/systemd/system/kube-apiserver.service`.
To see the running process, run `ps -aux | grep kube-apiserver`.


### Kube Controller Manager

The kube controller manager monitors node status and remediates situations. There are many controllers but these are the two most important:

The Node-Controller communicates through the `kube-apiserver`. It checks the nodes every 5 seconds but has a grace period of 40 seconds. After 40 seconds, it gives the pod an eviction time out of 5 minutes upon which it removes the unhealthy pod.

The Replication-Controller creates and provisions new pods as necessary.

View `controller-manager` options by running `cat /etc/systemd/system/kube-controller-manager.service`.
View running processes and effective options at `ps -aux | grep kube-controller-manager`.


### Kube Scheduler

The scheduler looks at each pod and finds the best node for the pod. It filters and then ranks the nodes from a score of 0-10 based on the amount of available resource remaining after attaching the pod.

To install, download the binary, extract, and run.

To view the options, run `cat /etc/kubernetes/manifests/kube-scheduler.yaml`
To view running processes and available options, run `ps -aux | grep kube-scheduler`

### Kubelet

Kubelets are the directors of the worker nodes and the only point of contact with other nodes or components.

The kubelet registers the node (i.e. its container runtime engine) with the `kube-apiserver`. It creates pods and monitors them, reporting to the `kube-apiserver`. `Kubeadm` does *not* deploy kubelets. Kubelets must always be manually installed.

View kubelet options by running `ps -aux | grep kubelet`.


### Kube Proxy

A pod network is an internal virtual network that spans all the nodes in the cluster, enabling communication between them. Normally, a service is used to expose databases. However, a service cannot be joined into the pod network directly, so a `kube-proxy` is needed to connect a service to the the pod network.

It can do this through IP-table rules that redirects IP routing properly.

It is installed by downloading the binary, extracting it, and running it as a service.

To view it, run `kubectl get pods -n kube-system`.
To view the deployed daemon set, run `kubectl get daemonset -n kube-system`.

### PODs

A POD is a single instance of an application. It is the smallest unit in Kubernetes.

If node usage increases, the node can launch another identical pod to share the workload. If the node is at maximum capacity, a new node can be provisioned with a pod launched inside.

There are multi-container pods. Helper containers can be run inside the pod and can share network or storage.

To see a list of pods, run `kubectl get pods`.

### ReplicaSets

Replication controllers are used to run multiple instances of the same pod to preserve high availability and assist with load balancing and scaling. Replica sets are newer and more recommended over the legacy replication controller.

Replication controller:
```
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end

spec:
  template: [POD template goes below]
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: frontPOD
       spec:
        containers:
          name: nginx-container
          image: nginx

replicas: 5

```
Then run, `kubectl create -f [file].yml`
To see controller, run `kubectl get replicationcontroller`.


Replica set:
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  label:
    app: myapp
    type: front-end
spec:
  template: [POD template goes below]
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: frontPOD
       spec:
        containers:
          name: nginx-container
          image: nginx
          
replicas: 5
selector:
  matchLabels:
    type: front-end

```
Then run, `kubectl create -f [file].yml`
To see replica set, run `kubectl get replicaset`.

Replica sets can manage pods that it has not created.

#### Labels and Selectors ####

Pods have labels that allow replica sets and other components to select pods based explicity on these labels.

### Deployments

Kubernetes deployments enable consistent uptime even with pod failure or during updates.

A deployment definition file is identical to the replicaset set except for `kind: Deployment`.

To create a deployment run, `kubectl create -f deployment-definition.yml`
To see replica set, run `kubectl get rdeployments.`

### Namespaces

Namespaces cluster entities into their respective groups (e.g. Default, `kube-system`, `kube-public`) that provide granularity and isolation so that names are not accidentally called across groups. Each namespace can be assigned a quota as well.

`mysql.connect("db-service.dev.svc.cluster.local")`
Corresponds to ("service-name.namespace.service.domain")

To create a pod in another namespace, run `kubectl create -f pod-definition.yml --namespace-dev`.
To set an alternative default namespace for a pod, change the YAML file:
```
...
metadata:
  name: myapp-pod
  namespace: dev
...
```

Create a namespace with a YAML file:
```
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```
Or by running `kubectl create -f namespace-dev.yml`.


### Services

Kubernetes services allow communication between applications and users.

A service like NodePort maps a port on the node to a port on the pod. The pod has the TargetPort on port 80 while the Service has the Port on Port 80. The NodePort faces outside the node and has a port range of 30000-32767.

```
## service.definition.yml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service

spec:
  type: NodePort
  ports:
   -targetPort: 80
    port: 80 [if not included, it is assumed to be the same as targetPort]
    nodePort: 30008 [automatically allocated by default]
  selector:
    app: myapp
    type: front-end
```
Run `kubectl create -f service-definition.yml`.
To see services, run `kubectl get services`.
Access the server through a browser or run, `curl http://192.168.1.2:[port]`.

### Services - Cluster IP

Because pods are constantly re-provisioned, IPs can be unreliable. A service clusters the IPs together for various pods to provide a consistent IP for external communication.

```
service.definition.yml

apiVersion: v1
kind: Service
metadata:
  name: back-end

spec:
  type: ClusterIP
  ports:
  - targetPort: 80
    port: 80
    
  selector:
    app: myapp
    type: back-end
```

### Services - Load balancer

The load balancer routes traffic to individual nodes. It is easiest to integrate with native cloud functionality. Under `spec:` change `type: NodePort` to `type: LoadBalancer`.

### Imperative vs. Declarative

An imperative IaaC provides steps to create an infrastructure. The declarative only states the requirements for what the infrastructure should have. The declarative approach is implemented with Ansible, Terraform, etc. and takes less manual work for repetitve workloads (if used correctly). Only the `kubectl apply` command is needed for the declarative approach.

Commands to know for Imperative approach:
`--dry-run` a resource is created from this. `--dry-run=client` to test it.
`o yaml` will output the resource definition in YAML format.
`kubectl run nginx --image=nginx` creates an NGINX pod.
`kubectl run nginx --image=nginx --dry-run=client -o yaml` generates a pod manifest file without creating the pod.

`kubectl create deployment --image=nginx nginx` creates a deployment.
`kubectl create deployment --image=nginx nginx --dry-run -o yaml` generates a deployment YAML file without generating deployment.
`kubectl create deployment nginx --image=nginx --replicas=4`

`kubectl scale deployment nginx --replicas=4` to create a deployment with 4 replicas.
`kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx.deployment.yaml` 

`kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml` creates a service of type ClusterIP.
`kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml` will do the same but without using the pod's labels as selectors.

`kubectl expose pod nginx --port=80 --name nginx-service --dry-run=client -o yaml` to create a service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes.
`kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`

### `kubectl apply` Command

The apply command takes into account the local file, the last applied configuration, and the live Kubernetes configuration. If a field is missing in the local file but shows up in the last applied configuration, it will then be removed from the live configuration.

The JSON file with the last applied configuration is stored on the live object configuration under `metadata:` -> `annotations:`.

## Scheduling

### Manual Scheduling

Without a scheduler, one has to set the node name fields in the specification file, but this can only be specified at creation time. If it is already created, one can create a binding object with a PUSH request to update the pod's specification. With this approach, the configuration has to be converted to JSON.

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

