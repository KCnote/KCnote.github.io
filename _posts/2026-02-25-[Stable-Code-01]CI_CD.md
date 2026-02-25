---
layout: post
title: "01. Continuous Integration(CI) & Continuous Deployment(CD)"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Stable, CI/CD]
tags: [Stable Code, CI/CD]
pin: false
math: true
mermaid: true
---

---
# <b>Continuous Integration(CI) & Continuous Deployment(CD)</b>
---
### <b>Prerequisites</b>
    1. Git

---
## <b>What is Continuous Integration(CI) & Continuous Deployment(CD)</b>

### 1. What is Continuous Integration(CI) & Continuous Deployment(CD)?
    - Continuous Integration(CI): practice of <span style="color:cyan">automatically building and testing code whenever changes are pushed</span> to a shared repository.
    
    - Continuous Deployment(CD): practice of <span style="color:cyan">automatically releasing code changes to production</span> after successful testing.

CI/CD is a development practice that automates the process of building, testing and deploying applications.

### 2. Why use Continuous Integration(CI) & Continuous Deployment(CD)?
     Minimize Mistake that we would usually made 

I heard CI/CD process, but i didn't meet the situation. Because the setting of CI/CD is charged by my boss for various security and stable setting. But Now i just wanna more understand how i can control the Integration process and testing for stable program. 

Fortunately(?), I met the problem that jeklly CI/CD occured and i hope to know how can i deal with the situation and how to make the setting of CI/CD.
![Catch-CI-CD](/assets/img/develop/Catch-CI-CD.png)

1. $Set\: up\: job$: GitHub created a virtual machine and initializaed the workflow.
2. $Checkout$: The repository code was cloned into the runner environment.
3. $Setup\: Pages$: GitHub Pages environment was configured.
4. $Setup\: Ruby$: Ruby was installed and configured properly for the build process.
5. $Build\: site$: Program build the static site and generated the directory.
6. $Test\: site$: Check the validation step.
7. $Upload\: site\: artifact$: Generated build output as an artifact so that it can be used in later stages of the workflow, such as deployment.
8. $Post\: Checkout$: Cleanup operations after the checkout step
9. $Complete\: job$: The workflow finished.

![hierarchical-CI-CD](/assets/img/develop/hierarchical-CI-CD.png)

### 3. How use Continuous Integration(CI) & Continuous Deployment(CD)?
    Command

``` yml
name: "Build and Deploy"
on:
  push:
    branches:
      - main
      - master
    paths-ignore:
      - .gitignore
      - README.md
      - LICENSE

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
          # submodules: true
          # If using the 'assets' git submodule from Chirpy Starter, uncomment above
          # (See: https://github.com/cotes2020/chirpy-starter/tree/main/assets)

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4

      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3.3
          bundler-cache: true

      - name: Build site
        run: bundle exec jekyll b -d "_site${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: "production"

      - name: Test site
        run: |
          bundle exec htmlproofer _site \
            \-\-disable-external \
            \-\-ignore-urls "/^http:\/\/127.0.0.1/,/^http:\/\/0.0.0.0/,/^http:\/\/localhost/"

      - name: Upload site artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: "_site${{ steps.pages.outputs.base_path }}"

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

<b>1. Section 1. name of CI pipeline</b>
```yml
name: "Build and Deploy"
```

<b>2. Section 2. trigger of CI pipline</b>
```yml
on:
  push:
    branches:
      - main
      - master
```
If we push the code on another branch or name, it won't be prcoessed

<b>3. Section 3. Ignore files of CI pipline when we'll push</b>
```yml
paths-ignore:
  - .gitignore
  - README.md
  - LICENSE
```

<b>4. Section 4. Allows you to run this workflow manually from the Actions tab</b>
```yml
workflow_dispatch:
```

<b>5. Section 5. Set authorization for CD</b>
```yml
permissions:
  contents: read
  pages: write
  id-token: write
```

<b>6. Section 6. Control process with another process at the same time</b>
```yml
concurrency:
  group: "pages"
  cancel-in-progress: true
```

<b>7. Section 7. CI step</b>

><b>7-1. Build job</b>
```yml
jobs:
  build:
    runs-on: ubuntu-latest
```
$\rightarrow Process\: on\: Ubuntu\: Virutal\: Server $

><b>7-2. Checkout</b>
```yml
steps:
    - name: Checkout
      uses: actions/checkout@v4
```
$\rightarrow Clone\: code\: from\: Github\: to\: CI\: Server $

><b>7-3. Setup Pages</b>
```yml
- name: Setup Pages
  id: pages
  uses: actions/configure-pages@v4
```
$\rightarrow Prepare\: environment\: of\: Github\: page$

><b>7-4. Setup Ruby</b>
```yml
- name: Setup Ruby
  uses: ruby/setup-ruby@v1
  with:
      ruby-version: 3.3
      bundler-cache: true
```
$\rightarrow Install\: Ruby\: and\: activate\: bundler\: caches$

><b>7-5. Build Site</b>
```yml
- name: Build site
  run: bundle exec jekyll b -d "_site${{ steps.pages.outputs.base_path }}"
  env:
      JEKYLL_ENV: "production"
```
$\rightarrow Trasnform\: Markdown\: to\: HTML\: and\: Make\: $ _ $site\: folder$

><b>❗7-6. Test Site</b>
```yml
- name: Test site
  run: |
      bundle exec htmlproofer _site \
      \-\-disable-external \
      \-\-ignore-urls "/^http:\/\/127.0.0.1/,/^http:\/\/0.0.0.0/,/^http:\/\/localhost/"
```
$\rightarrow Check\: Validation$

><b>7-7. Uplod Site Artifact for deploy job</b>
```yml
- name: Upload site artifact
  uses: actions/upload-pages-artifact@v3
  with:
      path: "_site${{ steps.pages.outputs.base_path }}"
```
$\rightarrow Save\: from\: $ _ $site\: to\: artifact$

<b>8. Section 8. CD step</b>

><b>8-1. Deploy Job</b>
```yml
deploy:
  environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
  runs-on: ubuntu-latest
  needs: build 
  steps:
      - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v4
```


## <b>Another Example</b>
<b>- Python</b>
>1. hierarchical structure
```
project/
project/
 ├── app.py
 ├── test_app.py
 └── .github/workflows/ci.yml
```
>2. yml
```yml
name: CI

on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: 3.11
      - run: pip install pytest
      - run: pytest
```

<b>- C++</b>
>1. hierarchical structure
```
project/
 ├── src/
 │    └── add.cpp
 ├── include/
 │    └── add.h
 ├── tests/
 │    └── test_add.cpp
 ├── CMakeLists.txt
 └── .github/workflows/ci.yml
```

>2. yml
```yml
name: C++ CI

on:
  push:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install dependencies
        run: sudo apt-get install -y cmake libgtest-dev

      - name: Configure
        run: cmake -S . -B build

      - name: Build
        run: cmake --build build

      - name: Run Tests
        run: ctest --test-dir build
```

>3. CMake
```cmake
#CMake
cmake_minimum_required(VERSION 3.14)
project(MyProject)

set(CMAKE_CXX_STANDARD 17)

add_library(add src/add.cpp)
target_include_directories(add PUBLIC include)

enable_testing()
find_package(GTest REQUIRED)

add_executable(runTests tests/test_add.cpp)
target_link_libraries(runTests add GTest::GTest GTest::Main)

add_test(NAME AddTests COMMAND runTests)
```
CMake is a build system generator that simplifies cross-platform compilation, dependency management, and testing in C++ projects. That's why we use CMake