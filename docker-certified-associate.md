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
19. 

## Docker Image Management
## Docker Engine - Security
## Docker Engine - Networking
## Docker Engine - Storage
## Docker Compose
## Docker Swarm
## Kubernetes
## Docker Engine Enterprise
## Docker Trusted Registry
## Disaster Recovery
