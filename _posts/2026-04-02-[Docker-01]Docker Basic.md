---
layout: post
title: "01. What is Docker"
date: 2026-04-02 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - Docker]
tags: [Deploy, Deploy - Docker]
pin: false
math: true
mermaid: true
---

# <b>What is Docker</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is Docker</b>

Docker is a **containerization platform** that allows developers to package an application and its dependencies into a **lightweight, portable unit called a container**.

Unlike virtual machines, containers share the host OS kernel, making them:

* Faster to start
* More lightweight
* Easier to deploy consistently

## <b>2. Key Components</b>

| Term       | Description                      |
| ---------- | -------------------------------- |
| Image      | Blueprint for containers         |
| Container  | Running instance of an image     |
| Dockerfile | Instructions to build an image   |
| Volume     | Persistent storage               |
| Network    | Communication between containers |
| Registry   | Storage for images               |

#### <b>2-1. Docker Engine</b>

The core runtime that manages containers.

It consists of:

* **Docker Daemon (dockerd)** → Runs in the background (Back-end server)
* **Docker CLI** → User interface (`docker run`, `docker build`, etc.)
* **REST API** → Communication layer between CLI and daemon

```
[ You (CLI) ]
      ↓
docker command (CLI)
      ↓
REST API Request
      ↓
Docker Daemon (dockerd)
      ↓
Container / Image
```

Daemon:

* build image (docker build)
* run container (docker run)
* manage container (docker start, stop, delete)
* manage network and volumn 

#### <b>2-2. Docker Image</b>

A **read-only template** used to create containers.

Includes:

* Application code
* Runtime (e.g., Python, Node.js)
* Libraries & dependencies

Built using a `Dockerfile`

#### <b>2-3. Docker Container</b>

A **running instance of an image**.

Characteristics:

* Isolated environment
* Has its own filesystem, network, and processes
* Can be started, stopped, deleted

#### <b>2-4. Dockerfile</b>

A script that defines how to build an image.

```dockerfile
FROM ubuntu:24.04
RUN apt-get update && apt-get install -y python3
COPY . /app
CMD ["python3", "main.py"]
```

#### <b>2-5. Docker Registry</b>

A place to store and share images.

* Docker Hub (public)
* Private registries

## <b>3. How Docker works</b>

#### <b>3-1. Write Dockerfile</b>

Defines environment and dependencies

#### <b>3-2. Build Image</b>

```bash
docker build -t myapp .
```

#### <b>3-3. Store Image (Optional)</b>

```bash
docker push myapp
```

#### <b>3-4. Run Container</b>

```bash
docker run myapp
```

## <b>4. Internal Working Principle</b>

Docker relies on Linux kernel features:

#### <b>4-1. Namespaces (Isolation)</b>

Provide separation for:

* Process IDs
* Network
* Filesystem
* Users

Each container thinks it’s running alone

#### <b>4.2 Cgroups (Resource Control)</b>

Control resource usage:

* CPU
* Memory
* Disk I/O

#### <b>4.3 Union File System (Layered FS)</b>

Images are built in layers:

```
Base Image (Ubuntu)
   ↓
Install Packages
   ↓
Copy App Code
```

Benefits:

* Reuse layers
* Faster builds
* Smaller size

## <b>5. Container vs Virtual Machine</b>


| Feature     | Container   | VM       |
| ----------- | ----------- | -------- |
| OS          | Shared      | Separate |
| Size        | Small       | Large    |
| Boot Time   | Seconds     | Minutes  |
| Performance | Near-native | Slower   |
