---
layout: post
title: "std::function"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b> std::function</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is a 'std::function'?</b>

`std::function` is a **general-purpose function wrapper** that can store and invoke:

- Function pointers
- Lambda expressions
- Functors (function objects)
- Member functions

```cpp
#include <functional>

std::function<return_type(parameter_types)>
```

"A container for anything callable"

Use it when:

- You need **generic callback interface**
- You need to store **different callable types**
- Flexibility is more important than raw performance

##### ✔️ Declaration

```cpp
std::function<int(int, int)> func;
```

##### ✔️ Assign Function

```cpp
int add(int a, int b)
{
    return a + b;
}

func = add;
```

##### ✔️ Call

```cpp
std::cout << func(2, 3);  // 5
```

## <b>2. Why Use 'std::function'?</b>

##### ✔️ 1. Unified Interface

```cpp
void execute(std::function<int(int, int)> f)
{
    std::cout << f(2, 3);
}
```

Accepts ANY callable

##### ✔️ 2. High Flexibility

- Can change behavior at runtime
- Supports lambdas with captures

| Method | Speed | Notes |
|--------|------|------|
| Direct Call | 🔥 Fastest | Inline 가능 |
| Lambda (no capture) | 🔥 Fast | Inline 가능 |
| Function Pointer | ⚡ Medium | Indirect call |
| `std::function` | 🐢 Slowest | Type erasure overhead |

## <b>3. Why is `std::function` slower?</b>

##### 1. Type Erasure

`std::function` hides actual type

```cpp
std::function<int(int,int)> f;
```

###### Internally stores:
- pointer to callable
- virtual-like dispatch mechanism

##### 2. Indirect Call

```cpp
f(2,3);
```

- Cannot be inlined  
- Requires indirect dispatch

##### 3. Possible Heap Allocation

Large objects (capturing lambda) may allocate memory

But:
- Overhead is usually **small**
- But matters in:
  - tight loops
  - real-time systems
  - high-frequency calls

| Feature | Function Pointer | Lambda | std::function |
|--------|----------------|--------|--------------|
| Flexibility | Low | Medium | High |
| Performance | Medium | High | Low |
| Inline | ❌ | ✅ | ❌ |
| Capture Support | ❌ | ✅ | ✅ |

##### Example: Lambda, std::function

```cpp
#include <iostream>
#include <functional>

int add(int a, int b) { return a + b; }

int main()
{
    std::function<int(int,int)> f;

    f = add;
    std::cout << f(2, 3) << "\n";

    f = [](int a, int b)
    {
        return a * b;
    };

    std::cout << f(2, 3) << "\n";
}
```

## <b>4. Common Mistakes</b>

##### ❌ Overusing in hot loops

```cpp
for (...)
{
    func(x, y);  // ❌ slow if called frequently
}
```

Consider lambda or function pointer

##### ❌ Forgetting empty check

```cpp
std::function<void()> f;

f();  // ❌ crash
```

✔️ Correct:

```cpp
if (f)
{
    f();
}
```
