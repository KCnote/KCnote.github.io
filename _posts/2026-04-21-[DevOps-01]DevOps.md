---
layout: post
title: "01. DevOps Flow"
date: 2026-04-21 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - DevOps]
tags: [Deploy, Deploy - DevOps]
pin: false
math: true
mermaid: true
---

# <b>DevOps Flow</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is DevOps?</b>

    DevOps = Development + Operations

DevOps is not just a set of tools. It is a **culture and methodology that integrates development and operations into a single continuous workflow**.

> DevOps is the practice of automating and integrating software development, deployment, and operations into a continuous pipeline.

## <b>2. The Problem Before DevOps</b>

Traditionally, software development looked like this:

```text
Developers → handoff code → Operations → deploy
```

* Slow deployment cycles
* Lack of ownership
* Environment mismatch (“works on my machine”)
* Slow incident response

## <b>3. Core Idea of DevOps</b>

DevOps unifies the entire lifecycle:

```text
Code → Build → Test → Deploy → Operate → Monitor → Code
```

!["DevOps"](assets/img/develop/DevOps.png)

The key is **automation across the entire pipeline**

##### ✅ Faster Delivery

* Multiple deployments per day

##### ✅ Higher Reliability

* Automated testing and monitoring

##### ✅ Better Collaboration

* No strict boundary between Dev and Ops

##### ✅ Environment Consistency

* Containers eliminate environment issues

#### <b>3-1. Build</b>

Transform source code into an executable

For C++ developers:

* `CMake + vcpkg` = Build stage

#### <b>3-2. CI (Continuous Integration)</b>

Triggered on every code push

* Build the project
* Run tests

Tools:

* GitHub Actions
* Jenkins

#### <b>3-3. CD (Continuous Deployment)</b>

Automatically deploy the application

* Build Docker image
* Push to registry
* Deploy to server or cloud

Tools:

* Docker
* Kubernetes

#### <b>3-4. Operate</b>

Run and manage the system in production

* Infrastructure management
* Scaling
* Failure handling

Examples:

* AWS EC2
* Kubernetes clusters

#### <b>3-5. Monitor</b>

Track system health and performance

Metrics:

* CPU / Memory
* Latency
* Error logs

Tools:

* Prometheus
* Nagios

#### End-to-End Flow

```text
[Developer]
   ↓
Git Push
   ↓
CI (Build + Test)
   ↓
Docker Image Build
   ↓
CD (Deploy)
   ↓
Production (Run)
   ↓
Monitoring
```

If something fails → fix → repeat

#### DevOps from a C++ Developer Perspective

A typical workflow:

```text
Write C++ code
→ Build with CMake
→ Run CI (GitHub Actions)
→ Build Docker image
→ Deploy to AWS / server
→ Monitor logs and performance
```

## <b>4. Example</b>

```bash
# Build
cmake -S . -B build
cmake --build build --config Release

# Docker
docker build -t my-app .
docker run my-app

# Deploy (example)
aws ecr push ...
```
