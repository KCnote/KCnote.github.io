---
layout: post
title: "02. Docker Layer"
date: 2026-04-02 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - Docker]
tags: [Deploy, Deploy - Docker]
pin: false
math: true
mermaid: true
---

# <b>Docker Layer</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is Docker Layer</b>

A Docker layer is a **read-only filesystem unit** that represents a single step in building a Docker image.

 A Docker image is essentially a **stack of layers**.

## <b>2. Docker Layer</b>

* A Docker image = **stack of layers**
* Each Dockerfile instruction → **creates a layer**
* Layers are **immutable and cacheable**
* Containers add a **writable layer on top**
* Proper layer ordering improves **build speed and efficiency**

Docker images are read-only.

When a container runs, Docker adds a writable layer on top:

```
[ Writable Layer (Container) ]
[ Image Layer 4 ]
[ Image Layer 3 ]
[ Image Layer 2 ]
[ Image Layer 1 ]
```
All changes happen in the top writable layer

#### <b>2-1. How Layers Are Created</b>

Each instruction in a `Dockerfile` creates a new layer.

```dockerfile
FROM ubuntu:22.04
RUN apt-get update
RUN apt-get install -y python3
COPY . /app
```

This results in:

```
Layer 1: Base image (ubuntu)
Layer 2: apt-get update
Layer 3: install python3
Layer 4: copy application files
```

Every step = one layer

#### <b>2-2. Why Layers Matter</b>

##### ✔️ 1. Build Cache (Performance)

Docker caches layers.

If nothing changes:

* It **reuses previous layers**
* Avoids rebuilding everything

##### ✔️ 2. Reusability

Multiple images can share layers:

```
Image A → ubuntu + python + app A
Image B → ubuntu + python + app B
```

`ubuntu + python` layers are shared

##### ✔️ 3. Smaller Size

* Layers are reused instead of duplicated
* Saves disk space and network bandwidth

##### ✔️ 4. Immutable

* Layers cannot be modified
* Any change creates a new layer

##### ✔️ 5. Copy-on-Write

* When a file is modified:

  * It is copied to the writable layer
  * Original layer remains unchanged

## <b>3. Layer Optimization Tips</b>

#### ❌ Inefficient

```dockerfile
RUN apt-get update
RUN apt-get install -y python3
```

👉 Creates multiple layers

#### ✅ Optimized

```dockerfile
RUN apt-get update && apt-get install -y python3
```

👉 Single layer

#### ✅ Optimized

Docker rebuilds layers from the point of change.

```dockerfile
COPY . /app
```

If this changes → all layers after it rebuild

#### ✅ Best Practice

```dockerfile
# Stable layers first
RUN apt-get install -y python3

# Frequently changing files last
COPY . /app
```
