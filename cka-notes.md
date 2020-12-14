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

### Rolling Updates and Rollbacks

Each rollout of an update is called a revision.

To see status of rollout, run `kubectl rollout status deployment/myapp-deployment`.
To see history of rollouts, run `kubectl rollout history deployment/myapp-deployment`.
To undo rollout, run `kubectl rollout undo deployment/myapp-deployment`.

There are two types of deployments: recreate and rolling. Recreate takes the application down and updates the parts. Rolling update switches components one at a time to maintain availability. Kubernetes does rolling update by default.


### Commands and Arguments in Docker

Containers exit upon completing its assigned task or process.

Can add `sleep 100` to keep a container live for 100 more seconds after running. A sleep function can be built into an image file. An entrypoint instruction can specify a value on start-up, such as the length of sleep for a sleep command. Entrypoints and CMD should be specified in JSON format.

### Commands and Arguments in Kubernetes
  To append arguments to an image container. Add `args:` in JSON format under `spec:` -> `containers:`


### ENV Variables in Kubernetes
Within a definition file, environment variables are set under `spec:` -> `containers` -> `env:` with associated `- name:` and `value:`.

With ConfigMap, environment variables are given a `valueFrom:` with `configMagKeyRef:`.

### Configure ConfigMaps in Applications

ConfigMaps are used to pass configuration information in the form of key:value pairs to individual definition files. It abstracts and streamlines configuration when there are many different configuration files.

To create a ConfigMap with the imperative approach, run `kubectl create configmap` with `<config-name> --from-literal=<key>=<value>`. The `from-literal` can be repeated to add more key-value pairs. Alternatively, one can create from a file using `app-config --from-file=app_config.properties`.

The declarative approach uses `kubectl create -f config-map.yaml` with a YAML file.

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod
```

Each ConfigMap should be named appropriately to maximize clarity.


### Secrets

Secrets are like ConfigMaps but are stored in a hashed format. 

There are two ways to create a secret:
Imperative: `kubectl create secret generic && <secret-name> --from-literal=<key>=<value>`
Declarative: `kubectl create -f \ && app-secret --from-literal=DB_HOST=mys`

To declare from a file:
Imperative: `<secret-name> --from-file=<path-to-file>`
Declarative: `app-secret --from-file=app_secret.properties`

Declarative file:
```
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: mysql
  DB_User: root
  DB_Password: paswrd
```
 
  To see current secrets, run `kubectl get secrets`.
 To see information on secrets, run `kubectl describe secrets`.
 To see encrypted secret information, run `kubectl get secret app-secret -o yaml`.
 
 To inject the secret in a pod add the following lines to the pod definition file:
 `spec:` -> `containers:` ->
 ```
 envFrom:
  - secretRef:
      name: app-secret
  ```
  
Secrets are encoded in base64 format and are therefore not very secure, but this functionality does provide base-level protection. Better tools include Helm Secrets and HashiCorp Vault.


### Multi-Container PODs

Multi-container pods can be developer to break up and segment workloads. They can be specified in the pod definiton file as follows:

```
spec:
  containers:
    - name: simple-webapp
      image: simple-webapp
      ports:
        containerPort: 8080
    - name: log-agent
      image: log-agent
```


### Init Containers

Normally, each container runs a process that stays alive as long as the pod's lifecycle. There are times though when it is necessary to run a process that runs to completion in a container such as when the pod is first created or when waiting for an external service. 

`initContainers` can solve this. An `initContainer` is run when the pod is first created and is completed before the container that hosts the application actually starts. Multiple `initContainers` can be configured at once to run in a sequential order.

Example syntax:
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```

### Self-Healing Application

Self-healing is supported through ReplicaSets and Replication Controllers. Kubernetes provides additional support to monitor the health of running applications through Liveness and Readiness probes. (These are a CKAD topic.)


## Cluster Maintenance

### OS Upgrades

When upgrading a pod, it has 5 minutes before it is by default considered dead and then terminated. To perform maintenance, it is important to keep track of how long an upgrade will take. 

It is best practice to
1. `kubectl drain node-1` to move pods to another node.
2. `kubectl cordon node-1` to ensure new pods are not scheduled on the node.
3. `kubectl uncordon node-1` so pods can be scheduled on the node again.


### Kubernetes Releases

Versioning is divided as follows: v1.11.13 (Major.Minor.Patch). In addition to stable releases are alpha and beta releases. Even if Kubernetes components are bundled under one single version package, individual components such as ETCD cluster and CoreDNS are their own projects and will have their own distinct versioning.

