# Practice Questions for DCA Exam

## Docker Engine

1. What are the components of the Docker Engine?
  * Docker CLI, Docker Daemon, REST API
2. What component of the Docker Engine manages the images, containers, volumes, and networks on a host?
  * Docker Daemon
3. What component of the docker architecture is responsible for managing containers on Linux on version 1.15 of Docker Engine?
  * LibContainer
4. We can run containters without installing Docker.
  * True
5. Which component is responsible for keeping the containers alive when the Docker Daemon goes down?
  * Containerd-Shim
6. What are the primary objects that Docker Engine manages?
  * Images, Containers, Volumes, Networks
7. By default, data stored inside the container is always persistent
  * False
8. By default, Docker is configured to look for images on Google Cloud Registry
  * False
9. Which component is a read-only template used for creating a Docker container?
  * Docker images
10. What are the default data directory for Docker?
  * `/var/lib/docker`
11. What does OCI stand for?
  * Open Container Initiative
12. What are the two specifications from OCI?
  * `runtime-spec`
  * `image-spec`
13. What is the command to view the version of docker engine installed?
  * `docker version`
14. What is the command to start Docker daemon manually?
  * `dockerd`
15. On what interfaces are the docker daemon made available by default?
  * Unix Socket
16. What is the port conventionally used to configure un-encrypted traffic on TCP?
  * Port 2375
17. What file is used to configure the Docker daemon?
  * `/etc/docker/daemon.json`
18. What flags are used to configure encryption on Docker daemon?
  * `tlsverify`, `tlscert`, `tlskey`
19. What is the default network driver used when a container is created?
  * Bridge
20. What is the command used to list the running containers on the Docker host?
  * `docker container ls`
21. Which of the below commands create a container with `nginx` image and name `nginx`?
  * `docker container create --name nginx nginx`
22. How to list all running and stopped containers and their status?
  * `docker container ls -a`
23. How to start a stopped container?
  * `docker container start nginx`
24. How does one get only the IDs of running containers?
  * `docker container ls -q`
25. What is the option used in docker run command to attach to the terminal of the container in an interactive mode?
  * `-it`
26. What is the command to change the container name "httpd" to "webapp"?
  * `docker container rename httpd webapp`
27. What is the command to run a "nginx" container in a detached mode with name "webapp"?
  * `docker container run -d --name webapp nginx`
28. You cannot start a killed container.
  * False
29. Delete the stopped container named "webapp".
  * `docker container rm webapp`
30. Run a container called webapp with image nginx, and in an interactive mode.
  * `docker container run -it --name webapp nginx`
31. Which combination of keys are used to escape from the shell and keep the container webapp running?
  * Ctrl + p + q
32. Which combination of keys are used to exit from the shell and stop the container webapp?
  * Ctrl + c
33. You have a running container and want to execute a command inside it. Which command will you use?
  * `exec`
34. We deployed a container called webapp. Inspect this container to get the IPPrefixLen.
  * `docker container inspect webapp } grep IPPrefixLen`
35. We have deployed some containers. What command is used to get the container with the highest memory?
  * `docker container stats`
36. How to display the running processes inside the container?
  * `docker container top container-name`
37. You have a webapp container and image httpd. Inspect the logs of the webapp container. Which command is used to get the stream logs of the webapp container so that you can view the logs live?
  * `docker container logs -f webapp`
38. Which command returns only new and/or live events?
  * `docker system events`
39. Which command returns events since the past 30 minutes?
  * `docker system events --since 30m`
40. Which command is used to get the events of the container named `webapp`?
  * `docker system events --filter 'container=webapp'`
41. Run a container named `webapp` with `nginx` image in detached mode.
  * `docker container run --detach --name=webapp nginx`
42. Stop the container named `nginx`
  * `docker container stop nginx`
43. How do you list running and stopped containers?
  * ` docker container ls -a`
44. How can one delete a container named "webapp".
  * `docker container rm webapp`
