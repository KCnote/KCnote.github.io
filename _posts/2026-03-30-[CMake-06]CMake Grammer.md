---
layout: post
title: "06. CMake Grammer"
date: 2026-03-30 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - CMake]
tags: [Deploy, Deploy - CMake]
pin: false
math: true
mermaid: true
---

# <b>CMake Grammer</b>

---

## <b>1. Overview</b>

CMake is a **command-based scripting language** where everything is treated as **strings**.

```cmake
command(arg1 arg2 ...)
```

There are no strong types like C++ — behavior depends on how strings are interpreted.

---

## <b>2. Variables</b>

### <b>2-1. set()</b>

```cmake
set(VAR value)
```

**Description:**

Creates or updates a variable.

**Behavior:**

* Stored in current scope
* Overwritten if called again

---

### <b>2-2. Access Variable</b>

```cmake
message(${VAR})
```

**Description:**

`${}` expands the variable into its value.

---

### <b>2-3. List Behavior</b>

```cmake
set(LIST a b c)
```

**Description:**

CMake treats lists as semicolon-separated strings.

```text
a;b;c
```

---

### <b>2-4. unset()</b>

```cmake
unset(VAR)
unset(VAR CACHE)
```

**Description:**

* Removes variable
* Optional `CACHE` removes persistent value

---

## <b>3. Variable Expansion</b>

### <b>3-1. Basic Expansion</b>

```cmake
${VAR}
```

**Meaning:**

Replace with value of `VAR`.

---

### <b>3-2. Indirect Expansion (Important)</b>

```cmake
set(name FOO)
set(FOO hello)

message(${${name}})
```

**Meaning:**

1. `${name}` → `FOO`
2. `${FOO}` → `hello`

✔ Used for dynamic variable access

---

### <b>3-3. Environment Variables</b>

```cmake
$ENV{HOME}
```

**Description:**

Access OS environment variables.

---

## <b>4. Condition (if)</b>

```cmake
if(condition)
elseif(condition)
else()
endif()
```

**Description:**

Evaluates condition expressions.

---

### <b>4-1. Numeric Comparison</b>

```cmake
if(A GREATER 10)
```

✔ Integer comparison

---

### <b>4-2. String Comparison</b>

```cmake
if(A STREQUAL "hello")
```

✔ Exact string match

---

### <b>4-3. Regex Match</b>

```cmake
if(A MATCHES "h.*o")
```

✔ Regular expression

---

### <b>4-4. Logical Operators</b>

```cmake
if(A AND B)
if(A OR B)
if(NOT A)
```

---

### <b>4-5. Existence Check</b>

```cmake
if(DEFINED VAR)
if(EXISTS file.txt)
```

✔ Check if variable or file exists

---

### <b>4-6. Combined Pattern (Advanced)</b>

```cmake
if(DEFINED ${variable} AND "${${variable}}")
```

**Meaning:**

* Check if referenced variable exists
* Check if value is TRUE

✔ Common in large projects (e.g., OpenCV)

---

## <b>5. Dynamic Expression Execution</b>

```cmake
set(cond 2 GREATER 1)

if(${cond})
```

**Behavior:**

```cmake
if(2 GREATER 1)
```

✔ CMake executes expanded string as condition

---

## <b>6. Loops</b>

### <b>6-1. foreach()</b>

```cmake
foreach(x a b c)
  message(${x})
endforeach()
```

**Description:**

Iterates over values.

---

### <b>6-2. List Iteration</b>

```cmake
foreach(x ${LIST})
```

✔ Expands list first

---

### <b>6-3. Range Loop</b>

```cmake
foreach(i RANGE 0 10)
```

✔ Numeric iteration

---

### <b>6-4. while()</b>

```cmake
while(condition)
endwhile()
```

✔ Loop while condition is true

---

## <b>7. Functions and Macros</b>

### <b>7-1. function()</b>

