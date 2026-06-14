# Docker Fundamentals Lab

## Objective

Understand Docker fundamentals by:

* Installing Docker Engine on Kali Linux / Debian-based systems
* Creating a custom Docker image
* Building a Dockerfile
* Running a container
* Accessing services inside the container
* Understanding images, containers, layers, networking and isolation

---

## Prerequisites

This repository already contains:

* `engine_installation_script.sh`
* `Dockerfile`

The installation script is intended for Debian-based distributions (Kali Linux, Debian, etc.).

---

## Docker Installation

Run the provided installation script:

```bash
chmod +x engine_installation_script.sh
./engine_installation_script.sh
```

The script:

* Adds Docker's official Debian repository
* Installs Docker Engine
* Installs Docker CLI
* Installs Buildx
* Installs Docker Compose Plugin
* Adds the current user to the Docker group

Verify installation:

```bash
docker --version
```

---

## Dockerfile Overview

The provided Dockerfile uses:

```dockerfile
FROM ubuntu:22.04
```

and performs the following actions:

1. Updates package repositories.
2. Installs:

   * Apache2
   * OpenSSH Server
   * sudo
3. Creates a user named `docker-user`.
4. Sets a password for the user.
5. Grants ownership of Apache directories.
6. Adds the user to the sudo group.
7. Exposes ports 22 and 80.
8. Starts SSH and Apache when the container launches.

Key Dockerfile instructions encountered:

```dockerfile
FROM
RUN
EXPOSE
CMD
```

---

## Building the Image

Build the image from the Dockerfile:

```bash
docker build -t apachenssh .
```

Verify:

```bash
docker images
```

Observation:

Docker reuses previously built layers through caching when Dockerfile instructions remain unchanged.

---

## Running the Container

Start the container:

```bash
docker run -d -p 8080:80 -p 6969:22 apachenssh
```

Port Mapping:

```text
Host 8080 -> Container 80
Host 6969 -> Container 22
```

Verify:

```bash
docker ps
```

---

## Discovering Container Names

Initially, no custom name was assigned to the container.

Docker automatically generated names such as:

```text
heuristic_goldwasser
priceless_bouman
vibrant_bell
```

These names can be viewed using:

```bash
docker ps -a
```

This demonstrated the difference between:

```text
Image Name
    vs
Container Name
```

Images are templates, while containers are running instances created from those templates.

---

## Accessing Services

### Apache

Visit:

```text
http://localhost:8080
```

Result:

Apache default page is displayed.

### SSH

Connect:

```bash
ssh docker-user@localhost -p 6969
```

Authentication succeeds using the credentials configured in the Dockerfile.

---

## Container Exploration

Enter the running container:

```bash
docker exec -it <container_name> bash
```

Useful commands:

```bash
whoami
hostname
pwd
cat /etc/os-release
```

Confirmed:

* Userspace is Ubuntu 22.04.
* Host remains Kali Linux.

---

## Important Discovery: Containers Are Not VMs

Executed:

```bash
uname -r
lsblk
```

Observation:

The container could see host kernel information and underlying block devices.

Key finding:

```text
Container != Virtual Machine
```

Containers share the host kernel while maintaining isolation through Linux namespaces and cgroups.

Architecture:

```text
Host Kernel
    |
 Docker
    |
Container
```

Unlike a VM, no separate kernel is booted.

---

## PID 1 Concept

Learned:

```text
PID 1 alive = Container alive
PID 1 exits = Container exits
```

Container startup command:

```dockerfile
CMD service ssh start && /usr/sbin/apache2ctl -D FOREGROUND
```

Apache runs in the foreground and becomes the primary process responsible for keeping the container alive.

---

## Debugging Experience

Initial build failed with:

```text
/var/run/apache2 not found
```

Root cause:

Some runtime directories are not created during package installation and may only exist after service startup.

Resolution:

Removed unnecessary ownership changes targeting non-existent runtime directories.

Lesson:

Read error messages carefully and identify the exact failing command before modifying configuration.

---

## Docker Networking

Observed container IP:

```text
172.17.0.x
```

Learned:

* Containers receive internal Docker bridge addresses.
* External systems communicate through the host IP and mapped ports.
* Other machines on the same network can connect using:

```bash
ssh docker-user@<host-ip> -p 6969
```

The connection reaches:

```text
Host:6969 -> Container:22
```

through Docker's port forwarding.

---

## Security Notes

Safe:

```bash
docker run ubuntu
```

Dangerous:

```bash
docker run -v /:/host ubuntu
```

Reason:

The container receives direct access to the host filesystem.

Key lesson:

Container isolation can be weakened through:

* Host filesystem mounts
* Privileged containers
* Exposed Docker sockets
* Excessive capabilities

---

## Useful Commands

Build image:

```bash
docker build -t image .
```

Run container:

```bash
docker run image
```

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

View logs:

```bash
docker logs <container>
```

Enter container:

```bash
docker exec -it <container> bash
```

Stop container:

```bash
docker stop <container>
```

Start container:

```bash
docker start <container>
```

Remove container:

```bash
docker rm <container>
```

Remove image:

```bash
docker rmi <image>
```

---

## Key Takeaways

* Images are templates.
* Containers are running instances of images.
* Containers use layered filesystems.
* Docker build creates images.
* Docker run creates containers.
* Docker exec enters running containers.
* Containers share the host kernel.
* Containers are not virtual machines.
* PID 1 determines container lifetime.
* Port mapping exposes container services.
* Host mounts can break isolation.
* Docker provides lightweight, disposable environments for testing, development and experimentation.