45. Stop all running containers on the host.
  * `docker container stop $(docker container ls -q)`
46. Delete all running and stopped containers on the host.
  * `docker container rm -f $(docker container ls -aq)`
47. Which command is used to delete the stopped containers?
  * `docker container prune`
48. What is the command to pause a running container?
  * `docker container pause`
49. What are the signals sent to a running container when the Docker container stop command is executed?
  * `SIGTERM` followed by `SIGKILL`
50. Run a container with image `nginx`, name `nginx`, and hostname `webapp`.
  * `dockercontainer run -d --name nginx --hostname=webapp nginx`
51. What is the hostname set on the container when the `docker container run -d --name webapp httpd` command is run?
  *  The container's unique id
52. What is the default restart policy?
  * `no`
53. Which policy would restart the containers even after the docker daemon is restarted?
  * `always`
54. Which policy is used to restart a container unless it is explicitly stopped or Docker is restarted.
  * `unless-stopped`
55. Which command can be used to check the restart polciy of `webapp` container?
  * `docker container inspect webapp`
56. Which command should be used to update the httpd container with the always policy?
  * `docker container update --restart always httpd`
57. Which command should be used to update all the running containers with unless-stopped policy?
  * `docker container update --restart unless-stopped $(docker container ls -q)`
58. Which option is used to reduce container downtime due to daemon crashes, planned outages, or upgrades?
  * Live restore
59. What is the path file which is used to add the live restore?
  * `/etc/docker/daemon.json`
60. How to enable the live restore setting to keep containers alive when the daemon becomes unavailable?
  * `echo '{"live-restore": true}' >> /etc/docker/daemon.json`
61. Which of the below commands may be used to copy a file /web.conf from a container named webapp with id 89683681 to the /tmp directory on the host?
  * `docker container cp 89683681:/web.conf /tmp/`
  * `docker container cp webapp:/web.conf /tmp/`
62. Copy the /etc/nginx directory from the webapp container to the docker host under /tmp/.
  * `docker container cp webapp:/etc/nginx /tmp/`
63. What is the command to copy the file /root/myfile.txt from the host to /root/ of the webapp container?
  * `docker container cp /root/myfile.txt webapp:/root/`
64. We can copy a file from a stopped container.
  * True
65. Data inside the container is persistent.
  * False
66. You can run multiple instances of the same application on the docker host.
  * True
67. You can map to the same port on the Docker host more than once.
  * False
68. Which option could be used to expose a webapp container to the outside world?
  * `-p`
  * `-P`
  * `--publish`
69. Map TCP port 80 in the container to port 8080 on the Docker host for connections to host IP 192.168.1.10
  * `-p 192.168.1.10:8080:80`
  * `-p 192.168.1.10:8080:80/tcp`
70. Unless specified otherwise, Docker publishes the exposed port on all network interfaces.
  * True
71. Map UDP port 80 in the container to port 8080 on the Docker host.
  * `-p 8080:80/udp`
72. How does the -P option in the docker container run command know what ports to publish on the container?
  * It uses the ExposedPorts field set on the container or the EXPOSE instruction in the Dockerfile.
73. How does Docker map a port on a container to a port on the host?
  * IP table rules
74. What IPTables chains does Docker modify to configure port mapping on a host?
  * `DOCKER`