```cmake
function(foo x)
  message(${x})
endfunction()
```

**Behavior:**

* Has its own scope
* Variables are local

---

### <b>7-2. macro()</b>

```cmake
macro(foo x)
  message(${x})
endmacro()
```

**Behavior:**

* No scope
* Acts like text substitution

✔ Used in OpenCV (OCV_OPTION)

---

### <b>7-3. Arguments</b>

```cmake
${ARGV0}
${ARGV1}
${ARGN}
```

---

## <b>8. Cache Variables</b>

```cmake
set(VAR value CACHE STRING "description")
```

**Description:**

Stores variable in `CMakeCache.txt`

---

### <b>Why Important</b>

* Persists between configure runs
* User can override via CLI or GUI

---

### <b>Types</b>

| Type     | Meaning   |
| -------- | --------- |
| STRING   | text      |
| BOOL     | ON/OFF    |
| PATH     | directory |
| FILEPATH | file      |

---

### <b>FORCE</b>

```cmake
set(VAR value CACHE STRING "" FORCE)
```

✔ Override existing cache value

---

## <b>9. option()</b>

```cmake
option(USE_CUDA "Enable CUDA" ON)
```

**Description:**

Creates user-configurable boolean option.

---

## <b>10. include() vs add_subdirectory()</b>

### <b>include()</b>

```cmake
include(utils.cmake)
```

✔ Executes file inline
✔ No target creation

---

### <b>add_subdirectory()</b>

```cmake
add_subdirectory(src)
```

✔ Executes another `CMakeLists.txt`
✔ Adds build targets

---

## <b>11. Targets</b>

### <b>11-1. Executable</b>

```cmake
add_executable(App main.cpp)
```

---

### <b>11-2. Library</b>

```cmake
add_library(MyLib STATIC lib.cpp)
```

---

### <b>11-3. Linking</b>

```cmake
target_link_libraries(App MyLib)
```

**Description:**

Defines dependency between targets.

---

### <b>11-4. Include Directories</b>

```cmake
target_include_directories(App PUBLIC include/)
```

---

### <b>11-5. Compile Options</b>

```cmake
target_compile_options(App PRIVATE -O2)
```

---

### <b>11-6. Definitions</b>

```cmake
target_compile_definitions(App PRIVATE USE_FEATURE)
```

---

## <b>12. install()</b>

```cmake
install(TARGETS App DESTINATION bin)
install(FILES a.h DESTINATION include)
```

**Description:**

Defines how build outputs are copied into a structured install directory.

---

### <b>12-1. Basic Concept</b>

`install()` does **not build anything**.

It **copies and organizes artifacts** produced during build.

Example result:

```
install/
 ├── bin/
 ├── lib/
 ├── include/
```

---

### <b>12-2. Common Signatures</b>

#### Install targets

```cmake
install(TARGETS App DESTINATION bin)
```

✔ Executables / libraries

---

#### Install files

```cmake
install(FILES a.h DESTINATION include)
```

✔ Headers or config files

---

#### Install directories

```cmake
install(DIRECTORY include/ DESTINATION include)
```

✔ Entire folder copy

---

### <b>12-3. COMPONENT (Important)</b>

```cmake
install(TARGETS App DESTINATION bin COMPONENT runtime)
install(FILES a.h DESTINATION include COMPONENT dev)
```

**Description:**

`COMPONENT` groups install rules into logical parts.

---

### <b>Why it exists</b>

Allows **partial installation**.

```bash
cmake --install build --component dev
```

✔ installs only development files (headers)

---

### <b>Typical Convention</b>

| Component | Meaning                |
| --------- | ---------------------- |
| runtime   | executables            |
| dev       | headers, cmake configs |
| library   | shared/static libs     |

---

### <b>Important Insight</b>

```text
COMPONENT is NOT a variable and NOT a folder name.
It is a label used by CMake to group install rules.
```


---

## <b>13. File Operations</b>