### Cluster Upgrade Process

No component should have a version higher than the Kube API server. `Controller-manager` and `kube-scheduler` can be one minor version lower, and `kubelet` and `kube-proxy` can be two versions lower. One exception is `kubectl` which can run at one version higher or lower than the Kube API server.

Kubernetes only supports three minor releases at a time. It is best practice to upgrade one minor version at a time. The process depends on the cluster configuration.

With Google Cloud, it is relatively easy to upgrade through the GUI or by running `kubeadm upgrade plan` or `kubeadm upgrade apply`. Self-managed Kubernetes solutions require more work.

To upgrade `kubeadm`, run `apt-get upgrade -y kubeadm=[version]` then to upgrade cluster components, run `kubeadm upgrade apply [version]`.

The general steps for upgrading each respective node:
1. `kubectl drain node-1`
2. `apt-get upgrade -y kubeadm=[version]`
3. `apt-get upgrade -y kubelet=[version]`
4. `kubeadm upgrade node config --kubelet-version v1.12.0`
5. `systemctl restart kubelet`
6. `kubectl uncordon node-1`

### Backup and Restore Methods

Backups can be made through resource configurations or ETCD clusters.

Resource configurations can take the imperative or declarative method, as seen above. Declarative is preferred for long-term convenience, and these should be stored on GitHub. One can also query the `kube-apiserver` for undocumented resource configurations. Velero is one tool that can automatically collect and store configurations.

ETCD cluster stores information about the cluster itself. These can be backed up through snapshots `snapshot save [name]`. They can be restored with `snapshot restore [name]` and a specified path. This builds a new cluster. A new cluster token must be specified to avoid misconfiguration.  Run `systemctl daemon-reload` and `service etcd restart`and then `service kube-apiserver start` to finish the restore process.

Alongside the other ETCD commands, specify the certificates, endpoints, and the key.

### Working with ETCDCTL

`etcdctl` is a CLI for ETCD. To use `etcdctl` make sure the API is set to version 3. This can be done by exporting the variable `ETCDCTL_API` prior to using the etcdctl client through `export ETCDCTL_API=3`.

On the master node run:
```
export ETCDCTL_API=3
etcdctl version
```

To take a snapshot of etcd, run `etcdctl snapshot save -h`. If the ETCD database is TLS enabled, the following are mandatory: `--cacert`, `--cert`, `--endpoints=[127.0.0.1:2379]` and `--key`.


## Security

### Kubernetes Security Primitives

The `kube-apiserver` is the first line of defense. The authentication mechanisms decide who can access the server thorugh users, passwords, tokens, certifications, LDAP, etc. The authorization mechanisms decide what they can do and include RBAC, ABAC, Node, as well as webhook mode.


### Authentication & Authorization

There are two general types of user accounts: administrators and developer and then there are service accounts for bots. User accounts cannot be created in Kubernetes but service accounts can.

Users are managed by `kube-apiserver` from a static password file, a static token file, certificates, identity services, or some combination of these. Static password files and static token files can be stored as a .csv file with password, user, and user ID columns. These can be passed to the `kube-apiserver` with the option `--basic-auth-file=user-details.csv` in the `kube-apiserver.service` file. To authenticate a user with a command run `curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"`. With a token file use the option `--token-auth-file=user-details.csv`. 

These are not recommended authentication mechanisms. Consider a volume mount while providing the auth file in a `kubeadm` setup.

### TLS Basics

Symmetric encryption means a single key is used to encrypt and decrypt the data. Asymmetric encryption means a public and private key pair. The former is used to encrypt the data and the latter to decrypt it. SSH uses asymmetric encryption.

Certificate authorities sign and validate certificates for browser traffic. The Certificate Signing Request is sent to the authorities, is validated, and then they sign and send the certificate.

A Public Key Infrastructure (PKI) is used to generate and manage and exchange certificates between the server, client, and certificate authority.

### TLS in Kubernetes

There are three kinds of certificates: root certificates (CA), client, and server. The public key is a `.crt` or `.pem` file. The private key is a `.key` or `-key.pem`.

For a secure connection in the cluster, one must have server certificates for servers and client certificates for clients for each component in the cluster. 

