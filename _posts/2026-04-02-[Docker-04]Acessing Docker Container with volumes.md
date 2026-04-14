---
layout: post
title: "04. Accessing Docker Containers & Using Volumes"
date: 2026-04-02 00:00:00 +0900
author: kang
categories: [Deployment, Deployment - Docker]
tags: [Deployment, Deployment - Docker]
pin: false
math: true
mermaid: true
---

# <b>Accessing Docker Containers & Using Volumes</b>

---

### <b>Prerequisites</b>


---

## <b>1. Accessing a Running Container</b>

### ✔️ Using `docker exec`

```bash
docker exec -it <container_id> bash
```

* `-it` → interactive terminal
* `bash` → shell inside container

Example:

```bash
docker exec -it 2a8dda3 bash
```

👉 Allows you to:

* Inspect files
* Run commands
* Debug application

| Command     | Purpose                         |
| ----------- | ------------------------------- |
| docker run  | Create + start new container    |
| docker exec | Enter already running container |

## <b>2. Volume (Persistent Storage)</b>

| Feature     | Bind Mount (`-v path:path`) | Volume (`-v name:path`) |
| ----------- | --------------------------- | ----------------------- |
| Location    | User-defined                | Docker-managed          |
| Use case    | Development                 | Production              |
| Portability | Low                         | High                    |

#### <b>2-1. Why Data Disappears (Important)</b>

Containers are **ephemeral**

* When container is deleted → data is gone
* Because data lives in **writable layer**

#### <b>2-2. Solution: Volume (Persistent Storage)</b>

##### ✔️ What is a Volume?

A volume is a way to **store data outside the container**

* Data persists even if container is deleted
* Can be shared across containers

### ✔️ Basic Syntax (Bind Mount)

```bash
docker run -v <host_path>:<container_path> <image>
```

### ✔️ Example

```bash
docker run -v C:\data:/app/data ubuntu
```

👉 Meaning:

```text
Host (C:\data)  ←→  Container (/app/data)
```

* Changes in container → reflected on host
* Changes in host → reflected in container

### ✔️ Create volume (Volume)

```bash
docker volume create my-volume
```

### ✔️ Use volume

```bash
docker run -v my-volume:/app/data ubuntu
```

Docker manages storage internally

```
[ Host Disk ]
   └── /var/lib/docker/volumes/my-volume/_data   ← Real Data
        or \\wsl$\docker-desktop-data\data\docker\volumes\my-volume

                ↑ mount

[ Container ]
   └── /app/data   ← Container
```