```cmake
file(GLOB SRC "*.cpp")
file(COPY src DESTINATION build)
```

---

## <b>14. string()</b>

```cmake
string(REPLACE "a" "b" OUT ${VAR})
string(TOUPPER ${VAR} OUT)
```

**Description:**

Performs string manipulation.

---

### <b>14-1. REPLACE</b>

```cmake
set(VAR "apple")
string(REPLACE "a" "b" OUT ${VAR})
```

Result:

```
OUT = bpple
```

---

### <b>14-2. TOUPPER / TOLOWER</b>

```cmake
string(TOUPPER "hello" OUT)
```

Result:

```
OUT = HELLO
```

---

### <b>14-3. CONCAT</b>

```cmake
string(CONCAT OUT "Hello " "World")
```

Result:

```
OUT = Hello World
```

---

### <b>14-4. REGEX</b>

```cmake
string(REGEX MATCH "h.*o" OUT "hello")
```

✔ Pattern matching

---

### <b>Important Insight</b>

```text
string() does NOT modify variables directly.
It always writes to an output variable.
```

---

## <b>15. list()</b>

```cmake
list(APPEND MYLIST item)
list(REMOVE_ITEM MYLIST item)
list(LENGTH MYLIST len)
```

**Description:**

Manipulates list variables.

---

### <b>15-1. Append</b>

```cmake
set(MYLIST a b)
list(APPEND MYLIST c)
```

Result:

```
a;b;c
```

---

### <b>15-2. Remove</b>

```cmake
list(REMOVE_ITEM MYLIST b)
```

Result:

```
a;c
```

---

### <b>15-3. Length</b>

```cmake
list(LENGTH MYLIST len)
```

Result:

```
len = 2
```

---

### <b>15-4. Get Element</b>

```cmake
list(GET MYLIST 0 first)
```

✔ Index-based access

---

### <b>Important Insight</b>

```text
CMake lists are just strings separated by ';'
```

---

## <b>16. message()</b>

```cmake
message("Hello")
message(STATUS "Info")
message(WARNING "Warning")
message(FATAL_ERROR "Stop")
```

**Description:**

Outputs messages during configure/build.

---

### <b>16-1. Levels</b>

| Level          | Meaning         |
| -------------- | --------------- |
| STATUS         | normal info     |
| WARNING        | warning message |
| AUTHOR_WARNING | dev warning     |
| FATAL_ERROR    | stop execution  |

---

### <b>Example</b>

```cmake
message(STATUS "Building project...")
```

---

### <b>Behavior</b>

```cmake
message(FATAL_ERROR "Something failed")
```

✔ Immediately stops CMake

---

### <b>Important Insight</b>

```text
message() is mainly used during configure phase
for debugging and logging
```

---

## <b>17. Built-in Variables</b>

```cmake
CMAKE_SYSTEM_NAME
CMAKE_SOURCE_DIR
CMAKE_BINARY_DIR
CMAKE_CURRENT_SOURCE_DIR
CMAKE_CURRENT_BINARY_DIR
```

---

## <b>18. Comments</b>

### <b>18-1. Single-line</b>

```cmake
# This is a comment
```

---

### <b>18-2. Multi-line</b>

```cmake
#[[
This is a multi-line comment
]]
```

---

### <b>Important Behavior</b>

```text
Comments are ignored during parsing
```

---

### <b>Common Usage</b>

```cmake
# Enable CUDA if available
option(USE_CUDA "Enable CUDA" ON)
```

---

### <b>Important Insight</b>

```text
CMake has no inline comment syntax (like // or /* */)
Only # and #[[ ]] are valid
```

---

## <b>19. Key Concepts</b>

```text
✔ Everything is string-based
✔ ${} controls execution
✔ if() evaluates expressions, not just values
✔ CACHE variables persist
✔ include executes code
✔ add_subdirectory builds structure
✔ target is the core abstraction
```

