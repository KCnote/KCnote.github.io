---
layout: post
title: "CMake Tutorial with OpenCV"
date: 2026-03-30 00:00:00 +0900
author: kang
categories: [CMake, CMake - Fundamental]
tags: [CMake, CMake - Fundamental]
pin: false
math: true
mermaid: true
---

# <b>CMake with OpenCV</b>

---

### <b>Prerequisites</b>


---

## <b>1. How to use CMake with OpenCV?</b>

main opencv: https://github.com/opencv/opencv
sub opencv:  https://github.com/opencv/opencv_contrib

```bat
rmdir /s /q C:\opencv\opencv_build
mkdir C:\opencv\opencv_build
cd /d C:\opencv\opencv
cmake --preset opencv 
cmake --build C:\opencv\opencv_build --config Release
cmake --build C:\opencv\opencv_build --config Release --target INSTALL
```

## <b>2. Tutorial</b>

#### <b>2-1. Project Structure</b>

```text
C:\opencv\
 ├─ opencv              // opencv main library
 ├─ opencv_build        // for build
 ├─ opencv_contrib      // opencv sub library
 └─ opencv_install      // for install
```

```text
opencv_build   = build workspace (temporary)
opencv_install = final usable OpenCV package

build  = compile
install = organize + copy
```

> After finishing install
```text
C:\opencv\opencv_install\
 ├─ include
 └─ x64
     └─ vc17
         ├─ lib
         └─ bin
```

#### <b>2-2. Context</b>

> Json, Name is "CMakePresets.json"

```json
{
  "version": 3,
  "configurePresets": [
    {
      "name": "opencv",
      "generator": "Visual Studio 17 2022",
      "binaryDir": "C:/opencv/opencv_build",
      "architecture": {
        "value": "x64"
      },
      "cacheVariables": {
        "CMAKE_INSTALL_PREFIX": "C:/opencv/opencv_install",
        "OPENCV_EXTRA_MODULES_PATH": "C:/opencv/opencv_contrib/modules",
        "BUILD_opencv_world": "ON",
        "BUILD_TESTS": "OFF",
        "BUILD_PERF_TESTS": "OFF",
        "BUILD_EXAMPLES": "OFF",
        "BUILD_DOCS": "OFF",
        "BUILD_JAVA": "OFF",
        "BUILD_opencv_python2": "OFF",
        "BUILD_opencv_python3": "OFF",
        "OPENCV_PYTHON_SKIP_DETECTION": "ON"
      }
    }
  ]
}
```

> CMakeLists, For build and install opencv

It already created in opencv folder files, just using

> CMakeLists, For creating test real project

```cmake
cmake_minimum_required(VERSION 3.10)
project(OpenCVTest)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(OpenCV_DIR "C:/opencv/opencv_install/x64/vc17/lib")

find_package(OpenCV REQUIRED)

add_executable(OpenCVTest main.cpp)

target_link_libraries(OpenCVTest PRIVATE ${OpenCV_LIBS})
```

> Main

```cpp
#include <opencv2/opencv.hpp>
#include <iostream>

int main()
{
    cv::Mat img = cv::Mat::zeros(300, 500, CV_8UC3);

    cv::putText(
        img,
        "OpenCV Install OK",
        cv::Point(40, 150),
        cv::FONT_HERSHEY_SIMPLEX,
        1.0,
        cv::Scalar(0, 255, 0),
        2
    );

    cv::imshow("Test", img);
    cv::waitKey(0);

    return 0;
}
```

#### <b>2-3. Context Detail</b>

##### 1. `set(CMAKE_CXX_STANDARD 17)`

* the compiler to use C++17

##### 2. `set(CMAKE_CXX_STANDARD_REQUIRED ON)`

* Enforces strict requirement

    * OFF → compiler may downgrade (e.g., C++14)
    * ON → fails if C++17 is not supported

##### 3. `set(OpenCV_DIR "C:/opencv/opencv_install/x64/vc17/lib")`

* Look here to find OpenCV

##### 4. `find_package(OpenCV REQUIRED)`

* Loads OpenCV into CMake
* find OpenCVConfig.cmake → Create OpenCV_LIBS etc..

##### 5. `target_link_libraries(OpenCVTest PRIVATE ${OpenCV_LIBS})`

* Links OpenCV libraries to your executable

#### <b>2-4. Set Environment</b>

Add to PATH:

```text
C:\opencv\opencv_install\x64\vc17\bin
```

#### <b>2-5. Link and Start project</b>

```text
C:\Users\...\Desktop\CMakeTest\Project\
 ├─ build
 ├─ CMakeLists.txt
 └─ main.cpp
```

```bat
cd /d "C:\Users\...\Desktop\CMakeTest\Project03"

cmake -S . -B build -G "Visual Studio 17 2022" -A x64
cmake --build build --config Release
```

```text
build\Release\OpenCVTest.exe
```