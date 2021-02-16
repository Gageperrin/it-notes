# Docker

These are my notes from [Kodekloud's Docker Certified Associate course](https://kodekloud.com/courses/1203948).

## Docker Engine

Docker was released in 2013 including a Docker CLI, the REST API, and the Docker Daemon. In the past LXC (Linux containers) were used to create isolated containers. This was replaced by `libcontainer` written in Go to reduce Docker's dependency on kernel versions. After that, container standards were developed and include runtime and image specification. `runC` was then developed to run containers. `containerd` was separated out as a daemon to manage containers. `containerd-shim` was added to make containers daemonless. 

Docker manages images, networks, containers, and volumes. Images are read-only templates. Containers are a read-write instance of an image. Networks help containers communicate. Containers are ephemeral so volumes can help persist data. Docker Registry is a repository for images. Docker is normally stored in `/var/lib/docker`.

When `docker container run -it ubuntu` is run, the API passes it into the Docker Daemon and downloads the image if necessary. `containerd` converts the image into a container which is handed over to `containerd`. `runc` then boots the containers.

`systemctl start docker` is used to launch docker while `systemctl status docker` can be used to check the status. `systemctl stop docker` stops the service. It can be started manually with `dockerd` for troubleshooting or debugging purposes (can use `--debug` flag). When it launches, the Docker containers listens on a Unix socket (communicates between different processes on the same host) through `/var/run/docker.sock`. To make the Docker daemon listen on an interface, run the flag `--host=tcp//[IP]:[Port]`. The TCP Socket makes it possible for the Docker Daemon to be accessible outside the host. To enable TLS encryption run `--tls=true` with TLS cert and key flags. The default Daemon configuration file is in `/etc/docker/daemon.json` so that parameters do not need to be passed in as flags.

Example commands with the newer syntax:
* `docker image build` builds an image.
* `docker container attach ubuntu` to attach an image to a container.
* `docker container kill ubuntu` to kill a container's image.
* `docker container ls` lists details for a running container. `-a` lists all running containers.

Containers exit as soon as their process is completed.

Once inside a container, use `CTRL+p+q` to go into detached mode where a container runs in the background. Use `CTRL+C` to exit and stop a container. Run `docker container exec` to run a command on the container. Run `docker container exec -it` to re-enter a container. `docker containers stats` gets the main stats for each container. `docker container top` lists the processes in a container.

To temporarily pause a container, run `docker container pause` and resume it with `docker container unpause`. To stop it, run `docker container stop web`. Docker will use a graceful cease command for a certain grace period before killing the container if necessary. `docker container prune` deletes all stopped containers and reclaims space. `docker container run --rm` will remove a container as soon as it exits.

To set a container hostname run `docker container run -it --hostname=webapp [image]`.

Container restart policies include `NO`, `ON-FAILURE`, `ALWAYS`, and `UNLESS STOPPED`.

To copy a file from a host to a container, run `docker container cp SRC_PATH DEST_PATH`. The source and destination can be swapped as needed.

To access a Docker container from a browser, a user can access it through the internal IP if the Host has a browser and the ports are mapped throguh `docker run -p 80:5000 [container]` for example. The host port could be removed from the command, and Docker will assign the port to a random local port. `-P` automatically publishes a port on all the host ports.

Docker relies on Linux's IP tables to manage port mappings. To see the IP rules created by Docker for a particular container, run `iptables -t nat -S DOCKER`.

To troubleshoot a Docker daemon, there are several steps to follow:
* Make sure to point the environment `DOCKER_HOST` variable to the right IP and port. (2375 for unencrypted traffic, 2376 for encrypted).
* Check service status by running `systemctl start docker` or `systemctl status docker`.
* View logs using `journalctl -u docker.service` or in tbe daemon configuration file `/etc/docker/daemon.json`.
* Free disk space on the host by running `df -h` or `docker container prune`. Can also prune images.
* Debug in Docker with `docker system info`.

Logging drivers implement Docker's various logging mechanisms. The Docker logging driver determines where logs are stored. By default logs are stored in `/var/lib/docker/containers/[container-id]/[container-id].json`.

When running a container, use the `--log-driver` flag to specify a custom log driver. `docker container inspect` can retrieve `HostConfig` and `LogConfig` information as well within a JSON format.


## Docker Image Management

Images are stored in an image registry. The official Docker image registry is Dockerhub. An image registry can be deployed inside an organizatoin using Docker Trusted Registry. Other options include Google Container Registry, Amazon Container Registry, and Azure Container Registry. There are three tiers of images: official, verified, and user. Images can be tagged for version labeling purposes, by default images are tagged `latest` when added.

To see a list of images available on the present host, run `docker image ls`. Possible flags include `--limit` and `--filter`. To pull an image from a repository, run `docker image pull`.

There are certain image addressing conventions in Docker. The general syntax is `[registry]/[account]/[image]`. The first two are optional, and be default `docker.io` is assumed for the registry and that the account and image names are shared for the second value.

Registries can be public or private. To authenticate into a Docker registry run `docker login docker.io` for Docker's registry for example. An image can be pushed to a remote registry using `docker image push`. To create a local version of an image, retag an image with `docker image tag [old-image-name] [new-image-tag].` Images cannot be recreated, only retagged and then pushed to a private registry where an isolated version can be kept. `docker system df` shows how much storage is taken up by images and how much is reclaimable from them.

To remove an image, run `docker image rm` after making sure that an image is not currently running on a container. To remove all unused images, run `docker image prune -a` but should be used carefully. 

To inspect an image, run `docker image ls` for a high level view. A deeper view can be provided by `docker image history` to look at image activity. For specific details on a specific image, run `docker image inspect`. JSON Path expressions can be used to extract particular data snippets from JSON output.

To save and load images, it is possible to use .tar files. Run `docker image save [image] -o [name].tar`. Transfer the .tar file to its destination (e.g. an environment without access to Docker Hub) and load it with `docker image load`. A container can be exported as an image using `docker export [container] > [name].tar` and imported with `docker image import [name]`.

Custom images can be used to containerize applications. To create an image:
1. Start with an OS
2. Update the repositories
3. Install dependencies using apt
4. Install Python dependencies using pip
5. Copy source code to `/opt`
6. Run the web server using `flask`.
7. Create a Dockerfile from this and run `docker build . -f Dockerfile -t [name]`.

Example:
```
FROM Ubuntu

RUN apt-get update && apt-get -y install python

RUN pip install flask flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```

It is also possible to build a custom image using `docker commit` from an existing container. Commit the state of a container using `docker container commit -a "message" [container-name] [new-name]`. This is not recommended because this process is not maintainable.

Build contexts determine where the Docker build configuration comes from. Essentially it is the path for the build configuration. This can be a local file or a remote URL. Build caching can save layers of the build for subsequent executions. Packages can be combined in a single line to make the build more clear. The `ADD` directive has more features than the `COPY` but `COPY` is still best practice in most cases.

The `CMD` determines the default command run in the container upon launch. By contrast, `ENTRYPOINT` appends the command line parameters to the CLI rather than merely replacing the command. A default value can be configured by combining the `ENTRYPOINT` instruction with `CMD` as the parameter in a JSON format.

Images can be built on top of parent images, in an ongoing parent-child relationship until it reaches the base image which is the image built from scratch.

Multi-stage builds build from a Dockerfile before extracting data from the first image to populate a second-stage build. This second build is then containerized for production. A multi-stage build will have multiple `FROM`'s in a single Dockerfile. The `COPY` command is used to copy information from the first image to the second. Multi-stage builds optimize Dockerfiles and keep them easy to read and maintain. They help reduce image size, avoid maintaining muliple Dockerfiles, and removes intermediate images.

Best practices tips:
* Do not build images that combine multiple applications, use modular images that each solve one specific problem.
* Do not store persistent data in a container but in an external volume or caching service.
* Create slim and minimal images.
* Only install necessary packages.
* Maintain different images for different environments.
* Use multi-stage builds to create lean production ready images.
* Avoid sending sensitive data to the build context.

## Docker Engine - Security

Someone with access to the Docker daemon can delete existing containers hosting applications, delete volumes storing data, and run containers to run their applications. It is also possible to gain root access to the host by running a privileged container. And they can target the other systems in the network and the network itself.

Add the host function to the `daemon.json` and expose the Daemon host properly only on private interfaces. Communication can be configured iwth certificates using a CA authority. Set Docker environment variables to target these certificates. When the required components are ready, set `DOCKER_TLS` to true. 

Docker uses namespaces to isolate workspaces. PID is a process namespace, and when processes start in a child container system, they also have the same PID. But because there is a namespace separating the parent and child systems, the PID is not confused. CGroups can allocate CPU, memory, network bandwidth, and block I/O.

Docker uses a set of Linux security protocols to ensure that the container root does not have root privileges on the host. The `cap-add` option can allow the container root to access the host with all privileges enabled.

CGroups can allocate CPU, memory, network bandwidth, and block I/O, and these can be used to restrict resource consumption. Resource limits can be used to restrict CPU usage or create reservations depending on computing resources. When a kernel detects that there is not enough CPU, it will start killing processes even native processes on the host. When there are two processes running in parallel that need the same CPU processor, the first process runs for a few cycles then it switches to the second one, etc. These are concurrent processes. The process scheduler manages this. 

Docker v1.13 and later supports a Realtime scheduler which handles restricting resources to containers. In the CLI this can be managed with the `--cpu-shares` option. The host can also restrict individual container CPU usage with`--cpuset-cpus=`.

## Docker Engine - Networking

Bridge is the default network that a container is attached to. It can also be attached to `none` and `host`. Bridge is an internal private network created on the host with IPs usually in the 172.17.0.0 series. They can reference each other using their internal IPs. To communicate outside the container they can be mapped to ports or associate the container to the `host` network. The latter removes any network isolation. When this happens multiple containers cannot be run on the same port. `none` means they are completely isolated with no networking.

User-defined networks can be used to create interanl networks with `docker network create --driver [driver] --subnet [subnet range] [name]`. Inspect the network settings with `docker inspect [container]`. Use embedded DNS to resolve connections between containers. Network namespaces are used with virtual ethernet pairs to network containers.

Commands:
* `docker network create -d bridge [name]` to create a bridge network. `-d` is short for `--driver`.
* `docker network ls` lists available networks.
* `docker network inspect` provides more information such as IP subnet and gateway and driver.
* `docker network connect [network] [container]` connects a container to a custom network.
* `docker network rm` to remove a network
* `docker network prune` removes unused networks.

### Namespaces

Namespaces provide isolated networking interface for work environments. Namespaces can be linked with a virtual cable. A virtual network switch is used when there are multiple containers. Linux Bride and OpenvSwitch can be used as this switch. A route needs to be set up from the switch for external connectivity to a NAT gateway.

Commands:
* `ip netns add` to add a network namespace.
* `ip netns` to list namespaces.
* `ip netns exec [ns] ip link` to execute `ip link` in a namespace. Can also do `ip -n [ns] link`.
* `ip link add [veth] type veth peer name [veth2]` to create a virtual ethernet cable. 
* `ip link set [veth] netns [name]` to set a virtual ethernet link point on a namespace.
* `ip -n [ns] link del [veth]` to delete one end of the link (and automatically the other end).
* `ip link add [v-net-0] type bridge` to set a virtual network switch.
* `ip link set dev [v-net-0] up` to enable the switch. Then set virtual ethernet cables between the namespaces and the network switch.
* `iptables -t nat -A POSTROUTING -s [source-IP] -j MASQUERADDE` to set up a NAT gateway for external connectivity.
* `iptables -t nat -A PREROUTING --dport 80 --to-destination [destination-IP] -j DNAT` to set up port forwarding rules for ingress traffic.


## Docker Storage

Docker creates a file structure at `/var/lib/docker` with subdirectories such as `aufs`, `containers`, `image`, and `volumes`. 

To recap, Docker has a layered architecture consisting of (1) the base OS layer, (2) changes in `apt` packages, (3) changes in `pip` packages, (4) source code, and (5) to update the entrypoint. Layers 1-5 are read-only. Docker only builds the layers that have not already been provisioned in the cache.

The sixth layer is the container layer where changes are both read/write, but the information persists only as long as the container is alive.

Create a Docker volume to persist data beyond the container's lifecycle. Docker uses storage drivers such as AUFS, ZFS, Overlay, and others to configure storage. Docker automatically chooses the best storage driver based on the configured OS. Volumes can be specified to be mounted as read only if desired.

Commands:
* `docker volume create` to create the volume
* `docker run -v [vol-name]:/var/lib/[file-path] [image-name]` to create a container with a mounted volume. Note this is legacy syntax.
* `docker run --mount type=bind,source=[source-container],target=/var/lib/[path] [image` also mounts the volume and is preferred because it is more verbose.
* `docker volume inspect` to inspect the volume.
* `docker volume remove` to remove a volume, this can only be done if it is umounted.
* `docker volume prune` can remove unused volumes.

## Docker Compose

`docker compose` builds a Docker container out of a YAML file to bring up an entire application stack in one command. This only works for running Docker on a single Docker host. Example:
```
// docker-compose.yml

redis:
  image: redis
db:
  image: postgres:9.4
vote:
  build: ./vote <- builds the container from another file
  ports:
   - 5000:80
  links:
   - redis
result:
  image: result-app
  ports:
   - 5001:30
  links:
   - redis
worker:
  image: worker
  links:
   - redis
   - db
 ```

Docker compose has multiple versions which must be specified at the top of the file. Version 2 automatically creates a bridge network and connects container to that network without any links or other networking specification. Version 2 also introduces a `depends on` feature to explicitly formulate dependencies.

Version 3 is the most recent version (February 2021) with support for Docker Swarm and has functionality for Docker stacks.

Commands:
* `docker-compose up` to bring up a Docker compose file stack.
* `docker-compose down` to bring it down.

## Docker Swarm

Docker swarm combines multiple docker hosts into a single cluster to provide better orchestration. It features simplified setup, declarative files, scaling, rolling updates, self healing, security, load balancing, and service discovery.

Swarm uses port TCP 2377 for cluster management communications, TCP and UDP 7946 for communication among nodes, and UDP 4789 for overlay network traffic.

Node availability can be `Active`, `Pause`, or `Drain` and the manager status for each node can be `Leader`, `Reachable`, or `Unavailable`.

Manager nodes are responsible for managing cluster state. Multiple manager nodes are recommended for fault tolerance and high availability. With multiple manager nodes, there is a need for conflict resolution. One node is designated as the leader to override other management decisions.

Docker solves the problem of distributed consensus by deploying the RAFT consensus algorithm. RAFT uses random timers for initiating requests to other nodes. The first one to finish the timer sends a request to the other nodes for votes to be made the leader. The node that receives a majority of yes votes first is designated as the leader. If the leader node is unavailable for a decision, then a quorum must be reached by available managers to resolve the decision. Docker recommends no more than 7 manager nodes for a cluster and always an odd number in case of nodes being partitioned between different networks and unable to form a quorum. It is best practice to bring failed nodes back online if possible instead of creating new nodes and a new cluster.

A TLS key can be used to lock a cluster by storing the key in a password manager.

Commands:
* `docker swarm init` to initiate swarm on the master node.
* `docker system info | grep -i swarm` says if swarm is active.
* `docker node ls` lists Docker nodes.
* `docker node promote [node]` to promote a node from a worker to a manager.
* `docker node demote [node]` to demote a node to worker.
* `docker node update --availability drain [node]` to drain and update a node.
* `docker node update --availability active [node]` to make a node active again.
* `docker swarm leave [node]` to take a node out of the cluster.
* `docker swarm init --autolock=true` to start a node locked with a TLS key.
* `docker swarm unlock` to unlock the cluster.

### Docker Service

Docker swarm orchestration depends on a service to schedule tasks on worker nodes. Replicated services are most common kind of service with replicated instances across nodes. The alternative is global which does not specify a replica count but only deploys one instance in each node of the cluster. Global services are good for monitoring agents.

Commands:
* `docker service create --replicas=3 httpd` example of creating a service.
* `docker service ls` lists services.
* `docker service inspect` to inspect a service.
* `docker service logs` to get the logs.
* `docker service rm` to delete a service.
* `docker service update --replicas=(3)` to scale the deployment to three replicas.
* `docker service update --image=(nginx)` to update a service to a new image.
* `docker service update --update-parallelism=3` to update three at a time.
* `docker service update --rollback` to roll back an update to a previous version.

### Docker Node Management

Labels and constraints can be used to specify and restrict container placement for different kinds of nodes.

Commands:
* `docker node update --label-add type=cpu-optimized [node]` to specify a label that the node is CPU-optimized.
* `docker service create --constraint=node.labels.type=cpu-optimized batch-processing` to ensure that the service only deploys tasks to the properly labeled node.

### Docker Config Objects

A Docker config object can be used to replicate configurations across different Docker hosts.

Commands: 
* `docker config create [object-name] [path]` creates a configuration file that is accessible by all Docker hosts across the swarm.
* `docker service update --config-rm [config-object]` to remove a configuration from a service.
* `docker config rm [config-object` to delete a config object.
* `docker service update --config-rm [config-1] --config-add [config-2]` to rotate configuration objects in and out.

### More Docker Networks

Overlay networks (`ingress`) are an alternative to the standard bridge and host networks by creating a distributed network across multiple Docker daemon hosts. The ingress overlay network has a load balancer that can redirect traffic across containers in the Docker host. This is automatically configured with the Docker service. Overlay network traffic typically happens on port UDP 4789. Ports can be published on a service with the `-p` flag or the more verbose `--publish published=[port],target=[port] [protocol]`. 

Default MACVLAN networks assign a MAC address to a virtual network interface to make it seem like a physical network interface. This can be done with `docker network create --driver mcvlan -o parent=[veth] [network]`. This can enable multiple containers to communicate across different docker hosts at Layer 3 traffic.

Docker service discovery happens through a custom network's DNS server.

### Docker Stack

Docker compose is limited to a single host, so Docker Stack is used to deploy containers across multiple hosts in a Docker swarm cluster. A stack groups containers and services within one ecosystem.

Commands:
* `docker stack deploy --compose file docker compose.yml` to deploy a Docker compose file as a stack.
* `docker stack ls` to list Docker stacks.
* `docker stack services` to list services in a stack.
* `docker stack ps` to list processes in a stack.
* `docker stack rm` to remove a stack.

## Kubernetes

This has already been covered more in-depth in my CKA notes.

## Docker Enterprise Edition (Mirantis Container Runtime)

Enterprise edition is managed by Mirantis and offers expanded features for business-critical applications including security and access control functions, trusted registry, a universal contorl plane, access to Kubernetes and Docker Swarm, and Mirantis Container Runtime.

Within Docker enterprise there are several layers: at the bottom is Docker Certified Infrastructure, Docker Enterprise Engine, Mirantis Kubernetes Engine, then Mirantis Secure Registry on top. The pre-requisites are Linux Kernel version 3.10 or higher, a static IP and persistent host name, network connectivity between all servers, time sync, an duser namespaces should not be configured on any node.

Since being acquired by Mirantis in 2019, Docker Enterprise Edition is transitioning some feature names:
* Docker Trusted Registry -> Mirantis Secure Registry
* Universal Control Plane -> Mirantis Kubernetes Engine
* Docker Enterprise Edition -> Mirantis Container Runtime

The MKE (Mirantis Kubernetes Engine) provides a single centralized management console for monitoring usage, security, among other things. Its minimum requirements are 8 GB of RAM for manager nodes, 4 GB of RAM for owrker nodes, 2 vCPUs for manager nodes, 10 GB of free disk space for the `/var` partition for manager nodes, 500 MB of free disk space for the `/var` partition for worker nodes. `ucp-agent` is installed on each node in a cluster. When another node is added to the cluster, it is automatically provided with a `ucp-agent`.

MKE setup steps:
1. Ensure Docker EE is up and running.
2. Run a container with `docker/ucp` image.
3. Set the admin username and password for the MKE console.
4. Log into the browser.
5. Download and provide the Mirantis Contaimer Runtime.
6. Add more manager and workers as per requirement.

The MSR (Mirantis Secure Registry) minimum requirements include 16 GB of RAM, 2 vCPUs, 10 GB of free disk space, and port 80 and 443 exposed. It ensures containers and their images are secure and do not contain any features that would threaten the system. When nodes are added to a cluster, they are automatically configured with a `ucp-proxy` that provides these security features. Three nodes are needed for fault-tolerance and high availability. When there are multiple instances, external storage needs to be configured whether NFS or AWS S3.

Docker EE can be deployed through the web GUI or the CLI. A client bundle is a group of certificates downloaded directly from the MKE and allows the user to authoirze remote Docker user permissions with RBAC.

### RBAC

Docker EE EBAC:

Verbs:
* View
* Create
* Delete 
* Update

Objects:
* Config
* Container
* Image
* Network
* Secret
* Service
* Volume

Built-in Roles:
* None
* View Only
* Restricted Control
* Scheduler
* Full Control

Resource sets determine what permissions a user can have certain objects.

RBAC general steps:
* Configure subjects
* Configure custom roles
* Configure resource sets
* Configure grants = subjects + roles + resource sets

Best practice is to configure a team with the right privilegs and add or remove users to it during organizational changes. Create users from the MKE console or integrate MKE with LDAP or AD. This integration can be done in the dashboard by specifying the LDAP server and other parameters for synchronization.

## Docker Trusted Registry (Mirantis Secure Registry)

To recap, image addressing convention follows this `[registry]/[user]/[image/repository]`. An image can be built and pushed to a remote repository in Docker Enterprise where it can be stored securely. Repositories can be set to public private to determine if users can access the repository when they attempt to access it. By default users can only work on their own repositories unless otherwise grouped into teams and organizations. Teams have three levels of permissions: `read`, `read-write`, and `admin`.

Mirantis Secure Registry can scan for vulnerabilities in any OS packages, images, or dependencies. Vulnerability criteria can be pulled from a variety of databases. The scan report divides vulnerabilities into critical, major, and minor categories. To fix vulnerabilities, one can usually check depenencies, upgrade packages, and rebuild the Docker image.

Images can be promoted from one environment to another through MSR. This can follow the standard development pipeline from Development to Test to Staging to Production. By default, a new build is generated across environments. Scripts can automatically promote the same image and build across environments based on certain specifications.

MSR has an automate garbage collection process that can remove untagged and/or unused images based on certain time-based triggers. It is disabled by default and, because it is CPU intensive, be scheduled for low-activity maintenance windows. During garbage collection, MSR becomes read-only.

## Disaster Recovery

Docker swarm has three major components to protect itself in case of disaster: (1) cluster recovery, (2), backup, and (3) restore.

Cluster recovery has to ensure that consistency and stability is maintained across nodes and containers for their workloads. If the manager node goes down and cannot be restored, a worker node should be promoted. If all else fails, force launch a new cluster.

A backup stores raft keys, cluster membership, services, networks, configs, secrets, but not swarm unlock keys. The keys must be stored securely somewhere else.

MKR can be backed up with whatever is configured from the console. Swarm configuration is backed up with the Docker swarm while Kubernetes objects are backed up with MKR. MKR can be restored to a Docker host if one has not already been configured there. There can only be one backup at a time. There cannot be a backup of a cluster that has already crashed. It is only possible to restore to the same Docker version as the one used during the backup. If restored to the same swarm cluster or to a Docker host, the swarm will be initialized automatically.

MSR can be backed up to back up and restore, but this should be done across three nodes saved to external storage. This information includes configurations, repository metadata, access control, notary data, scan results, and certificates. Before a restore can be executed, any pre-existing registries must be destroyed.

Commands:
* `docker service update --force` forces a restart.
* `tar cvzf /tmp/[path] [source]` to take a backup of a Docker swarm node. This must be done by after stopping the node.
* `tar xvzf [backup-source] -C` to initiate a restore. This must be done on a stopped node.