Naming conventions: The `kube-api server` should have an `apiserver.crt` and an `apiserver.key`. The ETCD Server should have an `etcdserver.crt` and an `etcdserver.key`. The kubelet server should have `kubelet.crt` and `kubelet.key`. 

The client certificates for clients are the users accessing the `kube-api server`. The admin, the scheduler, the `kube-proxy` and the controller manager should all have certificates and keys. Since the ETCD server only talks to the `kube-api server`, the `kube-api server` should have its own certificate and key as well.

Server certificates are needed for the ETCD server, the `kube-api server` and the kubelet server.

#### Certificate Creation ####

Certificate authority:
To generate keys, run `openssl genrsa -out ca.key 2048`.
To generate a certificate signing request, run `openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr`.
To sign certificates, run `openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt`.

Client certificates for admin user:
To generate keys, run `openssl genrsa -out admin.key 2048`.
To generate a certificate signing request, run `openssl req -new -key admin.key -subj \ "/CN=kube-admin" -out admin.csr`.
To sign, run `openssl x509 -req -in admin.csr -CA.crt -CAkey ca.key -out admin.crt`.

The same process for other components, but the scheduler, proxy, and controller manager must be prefixed with the keyword `system`.

For server side certificates:

ETCD Servers:
Same procedure as before, with peer ETCD servers, then peer certificates must be generates. The keys must be specified in the `etcd.yaml` file.

The `kube-api` server is the most requested component. In its case:
1. Run `openssl genrsa -out apiserver.key 2048`.
2. Run `openssl req -new -key apiserver.key -subj \ "/CN=kube-apiserver" -out apiserver.cs`
3. Create an `openssl.cnf` for all alternate names with corresponding DNS and IPs. Pass it as an option when generating the certificate signing request.
4. Sign with `openssl x509 -req -in apiserver.csr \ -CA ca.crt -CAkey ca.key -out apiserver.crt`

The `kubectl` nodes require a server certificate for each node, named after each node. Authentication and TLS certification must be specified in each node's configuration file.

### View Certificate Details

There are two ways to generate certificates, the hard way and through `kubeadm`. If the Kubernetes is deployed from scratch all the certificates are generated manually as seen above. `kubeadm` deploys certificates automatically as pods.

Run `openssl x509 -in [cert file path] -text -noout` to view the certificate. The name, alternative names, the certificate authority, applicable organization, and valid dates are the most important fields.

To troubleshoot, run `journalctl -u etcd.service -1` or (if set up with `kubeadm`), run `kubectl logs etcd-master`.

### Certificates API

The CA server is wherever the CA files are stored. By default, `kubeadm` stores CA files on the master node. Kubernetes can automate the certification process through its API. The admin can create a `CertificateSigningRequest` object which can review and approve requests to share certificates with users.

The administrator creates the `CertificateSigningRequest` object as a YAML file and then encoded using the `base64` command. Then run `kubectl get csr` to get the request and run `kubectl certificate approve [name]` to approve it. View the certificate with `kubectl get csr jane -o yaml` and decode it with `echo [encrypted cert] | base64 --decode.`

The controller manager carries out these tasks.

### KubeConfig

In a KubeConfig file one can specify certificates and the server. By default this is placed in `$HOME/.kube/config`.

The config file has three sections: clusters, contexts, and users. Contexts bridge clusters and users, deciding how users access clusters.

Example:
```
apiVersion: v1
kind: Config

clusters:
- name: my-kube-playground
    cluster:
      certificate-authority: ca.crt
      server: https://my-kube-playground:6443
 
context:
- name: my-kube-admin@my-kube-playground

users:
- name: my-kube-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
```

Run `kubectl config view` to see the current configuration.
To update context, run `kubectl config use-context [context-name]`.

Certificate credentials can be specified by a path or by the `certificate-authority-data` field which passes in base64 encoded certificate data.


### API Groups

There are several API groups:
* `/metrics`
* `/healthz`
* `/version`
* `/api`
* `/apis`
* `/logs`

Core: `/api`
Named: `/apis`

Core group contains namespaces, pods, events, endpooints, nodes, configmaps, secrets, services, etc. The named group is more organized with sub-groups that contain resources as a sub-category. Each resource contains several actions known as verbs:
* `/apps` -> `/deployments`, `/replicasets`, `/statefulsets`
* `/extensions`
* `/networking.k8s.io` -> `/networkpolicies`
* `/storage.k8s.io`
* `/authentication.k8s.io`
* `/certificates.k8s.io` -> `/certificatesigningrequests`

