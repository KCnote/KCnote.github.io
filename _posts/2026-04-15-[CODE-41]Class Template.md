---
layout: post
title: "Class Template"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b>Class Template</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is a Template</b>

A template allows you to write **generic, reusable code**. Instead of fixing a type, you parameterize it:

```cpp
template<typename T>
```

A function template defines a **generic function** that works with different types.

#### Example

```cpp
#include <iostream>

template<typename T>
T add(T a, T b)
{
    return a + b;
}

int main()
{
    std::cout << add(2, 3);        // int
    std::cout << add(2.5, 3.1);    // double
}
```

- Type is **deduced automatically**
- Simpler to use
- Used for **operations / algorithms**

#### ✔️ Multiple Types

```cpp
template<typename T, typename U>
auto add(T a, U b)
{
    return a + b;
}
```

##### Limitation

Cannot do **partial specialization**

## <b>2. What is a Class Template</b>

A class template defines a **generic class**.

#### Example

```cpp
#include <iostream>

template<typename T>
class Box
{
public:
    T value;

    Box(T v) : value(v) {}

    void print()
    {
        std::cout << value << "\n";
    }
};

int main()
{
    Box<int> b1(10);
    Box<std::string> b2("hello");

    b1.print();
    b2.print();
}
```

- Type must be **explicitly specified**
- Used for **data structures / containers**
- Supports **partial specialization**

#### Partial Specialization (Class Only)

```cpp
template<typename T>
class Box<T*>
{
public:
    T* value;
};
```

👉 Applies to pointer types only

| Feature | Function Template | Class Template |
|--------|------------------|---------------|
| Purpose | Behavior (functions) | Data structure |
| Type Deduction | ✅ Automatic | ❌ Explicit |
| Partial Specialization | ❌ Not allowed | ✅ Allowed |
| Overloading | ✅ Yes | ❌ No |
| Use Case | Algorithms | Containers |

#### <b>2-1. Performance Perspective</b>

Both are **compile-time constructs**

##### Benefits:
- zero runtime overhead
- no virtual dispatch
- aggressive inlining possible

#### <b>2-2. Modern C++</b>

##### function template + `auto` for simple logic

```cpp
auto add(auto a, auto b) 
{
    return a + b;
}
```

##### `if constexpr`

```cpp
template<typename T>
void print(T x) 
{
    if (std::is_pointer<T>::value) 
        std::cout << *x;
    else
        std::cout << x;
}
```

Both case are compliled.

```cpp
template<typename T>
void print(T x) 
{
    if constexpr (std::is_pointer_v<T>)
        std::cout << *x;
    else 
        std::cout << x;
}
```

If the condition is false, the code is automatically deleted.

##### concepts (C++20)

Before:
```cpp
template<typename T>
typename std::enable_if<std::is_integral<T>::value>::type
func(T x) {}
```

After C++20
```cpp
#include <concepts>

template<std::integral T>
void func(T x) 
{
    std::cout << x;
}
```

```cpp
template<typename T>
concept Addable = requires(T a, T b) 
{
    a + b;
};

template<Addable T>
T add(T a, T b) {
    return a + b;
}
```

Only allow to be possible "+" operation