75. How to check the logs of the docker daemon?
  * journalctl -u docker.service
  * less /var/log/messages
  * less /var/log/daemon.log
  * less /var/log/docker`
76. How does one enable debugging mode?
  * `echo '{"debug": true}' > /etc/docker/daemon.json`
77. How to check if the Docker service is running or not?
  * `sudo systemctl status docker`
78. Which environment variable will be used to connect a remote Docker server?
  * `DOCKER_HOST`
79. What may be the cause of this error: "unable to configure the Docker daemon with file /etc/docker/daemon.json: the following directives are specified both as a flag and in the configuration file: tls: (from flag: true, from file: false)"?
  * The tls flag is set to false in daemon.json file and true in the command line
80. What is the default logging driver?
  * `json-file`
81. Where is the log of the `webapp` container with id `78373635` on the Docker Host?
  * `/var/lib/docker/containers/78373635/78373635.json`
82. Which command is used to check the default logging driver?
  * `docker system info`
83. How to change the default logging driver to syslog?
  * `echo '{"log-driver": "syslog"}' > /etc/docker/daemon.json`
84. Run a `webapp` container and make sure that no logs are configured for this container.
  * `docker run -it --log-driver none webapp`

## Docker Image Management
85. What is the default public registry for Docker?
  * Docker Hub
86. What is the default tag if not specified when building an image with the name `webapp`?
  * `latest`
87. Run an Ubuntu container with the trusty tag.
  * `docker run ubuntu:trusty`
88. Which command is used to list the local images?
  * `docker image ls`
89. Which command lists the full length image IDs?
  * `docker images --no-trunc`
90. Display images with a name containing `postgres` and at least 12 stars.
  * `docker search --filter=stars=12 postgres`
91. Download `nginx` image from the Google Container Registry.
  * `docker image pull nginx`
92. Display images with a name containing `busybox`, that are at least 3 stars, and are official builds.
  * `docker search --filter is-official=true --filter stars=3 busybox`
93. What is the command to change the tag of `"httpd:latest"` to `"httpd:v1"`?
  * `docker image tag httpd:latest httpd:v1`
94. You have an nginx:v1 image with size 100M. You’ve now created your own version of the image - nginx:v2 by retagging the first image, what is the total size of both?
  * 100 M
95. Which command should be used to get the total size consumed by all images on a host?
  * `docker system df`
96. In the output of the `"docker system df"` command what does the ACTIVE field indicate on the images row?
  * Number of images with containers
97. Which command is used to authenticate with azr.com registry which listens on port 5000?
  * `docker login azr.com:5000`
98. You are required to store a copy of the official alpine image in your company's internal docker registry. What would be your approach?
  * Pull the image, tag it with the internal address of the Docker registry and push it.
99. When you log in to a registry, the command stores credentials in
  * `$HOME/.docker/config.json`
100. Which command is used to remove `webapp:v1` image locally?
  * `docker image rm webapp:v1`
  * `docker image remove webapp:v1`
101. Remove all unused imaegs on the Docker host.
  * `docker image prune -a`
102. Display all layers of `httpd` image along with the size on each layer.
  * `docker image history httpd`
103. Which command can be used to get the ExposedPorts of a `webapp` image?
  *  `docker image inspect webapp`
104. How to get the OS field alone of an `httpd` image?
  * `docker image inspect httpd -f '{{.Os]]`
105. Which subcommand will be used to get more info about images?
  * `inspect`
106. Which command can be used to get a backup of image `webapp`?
  * `docker image save webapp -o webapp.tar`
107. A tarfile - nginx.tar - has been created using the docker image save command. Which command can be used to extract it into your docker host.
  * `docker image load -i nginx.tar`
108. A government facility runs a secure data center with no Internet connectivity. A new application requires access to Docker images hosted on Docker hub. How can this be done?
  * Pull Docker images from a host with access to Docker hub, convert to a tarball using Docker image save, and copy to the restricted environment and extract the tarball.
109. Which command pulls an `ubuntu` image from a private registry at gcr.io
  * `docker pull gcr.io/kk/ubuntu`
110. How can an image be created from a container for sharing purpose?
  * `docker export`
111. How can an exported image be restored into a container?
  * `docker image import`
113. What command lists Docker images on the Docker host?
  * `docker image ls`
  * `docker images`