To retrieve API groups in a cluster, run `curl http://localhost:6443 -k`.
To retrieve resources in a group run `curl http://localhost:6443/apis -k | grep "name"`.

To access cluster API, authentication is required by passing in certification files through the CLI or by launching a `kubectl` proxy process through `kubectl proxy `.

Kube proxy is **not** `kubectl` proxy. Kube proxy is used to establish connectivity between parts and services across different nodes.  `kubectl` proxy is an HTTP proxy service created by the `kubectl` utility to access the `kube-api` server.


### Authorization

The Node authorizer grants privileges within a cluster. An ABAC (Attributes Based Access Control) gives users a set of permissions with a policy file written in JSON format. To update this, the file must be changed manually and the server restarted. RBAC (Role Based Access Control) is much more convenient and defines roles with certain permissions. Users are assigned to these roles. A modified role reflects on all users falling under that role. Webhook outsources authorization mechanisms to a third party such as Open Policy Agent. The last two modes of authorization are `AlwaysAllow` and `AlwaysDeny`.

The mode is specified in the `kube-apiserver` file. By default, it is set to `AlwaysAllow`. Mechanisms are invoked sequentially according to the order specified in the config file.


### Role Based Access Controls

To create a role, create a role object YAML file.

Example:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

Roles are linked to a user through a `RoleBinding` object.

To see created roles, run `kubectl get roles`.
To see role bindings, run `kubectl get rolebindings`.
To get more information, run `kubectl describe role [name]`. or `describe rolebinding [name]`.

To check for access, run `kubectl auth can-i [verb] [object] --as [role]`.


### Cluster Roles

Cluster roles and rolebindings are namespaced. Namespaces help group and divide resources. Cluster scope resources such as nodes cannot be namespaced. So there are cluster scoped and namespaced resources. `clusterroles` are roles for cluster scoped resources.

### Image Security

Images are stored in a registry. under an account and repository. A repository can be made private so that it can only be accessed with credentials.

Login on docker with `docker login private-registry.io`.
Then run the image with `docker run private-registry.io/apps/internal-app`.
Create a secret with `kubectl create secret docker-registry regcred`.

### Security Contexts

Security context can be configured on containers and/or pods. Containers inherit security contexts from pods.


### Network Policies

Web traffic comes into the web server at port 80, the web server sends the request to the API server at port 5000 which retrieves information from the database server at 3306. Ingress traffic comes into the server, egress traffic leaves the server. By default, all traffic is allowed between pods in a cluster.

Network policies are another kind of object in Kubernetes that specifies rules for ingress and egress traffic. Labels and selectors are used to associate policies to pods.

Not that Flannel does not support network policies.

### Kubectx and Kubens

Kubectx and Kubens are tools that streamline working in different namespaces and contexts. Kubectx switches between contexts without relying on the long `kubectl config` commands.

To list all contexts, run `kubectx`. To switch to a new context, run `kubectx <context>`, and to switch back run `kubectx -`. To see the current context, run `kubectx -c`.

Kubens allows users to switch between namespaces quickly. To switch to a new namespace, run `kubens <namespace>` and to switch back run `kubens -`.


## Storage

### Storage in Docker

Docker stores data by default in `/var/lib/docker`. Docker builds entities in a layered architecture. For example the base Ubuntu layer beneath changes in `apt` packages beneath changes in `pip` packages beneath the source code, etc.

The container layer is read-write while the image layer beneath is read-only. Docker can have volumes that are mounted to a container.

### Volume Driver Plugins in Docker

Storage drivers manage storage on images in containers. Volume drivers are for persistent data and handled by volume driver plugins.

### Container Storage Interface

Kubernetes developed container runtime interface to orchestrate communication with containers such as Docker, rkt, and cri-o. To integrate various networking solutions, the container network interface was developed for Kubernetes. The container storage interface was developed to allow different storage solutions to work with Kubernetes.

CSI is not Kubernetes exclusive but was intended to be a universal standard for any container orchestration tool. 

### Volumes

Kubernetes pods are transient, so volumes are attached to retain data permanently.

A volume can be configured in the pod configuration file with a `hostPath` for storage.

### Persistent Volumes

In a large environment, it can become difficult to configure storage on each individual pod. Persistent volumes abstract this process out of the pod configuration file.

Example:

```
pv-definition.yaml

apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-vol1
spec:
  accessModes:
    - ReadWriteOnce
  capacity:
    stroage: 1Gi
  
  awsElasticBlockStore:
    volumeID: <volume-id>
    fsType: ext4
```

