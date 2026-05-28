# Apache2 & Web Server Basics

## What is a Web Server?

A web server is software that delivers files, webpages, applications, and data over the network using the HTTP protocol.

Examples:
- Apache2
- Nginx
- Lighttpd
- Caddy

A browser sends an HTTP request:

```http
GET /index.html HTTP/1.1
```

The web server receives it and responds with files/content.

---

# Important Networking Concept

Apache is simply:

```text
A process listening on TCP port 80
```

Flow:

```text
Browser
↓
HTTP
↓
TCP
↓
IP
↓
Apache2
↓
Filesystem (/var/www/html)
```

Apache maps:

```text
URL → Filesystem Content
```

Example:

| URL | Filesystem |
|---|---|
| `/` | `/var/www/html/index.html` |
| `/secret.txt` | `/var/www/html/secret.txt` |

---

# Installation

Install Apache2:

```bash
sudo apt install apache2 -y
```

---

# Start Apache Service

Start service:

```bash
sudo systemctl start apache2
```

Enable on boot:

```bash
sudo systemctl enable apache2
```

Check service status:

```bash
sudo systemctl status apache2
```

---

# Check Listening Port

```bash
sudo ss -tulpn | grep :80
```

Apache listens on:

```text
0.0.0.0:80
```

Meaning:

```text
Apache accepts HTTP connections on TCP port 80
```

---

# Accessing the Web Server

Local machine:

```bash
curl localhost
```

or:

```text
http://localhost
```

Remote access:

```text
http://<SERVER_IP>
```

Example:

```text
http://10.73.177.250
```

Requirement:
- both devices must be on the same network
- firewall should allow port 80

---

# Important Apache Paths

| Path | Purpose |
|---|---|
| `/etc/apache2/apache2.conf` | main config |
| `/etc/apache2/sites-available/` | website configs |
| `/etc/apache2/sites-enabled/` | enabled sites |
| `/var/www/html` | public web files |
| `/var/log/apache2/` | logs |

---

# Public Web Directory

```text
/var/www/html
```

This directory is PUBLICLY EXPOSED through Apache.

Anything placed here may become accessible from the browser.

---

# Practical Experiment — Exposing a File

Create a file inside the document root:

```bash
sudo touch /var/www/html/secret.txt
```

Add content:

```bash
echo "This is a demo public file exposed through Apache." | sudo tee /var/www/html/secret.txt
```

Access from browser:

```text
http://10.73.177.250/secret.txt
```

Observation:

```text
Files inside /var/www/html become accessible through HTTP
```

---

# Viewing Logs

Apache logs incoming requests.

View live logs:

```bash
sudo tail -f /var/log/apache2/access.log
```

Example log:

```text
GET /secret.txt HTTP/1.1
```

Logs reveal:
- client IP
- requested files
- user-agent
- response status

Important for:
- monitoring
- detection
- forensics
- attack analysis

---

# Apache Directory Listing

If no `index.html` exists, Apache may show the file structure.

Experiment:

```bash
sudo mv /var/www/html/index.html ~/
```

Now browsing:

```text
http://10.73.177.250
```

may display:

```text
Index of /
```

showing all files in the directory.

This is controlled by:

```apache
Options Indexes
```

inside Apache configuration.

---

# Apache Configuration

Main config:

```text
/etc/apache2/apache2.conf
```

Example:

```apache
<Directory /var/www/html>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

---

# Understanding the Configuration

## Options Indexes

If no index file exists:
- Apache shows directory contents

Potential risk:
- information disclosure

---

## FollowSymLinks

Apache follows symbolic links.

---

## AllowOverride All

Allows `.htaccess` files to override settings.

---

## Require all granted

Allows everyone to access the directory.

---

# .htaccess

Apache supports per-directory configuration using:

```text
.htaccess
```

Used for:
- authentication
- redirects
- blocking access
- rewrite rules

---

# Apache Modules

Apache supports modules for additional features.

Examples:

| Module | Purpose |
|---|---|
| `mod_rewrite` | URL rewriting |
| `mod_security` | web firewall |
| `mod_ssl` | HTTPS support |

---

# Cybersecurity Concepts Learned

- Apache is a network service
- Services expose attack surface
- HTTP runs over TCP port 80
- Web servers map URLs to files
- Public directories can leak sensitive data
- Logs record attacker activity
- Directory listing can expose internal files
- Network reachability determines exposure

---

# Key Takeaways

```text
Apache is just a process listening on port 80
It maps URLs to filesystem content
Config controls exposure/access
Logs record requests
Network exposure creates attack surface
```


---

# Personal Learnings & Observations

## Networking & Exposure

- Learned that Apache2 is just a process listening on TCP port 80
- Understood that starting Apache exposes a network service
- Learned the difference between localhost exposure and network exposure
- Learned that exposure depends on:
  - network mode
  - firewall
  - routing
  - NAT/bridged networking
- Understood that devices must be on the same network to access the web server directly
- Learned how HTTP works over:
  - HTTP → TCP → IP → Ethernet/Wi-Fi

---

## Apache2 Understanding

- Learned how to install and manage Apache2 using `systemctl`
- Learned how Apache maps URLs to filesystem content
- Learned that `/var/www/html` is the public document root
- Learned that files placed inside `/var/www/html` become accessible through the browser
- Learned how Apache serves:
  - `index.html`
  - text files
  - directory contents

---

## Misconfigurations & Security Concepts

- Learned that removing `index.html` can expose directory listing
- Learned that `Options Indexes` may leak internal files
- Learned how sensitive files can accidentally become publicly exposed
- Learned why public directories are dangerous if secrets are stored inside them
- Learned that logs record:
  - requests
  - IP addresses
  - user-agents
  - accessed files
- Learned how attackers can leave traces inside logs

---

## Practical Cybersecurity Concepts

- Learned how to inspect listening ports using:

```bash
sudo ss -tulpn
```

- Learned how to inspect Apache access logs
- Learned how HTTP requests appear inside logs
- Learned how browsers request:
  - webpages
  - favicons
  - files
- Learned how attack surface increases when services are exposed

---

## VM & Infrastructure Learnings

- Learned the difference between:
  - NAT
  - Bridged networking
  - Host-only networking
- Learned how virtual machines communicate over networks
- Learned how Apache became reachable from another machine
- Learned how networking directly affects service exposure

---

## Core Foundation Built

This lab strengthened understanding of:

- Linux services
- Networking
- TCP/IP
- HTTP basics
- Web server exposure
- Filesystem mapping
- Logs & monitoring
- Basic web server misconfigurations
- Attack surface awareness
