---
layout: post
title: "Mini.Project 01-04. Deploy: Integrated CMake, Docker and CI/CD"
date: 2026-04-01 00:00:00 +0900
author: kang
categories: [Mini-Project, Mini-Project - Deployment]
tags: [Mini-Project , Mini-Project - Deployment]
pin: false
math: true
mermaid: true
---

# <b>Mini.Project 01-04. Deploy: Integrated CMake, Docker and CI/CD</b>

---

### <b>Prerequisites</b>
        C++

---

## <b>1. Docker</b>

> C++ Project + CMake + Docker

```yml
/Project:
  - "Dockerfile"        # Docker
  - ".dockerignore"     # Docker
  - CMakeLists.txt
  - /cmake:
        - CMakeGenLib.cmake
  - main.cpp
  - /tests:
    - test_svstring.cpp
  - /lib:
    - CMakeLists.txt
    - /structlib:
      - CMakeLists.txt
      - /src:
        - SVBase.cpp
        - SVString.cpp
      - /include:
        - SVBase.h
        - SVString.h
```

* Dockerfile acts like "recipe" for building the container image
* .dockerignore defines which files are excluded when building/pushing the image

## <b>2. What is Docker</b>

Docker is a platform that allows developers to package an application together with all its dependencies into a single, portable unit.

Instead of relying on the host system’s environment, the entire runtime—including libraries, configurations, and required binaries—is encapsulated inside a Docker image. This image can then be executed as a container.

By running the application inside a container, Docker ensures that the program behaves consistently across different environments, regardless of the underlying system. In other words, once an application is built as a Docker image, it can be run on any machine with Docker installed, producing the same results every time.

This approach eliminates issues caused by environment differences and simplifies deployment, testing, and distribution.

### <b>2-1. Dockerfile</b>

```docker
# Builder Stage
FROM ubuntu:24.04 AS builder

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    cmake \
    g++ \
    make && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY . .

RUN cmake -S . -B build -DCMAKE_BUILD_TYPE=Release && \
    echo "===== cmake configure done ====="
RUN cmake --build build && \
    echo "===== cmake build done ====="
RUN echo "===== build dir =====" && \
    ls -la /app
RUN echo "===== build contents =====" && \
    ls -la /app/build

# Runtime Stage
FROM ubuntu:24.04

WORKDIR /app
COPY --from=builder /app/build/deploy-project /app/execute

CMD ["./execute"]
```

#### <b>builder environment</b>

```docker
FROM ubuntu:24.04 AS builder
```

Choose OS (usually ubuntu) and use LTS version. The builder is just name.

#### <b>Update packages lists</b>

```docker
apt-get update
```

Downloads the latest package index from Ubuntu repositories. Ensures that the package manager installs the most up-to-date versions

#### <b>Install required packages</b>

```docker
apt-get install -y --no-install-recommends cmake g++ make
```

* -y → Automatically answers "yes" to prompts
* --no-install-recommends → Installs only essential packages (reduces image size)

#### <b>Clean required cache</b>

```docker
rm -rf /var/lib/apt/lists/*
```

* Removes cached package lists
* Reduces final image size

#### <b>Work directory</b>

```docker
WORKDIR /app
```

#### <b>Copy source code</b>

```docker
COPY . .
```

This command copies all files from the current directory on the host machine into the current working directory inside the container.

#### <b>RUN command</b>

```docker
RUN cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
RUN cmake --build build
```

For creating images, creating from source code to executable file.

#### <b>COPY executable file</b>

```docker
COPY --from=builder /app/build/deploy-project /app/execute
```

The --from=builder option tells Docker to copy files from a previous build stage instead of the local host.

* Source: /app/build/deploy-project (inside builder container)
* Destination: /app/execute (in current container)

#### <b>CMD</b>

This instruction defines the default command that will be executed when the container starts.

### <b>2-2. .dockerignore</b>

```ignore
/build/
/install/
```

The .dockerignore file specifies which files and directories should be excluded when copying files into the Docker image.

## <b>3. Docker command</b>

```bash
docker build -t image_name .
docker run --rm image_name
docker tag image_name username/image_name
docker login
docker push username/image_name
docker pull username/image_name
docker run username/image_name
```

### <b>3-1. build the image</b>

```bash
docker build -t image_name .
```

Builds a Docker image from the current directory using the Dockerfile.

### <b>3-2. run the container</b>

```bash
docker run --rm image_name
```

Runs the container to verify that the application works correctly.

* --rm automatically removes the container after it exits

### <b>3-3. tag the image for Docker Hub</b>

```bash
docker tag image_name username/image_name
```

Assigns a new tag to the image so it can be pushed to Docker Hub.

### <b>3-4. login</b>

```bash
docker login
```

Authenticates with Docker Hub (required before pushing images).

### <b>3-5. push the image</b>

```bash
docker push username/image_name
```

Uploads the image to Docker Hub so it can be accessed remotely.

### <b>3-6. pull the image</b>

```bash
docker pull username/image_name
```

Downloads the image from Docker Hub.

### <b>3-7. run the pulled image</b>

```bash
docker run username/image_name
```

Runs the container using the image pulled from Docker Hub.

### <b>3-8. check or remove images</b>

```bash
docker ps
docker ps -a
docker images
docker rm address
docker rmi image_name
```
