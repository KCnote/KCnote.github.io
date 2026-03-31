---
layout: post
title: "CMake Step-by-Step Guide"
date: 2026-03-31 00:00:00 +0900
author: kang
categories: [CMake, CMake - Fundamental]
tags: [CMake, CMake - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>CMake Step-by-Step Guide (From Basic to External Libraries)</b>

---

### <b>Prerequisites</b>

---

## <b>1. Step-by-Step Guide</b>

This post walks through **5 progressive stages of using CMake**, starting from a minimal setup to modular projects and external dependencies.

## <b>2. Basic Executable</b>

### <b>2-1. CMakeLists</b>

```cmake
cmake_minimum_required(VERSION 3.10)
project(myproject)

add_executable(myprogram main.cpp)
```

### <b>2-2. Project Structure</b>

```
project/
├── CMakeLists.txt
├── main.cpp
└── build/
```

### <b>2-3. Explanation</b>
- `cmake_minimum_required`: Defines required CMake version
- `project`: Sets project name
- `add_executable`: Creates an executable target

## <b>3. Using PROJECT_NAME Variable</b>

### <b>3-1. CMakeLists</b>

```cmake
cmake_minimum_required(VERSION 3.10)
project(myproject)

add_executable(${PROJECT_NAME} main.cpp)
```

### <b>3-2. Project Structure</b>

```
project/
├── CMakeLists.txt
├── main.cpp
└── build/
```

### <b>3-3. Explanation</b>

- `${PROJECT_NAME}` automatically uses the name defined in `project()`
- Avoids hardcoding target names

## <b>4. Adding a Library</b>

### <b>4-1. CMakeLists</b>

```cmake
cmake_minimum_required(VERSION 3.10)
project(myproject)

add_executable(${PROJECT_NAME} main.cpp)

add_library(mylib SHARED lib.cpp)

target_link_libraries(${PROJECT_NAME} mylib)
```

### <b>4-2. Project Structure</b>

```
project/
├── CMakeLists.txt
├── lib.cpp
├── lib.h
├── main.cpp
└── build/
```

### <b>4-3. Explanation</b>

- `add_library`: Creates a shared library
- `target_link_libraries`: Links library to executable

## <b>5. Using Subdirectories (Modular Structure)</b>

### <b>5-1. Root CMakeLists</b>

```cmake
cmake_minimum_required(VERSION 3.10)
project(myproject)

add_executable(${PROJECT_NAME} main.cpp)

target_link_libraries(${PROJECT_NAME} mylib)

add_subdirectory(lib)
```

### <b>5-2. lib/CMakeLists</b>

```cmake
add_library(mylib SHARED lib.cpp)

target_include_directories(mylib INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
```

### <b>5-3. Project Structure</b>

```
project/
├── CMakeLists.txt
├── main.cpp
├── lib/
│   ├── lib.cpp
│   ├── lib.h
│   └── CMakeLists.txt
└── build/
```

### <b>5-4. Explanation</b>

- `add_subdirectory`: Includes another CMake project
- `target_include_directories`: Exposes header path
- `INTERFACE`: Consumers inherit include paths

## <b>6. Adding External Library</b>

### <b>6-1. Root CMakeLists</b>

```cmake
cmake_minimum_required(VERSION 3.10)
project(myproject)

find_package(fmt REQUIRED)

add_executable(${PROJECT_NAME} main.cpp)

target_link_libraries(${PROJECT_NAME} mylib fmt::fmt)

add_subdirectory(lib)
```

### <b>6-2. lib/CMakeLists</b>

```cmake
add_library(mylib SHARED lib.cpp)

target_include_directories(mylib INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
```

### <b>6-3. Project Structure</b>

```
project/
├── CMakeLists.txt
├── main.cpp
├── lib/
│   ├── lib.cpp
│   ├── lib.h
│   └── CMakeLists.txt
└── build/
```

### <b>6-4. Explanation</b>

- `find_package(fmt REQUIRED)`: Finds installed fmt library
- `fmt::fmt`: Imported target provided by fmt
- Automatically links include paths and libraries