Run `kubectl create -f pv-definition.yaml` to create the persistent volume.
Run `kubectl get persistentvolume` to see which persistent volumes are available.


### Persistent Volume Claims

Persistent volumes and persistent volume claims are two separate objects. The claims are used to bind requests to volumes. Kubernetes matches volumes and volume claims by sufficient capacity, access modes, volume modes, and storage class. Labels and selectors can be used to pair specific volumes and claims.

Example:
```
pvc-definition.yaml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  
  resources:
    requests:
      storage: 500Mi
```

Create it with `kubectl create -f pvc-definition.yaml`.
Delete with `kubectl delete persistentvolumeclaim myclaim`.

When a claim is deleted, by default the volume is set to retain until the user deletes it. It can be configured to delete the volume alongside the claim, or it can recycle the volume by scrapping the data when the claim is deleted. 

Persistent volume claims can be specified in a podfile under `spec:` -> `volumes:` -> `persistentVolumeClaim:`.

### Storage Class

Static provisioning requires manual configuration of storage on the external service. Dynamic provisioning can automatically provision resources on cloud or other storage solutions using storage classe objects.

Example:
```
sc-definition.yaml

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: google-storage

provisioner: kubernetes.io/gce-pd
```

Parameters can also be passed in to customize the provisioned storage solution as well, but this depends on the provisioner's specifications.



## Networking

### Pre-Requisites
#### Switching, Routing, Gateways ####

When on the same network, System A connects to System B through a switch that is physical or logical. Run `ip link` to list host interfaces. To add a link, run ` ip addr add <IP> <link>`. To see IPs assigned to host interfaces, run `ip addr`.

Routers connect systems on different networks. A router is assigned an IP address for each network it is connected to. To see existing routing configuration, run `route`. To add a route, run `ip route add <ip> via <route-ip>`. Routes must be configured on all systems.

Gateways connect routers to the Internet and are helpful for organizing routers. If no gateway is needed, configure `0.0.0.0` for gateway. 

Routing table configuration can be used to send packets through intermediate systems. By default, Linux does not forward packets unless explicitly specified. Run `cat /proc/sys/net/ipv4/ip_forward` which returns a Boolean to see if forwarding is enabled.

#### DNS ####

DNS assigns names to IP addresses. Locally this is done in `/etc/host` and is effective for small scale solutions. For larger systems, abstract DNS to a dedicated server. Specify the target server in `cat /etc/resolv.conf` as `nameserver     192.168.1.100`. 

If there are conflicting names, by default the system will prioritize the local configuration over the DNS server.

Domain names are broken down to several parts. `.` is the root, `.com` is the top level domain name, `google` is the domain name, `www.` is the sub-domain.

Record types are stored in the DNS server. `A` maps a name to the IPv4 address. `AAAA` maps a name to the IPv6 address. `CNAME` maps to another name.

`nslookup` looks up basic DNS information for a name. `dig` can do this as well in more detail.

CoreDNS is a common solution for retrieving the most used DNS requests. The binary can be downloaded and by default it listens on port 53 (the default port for a DNS server).

#### Network Namespaces ####

Namespaces create isolation between containers. The underlying host however can still see other processes and containers. 

To create a new network namespace, run `ip netns add [name]`. To list them, run `ipnetns exec [name] ip link` or `ip -n [name] link`. The same is true for an `arp` table or routing table.

To create a cable, run `ip link add veth-[red] type veth peer name veth-[blue]` then `ip link set veth-[red] netns [red]` and `ip link set veth-[blue] netns blue`. To delete, run `ip -n [red] link del veth-[red]`. The other end of a link is automatically deleted.

With multiple namespaces, create a virtual network switch within the host. There are multiple ways to do this. With Linux Bridge, run `ip link add v-net-0 type bridge` then `ip link set dev v-net-0 up`. Connect the namespaces to this bridge  with `ip link add veth-[red] type veth peer name veth-[red]-br`. Attach each end of the bridge with `ip link set veth-[red] netns [red]` and `ip link set veth-[red]-br master v-net-0`.

The bridge gateway can be connected to a gateway as well to connect to the Internet.

#### Docker Networking ####

With a `--network none` the Docker container is entirely isolated. With a `--network host` then the container is available on the host by default. With a `--network bridge` the container connects to a bridge that adjudicates between various containers and allots containers an internal IP.

