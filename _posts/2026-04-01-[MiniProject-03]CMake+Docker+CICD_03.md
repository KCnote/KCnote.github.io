---
layout: post
title: "Mini.Project 01-02. Deploy: Integrated CMake, Docker and CI/CD"
date: 2026-04-01 00:00:00 +0900
author: kang
categories: [Mini-Project, Mini-Project - Deployment]
tags: [Mini-Project , Mini-Project - Deployment]
pin: false
math: true
mermaid: true
---

# <b>Mini.Project 01-03. Deploy: Integrated CMake, Docker and CI/CD</b>

---

### <b>Prerequisites</b>
        C++

---

## <b>1. CMake</b>

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

## <b>2. CMakeLists.txt</b>

Each folder in the hierarchy contains a CMakeLists.txt. CMakeLists.txt acts like a "recipe" for building the project. So CMake acts following the CMakeLists script.

The order is like this:

Following to CMakeLists, the source files are integrated and build. By means that is we should understand the flow of CMakeLists script.

Let's see the CMakeLists of project

### <b>2-1. /Project/CMakeLists.txt</b>

```cmake
cmake_minimum_required(VERSION 3.10)
project(deploy-project)

include(CTest)
enable_testing()

include(cmake/CMakeGenLib.cmake)

add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE structlib)

install(TARGETS ${PROJECT_NAME} 
	RUNTIME DESTINATION bin
)

install(TARGETS structlib 
	RUNTIME DESTINATION bin 
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
)

add_executable(test_svstring tests/test_svstring.cpp)
target_link_libraries(test_svstring PRIVATE structlib)

add_test(NAME SVStringTest COMMAND test_svstring)
```

### <b>2-2. /Project/cmake/CMakeGenLib.cmake</b>

```cmake
add_subdirectory(lib)
```

#### <b>cmake version and project names</b>

```cmake
cmake_minimum_required(VERSION 3.10)
project(deploy-project)
```

#### <b>include optional test environment</b>

```cmake
include(CTest)
enable_testing()
```

#### <b>include</b>

```cmake
include(cmake/CMakeGenLib.cmake)
```

The include script will be copy and paste from "cmake/CMakeGenLib.cmake" to " CMakeLists.txt".
This meaning is there will be like that:

```cmake
include(cmake/CMakeGenLib.cmake) -> add_subdirectory(lib)
```

#### <b>explore subdirectory</b>

```cmake
add_subdirectory(lib)
```

add_subdirectory(lib) script explores subdirectory "/lib"

#### <b>create executable file</b>

```cmake
add_executable(${PROJECT_NAME} main.cpp)
```

It depends on OS, on Window, extension .exe will be created.
The \${PROJECT_NAME} is same XXXX of project(XXXX) script. In this case, \${PROJECT_NAME} is "deploy-project"

#### <b>link libraries</b>

```cmake
target_link_libraries(${PROJECT_NAME} PRIVATE structlib)
```

This script is link process on compile step "link".
At the first time, I'm little bit surprised because there's no complie trigger. But we do not need to think compile timing.

The "structlib.lib" is linked with "project-deploy"

| Keyword   | Used in Target | Propagated to Dependents | Include Directories | Compile Options |
| --------- | -------------- | ------------------------ | ------------------- | --------------- |
| PRIVATE   | Yes            | No                       | No                  | No              |
| PUBLIC    | Yes            | Yes                      | Yes                 | Yes             |
| INTERFACE | No             | Yes                      | Yes                 | Yes             |

#### <b>install</b>

```cmake
install(TARGETS ${PROJECT_NAME} 
	RUNTIME DESTINATION bin
)

install(TARGETS structlib 
	RUNTIME DESTINATION bin 
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
)
```

The install step is following on this script. Actually the install step is not creating anythings. It will rearrange build files with Copy and Paste.

| Type      | Role          |
| ------- | ----------- |
| RUNTIME | Executable files |
| LIBRARY | Share library    |
| ARCHIVE | Static library    |