114. Which command matches an image with the `com.example.version` label?
  * `docker images --filter "label=com.example.version"
115. What are some instructions supported in Dockerfile?
  * `EXEC`
  * `COPY`
116. The *** is a text document that contains all the commands a user could call on the command line to assemble an image.
  * Dockerfile
117. Which methods can be used to build an image using existing containers?
  * `docker commit`
  * `docker export`
118. The container being committed and its processes will be paused while the image is committed.
  * True
119. We have a running container named `webapp` with the `nginx` image. How can an image be created from this container?
  * `docker container commit nginx mynginx`
120. Docker container commit is the recommended approach for building a custom image?
  * False
121. How should an image be created from an existing image?
  * Use `docker image save` and `docker iamge load`.
122. How should an image be created from an existing container?
  * Use `docker container export` and `docker image import`.

```
FROM python:3.6
RUN pip install flask
COPY . /opt/
EXPOSE 8080
WORKDIR /opt
ENTRYPOINT ["python", "app.py"]
```

123. What is the parent image?
  * `python:3.6`
124. Into which directory is the application code copied to?
  * `/opt/`
125. What is the command used to RUN the application in the Dockerfile?
  *  `python app-py`
126. What is the port of the web application configrued for the service to lsiten within the container?
  *  8080


127. Whenever a build is initiated by running the Docker build command, the build context is transferred to the Docker daemon. Where is that stored?
  * `/var/lib/docker/tmp`
128. Which commands can build an image with the Dockerfile filename?
  * `docker build .`
  * `docker build -f Dockerfile .`
129. While building a Docker image from code stored in a remote URL, which command will build from a directory called Docker in the branch dev?
  * `docker build https://github.com/kk/dca.git#dev:docker`
130. What kind of resoruces can the build context PATH refer to?
  * Git repositories
  * pre-packaged tarball contexts
  * path to a local directory
131. Build an image using a context build under path `/tmp/docker` and name it `webapp`.
  * `docker build /tmp/docker -t webapp`
132. What is the default tag when building an image?
  * `latest`
133. What is the command to build an image using a Dockerfile.dev file under path `/opt/myapp` with the name webapp. The current directory you are in is `/tmp`.
  *  `docker build -f /opt/myapp/Dockerfile.dev /opt/myapp -t webapp`
134. What is the file used to exclude temporary files such as log files or builds from the context during a build?
  * `.dockerignore`
135. What is the recommended approach for installing packages and libraries while buildign an image?
  * Use the RUN instruction and ahve the `apt-get update` and `apt-get install` commands on the same instruction.
136. Using `RUN apt-get update && apt-get install -y` ensures the Dockerfile installs the latest package versions with no further coding or manual intervention. This is known as...
  * Cache busting
137. What leads to Docker invalidating cahce on a given layer?
  * Change in instruction
  * Change in a file used with the ADD instruction
138. *** forces the build to install a particular version of a package regradless of what is in the cache.
  * version pinning
139. What can help reduce build time?
  * Putting instructions that are more likely to change at the bottom of the Dockerfile.
140. A Dockerfile is built from an Ubuntu image as the base image. What would happen to the cahce when a new version of the Ubuntu image is made available at Dockerhub.
  * Cache is not invalidated and Docker continues to use existing cache.
141. Which option can be used to disable the cache while building a Docker image?
  * `--no-cache=true`
142. COPY instruction only supports the basic copying of local files into the container.
  * True
143. What is the right instruction to download a file from "https://file.tar.xz" and auto-extract it into "/testdir" in the image?
  * `ADD https://file.tar.xz /testdir`
144. COPY instruction has some features like local-only tar extraction and remote URL support.
  * False
145. Which instructions can be used in the Dockerfile to copy content from the local filesystem into the containers?
  * `ADD`
  * `COPY`
146. Which of the following are the correct format for CMD instruction?
  *  `CMD ["executable", "param1", "param2"]`
  *  `CMD command param1, param2`
147, If CMD is used to provide default arguments for the ENTRYPOINT instruction, both the CMD and ENTRYPOINT instructions should be specified.
  * True
148. When a user runs the command `docker run my-custom-image sleep 1000`...
 * Docker overrides the CMD instruction with "sleep 1000"
