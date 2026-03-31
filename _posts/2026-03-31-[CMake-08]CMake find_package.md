---
layout: post
title: "find_package() Works in CMake"
date: 2026-03-31 00:00:00 +0900
author: kang
categories: [CMake, CMake - Fundamental]
tags: [CMake, CMake - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>How `find_package()` Works in CMake (Step-by-Step)</b>

---

### <b>Prerequisites</b>

---

## <b>1. Step-by-Step Guide</b>

This post explains what actually happens when you call `find_package()` in CMake, including how `.cmake` files are located and executed.

## <b>2. Executable</b>

### <b>2-1. Starting Point</b>

```cmake
find_package(OpenCV REQUIRED)
```

This single line triggers a multi-step process inside CMake.

### <b>2-2. Determine Search Mode</b>

CMake tries two modes:

#### 1) Config Mode (Preferred)

Search for:

```text
OpenCVConfig.cmake
opencv-config.cmake
```

#### 2) Module Mode (Fallback)

Search for:

```text
FindOpenCV.cmake
```

Modern libraries (like OpenCV, fmt) use **Config Mode**

### <b>2-3. Search Paths</b>

CMake searches in the following order:

1. `OpenCV_DIR` (if manually set)
2. `CMAKE_PREFIX_PATH`
3. System install locations

Example:

```cmake
set(OpenCV_DIR "C:/opencv/install/lib/cmake/opencv4")
```

### <b>2-4. Load the Config File</b>

Once found:

```cmake
include(OpenCVConfig.cmake)
```

This is the most important step.
`find_package()` is essentially:

```
"find + include"
```

### <b>2-5. Config File Execution</b>

The `.cmake` file is **executed as code**, not just read.

Inside `OpenCVConfig.cmake`:

#### 1) Environment Detection

```cmake
set(OpenCV_ARCH x64)
set(OpenCV_RUNTIME vc17)
```

Detect platform, compiler, architecture

#### 2) Locate Actual Library Path

```cmake
check_one_config(OpenCV_LIB_PATH)
```

Finds correct directory like:

```text
x64/vc17/lib
```

#### 3 Include Secondary Config

```cmake
include("${OpenCV_LIB_PATH}/OpenCVConfig.cmake")
```

Delegates to the real config file

### <b>2-6. Define Variables</b>

Inside the final config file:

```cmake
set(OpenCV_INCLUDE_DIRS ...)
set(OpenCV_LIBS ...)
set(OpenCV_VERSION ...)
```

These become available in your project

### <b>2-7. Create Imported Targets</b>

Modern CMake defines targets:

```cmake
add_library(OpenCV::OpenCV IMPORTED)
```

And sets properties:

```cmake
INTERFACE_INCLUDE_DIRECTORIES
IMPORTED_LOCATION
```

### <b>2-8. Dependency Propagation</b>

```cmake
target_link_libraries(OpenCV::OpenCV INTERFACE opencv_core opencv_imgproc)
```

👉 Dependencies are automatically passed to your target

### <b>2-9. Your Code Uses It</b>


```cmake
target_link_libraries(myapp OpenCV::OpenCV)
```

* adds include directories
* links libraries
* propagates dependencies

## <b>3. Full Flow Summary</b>

```
find_package(OpenCV)
        ↓
search OpenCVConfig.cmake
        ↓
include(OpenCVConfig.cmake)
        ↓
detect platform (x64, vc17, etc.)
        ↓
resolve actual library path
        ↓
include secondary config
        ↓
define variables + targets
        ↓
ready to use in your project
```