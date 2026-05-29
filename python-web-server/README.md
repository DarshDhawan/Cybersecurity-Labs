# Python Web Server Basics

## What is Python Web Server?

Python comes with a built-in web server that allows us to quickly share files and folders over HTTP without installing Apache or Nginx.

It is lightweight, simple, and commonly used by developers and penetration testers for quick file transfers and temporary hosting.

---

## Starting the Web Server

```bash
python3 -m http.server
```

Default:
- TCP port 8000
- Shares current directory
- Creates directory listing

---

## Accessing the Server

http://SERVER_IP:8000

---

## Hosting Another Directory and Changing Port

```bash
python3 -m 9000 http.server --directory /home/jack/python_web
```

---


## Common Cybersecurity Use Case

Quick file transfer.

Target:

```bash
wget http://SERVER_IP:8000/file.txt
```
- or use -r flag for recursive downloading

---

## What I Learned

- Learned that Python includes a built-in web server
- Learned how to expose a folder over HTTP using a single command
- Learned that the default port is TCP/8000
- Learned that another machine on the same network can access hosted files
- Learned that Python Web Server can be used for quick file sharing and transfers
- Learned how to host a specific directory
- Learned how to change the listening port
- Learned that port 1-1023 requires elevated privileges
- Learned that only one process can use a specific port at a time
- Reinforced understanding of:
  - HTTP
  - TCP ports
  - Network exposure
  - File transfer over a network

---

## Key Takeaway

Python Web Server is a lightweight alternative to Apache for temporary file hosting and quick file transfers.