149. What is the output of the following Dockerfile snippet when container runs as `docker run -it < image> ? ENTRYPOINT ["/bin/echo", "Hello"] CMD ["world"]`?
 * Hello world
150. What is the output of the following Dockerfile snippet when container runs as `docker run -it <image> kk ? ENTRYPOINT ["/bin/echo", "Hello"] CMD ["World"]`?
  * Hello kk
151. If you list more than one CMD instruction in the Dockerfile then only the last CMD will take effect.
  * True
152. How do you identify if a Docker file is configured to use multi-stage builds?
  * The Dockerfile has multiple FROM instructions
153. The `"--from=0"` in the following Dockerfile instruction line refers to: `"COPY --from=0 /go/src/github.com/alexellis/href-counter/app .` 
  * The image built using the first set of instructions in the Dockerfile
154. Name the stage which uses nginx as a base image to builder in the Dockerfile
  * `FROM nginx AS builder`
155. What is the instruction used to copy a file from an external image named `redis` not part of any stage in the multi-stage build process.
  *  `--from=redis`
156. What can help minimize image size?
  * only install necessary packages on the image
  * combine multiple dependent instructions into a single one and clean up temporary files
  * use multi-stage builds
157. What can help reduce image build time?
  * avoid sending unwanted files to the build context using .dockerignore
  * move the instructions that are likely to change most frequently to the bottom of the Dockerfile

## Docker Engine - Security

158. Which of the following is true?
  * By default a container runs with unlimited CPU and memory resources
159. What will happen if the `--memory-swap` is set to -1?
  * The container is allowed to use unlimited swap.
160. Each container gets a CPU share of *** by default?
  * 1024
161. What in a Linux feature that prevents a process within the container from accessing raw sockets?
  * Kernel capabilities
162. By default, all containers get the same share of CPU cycles. How cano ne modify the shared amount?
  * `docker container run --cpu-shares=512 nginx`

## Docker Engine - Networking

163. What command is used to list the default available networks?
  * `docker network ls`
164. What is the default network driver used on a container?
  * `bridge`
165. Overlay networks connect multiple Docker daemons together and enable swarm services to commmunicate with each other.
  * True
166. If you use the *** network mode for a container, that container’s network stack is not isolated from the Docker host (the container shares the host’s networking namespace), and the container does not get its own IP-address allocated.
  * host
167. Which of the following commands would create a user-defined bridge network called my-net?
  * `docker network create my-net`
  * `docker network create -d bridge my-net`
  * `docker network create --drive bridge my-net`
168. What is the command to connect a running container with name myapp to the existing bridge network my-net?
  * `docker network connect my-net myapp`

## Docker Engine - Storage

169. Volumes are mounted as “readonly” by default inside the container if no options are specified.
  * False
170. You can remove a `vol1` which is in use by a container using the command `docker volume rm --force vol1`.
  * False
171. Which options can be used to mount a volume?
  * `-v`
  * `--volume`
  * `--mount`
172. Which among the below is a correct command to start a `webapp` container with the volume `vol2`, mounted to the destination directory `/app`?
  * `docker run -d --name webapp --mount source=vol2,target=/app httpd`
  * `docker run -d --name webapp -v vol2:/app httpd`
  * `docker run -d --name webapp --volume vol2:/app httpd`
173. Which among the below is a correct command to start a `webapp` container with the volume `vol3` mounted to the destination directory `/opt` in read-only mode?
  * `docker run -d --name webapp --mount source =vol3,target=/opt,readonly httpd`
  * `docker run -d --name webapp -v vol3:/opt:ro httpd`
  * `docker run -d --name webapp --volume vol3:/opt:ro httpd`

## Docker Compose

174. Which command can be used to create and start containers in foreground using the existing docker-compose.yml?
 * `docker-compose up`
175. Which command can be used to create and start containers in background or in detached mode in compose using the existing docker-compose.yml?
  * `docker-compose up --detach`
  * `docker-compose up -d`
