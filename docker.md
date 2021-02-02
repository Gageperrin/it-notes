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

