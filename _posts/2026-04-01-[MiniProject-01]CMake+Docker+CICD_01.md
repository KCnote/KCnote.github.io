---
layout: post
title: "Mini.Project 01-01. Deploy: Integrated CMake, Docker and CI/CD"
date: 2026-04-01 00:00:00 +0900
author: kang
categories: [Mini-Project, Mini-Project - Deployment]
tags: [Mini-Project , Mini-Project - Deployment]
pin: false
math: true
mermaid: true
---

# <b>Mini.Project 01-01. Deploy: Integrated CMake, Docker and CI/CD</b>

---

### <b>Prerequisites</b>
        C++

---

## <b>1. Goal</b>

To understand deployment, I will create a very simple C++ example and use CMake, Docker and CI/CD.

Acutally, I didn't know CMake, Docker and CI/CD very well. I mainly focus on C++ itself, So these are my weakness points as programmer. Through this project, I gained a basic understanding of automated deployment.

For begineers, I will explain everthing step by step, and I hope we can all reach a more higher level together.

## <b>2. Design of this project</b>

I have already gone through some trial and error with this project. I will show the overall structure and order.

### <b>2-1. Clear the roles of programs</b>

CMake: CMake is tool that Integrates a C++ project and automatcially handles the C++ build and installation. It generates libraries or executable files.

Docker: Docker is based on Linux, so we should understand basic Linux commands. However it is not about Linux itself, but about deployment systems. So we will only use Linux commands as needed to build and create images. A docker is essentially a packaged environment containing the program. These images can be pushed to and pulled from Docker Hub, and later deployed to platforms like AWS.

CI/CD: To ensure a stable program, we need to validate various aspects befor deployments. CI/CD helps prevent unintended error that we might miss. I will use Github Action. 

### <b>2-2. Design of hierarchy</b>

> C++ Project

```yml
/Project:
  - main.cpp
  - /tests:
    - test_svstring.cpp
  - /lib:
    - /structlib:
      - /src:
        - SVBase.cpp
        - SVString.cpp
      - /include:
        - SVBase.h
        - SVString.h
```

* Project/lib/structlib: Create a library (.lib)
* Project/lib: Collect multiple libraries
* Project/main.cpp: Entry point that depends on libraries

### <b>2-3. Add CMake</b>

> C++ Project + CMake

```yml
/Project:
  - "CMakeLists.txt"            # CMake
  - "/cmake:"
        - "CMakeGenLib.cmake"   # CMake
  - main.cpp
  - /tests:
    - test_svstring.cpp
  - /lib:
    - "CMakeLists.txt"          # CMake
    - /structlib:
      - "CMakeLists.txt"        # CMake
      - /src:
        - SVBase.cpp
        - SVString.cpp
      - /include:
        - SVBase.h
        - SVString.h
```

* Each folder in the hierarchy contains a CMakeLists.txt (just one)
* CMakeLists.txt acts like a "recipe" for building the project

### <b>2-4. Add Docker</b>

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

### <b>2-5. Add CI/CD</b>

```yml
/Project:
  - "/.github:"         # Github CI/CD
    - "/workflows:"     # Github CI/CD
      - "ci.yml"        # Github CI/CD
      - "docker_ci.yml" # Github CI/CD
  - Dockerfile
  - .dockerignore
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

* /.github/workflows/*.yml defines Github Actions workflows for CI/CD

## <b>3. Goal of step-by-step</b>

First of all, I will create a very simple C++ example with basic class structur. All of source (.cpp) has dependencies on each others. and library also has linked to the main executable program.

Second, with this hierarchy, I'll just use CMake to handle the entire build process. With CMake, users can easily build the project once they have the source code.

Third, instead of dealing with different environments, we will use Docker images to ensure the program runs consistently across systems.

Finally, we will set up CI/CD to automatically test and validate the project, helping prevent mistake and ensuring stability during deployment.