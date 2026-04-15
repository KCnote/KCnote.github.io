---
layout: post
title: "Template Specialization"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b>Template Specialization</b>

---

### <b>Prerequisites</b>
    - Template

---

## <b>1. What is Template Specialization</b>

Template specialization allows you to provide a **custom implementation** of a template for specific types.

> "What if I want different behavior for certain types?"

Use it when:

- behavior must change for specific types
- performance is critical (compile-time dispatch)
- writing generic libraries or frameworks

#### Basic Template

```cpp
template<typename T>
void print(T value)
{
    std::cout << value << "\n";
}
```

👉 This works for most types.

#### Full Specialization

Provide a completely different implementation for a specific type.

```cpp
template<>
void print<const char*>(const char* value)
{
    std::cout << "C-string: " << value << "\n";
}
```

#### Example

```cpp
#include <iostream>

template<typename T>
void print(T value)
{
    std::cout << value << "\n";
}

template<>
void print<const char*>(const char* value)
{
    std::cout << "Specialized: " << value << "\n";
}

int main()
{
    print(10);            // generic
    print("hello");       // specialized
}
```

## <b>2. Why Specialization is Needed</b>

Example problem:

```cpp
template<typename T>
bool isEqual(T a, T b)
{
    return a == b;
}
```

Works fine for most types, but:

```cpp
const char* a = "hello";
const char* b = "hello";
```

This compares **addresses**, not content

#### Fix with Specialization

```cpp
#include <cstring>

template<>
bool isEqual<const char*>(const char* a, const char* b)
{
    return std::strcmp(a, b) == 0;
}
```

## <b>3. Full vs Partial Specialization</b>

#### Full Specialization

```cpp
template<>
class MyClass<int>
{
    // specific implementation
};
```

Completely replaces template for `int`

#### Partial Specialization (Only for Class Templates)

```cpp
template<typename T>
class MyClass<T*>
{
    // pointer-specific implementation
};
```

Applies to **all pointer types**

##### ❗ Important Rule

Function templates **cannot be partially specialized**

Instead, use:
- overloading

#### ❌ Trying partial specialization on functions

```cpp
template<typename T>
void func<T*>(T* value);  // ❌ not allowed
```