176. *** is the command to list the container created by the compose file.
  * `docker-compose ps`
177. Which command can be used to stop (only and not delete) the whole stack of containers created by compose file?
  * `docker-compose stop`
178. Which command can be used to delete the application stack created using compose file?
  * `docker-compose down`
179. Compose files that do not declare a version are considered version 0.
  * False
180. Compose files using the version 2 and 3 syntax must indicate the version number at the root of the document.
  * True
181. With the docker-compose up command, we can run containers on multiple docker hosts.
  * False

```
version: "3.8"
services:
  web:
    build: .
    depends_on:
      - db
      - redis
    volumes:
      - .:/code
      - logvolume01:/var/log
    ports:
      - "8080:80"
  redis:
    image: redis
  db:
    image: postgres
volumes:
  logvolume01: {}
```

182. What is the host port on which the application will be exposed?
  * 8080
184. Which of the following are true?
  * The web image will be built and the redis image will be pulled from Dockerhub if it does not already exist on the host.
185. How can the web application address `redis`?
  * Using the name redis
186. What kind of volume mount is configured on the web application for the /code directory inside the container?
  * Bind mount
187. What kind of volume mount is configured on the web application for the /var/log directory inside the container?
  * Volume mount


## Docker Swarm

188. Promote `worker1` to a manager node.
  * `docker node promote worker1`
189. Change `manager1` to a worker node.
  * `docker node demote manager1`
190. What does the Reachable status of a node indicate in docker swarm?
  * The node is a manager node and is reachable and is not the leader
191. Which command is used to check the status of manager/worker nodes?
  * `docker node ls`
192. Select the best way to perform maintenance tasks on node - worker1 - for performing patching and updates
  * `docker node update --availability drain worker1`
193. It is recommended to have one manager node in a cluster.
  * False
194. What is the maximum number of managers possible in a swarm cluster?
  * No limit
195. What is the maximum number of managers recommended by Docker in a swarm cluster?
  * 7
196. Assume that you have 3 managers in your cluster, what will happen if 2 managers fail at the same time?
  * The services hosted on the available worker nodes will continue to run.
  * New services/workers cannot be created or added.
197. How many manager nodes must be online in a cluster with 13 manager nodes for the swarm cluster to continue to operate?
  * 7
198. The RAFT logs are stored in memory on the manager nodes.
  * False
199. The RAFT logs are stored on disk and not protected.
  * False
200. The default behavior requires you to unlock the swarm when a new node joins the swarm cluster.
  * False
201. After restarting the docker service and trying to run docker service ls, you get an error "Error response from daemon: Swarm is encrypted and needs to be unlocked before it can be used. How can you solve this error?
  * `docker swarm unlock`
202. Which command can be used to return the current key which is used inside the cluster?
  * `docker swarm unlock-key`
203. Which command can be used to enable auto lock on an existing swarm?
  * `docker swarm update --autolock-true`
204. Which command can be used to run an instance on swarm?
  * `docker service create webapp`
205. What is the command to run 3 instances of httpd on a swarm cluster?
  * `docker service create --replicas=3 httpd`
206. Create a swarm service webapp with image httpd and expose port 8080 on host to port 80 in container.
  * `docker service create --name=webapp -p 8080:80 httpd`
207. Which command can be used to list the running service inside the swarm?
  * `docker service ls`
208. Which command can be used to list the tasks that are running as part of a specified service?
  * `docker service ps SERVICE-NAME`
209. The webapp:v1 had some bugs and we fixed them in webapp:v2. We want to apply webapp:v2 to webapp service.
  * `docker service update --image=webapp:v2 webapp`
210. An image update operation in a swarm service happens all at once by default.
  *  False
211. Which option of the docker service command can be used to update 4 replicas of mywebapp service at a time?
  * `--update-parallelism 4`
212. What option may be used to change the default behaviour of a failed task during an update in swarm?
  * `--update-failure-action`
213. What are the actions which can be used with `--update-failure-action`?
  * `pause`
  * `continue`
  * `rollback`
