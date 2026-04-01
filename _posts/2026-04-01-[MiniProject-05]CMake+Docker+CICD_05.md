---
layout: post
title: "Mini.Project 01-05. Deploy: Integrated CMake, Docker and CI/CD"
date: 2026-04-01 00:00:00 +0900
author: kang
categories: [Mini-Project, Mini-Project - Deployment]
tags: [Mini-Project , Mini-Project - Deployment]
pin: false
math: true
mermaid: true
---

# <b>Mini.Project 01-05. Deploy: Integrated CMake, Docker and CI/CD</b>

---

### <b>Prerequisites</b>
        C++

---

## <b>1. CI/CD</b>

> C++ Project + CMake + Docker + CI/CD

```yml
/Project:
  - "/.github:"         # Github CI/CD
    - "/workflows:"     # Github CI/CD
      - "cmake_ci.yml"  # Github CI/CD
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

## <b>2. What is CI/CD</b>

CI/CD stands for Continuous Integration and Continuous Deployment (or Delivery). It is a set of practices that automate the process of building, testing, and deploying software.

### <b>2-1. Continuous Integration (CI)</b>

Continuous Integration is the practice of automatically building and testing code whenever changes are made to the repository.

When a developer pushes code:

* The project is automatically built
* Tests are executed
* Errors are detected early

This helps ensure that new code integrates smoothly with the existing codebase.

### <b>2-2. Continuous Deployment / Delivery (CD)</b>

Continuous Deployment (or Delivery) focuses on automatically releasing the application after it has been successfully built and tested.

* Continuous Delivery → The application is ready to be deployed
* Continuous Deployment → The application is automatically deployed

This reduces manual work and speeds up the release process.

## <b>3. yml files</b>

### <b>3-1. .github/workflows/cmake_ci.yml</b>

```yml
name: CMake CI

on: [push]

jobs:
  build:
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@v4

      - run: cmake -S . -B build -G "Visual Studio 17 2022" -A x64
      - run: cmake --build build --config Release
      - run: ctest --test-dir build -C Release --output-on-failure
```

#### <b>Trigger</b>

```yml
on: [push]
```

* The workflow runs every time code is pushed
* Ensures that new changes are automatically validated

#### <b>Job Definition</b>

```yml
jobs:
  build:
    runs-on: windows-latest
```

* Defines a job named build
* Runs on a Windows environment provided by GitHub Actions
* Uses the latest available Windows runner with Visual Studio installed

#### <b>Step</b>

```yml
- uses: actions/checkout@v4
```

* Clones the repository into the runner
* Makes the project files available for the next steps

#### <b>Run</b>

```yml
- run: cmake -S . -B build -G "Visual Studio 17 2022" -A x64
- run: cmake --build build --config Release
- run: ctest --test-dir build -C Release --output-on-failure
```

The step is similar or same on cmake process. This is CI for test.

### <b>3-2. .github/workflows/docker_ci.yml</b>

```yml
name: Docker Publish

on:
  push:
    branches:
      - main

env:
  IMAGE_NAME: ${{ secrets.DOCKERHUB_USERNAME }}/deploy-practice

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Extract short SHA
        id: vars
        run: echo "sha_short=${GITHUB_SHA::7}" >> $GITHUB_OUTPUT

      - name: Build and Push
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: |
            ${{ env.IMAGE_NAME }}:latest
            ${{ env.IMAGE_NAME }}:${{ steps.vars.outputs.sha_short }}
```

#### <b>Environment Variable</b>

```yml
env:
  IMAGE_NAME: ${{ secrets.DOCKERHUB_USERNAME }}/deploy-practice
```

* Defines the Docker image name
* Uses a secret (DOCKERHUB_USERNAME) for security

#### <b>Set up Docker Buildx</b>

```yml
- name: Set up Docker Buildx
  uses: docker/setup-buildx-action@v3
```

* Enables advanced Docker build features
* Required for efficient image building and multi-platform support

#### <b>Log in to Docker Hub</b>

```yml
- name: Log in to Docker Hub
  uses: docker/login-action@v3
  with:
    username: ${{ secrets.DOCKERHUB_USERNAME }}
    password: ${{ secrets.DOCKERHUB_TOKEN }}
```

* Authenticates with Docker Hub
* Uses secrets to securely store credentials

#### <b>Extract short commit SHA</b>

```yml
- name: Extract short SHA
  id: vars
  run: echo "sha_short=${GITHUB_SHA::7}" >> $GITHUB_OUTPUT
```

* Extracts the first 7 characters of the commit hash
* Stores it as sha_short
* Used for version tagging

#### <b>Build and push Docker image</b>

```yml
- name: Build and Push
  uses: docker/build-push-action@v6
  with:
    context: .
    push: true
    tags: |
      ${{ env.IMAGE_NAME }}:latest
      ${{ env.IMAGE_NAME }}:${{ steps.vars.outputs.sha_short }}  
```

* Builds the Docker image from the current directory
* Pushes the image to Docker Hub

Two tags are created:

* latest → always points to the newest version
* commit SHA → uniquely identifies the build