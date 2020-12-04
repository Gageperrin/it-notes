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


### Labels and Selectors

Labels and selectors can be used to filter and select items based on one or multiple criteria. Labels are properties assigned to an object. Selectors filter based on that property.

Label syntax:
Under `metadata:`
```
  labels:
    app: App1
    function: Front-end
```

To select it, run `kubectl get pods --selector app=App1`.

Labels and selectors can also be used for other components such as ReplicaSet. Annotations can be used to add more information as needed.

### Taints and Tolerations

Taints and tolerations apply restrictions to what can be done on nodes. Nodes have taints, and pods have toleration. A taint placed on the node would exclude all pods that do not have a toleration for that particular taint. A toleration must be added to the pod.

To apply a taint to a node, run `kubectl taint nodes node-name key=value:taint-effect`

There are three kinds of taint effect:
`NoSchedule`: Excludes a pod from the node
`PreferNoSchedule`: Only possibly excludes a pod from the node
`NoExecute`: New pods will not be scheduled, current ones will be evicted if they do not have the corresponding tolerance.

To apply a toleration to a pod, run `kubectl taint nodes node1`.
To see a taint, run `kubectl describe node kubemaster | grep Taint`

### Node Selectors

Node Selectors are added under `spec:` as:
```
nodeSelector:
  size: Large
```

To label a node, run `kubectl label nodes <node-name> <label-key>=<label-value>`.

Node Selectors are limited and cannot account for exclusionary or disjunctive conditions.

### Node Affinity

Node affinity ensures that particular pods are hosted on particular nodes. 

However it has more complex syntax than node selectors:

```
...
 spec:
  ...
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoreDuringExecution:
        nodeSelectorTerms:
         - matchExpressions:
          - key: size
            operator: In
            values:
            - Large
```

Node Affinity Types:

* `requiredDuringSchedulingIgnoredDuringExecution`
* `preferredDuringSchedulingIgnoredDuringExecution`

Under development as of December 2020:

* `requiredDuringSchedulingRequiredDuringExecution`
* `preferredDuringSchedulingRequiredDuringExecution`

`DuringScheduling`: The state when the pod does not exist and is created for the first time
`DuringExecution`: When the pod is live.

Required will not place a pod if no matching node is found. Preferred will still place a pod in the event that no matching node is found. In the future, the `RequiredDuringExecution` will evict pods from nodes as needed.

### Taints & Tolerations vs. Node Affinity

Taints and tolerations does not ensure that a pod ends up in a particular node, it is primarily for exclusion. Node affinity ensures that particular pods end up in the right node, but it does not guarantee that other pods will not end up in the node as well. A combination of the two should be used for the most granular selectivity.

### Resource Limits

Each pod has specific resource limits in terms of CPU, memory, and disk. Each resource request assumes at least .5 CPU and 256 Mi of memory are needed. These defaults can be changed in the pod definition.

```
spec:
  containers:
    ...
     resources:
        requests:
          memory: '1Gi'
          cpu: 1
```

.1 CPU is the minimum (also called 100m). 1 CPU is 1 AWS vCPU, 1 GCP Core, 1 Azure Core, or 1 Hyperthread.

The default maximum limits are 1 vCPU and 512 Mi. If a pod exceeds resources, the pod will throttle the CPU. If a pod repeatedly consumes more memory than it has been allotted, the pod will be terminated.

Memory syntax:
`1 G` is a gigabyte.
`1 M` is a megabyte.
`1 K` is a kilobyte.
`1 Gi` is a Gibibyte (1,073,741,824 bytes)
`1 Mi` is a Mebibyte (1,048,576 bytes)
`1 Ki` is a Kibibyte (1,024 bytes)

### Daemon Sets

Daemon sets are like replica sets, but they run one copy of each pod on each node. It ensures that one copy of a pod is always present in the node of any given cluster. This is optimal for monitoring and logging. Kube proxies can be deployed as a daemon set.

This used to be done by giving the pod a special property with each nodeName to land on each node. From v1.12, daemon sets uses Node Affinity.

Syntax:
```
daemon-set-definiton.yaml

apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon
spec:
  selector:
    matchLabels:
      app: monitoring-agent
    template:
      metadata:
        labels:
          app: monitoring-agent
      spec:
        containers:
        - name: monitoring-agent
          image: monitoring-agent
```

To create, run `kubectl create -f daemon-set-definition.yaml`.
To view, run `kubectl get daemonsets`.

### Static PODs

Kubelets can be configured to read pod definition files locally within the host rather than through the Kube API server or other cluster components. The directory of the local pod configuration folder is found in `kubelet.service` as `--pod-manifest-path=...\`. Alternatively, this can be defined as `config=kubeconfig.yaml` in `kubelet.serice` with a corresponding path of `staticPodPath: /etc/Kubernetes/manifest` in `kubeconfig.yaml`.

Static pods are still listed in the master node even if not generated through the API server. It is best to use static pods to deploy control plane configuration files (e.g. `controller-manager.yaml`) across nodes.

To recap, static pods are created by the kubelet and deploy control plane components as static pods. DaemonSets are created by the kube-api server and deploy monitoring agents and logging agents on nodes. Both are ignored by the Kube Scheduler.

### Multiple Schedulers

It is possible to specify a scheduler in `kube-scheduler.service` as `--scheduler-name=[scheduler]`. With `-kubeadm`, copy the `kube-scheduler.yaml` file and then add a line under `spec:` -> `containers:` -> `- command:` that specifies scheduler name `- --scheduler-name-my-custom-scheduler`.

Further Reading:
* https://github.com/kubernetes/community/blob/master/contributors/devel/scheduler.md
* https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes
* https://jvns.ca/blog/2017/07/27/how-does-the-kubernetes-scheduler-work/
* https://stackoverflow.com/questions/28857993/how-does-kubernetes-scheduler-work


## Logging & Monitoring

### Monitor Cluster Components

As of December 2020, Kubernetes does not include native all-encompassing monitoring solutions, but open-source solutions are available such as Metrics Server, Prometheus, Elastic Stack, DataDog, and Dynatrace.

Heapster was originally the primary monitoring solution for Kubernetes (and has been referenced in many places), but it has been deprecated in favor of Metrics Server. There can be one Metrics Server per Kubernetes cluster. It is an in-memory monitoring solution that does not save on disk. It does not offer historical data.

Kubelets contain a cAdvisor that retrieves performance metrics and exposes them on a port for Metrics Server.

Run `kubectl top node` and `kubectl top pod` to get node and pod metrics respectively.


### Managing Application Logs

Run `kubectl logs -f [pod-name]` to get the logs for a pod. `-f` streams the logs live. If there are multiple containers in a pod, the logs command must explicitly specify which container it should retrieve logs for.



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

