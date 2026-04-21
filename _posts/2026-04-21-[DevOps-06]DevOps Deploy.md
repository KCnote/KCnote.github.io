---
layout: post
title: "03. DevOps about Deploy"
date: 2026-04-21 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - DevOps]
tags: [Deploy, Deploy - DevOps]
pin: false
math: true
mermaid: true
---

# <b>DevOps about Deploy</b>

---

### <b>Prerequisites</b>

      - Docker
      - C++

---

## <b>1. What is `Deploy` in DevOps?</b>

The **Deploy phase** is where the built artifact is delivered into a target environment so it can actually run.

For a C++ system, deployment is not just “copy the executable somewhere.”
It also includes:

* packaging the application
* preparing runtime dependencies
* defining the target environment
* releasing the artifact safely

> Deploy is the phase where a tested build artifact is released into a target environment so it can run reliably in production.

## <b>2. Why Deployment Matters</b>

A program can build successfully and still fail after deployment.

Typical reasons:

* missing shared libraries
* different OS or compiler environments
* incorrect runtime configuration
* wrong file paths or permissions
* inconsistent deployment steps

#### ❌ Without a proper deployment process

* “works on my machine” problems
* broken production releases
* hard-to-reproduce environment issues
* manual mistakes during release

#### ✔ With a proper deployment process

* repeatable releases
* consistent runtime environment
* safer rollouts
* easier rollback when needed

## <b>3. Deploying a Real-Time Image Preprocessing System</b>

Assume we built:

> **A C++ real-time image preprocessing service for industrial inspection**

The system may:

* receive live image frames
* preprocess them in real time
* provide output to downstream modules
* run continuously in production

For this kind of system, deployment must ensure:

* correct runtime environment
* stable performance
* minimal interruption
* predictable behavior after release

## <b>4. Goals of the Deploy Phase</b>

The Deploy phase should guarantee:

1. the correct artifact is released
2. the target environment is consistent
3. runtime dependencies are available
4. the system starts successfully
5. the release can be validated and rolled back if needed

## <b>5. What Gets Deployed?</b>

In C++, deployment usually includes more than just the executable.

Possible artifacts:

* executable binary
* shared libraries
* configuration files
* model/data files
* startup scripts
* container image

##### Example

```text
deploy/
 ├── app
 ├── config.yaml
 ├── libopencv_core.so
 └── start.sh
```

Or as a container:

```text
docker image
 └── app + dependencies + runtime environment
```

## <b>6. Why Containers Are Common for Deployment</b>

A modern deployment approach is to package the application in a container.

This is useful because C++ applications often depend on:

* shared libraries
* compiler/runtime compatibility
* specific OS packages

Using Docker helps make the runtime environment reproducible.

##### Example Dockerfile

```dockerfile id="q8h5tr"
FROM ubuntu:22.04

RUN apt-get update && apt-get install -y \
    libstdc++6

WORKDIR /app

COPY build/app ./app
COPY config.yaml ./config.yaml

CMD ["./app"]
```

##### Benefits

* same runtime environment everywhere
* easy CI/CD integration
* easier rollback and versioning
* fewer environment mismatch issues

## <b>7. Deployment Flow in DevOps</b>

A typical flow looks like this:

```text
Code
   ↓
Build
   ↓
Test
   ↓
Package artifact
   ↓
Deploy to target environment
   ↓
Validate release
```

In many teams:

```text
GitHub Actions
   ↓
Build binary
   ↓
Build Docker image
   ↓
Push image to registry
   ↓
Deploy to server / cloud
```

## <b>8. Deployment Targets</b>

Deployment depends on where the application runs.

#### <b>8-1. Bare-metal or local server</b>

Used for industrial systems or on-premise environments

#### <b>8-2. VM / cloud instance</b>

AWS EC2

#### <b>8-3. Container platform</b>

Docker-based deployment, Kubernetes

#### <b>8-4. Edge device / embedded Linux</b>

Often used when the system must run near hardware

For a real-time image preprocessing system, deployment often happens on:

* factory servers
* edge devices
* dedicated processing machines
* cloud-connected operational nodes

## <b>9. What Must Be Checked During Deployment</b>

Deployment is not finished when the file is copied.

You must verify:

* does the process start correctly?
* are required libraries available?
* is configuration loaded correctly?
* are permissions correct?
* are input/output paths valid?
* is runtime performance still acceptable?

#### Example Post-Deployment Validation

After deployment:

```text
- Service starts successfully
- Frame input is received
- Output is generated correctly
- CPU / memory usage is normal
- Latency remains within target
```

## <b>10. Common Deployment Risks</b>

#### ❌ Missing runtime libraries

The binary runs locally but fails in production

#### ❌ Wrong build artifact

Debug build accidentally deployed instead of Release

#### ❌ Config mismatch

Wrong file paths, ports, or device settings

#### ❌ Environment mismatch

Different OS packages or library versions

#### ❌ Manual deployment mistakes

Copying wrong files or missing steps

## <b>11. Deployment Strategies</b>

Not all deployments should replace production immediately.

Common strategies:

#### <b>11-1. Full replacement</b>

Old version is stopped, new version starts

Simple, but may cause downtime

#### <b>11-2. Rolling deployment</b>

Gradually replace running instances

Better for availability

#### <b>11-3. Blue-green deployment</b>

Keep two environments:

* Blue = current production
* Green = new version

Switch traffic only after validation

#### <b>11-4. Canary deployment</b>

Release to a small subset first. Useful for reducing production risk

For industrial or real-time environments, the strategy depends on:

* downtime tolerance
* hardware constraints
* operational risk

## <b>12. Rollback Matters</b>

A good deployment process always considers failure.

If the new version causes issues, rollback should be possible.

Rollback may mean:

* redeploy previous binary
* restart previous container image
* switch back to old environment

> Deployment is not complete unless recovery is possible.

## <b>13. Deployment Automation</b>

In DevOps, deployment should be automated whenever possible.
Manual deployment creates risk. A simple automated flow might look like this:

```text
git push
   ↓
CI builds project
   ↓
tests pass
   ↓
Docker image created
   ↓
image pushed to registry
   ↓
server pulls new image
   ↓
service restarts
```

* fewer human errors
* faster release cycle
* repeatable process
* easier auditing

## <b>14. Common Mistakes in the Deploy Phase</b>

#### ❌ Treating deployment as simple file copying

There are runtime dependencies and environment assumptions

#### ❌ Deploying without validation

Application may start but behave incorrectly

#### ❌ No rollback plan

One bad release can break production

#### ❌ Manually changing production machines

Creates drift and inconsistency

#### ❌ Ignoring performance after deployment

A system may function correctly but still fail latency targets
