---
layout: post
title: "05. CMakeLists.txt vs .cmake"
date: 2026-03-30 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - CMake]
tags: [Deploy, Deploy - CMake]
pin: false
math: true
mermaid: true
---

# <b>CMakeLists.txt vs .cmake</b>

---

### <b>Prerequisites</b>

* Basic understanding of CMake workflow (Configure → Build → Install)
* `cmake -S . -B build`

---

## <b>. Why CMake Structure Matters</b>

In small projects, a single `CMakeLists.txt` is often enough.

However, in real-world or large-scale projects (e.g., OpenCV),
the build configuration becomes too complex to maintain in a single file.

To solve this, CMake supports **modularization using `.

## <b>2. Core Concep</b>

```
CMakeLists.txt = Entry Point
.cmake         = Modular Components
```

* `CMakeLists.txt` → main control file
* `.cmake` → reusable logic, functions, and configurations

| Command          | Purpose             |
| ---------------- | ------------------- |
| include          | load reusable logic |
| add_subdirectory | add build unit      |


## <b>3. CMakeLists.txt</b>

### <b>3-1. Role</b>

* Starting point of the project
* Defines build targets
* Controls overall flow

### <b>3-2. Example</b>

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyApp)

include(cmake/utils.cmake)

add_executable(MyApp main.cpp)
```

### <b>3-3. Key Characteristics</b>

* Required in every directory that participates in the build
* Can exist in multiple directories
* Connected via:

```cmake
add_subdirectory(subdir)
```

## <b>4. .cmake Files</b>

### <b>4-1. Role</b>

* Split complex logic into reusable modules
* Store:

  * variables
  * functions / macros
  * configuration rules

### <b>4-2. Example</b>

```cmake
# cmake/utils.cmake

set(CMAKE_CXX_STANDARD 17)

function(print_message msg)
  message(${msg})
endfunction()
```

### <b>4-3. Usage</b>

```cmake
include(cmake/utils.cmake)
```

include() does NOT import like a module. It executes the file in-place.

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyApp)

-------include(cmake/utils.cmake) part---------
set(CMAKE_CXX_STANDARD 17)

function(print_message msg)
  message(${msg})
endfunction()
-----------------------------------------------

add_executable(MyApp main.cpp)
```

* Executes code directly
* Used for:

  * utilities
  * shared logic

## <b>5. Typical Large Project Structure</b>

```
project/
 ├── CMakeLists.txt
 ├── cmake/
 │    ├── utils.cmake
 │    ├── options.cmake
 │    └── install.cmake
 ├── src/
 │    └── CMakeLists.txt
 └── lib/
      └── CMakeLists.txt
```

```
CMakeLists.txt
   ↓
include(.cmake files)
   ↓
add_subdirectory(...)
   ↓
Multiple CMakeLists.txt executed
   ↓
Single build graph generated
```
## <b>6. Why Use .cmake Files?</b>

Without modularization:

```cmake
# thousands of lines in one file
set(...)
if(...)
foreach(...)
install(...)
```

With `.cmake`:

```cmake
include(options.cmake)
include(compiler.cmake)
include(install.cmake)
```

```text
✔ CMakeLists.txt is the entry point
✔ .cmake files are modular building blocks
✔ include() executes code inline
✔ add_subdirectory() links multiple build units
✔ Large projects rely heavily on .cmake modularization
```
