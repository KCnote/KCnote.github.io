---
layout: post
title: "03. Docker CLI Commands"
date: 2026-04-02 00:00:00 +0900
author: kang
categories: [Docker, Docker - Fundamental]
tags: [Docker, Docker - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>Docker</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is Docker CLI Commands</b>

This section introduces commonly used Docker commands with clear explanations and examples.

## <b>2. CLI Commands</b>

```bash
docker run <image>
docker run -it --entrypoint bash <image>
docker start <container_id>
docker stop <container_id>
docker rm <container_id>
docker rm -f $(docker ps -aq)
docker rm -f $(docker ps -q)
docker cp <container_id>:<path> <host_path>
docker commit <container_id> <new_image_name>
docker rmi <image>
```

#### <b>2-1. Run Container</b>

```bash
docker run <image>
```

Creates and starts a new container from an image.

Example:

```bash
docker run nginx
```

#### <b>2-2. Run with Interactive Shell</b>

```bash
docker run -it --entrypoint bash <image>
```

* `-it` → interactive terminal
* `--entrypoint bash` → override default command

Example:

```bash
docker run -it --entrypoint bash ubuntu
```

Useful for debugging inside container

#### <b>2-3. Start Existing Container</b>

```bash
docker start <container_id>
```

Starts a stopped container.

#### <b>2-4. Stop Running Container</b>

```bash
docker stop <container_id>
```

Gracefully stops a running container.

#### <b>2-5. Remove Container</b>

```bash
docker rm <container_id>
```

Deletes a stopped container.

#### <b>2-6. Force Remove Container</b>

```bash
docker rm -f <container_id>
```

* Stops + removes container immediately

#### <b>2-7. Remove All Containers</b>

```bash
docker rm -f $(docker ps -aq)
```

* `docker ps -aq` → all container IDs
* Removes everything (running + stopped)

#### <b>2-8. Remove Running Containers Only</b>

```bash
docker rm -f $(docker ps -q)
```

* `docker ps -q` → running containers only

#### <b>2-9. Copy Files Between Host and Container</b>

```bash
docker cp <container_id>:<path> <host_path>
```

Example:

```bash
docker cp my_container:/imgs C:\backup
```

#### <b>2-10. Create Image from Container (Commit)</b>

```bash
docker commit <container_id> <new_image_name>
```

Example:

```bash
docker commit 123abc my-image
```

Saves container state as a new image

Not recommended for production → use Dockerfile instead

#### <b>2-11. Remove image</b>

```bash
docker rmi <image>
```

Deletes a stopped image.