Docker creates a namespace, a pair of interfaces and attaches them everytime a new container is created. To allow external applications to access a container, create port mappings from port 8080 to the appropriate container. Docker manages the IP tables as well.

#### CNI ####

A container networking interface (CNI) provides a universal standard for container bridge networking. CNI defines a set of responsibilities for container runtimes and what plugins must be invoked upon creation and deletion. The plugin also has specified requirements for supporting certain arguments, parameters, and manage IP address assignment.

Supported plugins include bridge, vlan, ipvlan, macvlan, and windows as well as DHCP and host-local. Third party solutions include Weaveworks, Flannel, VMware NSX. Docker is not on this list because it is based on the container network model (CNM). Docker must be created without a network and then have the bridge plugin invoked. Kubernetes does this automatically.


### Cluster Networking

Kubernetes clusters have master and worker nodes The host must have a unique hostname and MAC address. Some ports must also be opened. The master must be able to listen on port 6443. The kubelets listen on the port 10250. Kube-scheduler requires 10251 and the kube-controller-manager requires 10252. The various services should have 30000-32767 open too. The ETCD server needs port 2379 open.


### Pod Networking

Kubernetes does not come with a pre-built solution for pod networking, it must be configured. Every pod should have an IP address and should be able to communicate with every other pod in the same and other nodes without NAT.

Each node should get a bridge network with its own subnet. To attach a container to the bridge network, use `ip link add [veth-pair]`. Attach the pair and assign the IP address. Bring up the interface so the second container is connected to the first. 

To reach other pods on other nodes, add IP routes between nodes. For larger network architectures, use a router as a default gateway between nodes.

### CNI in Kubernetes

CNI plugin is configured in `kubelet.service`. View `kubelet` options in `/etc/cni/net.d`.

### CNI WeaveWorks

WeaveWorks is a CNI solution that is well suited for handling a wide array of routes. WeaveWorks places agents within each node. These agents intercept packet and, using their familiarity with the overall agent network, re-packages them to pass them along more effectively. 

Weave can be deployed as services, daemons, or even pods. Deploying as a daemon-set ensures sufficient pods are always available.

### `ipam weave`

IP addresses and subnets are managed by the CNI plugin for bridge networks. The easiest way to do this is to store IPs in a text file.

This configuration can be scripted in `cat /etc/cni/net.d/net-script.conf` under `ipam` by specifying the appropriate CNI plugin.

### Service Networking

When a service is created, it is accessible from all pods in a cluster regardless of their respective nodes. This is ClusterIP. NodePort is used for access outside the cluster by assigning the node an IP with an exposed port.

Each kubelet service watches the changes in a cluster through the `kube-api`. If a pod is created, the CNI is invoked to provide the proper networking. Services are not provisioned this kind of networking service when they are created.

When created, a service object is assigned an IP address from a pre-defined range. The proxy components on each node inherit an IP address and automatically generate port forwarding rules. `kube-proxy` creates and manages these rules. So service networking falls under this category of IP assignment through IP tables.

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

`kubeadm` provides an easy, efficient process for provisioning individual components of a Kubernetes cluster.

### Provision VMs with Vagrant

1. Download the appropriate Vagrant file and ensure valid IPs are specified for the VM
2. Run `vagrant up` to provision the containers.
3. Run `vagrant status` to ensure the system is up.
4. Run `vagrant ssh kubemaster` to SSH into the master node.
5. Run `uptime` or some other command to test connectivity.
6. Run `logout` and repeat steps 4-6 for other nodes.
7. Proceed to bootstrap a cluster with `kubeadm` below.

### `kubeadm` Deployment

1. Ensure pre-requisites are met.
2. Run `lsmod | grep br_netfilter` to see if the kernel module is loaded to bridge IP traffic. If not, run `sudo modprobe br_netfilter`.
3. Run a set of commands [insert documentation link] to configure IP table mapping.
4. Install container runtime.
5. After installation, run `systemctl daemon-reload` and `systemctl restart docker` to restart with the proper configurations.
6. Configure the `kubadm` cluster by initializing the `control-plane` node. Run `kubeadm init` with the appropriate parameters.
7. Create a `.kube` directory and apply the right permissions.
8. Add worker nodes to the cluster then check the status.
9. Test by creating pods.

## Troubleshooting

### Application Failure
### Worker Node Failure
### Control Plane Failure
### Networking

## Other Topics

