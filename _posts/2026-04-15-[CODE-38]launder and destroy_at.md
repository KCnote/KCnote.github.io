---
layout: post
title: "std::launder and std::destroy_at"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::launder` and `std::destroy_at`</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is `std::launder` and `std::destroy_at`</b>

These two utilities are not used every day, but they become important when working with:

- manual object lifetime management
- placement `new`
- low-level memory handling
- custom containers / allocators
- performance-critical systems

They are part of modern C++ utilities for handling objects more safely at a low level.

## <b>2. What is `std::destroy_at`?</b>

`std::destroy_at` explicitly destroys an object at a given address.

```cpp
#include <memory>
```

```cpp
std::destroy_at(ptr);
```

It calls the destructor of the object pointed to by `ptr`.

Use it when:
- you constructed an object manually in raw storage
- you are managing lifetime explicitly
- you are writing allocator/container internals

##### ✔️ Basic Example

```cpp
#include <iostream>
#include <memory>

struct A
{
    ~A()
    {
        std::cout << "Destructor called\n";
    }
};

int main()
{
    A obj;
    std::destroy_at(&obj);
}
```

This is usually **not** meant for ordinary stack objects like above in real code.

Because:

- stack objects are automatically destroyed at scope end
- calling destruction manually may lead to **double destruction**

`std::destroy_at` is mainly for objects whose lifetime you manage manually.

It is commonly used with:

- raw storage
- placement `new`
- custom allocators
- container internals

##### Example with Placement `new`

```cpp
#include <memory>
#include <new>
#include <iostream>

struct A 
{
    int x;
    A(int v) : x(v) 
    {
        std::cout << "construct " << x << "\n";
    }
    ~A() 
    {
        std::cout << "destroy " << x << "\n";
    }
};

int main() {
    const int N = 3;

    void* raw = operator new(sizeof(A) * N);
    A* arr = static_cast<A*>(raw);

    int size = 0;

    for (int i = 0; i < N; ++i) 
    {
        new(&arr[size]) A(i); // placement new
        size++;
    }

    std::cout << "---- reuse ----\n";

    // pop_back
    std::destroy_at(&arr[--size]);

    // push_back again
    new(&arr[size]) A(100);

    std::cout << "---- cleanup ----\n";

    for (int i = 0; i < size; ++i) 
        std::destroy_at(&arr[i]);

    operator delete(raw);
}
```

Allocate memory using `operator new`, destroy only the object (not the memory) when calling `pop_back`, and reconstruct a new object in the same memory location using placement new (`new(...)`) when needed.


This is the correct low-level pattern

#### <b>2-1. Why `std::destroy_at` Exists</b>

Before modern C++, people often wrote:

```cpp
ptr->~A();
```

That works, but it is:

- more explicit and harder to generalize
- less readable
- more error-prone in template code

With `std::destroy_at`:

```cpp
std::destroy_at(ptr);
```

Cleaner and safer in generic code

#### <b>2-2. Performance</b>

`std::destroy_at` itself has essentially **no overhead**. It simply calls the destructor.
For trivial types like `int`, there is effectively nothing to destroy. For non-trivial types, its cost is just the destructor cost.

- essentially just destructor call
- zero abstraction overhead

#### <b>2-3. Common Mistake</b>

##### ❌ Destroying an object twice

```cpp
A obj;
std::destroy_at(&obj);  // dangerous
```

If `obj` later goes out of scope, destructor runs again. Only manually destroy objects whose lifetime you manually control.



## <b>3. What is `std::launder`?</b>

`std::launder` is used when you need to obtain a valid pointer to an object after its lifetime has been changed or restarted in the same storage.

```cpp
#include <new>

std::launder(ptr);
```

It tells the compiler:

> "Treat this as a pointer to the newly created object in this storage."

Use it when:
- an object is reconstructed in the same storage
- old pointers may not properly refer to the new object
- you are doing advanced lifetime manipulation

#### <b>3-1. Why Do We Need `std::launder`?</b>

This is about **object lifetime** and **compiler assumptions**.

When an object is destroyed and another object is created in the same memory location, the old pointer may no longer be enough for the compiler to safely reason about the new object.

`std::launder` helps in these tricky cases.

##### Example Pattern

```cpp
#include <iostream>
#include <new>

struct A
{
    int x;
};

int main()
{
    alignas(A) unsigned char buffer[sizeof(A)];
    A* p = new (buffer) A{10};
    p->~A();
    new (buffer) A{20};

    A* q = std::launder(reinterpret_cast<A*>(buffer));

    std::cout << q->x << "\n";  // 20
}
```

#### <b>3-2. What Problem Does `std::launder` Solve?</b>

Without `std::launder`, the compiler may assume things based on the previous object lifetime.
In very optimized code, this can create undefined behavior or incorrect assumptions.
This is especially relevant when:

- reusing storage for a new object
- replacing an object in-place
- low-level memory tricks
- object identity changes in the same location

#### <b>3-3. Placement `new` into Existing Storage</b>

```cpp
T* p = ...;
std::destroy_at(p);
new (p) T(...);
```

After reconstructing the object in the same storage, in some cases you should use:

```cpp
p = std::launder(p);
```

This makes the pointer refer properly to the new object.

#### <b>3-4. Performance Perspective</b>

- usually no runtime cost
- mostly a compiler-facing semantic tool
- helps correctness more than raw speed

## <b>4. `std::destroy_at` vs `std::launder`</b>

These two solve very different problems.

| Utility | Purpose |
|--------|---------|
| `std::destroy_at(ptr)` | destroy object at address |
| `std::launder(ptr)` | get valid pointer to object after lifetime/storage reuse |

#### Example

```cpp
#include <iostream>
#include <memory>
#include <new>

struct A
{
    int value;

    A(int v) : value(v)
    {
        std::cout << "Constructed: " << value << "\n";
    }

    ~A()
    {
        std::cout << "Destroyed: " << value << "\n";
    }
};

int main()
{
    alignas(A) unsigned char buffer[sizeof(A)];

    A* ptr = new (buffer) A(10);
    std::cout << ptr->value << "\n";
    std::destroy_at(ptr);

    new (buffer) A(20);
    ptr = std::launder(reinterpret_cast<A*>(buffer));

    std::cout << ptr->value << "\n";
}
```

```cpp
Constructed: 10
10
Destroyed: 10
Constructed: 20
20
```