214. After an update we realized that something is wrong with the new version and we want to revert back to the old version. How can we achieve that?
  * `docker service rollback webapp`
215. The default type of a service in docker swarm is global.
  * False
216. With a global service, you can specify a minimum number of replicas for the service.
  * False
217. A global service will always deploy exactly one instance of the application on all the nodes in the cluster.
  * True
218. With a global service if a node is removed from the cluster, then that instance is removed as well and is rescheduled on another available node.
  * False
219. Create a replicated service webapp with 2 replicas.
  * `docker service create --replicas=2 webapp`
  * `docker service create --mode=replicated --replicas=2 webapp`
220. Deploy exactly one instance of the application on all the nodes in the cluster.
  * `docker service create --mode=global webapp`
221. You are required to deploy an agent of splunk on all nodes in the swarm cluster to monitor the health of the nodes and gather logs. What is the best approach to achieve this?
  * Deploy the agent as a global service in teh swarm cluster.
222. Which option can be used to control the workload placements with the help of labels and constraints?
  * Placement constraints
223. What is the command to deploy a service named webapp on a node which has a 'type=cpu-optimized' label.
  * `docker service create --constraint=node.labels.type==cpu-optimized webapp`
224. What is the command to apply `'disk=ssd'` label to worker1 in a swarm cluster.
  * `docker node update --label-add disk=ssd worker1`
225. When you create a swarm service and do not connect it to a user-defined overlay network, it connects to the *** network by default.
  * `ingress`
226. Which network will be created when you initialize a swarm or join a Docker host to an existing swarm?
  * `bridge`
  * `ingress`
227. What is the command to create an overlay network driver called `my-overlay`?
  * `docker network create -d overlay my-overlay`
228. Create an overlay network driver called my-overlay with subnet 10.15.0.0/16 using a docker command.
  * `docker network create --driver overlay --subnet 10.15.0.0/16 my-overlay`
229. Create an overlay network that can also be connected by standalone containers that were not created as part of a swarm service.
  * `docker network create --driver overlay --attachable my-overlay-network`
230. By default, all swarm service management traffic is encrypted using *** algorithm.
  * AES
231. Encrypt the application data and enable IPSEC encryption while creating the overlay network called my-overlay-network using a docker command.
  * `docker network create --driver overlay --opt encrypted my-overlay-network`
232. Delete an overlay network driver called `my-overlay`.
  * `docker network rm my-overlay`
233. Which port should be opened to allow the overlay and ingress network traffic?
  * 4789
234. The *** port should be opened to allow communication among nodes/Container Network Discovery.
  * 7946
235. Map UDP port 80 in the container to port 5000 on the overlay newtork using the `my-web-server` image.
  * `docker service create -p 5000:80/udp my-web-server`
  * `docker service create --publish published=5000,target=80,protocol=udp my-web-server`
236. The routing mesh enables each node in the swarm to accept connections on published ports for any service running in the swarm, even if there’s no task running on the node.
  * True
237. Docker requires an external DNS server to be configured during installation to help the containers resolve each other using the container name.
  * False
238. The built-in DNS server in Docker always runs at IP address...
  * 127.0.0.11
239. Attach the application my-web-server to a service so that we can access it using its name with the existing overlay network driver my-overlay.
  * `docker service create --name=web-server --network=my-overlay my-web-server`
240. Attach the application my-web-server to a service so that we can access it using its name with the existing overlay network driver my-overlay and the custom DNS 8.8.8.8
  *  `docker service create --name=my-web-server --dns=8.8.8.8 --network=my-overlay my-web-server`
250. Which command can be used to deploy the STACKDEMO stack from a compose file?
  * `docker stack deploy --compose-file docker-compose.yml STACKDEMO
  *  `cat docker-compose.yml | docker stack deploy --compose-file - STACKDEMO`


```
version: 3
services:
  redis:
    image: "redis:alpine"
    deploy:
      replicas: 3
  db:
    image: postgres:9.4
    deploy:
      replicas: 1
      placement:
        constraints:
          - "node.role==manager"
  web:
    image: webapp
    deploy:
      replicas: 5
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 5s
      retries: 5
      start_period: 120s
