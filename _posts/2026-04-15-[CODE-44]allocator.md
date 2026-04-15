---
layout: post
title: "std::allocator"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>std::allocator</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is an Allocator</b>

An **allocator** is a mechanism that handles **memory allocation and deallocation**

for objects used in containers like:

- `std::vector`
- `std::list`
- `std::map`

Containers **do not allocate memory directly**

Instead:

```
Container → Allocator → Memory
```

When It Matters:

- high-frequency allocation
- real-time systems
- game engines
- memory-constrained environments

#### Basic Usage

```cpp
#include <memory>

std::allocator<int> alloc;
int* p = alloc.allocate(5);  // allocate space for 5 ints
```

Only allocates memory (no construction)

##### Construct Object

```cpp
std::construct_at(p, 10);
```

##### Destroy Object

```cpp
std::destroy_at(p);
```

##### Deallocate Memory

```cpp
alloc.deallocate(p, 5);
```

#### Full Example

```cpp
#include <iostream>
#include <memory>

int main()
{
    std::allocator<int> alloc;

    int* p = alloc.allocate(1);
    std::construct_at(p, 42);       // new(p) int(42);

    std::cout << *p << "\n";

    std::destroy_at(p);
    alloc.deallocate(p, 1);
}
```

## <b>2. Why Allocator Exists</b>

#### <b>2-1. Separation of Concerns</b>

- Container → data structure logic
- Allocator → memory management

#### <b>2-2. Custom Memory Strategies</b>

You can replace default allocator with:

- memory pool
- arena allocator
- stack allocator

#### <b>2-3. Performance Optimization</b>

👉 Avoid frequent `new/delete`

#### Example

```cpp
std::vector<int> v;
```

Internally:

```cpp
std::vector<int, std::allocator<int>>
```

#### Custom Allocator Example

```cpp
template<typename T>
struct MyAllocator
{
    using value_type = T;

    T* allocate(size_t n)
    {
        return static_cast<T*>(::operator new(n * sizeof(T)));
    }

    void deallocate(T* p, size_t)
    {
        ::operator delete(p);
    }
};
```

```cpp
std::vector<int, MyAllocator<int>> v;
```

## <b>3. Advanced Concepts</b>

#### Rebinding (Old concept)

Used to allocate different types. Mostly replaced by `allocator_traits`

```cpp
std::allocator<int>
```

```cpp
Alloc::rebind<U>::other
```

```cpp
std::allocator<int> alloc; // std::allocator<double>: want to be exchanged
typename std::allocator<int>::rebind<double>::other new_alloc;
```

#### Allocator Traits

```cpp
std::allocator_traits<Alloc>
```

```cpp
using Alloc = std::allocator<int>;
using NewAlloc = std::allocator_traits<Alloc>::rebind_alloc<double>;
```

Standard way to interact with allocators

#### construct/destroy

##### ❗ Deprecated

```cpp
alloc.construct(...)
alloc.destroy(...)
```

##### ✔️ Use Instead

```cpp
std::construct_at(ptr, args...);
std::destroy_at(ptr);
```

| Feature | Allocator | new/delete |
|--------|----------|------------|
| Abstraction | High | Low |
| Flexibility | High | Low |
| Customization | Yes | No |
| Used in STL | Yes | No |

## <b>4. Advanced Concepts</b>

#### ❌ Mixing new/delete with allocator

```cpp
int* p = new int;
alloc.deallocate(p, 1);  // ❌ wrong
```
