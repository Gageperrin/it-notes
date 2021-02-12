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

Namespaces provide isolated networking interface for work environments. Namespaces can be linked with a virtual cable. A virtual network switch is used when there are multiple containers. Linux Bride and OpenvSwitch can be used as this switch.

Commands:
* `ip netns add` to add a network namespace.
* `ip netns` to list namespaces.
* `ip netns exec [ns] ip link` to execute `ip link` in a namespace. Can also do `ip -n [ns] link`.
* `ip link add [veth] type veth peer name [veth2]` to create a virtual ethernet cable. 
* `ip link set [veth] netns [name]` to set a virtual ethernet link point on a namespace.
* `ip -n [ns] link del [veth]` to delete one end of the link (and automatically the other end).
* `ip link add [v-net-0] type bridge` to set a virtual network switch.
* `ip link set dev [v-net-0] up` to enable the switch. Then set virtual ethernet cables between the namespaces and the network switch.












