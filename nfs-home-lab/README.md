# Linux NFS Home Lab

## Objective

Set up a basic NFS file sharing lab using Kali Linux as the server and Windows 11 as the client.

---

# Lab Setup

| Component      | Details             |
| -------------- | ------------------- |
| Server         | Kali Linux VM       |
| Client         | Windows 11 Host     |
| Protocol       | NFS                 |
| Virtualization |  VMware |

---


# Creating Shared Directory

```bash
mkdir ~/nfs_shared
```

---

# Installing NFS Server

```bash
sudo apt update
sudo apt install nfs-kernel-server
```

---

# Export Configuration

Configuration:

```bash
echo "/home/jack/nfs_shared Client_ip_here(ro,no_root_squash,async)" >> /etc/exports 
```

Checking active non-commented configuration:

```bash
cat /etc/exports | grep -v "#"
```

---

# Starting NFS Service

```bash
sudo systemctl enable --now nfs-kernel-server

sudo systemctl start --now nfs-kernel-server
```

Re-apply exports:

```bash
sudo exportfs -ar
```

Verify exports:

```bash
sudo exportfs -v
```

---

# Windows Client Setup

Enabled:

* NFS Client from Windows Features / Control Panel

Mounted NFS share using CMD.

```cmd
mount -o anon \\server_IP_here\home\jack\nfs_shared Z:
```

Unmounted share later using:

```cmd
umount Z:
```

---

# What I Learned

* Basic NFS client-server architecture
* Linux network file sharing concepts
* Export configurations and access control
* Restricting access to trusted hosts using IP-based rules
* Basic Linux service management
* Interoperability between Linux and Windows systems

---

# Additional Setup Steps

- Enabled **NFS Client** from Windows Features / Control Panel before mounting the share.
- Found Kali Linux server IP using:

```bash
ip a
```

- Found Windows host IP using:

```cmd
ipconfig
```

- Verified connectivity between systems using:

```bash
ping <WINDOWS_IP>
```

and:

```cmd
ping <KALI_IP>
```

- Confirmed network communication before attempting the NFS mount.