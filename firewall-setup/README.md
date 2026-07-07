# Linux Firewall Lab using IPTables

Hands-on implementation of common firewall rules using Linux IPTables.

---

## Objectives

- Launch a web server
- Block a TCP port
- Re-allow the blocked port
- Block a specific IP address
- Allow a specific IP address
- Block traffic based on protocol
- Allow traffic based on protocol
- Create a custom chain
- Forward packets to a custom chain
- Delete firewall rules
- List existing firewall rules

---

# Lab 1 — Launching a Web Server

## Objective

Host a simple HTTP server on TCP port 7777.

### Command

```bash
python3 -m http.server 7777
```

### Result

The server becomes accessible from other machines on the LAN.

**Screenshot**

![Web Server](images/web%20file%20sharing.png)

---

# Lab 2 — Blocking TCP Port

## Command

```bash
sudo iptables -A INPUT -p tcp --dport 7777 -j REJECT
```

### Verification

Remote clients can no longer access the web server.

**Before**

![Before](images/Before%20rejecting.png)

**After**

![After](images/After%20rejecting.png)

---

# Lab 3 — Allowing the TCP Port Again

The blocking rule was removed to restore access.

Example:

```bash
sudo iptables -D INPUT <rule_number>
```

The web server became reachable again.

---

# Lab 4 — Blocking a Specific IP Address

## Command

```bash
sudo iptables -A INPUT -s <IP> -j DROP
```

### Result

Traffic from the selected client was blocked.

**Screenshot**

![Blocked IP](images/blocked%20specific%20ip.png)

---

# Lab 5 — Allowing Only One Specific IP

## Commands

```bash
sudo iptables -I INPUT -s <Trusted_IP> -j ACCEPT
sudo iptables -A INPUT -j REJECT
```

### Result

Only the trusted client could access the server.

**Screenshot**

![Allowed IP](images/Only%20allowing%20specific%20IP.png)

---

# Lab 6 — Blocking Traffic Based on Protocol

## Command

```bash
sudo iptables -A INPUT -p icmp -j DROP
```

### Result

- Ping requests failed.
- HTTP remained accessible.

**Screenshot**

![Protocol Filtering](images/blocking%20particular%20protocol.png)

---

# Lab 7 — Allowing Traffic Based on Protocol

Removed the ICMP blocking rule.

```bash
sudo iptables -D INPUT <rule_number>
```

Ping responses resumed successfully.

---

# Lab 8 — Creating a Custom Chain

## Command

```bash
sudo iptables -N MahChayn
```

---

# Lab 9 — Forwarding Packets to the Custom Chain

## Command

```bash
sudo iptables -A INPUT -j MahChayn
```

Packets matching the INPUT chain were forwarded to the custom chain for further processing.

---

# Lab 10 — Deleting Rules and Chains

Rules were removed using:

```bash
sudo iptables -D
```

Custom chain deletion:

```bash
sudo iptables -F MahChayn
sudo iptables -D INPUT -j MahChayn
sudo iptables -X MahChayn
```

---

# Lab 11 — Listing Existing Rules

Human-readable view

```bash
sudo iptables -L
```

Command format

```bash
sudo iptables -S
```

---

# Skills Demonstrated

- Linux Firewall Administration
- IPTables Rule Management
- TCP Filtering
- Protocol Filtering
- Source IP Filtering
- Rule Ordering
- User-defined Chains
- Packet Flow
- HTTP Service Testing
- Firewall Verification
- Network Service Testing
---

## Concepts Learned

- INPUT chain processing
- ACCEPT vs DROP vs REJECT
- Rule precedence
- User-defined chains
- Packet matching
- Source and destination filtering
- Protocol-based filtering
