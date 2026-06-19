# Linux Containers (LXC) Lab

## Overview

This lab explores Linux Containers (LXC), a lightweight virtualization technology that allows multiple isolated Linux environments to run on a single host while sharing the host kernel.

Unlike traditional virtual machines, LXC containers do not emulate hardware or run their own kernels. Instead, they use Linux kernel features such as namespaces and cgroups to provide isolation and resource management.

The goal of this lab was to understand containerization concepts, create and manage containers, configure networking, explore resource limits, and compare LXC with Docker.

---

## What is LXC?

LXC (Linux Containers) provides operating-system-level virtualization.

A container behaves like an independent Linux system while sharing the host kernel.

Key characteristics:

* Lightweight compared to virtual machines
* Fast startup time
* Low resource usage
* Shared host kernel
* Isolated filesystem, processes, and networking

---

## Concepts Learned

### Namespaces

Namespaces isolate resources between the host and containers.

Examples:

* PID Namespace → Isolates processes
* NET Namespace → Isolates networking
* MNT Namespace → Isolates filesystem views
* UTS Namespace → Isolates hostnames

Namespaces determine:

"What the container can see"

---

### cgroups

Control Groups (cgroups) manage resource allocation.

Examples:

* Memory limits
* CPU allocation
* Process limits

cgroups determine:

"How much the container can use"

---

### LXC vs Docker

| LXC                         | Docker                            |
| --------------------------- | --------------------------------- |
| System Container            | Application Container             |
| Feels like a lightweight VM | Usually runs a single application |
| Multiple services possible  | One main process is common        |
| Full Linux environment      | Application-focused               |

Both share the host kernel.

---

## Lab Tasks Performed

### Container Management

Created containers using Ubuntu templates.

Commands used:

```bash
lxc-create -n Con -t download -- --dist ubuntu --release jammy --arch amd64

lxc-start -n Con -F

lxc-stop -n Con

lxc-info -n Con

lxc-attach -n Con

lxc-destroy -n Con
```

---

### Exploring Isolation

Compared host and container environments.

Commands:

```bash
hostname

ip addr

ps aux

mount
```

Observations:

* Different hostname
* Different process tree
* Different network interface
* Shared kernel

---

### SSH Access

Installed and configured SSH inside the container to enable remote administration.

Commands:

```bash
apt install openssh-server

service ssh start

passwd <user>
```

From host:

```bash
ssh user@<container-ip>
```

Concepts learned:

* Remote container management
* Service management inside containers
* Container networking and connectivity
* Authentication and user access

---

### Networking

Configured a static IPv4 address for the container.

Example:

```text
lxc.net.0.ipv4.address=10.69.69.67/24
lxc.net.0.ipv4.gateway=10.69.69.69
```

Concepts learned:

* Bridge networking
* Gateway configuration
* DNS resolution
* NAT
* Private IPv4 addressing

---

### DNS Troubleshooting

Issue:

Container could communicate with host but could not resolve domain names.

Investigation:

```bash
ping 8.8.8.8

ping google.com

resolvectl status

cat /etc/resolv.conf
```

Root Cause:

Container was configured to use an unreachable IPv6 DNS server.

Resolution:

Configured a valid DNS server manually.

---

### Resource Limits (cgroups v2)

Configured memory, CPU, and PID limits.

Container configuration:

```text
lxc.cgroup2.memory.max=512M
lxc.cgroup2.cpu.weight=500
lxc.cgroup2.pids.max=67
```

Verification:

```bash
cat /sys/fs/cgroup/memory.max

cat /sys/fs/cgroup/cpu.weight

cat /sys/fs/cgroup/pids.max
```

Tested memory limits using Python memory allocation and observed allocation failure when limits were exceeded.

---

### Container Persistence

Observed that LXC containers are persistent by default.

Changes such as:

* Installed packages
* Created users
* Modified files

remained available after stopping and restarting the container.

---

### Custom Container Image

Created a customized container environment and explored cloning/reusing containers.

Concept learned:

```text
Container
    ↓
Customize
    ↓
Clone / Reuse
```

Similar in purpose to Docker images.

---

## Storage Locations

Typical LXC storage paths:

```text
/var/lib/lxc/<container-name>/

/var/lib/lxc/<container-name>/config

/var/lib/lxc/<container-name>/rootfs
```

Observed container storage usage:

```bash
du -sh /var/lib/lxc
```

---

## Key Takeaways

* Containers share the host kernel.
* Namespaces provide isolation.
* cgroups provide resource control.
* LXC behaves more like a lightweight virtual machine than Docker.
* Networking involves bridges, gateways, DNS, and NAT.
* Containers are persistent by default.
* Resource limits can be enforced using cgroups v2.
* LXC is useful for cybersecurity labs, exploit testing, malware analysis, and isolated service deployment.

---

## Skills Practiced

* Linux Administration
* Containerization
* Networking Fundamentals
* DNS Troubleshooting
* Resource Management
* Process Isolation
* Cybersecurity Lab Setup

---

