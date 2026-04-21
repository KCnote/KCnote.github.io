---
layout: post
title: "03. DevOps about Build"
date: 2026-04-21 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - DevOps]
tags: [Deploy, Deploy - DevOps]
pin: false
math: true
mermaid: true
---

# <b>DevOps about Build</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `Build` in DevOps?</b>

The **Build phase** is where source code is transformed into a runnable artifact.

For C++ systems, this is not a trivial step.
It includes:

* compiling source code
* resolving dependencies
* linking libraries
* generating executables

> Build is the phase where source code is compiled, linked, and packaged into a runnable artifact.

## <b>2. Why Build is Critical in C++</b>

Compared to languages like Python:

* C++ requires compilation
* dependency management is complex
* platform differences matter
* build configuration affects performance

#### ❌ Poor build setup leads to:

* inconsistent environments
* broken CI pipelines
* hard-to-reproduce bugs
* long build times
* incorrect optimizations

#### ✔ Good build setup enables:

* reproducible builds
* fast iteration
* reliable CI/CD
* consistent deployment artifacts

## <b>3. Build Goals</b>

The Build phase should guarantee:

1. correct compilation
2. consistent dependency resolution
3. optimized binary output
4. reproducibility across environments
5. integration with CI/CD pipelines

## <b>4. Core Components of the Build Phase</b>

#### <b>4-1. Build System — CMake</b>

C++ projects require a build system to manage:

* source files
* compiler flags
* dependencies
* targets

### Example

```bash
cmake -S . -B build
cmake --build build --config Release
```

* cross-platform
* widely adopted
* integrates with CI/CD
* works with dependency managers

#### <b>4-2. Dependency Management — vcpkg</b>

C++ libraries are not trivial to manage.

Using **vcpkg**:

```bash
vcpkg install opencv
```

Then connect to CMake:

```bash
cmake -B build -S . \
 -DCMAKE_TOOLCHAIN_FILE=[vcpkg]/scripts/buildsystems/vcpkg.cmake
```

* consistent dependency versions
* automatic include/link setup
* cross-platform compatibility

#### <b>4-3. Build Configuration</b>

Different build types affect performance:

| Type           | Purpose                    |
| -------------- | -------------------------- |
| Debug          | debugging, no optimization |
| Release        | optimized performance      |
| RelWithDebInfo | optimized + debug symbols  |

```bash
cmake --build build --config Release
```

For real-time systems, **Release build is mandatory**

#### <b>4-4. Compiler Optimization</b>

Build is where performance begins.

Typical flags:

* `-O2` / `-O3`
* architecture-specific optimizations
* vectorization enabled

Bad flags = bad performance
Good flags = free speed improvement

#### <b>4-5. Artifact Generation</b>

The final output:

* executable
* shared library
* static library

Example:

```text
build/
 ├── app.exe
 ├── libpreprocessor.so
```

This artifact is what gets deployed

#### Build Flow in DevOps

```text
Code committed
   ↓
CI triggered
   ↓
CMake configure
   ↓
Compile + link
   ↓
Artifact generated
   ↓
Artifact passed to next stage (Test / Deploy)
```

#### Build in Docker (Best Practice)

Example Dockerfile:

```dockerfile
FROM ubuntu:22.04

RUN apt-get update && apt-get install -y \
    build-essential cmake git

WORKDIR /app

COPY . .

RUN cmake -S . -B build \
 && cmake --build build --config Release

CMD ["./build/app"]
```

* identical build environment
* easy CI integration
* eliminates environment mismatch

## <b>5. Common Mistakes</b>

### ❌ Mixing Debug and Release artifacts

→ inconsistent behavior

### ❌ Hardcoding include paths

→ breaks portability

### ❌ Not using a build system

→ manual builds become unmanageable

### ❌ Ignoring dependency versions

→ unexpected breakages

### ❌ No CI integration

→ builds fail later in pipeline
