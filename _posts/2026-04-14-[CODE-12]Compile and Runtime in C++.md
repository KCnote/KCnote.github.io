---
layout: post
title: "Compile-Time and Runtime in C++"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Compile-Time vs Runtime in C++</b>

---

### <b>Prerequisites</b>
    - Compile
    - Runtime

---

## <b>1. What is difference between Compile-Time vs Runtime in C++</b>

In C++, operations can be executed at either:

- **Compile-time** → before the program runs
- **Runtime** → while the program is running

👉 Understanding this difference is critical for **performance, safety, and optimization**

1. Is this computed at **compile-time or runtime**?
2. If runtime → **can it be moved to compile-time?**

> Move as much work as possible to compile-time for better performance

| Category | Compile-Time | Runtime |
|----------|-------------|--------|
| Timing | Before execution | During execution |
| Performance | Faster | Slower |
| Flexibility | Less flexible | More flexible |
| Safety | High | Depends |


## <b>2. Compile-Time</b>

Computed **during compilation**, no runtime cost

#### 🔹 `constexpr`
```cpp
constexpr int square(int x) 
{
    return x * x;
}
```

Evaluated at compile-time if inputs are known

#### 🔹 `const`

```cpp
const int x = 10;
```
Compile-time **only if initialized with constant expression**. If assigned from function → runtime

#### 🔹 `consteval` (C++20)

```cpp
consteval int square(int x) 
{
    return x * x;
}
```
**Must** be evaluated at compile-time

#### 🔹 `constinit` (C++20)

```cpp
constinit int x = 10;
```
Ensures **static initialization at compile-time**

#### 🔹 Template

```cpp
template<int N>
struct Square 
{
    static constexpr int value = N * N;
};
```

Fully compile-time computation

#### 🔹 `inline`

Suggests compile-time expansion (not guaranteed)

#### 🔹 Macro

```cpp
#define SQUARE(x) ((x)*(x))
```

Preprocessor substitution (before compilation)

#### 🔹 `static_assert`

```cpp
static_assert(sizeof(int) == 4);
```

Compile-time validation


#### 🔹 `enum`

```cpp
enum { SIZE = 10 };
```

Compile-time constant

#### 🔹 `sizeof`

```cpp
sizeof(int);
```

Always compile-time

#### 🔹 Array size

```cpp
int arr[10];
```

Must be known at compile-time

#### 🔹 `typeid`
```cpp
typeid(int);
```

Compile-time (non-polymorphic types)

## <b>3. RunTime</b>

Computed **during program execution**

#### 🔹 Dynamic allocation

```cpp
int* p = new int(10);
```

#### 🔹 Virtual function

```cpp
class Base 
{
public:
    virtual void foo() {}
};
```

Resolved via vtable at runtime

#### 🔹 `std::function`

```cpp
std::function<int(int)> f = [](int x){ return x * x; };
```

Type-erased, runtime overhead

#### 🔹 Async / Thread

```cpp
std::async(...);
std::thread(...);
```

#### 🔹 `dynamic_cast`

```cpp
dynamic_cast<Derived*>(basePtr);
```

Runtime type checking (RTTI)

#### 🔹 `reinterpret_cast`

```cpp
reinterpret_cast<int*>(ptr);
```

Unsafe, runtime reinterpretation

#### 🔹 `unknown function`

```cpp
int x = getValue(); // unknown at compile-time
```