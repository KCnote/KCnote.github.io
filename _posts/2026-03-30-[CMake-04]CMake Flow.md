---
layout: post
title: "CMake Flow"
date: 2026-03-30 00:00:00 +0900
author: kang
categories: [CMake, CMake - Fundamental]
tags: [CMake, CMake - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>CMake Flow</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is flow of CMake</b>

CMake follows a structured three-step workflow:
Configure → Build → Install
Each step has a clear and distinct purpose.

## <b>2. Workflow</b>

#### <b>2-1. Project Structure</b>

A typical CMake project contains:

* `CMakeLists.txt`
* Source files (`.cpp`)
* Header files (`.h` / `.hpp`)

CMake uses **CMakeLists.txt** as the main entry point.

#### <b>2-2. Out-of-Source Build</b>

Create a separate build directory to keep generated files isolated:

```
/build
```

This prevents mixing source files with build artifacts.


#### <b>2-3. Configure Step</b>

Run the following command:

```
cmake -S . -B build
```

##### What happens during configure:

* Parses `CMakeLists.txt`
* Detects and configures compiler
* Generates build system files:

  * `Makefile`
  * `.sln` (Visual Studio)
  * `build.ninja`
* Creates:

  * `CMakeCache.txt`
  * `CMakeFiles/`

⚠️ No compilation happens at this stage.

#### <b>2-3 (Optional). Configure Step Using CMakePresets</b>

Instead of writing long CLI commands, you can define presets in:

```
CMakePresets.json
```

Usually placed at the same level as `CMakeLists.txt`.

```
cmake --preset <preset-name>
```

##### Important:

* `cmake -S -B` and `cmake --preset` both perform **configure**
* You should use **only one of them**

Example (CLI):

```
cmake -S . -B build \
  -DCMAKE_BUILD_TYPE=Release \
  -DOpenCV_DIR=... \
  -G Ninja
```

Example (Preset):

```
cmake --preset <preset-name>
```

* CLI → manual input
* Preset → predefined configuration (JSON)

#### <b>2-4 Build Step (Compile + Link)</b>

```
cmake --build build
```

* `.cpp → .o / .obj` (compile)
* `.o / .obj → executable / library` (link)

Inside the `build/` directory, you may see:

* Object files (`.o`, `.obj`)
* Executables (`.exe`, binary)
* Static libraries (`.lib`, `.a`)
* Shared libraries (`.dll`, `.so`)

#### <b>2-5 Install Step</b>

```
cmake --build build --target install
```

Install takes build outputs and organizes them based on rules defined in `CMakeLists.txt`.
When we process install, it calls cmake_install.cmake files

Typical structure:

```
install/
 ├── bin/
 ├── lib/
 ├── include/
```

Install is NOT just "cleaning up".

It is copying and organizing build outputs into a structured layout for distribution or reuse.

## <b>3. Summary</b>

```
Configure → Build → Install
   ↓          ↓        ↓
Setup     Compile   Package/Deploy
```

* `cmake -S -B` and `cmake --preset` both perform **configure**
* Presets are just a way to **store configuration**
* Build step performs actual compilation
* Install step prepares a **clean, usable structure**
