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
109. You have created a nginx container and customized it to create your own webpage. How can you create an image out of it to share with others?




## Docker Engine - Security
## Docker Engine - Networking
## Docker Engine - Storage
## Docker Compose
## Docker Swarm
## Kubernetes
## Docker Engine Enterprise
## Docker Trusted Registry
## Disaster Recovery
