---
layout: post
title: "Smart Pointers"
date: 2026-04-16 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>Smart Pointers</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is a Smart Pointers</b>

A **smart pointer** is a wrapper around a raw pointer that **automatically manages memory**.

It prevents:
- memory leaks
- double free
- dangling pointers

```text
Raw Pointer → manual delete
Smart Pointer → Automatically management (RAII)
```

Smart pointers are based on **RAII (Resource Acquisition Is Initialization)**

- resource acquired in constructor
- released in destructor

## <b>2. Why Use Smart Pointers</b>

#### ❌ Raw Pointer Problem

```cpp
int* p = new int(10);
// forgot delete → memory leak
```

#### ✔️ Smart Pointer Solution

```cpp
std::unique_ptr<int> p = std::make_unique<int>(10);
// 자동으로 delete됨
```

## <b>3. Types of Smart Pointers</b>

#### <b>3-1. `std::unique_ptr`</b>

- **exclusive ownership**
- copy impossible
- move possible

##### Example

```cpp
#include <memory>

auto p = std::make_unique<int>(10);
```

##### ✔️ Move

```cpp
auto p1 = std::make_unique<int>(10);
auto p2 = std::move(p1);
```

#### <b>3-2. `std::shared_ptr`</b>

- **shared ownership**
- reference counting

##### Example

```cpp
#include <memory>

auto p1 = std::make_shared<int>(10);
auto p2 = p1;  // 공유
```

##### ✔️ Reference Count

```cpp
p1.use_count();  // how many shared pointer shared
```

### Internal architecture

```text
Control Block:
- reference count
- weak count
- pointer
```

#### <b>3-3. `std::weak_ptr`</b>

- References a `shared_ptr` without owning it
- Does not increase the reference count

##### Example

```cpp
std::shared_ptr<int> sp = std::make_shared<int>(10);
std::weak_ptr<int> wp = sp;
```

##### ✔️ Access

```cpp
if (auto s = wp.lock())
    std::cout << *s;
```

**prevent circular reference**

###### Circular Reference Problem

```cpp
struct A;
struct B;

struct A
{
    std::shared_ptr<B> b;
};

struct B
{
    std::shared_ptr<A> a;
};
```

```cpp
auto a = std::make_shared<A>();
auto b = std::make_shared<B>();

a->b = b;
b->a = a;
```

```cpp
A: count = 2 (a, b->a)
B: count = 2 (b, a->b)
```

They hold references to each other → memory is never released

```
a.reset();
b.reset();
```

```
A: count = 1 (B hold A)
B: count = 1 (A hold B)
```

##### ✔️ Resolution

```cpp
struct B
{
    std::weak_ptr<A> a;
};
```

## <b>4. Performance Comparison</b>

#### `unique_ptr` / `shared_ptr` / `weak_ptr`

| Type | Speed | Overhead |
|------|------|---------|
| `unique_ptr` | 🔥 fastest | None |
| `shared_ptr` | ⚡ medium | reference count |
| `weak_ptr` | ⚡ low | control block |

#### Smart Pointer vs Raw Pointer

| Feature | Raw Pointer | Smart Pointer |
|--------|------------|--------------|
| Memory safety | ❌ | ✅ |
| Ownership tracking | ❌ | ✅ |
| Performance | 🔥 fastest | slightly slower |
| Ease of use | ❌ | ✅ |

## <b>5. Miscellaneous</b>

#### Custom Deleter

```cpp
std::unique_ptr<FILE, decltype(&fclose)>
    file(fopen("test.txt", "r"), &fclose);
```

possible to resource management