#### <b>test</b>

```cmake
add_executable(test_svstring tests/test_svstring.cpp)
target_link_libraries(test_svstring PRIVATE structlib)

add_test(NAME SVStringTest COMMAND test_svstring)
```

"add_executable" and "target_link_libraries" is already explained.
"add_test" regists the test_svstring execuatble file as test and name will be SVStringTest.
on ctest, the name is SVStringTest.


### <b>2-3. /Project/lib/CMakeLists.txt</b>

```cmake
add_subdirectory(structlib)
```

### <b>2-4. /Project/lib/structlib/CMakeLists.txt</b>

```cmake
file(GLOB SRC_FILES "src/*.cpp")

add_library(structlib STATIC ${SRC_FILES})
target_include_directories(structlib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
```

#### <b>collect to source files on var</b>

```cmake
file(GLOB SRC_FILES "src/*.cpp")
```

"file" is function related in file command
"GLOB SRC_FILES "src/*.cpp"" command searches matching on source files within src directory. The files will be included in SRC_FILES var.

#### <b>make library</b>

```cmake
add_library(structlib STATIC ${SRC_FILES})
```

Using SRC_FILES files, creating a library. name is structlib library

| Type    | Link Time        | Runtime Requirement | Output Files        |
|---------|------------------|---------------------|---------------------|
| STATIC  | Compile time     | None                | `.lib` / `.a`       |
| SHARED  | Runtime          | Required            | `.dll` / `.so`      |
| MODULE  | Runtime (manual) | Required            | `.dll` / `.so`      |

#### <b>include directory</b>

```cmake
target_include_directories(structlib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
```

Include the .h to structlib library.

| Scope     | Used by structlib | Used by dependents |
|-----------|-------------------|--------------------|
| PRIVATE   | Yes               | No                 |
| PUBLIC    | Yes               | Yes                |
| INTERFACE | No                | Yes                |

### <b>3. CMake command</b>

    Configuration → Build → Install

The commands can differ depending on the OS you are using.

### <b>3-1. Configuration</b>

```cmake
cmake -S . -B build 
```

##### <b>Configuration → Generate build system (no compilation)</b>

This step configures the project by reading the CMakeLists.txt file and generating the build system (e.g., Visual Studio solution, Makefiles, or Ninja files) inside the build directory.

* -S . specifies the source directory (current directory)
* -B build specifies the output directory for build files

👉 No compilation happens at this stage. It only prepares the build environment.

**OR** 

```cmake
make -S . -B build -G "Visual Studio 17 2022" -A x64
```
### <b>3-2. Build</b>

```cmake
cmake --build build --config Release
```

##### <b>Build → Compile and link targets</b>

This step compiles the source code and links the targets defined in the project.

* --build build tells CMake to build using the generated build system
* --config Release specifies the build configuration (required for multi-config generators like Visual Studio)

👉 This is where actual compilation and linking occur, producing executables and libraries.

**OR**  Linux OS

```cmake
cmake --build build --config Release
```

### <b>3-3. Install</b>

```cmake
cmake --install build --config Release --prefix install
```

##### <b>Install → Copy artifacts to distribution structure</b>

This step installs the built artifacts (executables, libraries, etc.) into a structured directory for distribution.

* --install build runs the install rules defined in CMakeLists.txt
* --config Release ensures the correct build configuration is used
* --prefix install sets the installation directory (e.g., ./install)

👉 Files are copied into organized folders such as bin/, lib/, etc., based on the install() rules.

### <b>3-4. Test</b>

```cmake
ctest --test-dir build -C Release --output-on-failure
```

This step runs the tests registered in the project using CTest.

* --test-dir build specifies the directory where the build was generated
* -C Release selects the build configuration (required for multi-config generators like Visual Studio)
* --output-on-failure prints detailed output only when a test fails

👉 CTest executes the test commands defined by add_test() in CMakeLists.txt.