```

251. How many containers would be created in total for all services together?
  * 9
252. Which statement is true?
  * The postgres container will only be deployed on the manager node
  * The web container may be deployed on any node - manager or worker


## Kubernetes

253. Which statement best describes a control plane component?
  * The control plane's components decide how workloads are placed across the nodes in the cluster.
  * `kube-scheduler` is one of the control plane components
  * `kube-controller` is one of the control plane components
254. Which statement best describes the worker node component?
  * `kubelet` and container runtime are the worker node components
  * `kube-proxy` is one of the worker node components
255. Which of the following best describes ETCD?
  * ETCD serves as the backing datastore for Kubernetes cluster data
  * ETCD is a distributed reliable key-value store
256. ETCD by default listens on port 2780
  * False
257. Which of the following are components deployed only on a master node in a Kubernetes cluster?
  * Kube scheduler
  * Kube controller manager
  * Kube api-server
258. Which of the following is the ETCD command line tool?
  * `etcdctl`
259. Which component on the worker node is responsible for maintaining network rules on nodes?
  * `kube-proxy`
260. Which statement best describes multi-container pods.
  * Multi-container pods can share resources and dependencies, communicate with one another, and coordinate when and how they are terminated.
  * A single pod can have multiple containers.
261. What are the four top level fields for a Kubernetes definition file?
  * `apiVersion`
  * `metadata`
  * `kind`
  * `spec`
262. Which statements best describe replication controllers and replica sets? 
  * Replication controller is the older technology that is being replaced by a ReplicaSet.
  * The replication controller supports equality based selectors whereas the replica set supports equality based as well as set based selectors.
  * ReplicaSet is the new way to set up replication.
263. What is the command to list all the labels of a ReplicaSet?
  * `kubectl get rs --show-labels`
264. What is the command to delete a replication controller `nginx`?
  * `kubectl delete rc nginx`
265. Where do you configure the selector labels in the deployment YAML file?
  * `spec.selector`
266. Where do you configure the pod images in the deployment YAML file?
  * `spec.template.spec.containers.image`
267. What is the command to check the status of a deployment rollout named nginx-deploy?
  * `kubectl rollout status deployment/nginx-deploy`
268. Which of the following statements are correct about NodePort?
  * NodePort exposes a service to make it externally accessible on a port on the nodes.
269. What is the default range of ports that Kubernetes uses for NodePort if one is not specified?
  * Ports 30000 - 32767
270. A NodePort service exposes a deployment only on the nodes on which the PODs of that deployment are running.
  * False
271. ClusterIP is the default service type for Kubernetes service.
  * True
272. Which field of Kubernetes pod definition file corresponds to the entrypoint instruction in the Dockerfile?
  * `ENTRYPOINT` instruction in Dockerfile corresponds to command in Kubernetes definition file.
  * `CMD` instruction in Dockerfile corresponds to arguments in Kubernetes definition file.
273. Where is the env instruction set in a Kubernetes pod definition file?
  * `spec.containers.env`
274. What is the command to create config maps?
  * `kubectl create configmap CONFIGMAP-NAME --from-literal=KEY1=VALUE1 --from-literal=KEY2=VALUE2`
275. How do you inject configmap into a pod?
  * Using `envFrom` and `configMapRef`
276. Where do you configure the configMapKeyRef in a pod to use environment variables defined in a ConfigMap?
  * `spec.containers.env.valueFrom`
277. How do you configure all key-value pairs in a Secret as container environment variables?
  * `envFrom.secretRef`
278. What is the default Secret type if omitted from a Secret configuration file?
  *  Opaque


## Docker Engine Enterprise
## Docker Trusted Registry
## Disaster Recovery
