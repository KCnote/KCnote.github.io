---
layout: post
title: "02. CMake Tutorial, Multi-File C++"
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

## <b>1. How to use CMake with Multi-File?</b>

In this tutorial, we extend the basic example to a more realistic project structure:

* 2 header files (`.h`)
* 2 source files (`.cpp`)
* 1 `main.cpp`

We will learn how to organize and build a multi-file C++ project using CMake.

## <b>2. Tutorial</b>

#### <b>2-1. Project Structure</b>

```bash
project/
 ├── CMakeLists.txt
 ├── include/
 │    ├── math_utils.h
 │    └── print_utils.h
 └── src/
      ├── main.cpp
      ├── math_utils.cpp
      └── print_utils.cpp
```

#### <b>2-2. Context</b>

> Header

##### include/math_utils.h

```cpp
#pragma once

int add(int a, int b);
int multiply(int a, int b);
```

##### include/print_utils.h

```cpp
#pragma once

void print_result(const char* label, int value);
```

---

> Source Files

##### src/math_utils.cpp

```cpp
#include "math_utils.h"

int add(int a, int b) 
{
    return a + b;
}

int multiply(int a, int b) 
{
    return a * b;
}
```
##### src/print_utils.cpp

```cpp
#include <iostream>
#include "print_utils.h"

void print_result(const char* label, int value) 
{
    std::cout << label << ": " << value << std::endl;
}
```

> Main 

##### src/main.cpp

```cpp
#include "math_utils.h"
#include "print_utils.h"

int main() 
{
    int sum = add(3, 4);
    int product = multiply(3, 4);

    print_result("Sum", sum);
    print_result("Product", product);

    return 0;
}
```

> CMakeLists

```cmake
cmake_minimum_required(VERSION 3.10) 
project(MyApp) 

# Specify include directory 
include_directories(include) 

# Define executable and source files 
add_executable(MyApp 
	src/main.cpp 
	src/math_utils.cpp
	src/print_utils.cpp 
)
```

#### <b>2-3. Context Detail</b>

##### 1. `include_directories(include)`

* Adds `include/` folder to the compiler's header search path

##### 2. `add_executable(...)`

```cmake
add_executable(MyApp
    src/main.cpp
    src/math_utils.cpp
    src/print_utils.cpp
)
```

* Compile all `.cpp` files
* Link them together
* Produce executable `MyApp`

#### <b>2-4. Build Process</b>

```bash
mkdir build
cd build
cmake ..
cmake --build .
Debug\MyApp.exe
```

## <b>3. Compilation Model</b>

Each `.cpp` file is compiled separately:

```bash
math_utils.cpp → math_utils.o
print_utils.cpp → print_utils.o
main.cpp → main.o
```

Then linked:

```bash
math_utils.o + print_utils.o + main.o → MyApp
```