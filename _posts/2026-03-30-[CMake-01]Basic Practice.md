---
layout: post
title: "01. CMake Tutorial"
date: 2026-03-30 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - CMake]
tags: [Deploy, Deploy - CMake]
pin: false
math: true
mermaid: true
---

# <b>CMake</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is CMake?</b>

This tutorial explains how to build a simple C++ program (main.cpp) using CMake,
with a clear explanation of every command and option.

The goal is not just to run commands, but to understand what each part actually does.

### Flow:

```
CMakeLists.txt
   ↓
CMake (configure)
   ↓
Build system (Makefile / Ninja / MSBuild)
   ↓
Compiler (g++, clang)
   ↓
Executable
```

## <b>2. Tutorial</b>

#### <b>2-1. Project Structure</b>

project/ 
├── CMakeLists.txt 
└── main.cpp

#### <b>2-2. Context</b>

>main.cpp
```cpp
#include <iostream> 
int main() 
{ 
    std::cout << "Hello CMake!" << std::endl; 

    return 0; 
}
```

>CMakeLists.txt
```cmake
cmake_minimum_required(VERSION 3.10) 
project(MyApp) 

add_executable(MyApp main.cpp)
```

#### <b>2-3. Context Detail</b>


##### 1. `cmake_minimum_required(VERSION 3.10)`

* Specifies the **minimum required version of CMake**
* Ensures compatibility and avoids unexpected behavior

##### 2. `project(MyApp)`

* Defines the **project name**
* Internally sets variables like:

  * `PROJECT_NAME`
  * `PROJECT_SOURCE_DIR`

##### 3. `add_executable(MyApp main.cpp)`

**This is one of the most important commands**

**Meaning:**

* Create an executable file named `MyApp`
* Compile `main.cpp`
* Link everything into a runnable program

**Equivalent concept (low-level):**

```bash
g++ main.cpp -o MyApp
```

#### <b>2-4. Build Process</b>
    Build Directory → Run CMake → Build Project → Run

##### 1. Create Build Directory

```bash
mkdir build
cd build
```

* Separates source files and build artifacts
* Prevents clutter in your project
* This is called an **out-of-source build** (industry standard)

##### 2. Run CMake

```bash
cmake ..
```

* `cmake` → run the CMake configuration process
* `..` → path to the directory containing `CMakeLists.txt`

> What happens internally:

* Detects compiler (e.g., `g++`, `clang`)
* Generates build files:

  * `Makefile` (Linux)
  * `.sln` / `.vcxproj` (Windows)

##### 3. Build the Project

```bash
cmake --build .
```

* `--build` → tells CMake to actually compile
* `.` → current build directory

###### 4. Run the Program

```bash
./MyApp
```

(Windows: `MyApp.exe`)

### Full Command Summary

```bash
mkdir build
cd build
cmake ..
cmake --build .
./MyApp
```

## <b>3. Important CMake Options</b>

### 3.1 `-DCMAKE_BUILD_TYPE=Release`

```bash
cmake -DCMAKE_BUILD_TYPE=Release ..
```

### 3.2 `-DCMAKE_CXX_COMPILER=g++`

```bash
cmake -DCMAKE_CXX_COMPILER=g++ ..
```

