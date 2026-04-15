---
layout: post
title: "Function Pointers"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b> Function Pointers</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is a Function Pointer?</b>

A **function pointer** is a variable that stores the **address of a function**. Just like pointers to variables, but pointing to **functions**

```cpp
return_type (*pointer_name)(parameter_types);
```

Use when:

- You need **C-style callbacks**
- You want **low-overhead dynamic dispatch**
- You are working with **legacy or embedded systems**

##### ✔️ Example

```cpp
#include <iostream>

int add(int a, int b)
{
    return a + b;
}

int main()
{
    int (*funcPtr)(int, int) = add;

    std::cout << funcPtr(2, 3);  // 5
}
```

`funcPtr` now points to `add`

## <b>2. Why Use Function Pointers?</b>

#### <b>2-1. Callback Functions</b>

```cpp
void execute(int (*func)(int, int))
{
    std::cout << func(2, 3);
}
```

#### <b>2-2. Dynamic Behavior</b>

```cpp
int add(int a, int b) { return a + b; }
int sub(int a, int b) { return a - b; }

int (*op)(int, int);

op = add;
std::cout << op(5, 3);  // 8

op = sub;
std::cout << op(5, 3);  // 2
```

#### <b>2-3. Table of Functions</b>

```cpp
int add(int a, int b) { return a + b; }
int sub(int a, int b) { return a - b; }

int (*ops[2])(int, int) = {add, sub};

std::cout << ops[0](3, 2);  // add
std::cout << ops[1](3, 2);  // sub
```

## <b>3. Performance Characteristics</b>

##### ✔️ Direct Call

```cpp
add(2, 3);
```

##### ✔️ Function Pointer Call

```cpp
funcPtr(2, 3);
```

| Aspect | Direct Call | Function Pointer |
|--------|------------|------------------|
| Inline Optimization | ✅ Possible | ❌ Not possible |
| Branch Prediction | Better | Slightly worse |
| Overhead | Minimal | Small overhead |
| Flexibility | Low | High |

#### <b>3-1. No Inlining</b>

```cpp
int add(int a, int b) { return a + b; }
```

Compiler may inline:

```cpp
int result = 2 + 3;  // optimized
```

But with function pointer:

```cpp
funcPtr(2, 3);
```

Compiler **cannot inline** (target unknown at compile time)

#### <b>3-2. Indirect Call</b>

Function pointer introduces an **indirect jump**

```text
Direct call: call add
Pointer call: call [address]
```

This affects CPU pipeline & branch prediction

- Usually **very small overhead**
- Becomes noticeable in:
  - tight loops
  - high-frequency calls
  - performance-critical systems

## <b>4. Function Pointer vs Modern Alternatives</b>

##### ✔️ Function Pointer

```cpp
int (*func)(int, int);
```

##### ✔️ `std::function`

```cpp
#include <functional>

std::function<int(int, int)> func = add;
```

More flexible but **slower**

#### ✔️ Lambda

```cpp
auto func = [](int a, int b) { return a + b; };
```

Often **inlined → fastest**

| Method | Speed | Flexibility |
|--------|------|------------|
| Direct Call | 🔥 Fastest | ❌ |
| Lambda | 🔥 Fast | ✅ |
| Function Pointer | ⚡ Medium | ✅ |
| std::function | 🐢 Slowest | 🔥 Most flexible |

## <b>5. Common Mistakes</b>

#### ❌ Forgetting pointer syntax

```cpp
int *func(int, int);  // ❌ wrong
```

✔️ Correct:

```cpp
int (*func)(int, int);
```

#### ❌ Null pointer call

```cpp
int (*func)(int, int) = nullptr;

func(2, 3);  // ❌ crash
```

✔️ Always check:

```cpp
if (func)
{
    func(2, 3);
}
```

## <b>6. Function Pointer Typedef</b>

```cpp
typedef int (*FuncPtr)(int, int);

FuncPtr f = add;
```

Or (modern C++):

```cpp
using FuncPtr = int (*)(int, int);

FuncPtr f = add;
```

#### Example

```cpp
#include <iostream>

using Operation = int (*)(int, int);

int add(int a, int b) { return a + b; }
int mul(int a, int b) { return a * b; }

int compute(Operation op, int a, int b)
{
    return op(a, b);
}

int main()
{
    std::cout << compute(add, 2, 3);  // 5
    std::cout << compute(mul, 2, 3);  // 6
}
```
