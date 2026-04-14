---
layout: post
title: "Operator Overloading"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b> Operator Overloading in C++</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is Operator Overloading</b>

> Operator overloading allows you to **define custom behavior for operators** (`+`, `-`, `*`, `==`, etc.) on user-defined types.

| Category | Operators |
|--------|---------|
| Arithmetic | `+ - * /` |
| Comparison | `== != < > <= >=` |
| Assignment | `= += -=` |
| Increment | `++ --` |
| Access | `[] () ->` |
| Stream | `<< >>` |

Makes code more intuitive and expressive.

##### ✔ Without overloading

```cpp
Vector c = add(a, b);
```

##### ✔ With overloading

```cpp
Vector c = a + b;
```

Cleaner and closer to mathematical notation  

## <b>2. Operator Overloading</b>

#### <b>2-1. Basic Syntax</b>

```cpp
return_type operatorOP(parameters) 
{
    // implementation
}
```

Example:

```cpp
class Vec 
{
public:
    int x, y;

    Vec operator+(const Vec& other) const 
    {
        return {x + other.x, y + other.y};
    }
};
```

#### <b>2-2. Member/Non-member</b>

##### ✔ Member function

```cpp
Vec operator+(const Vec& other) const;
```

Left operand = object itself  

##### ✔ Non-member (free function)

```cpp
Vec operator+(const Vec& a, const Vec& b);
```

More flexible (especially for symmetry)

| Case | Recommendation |
|-----|--------------|
| Access private members | Member or friend |
| Symmetric operation | Non-member |
| Assignment-like (`=`) | Member |

#### <b>2-3. Example</b>

```cpp
class Vec 
{
public:
    Vec(float x, float y) : x(x), y(y) {}

    Vec operator+(const Vec& other) const 
    {
        return Vec(x + other.x, y + other.y);
    }

    Vec operator*(float scalar) const 
    {
        return Vec(x * scalar, y * scalar);
    }

public:
    float x, y;
};
```

##### Stream Operator (`<<`)

###### ✔ Must be non-member

```cpp
#include <iostream>

std::ostream& operator<<(std::ostream& os, const Vec& v) 
{
    os << "(" << v.x << ", " << v.y << ")";
    return os;
}
```

##### Comparison Operator

```cpp
bool operator==(const Vec& other) const 
{
    return x == other.x && y == other.y;
}
```

##### Assignment Operator

```cpp
Vec& operator=(const Vec& other) 
{
    if (this != &other) 
    {
        x = other.x;
        y = other.y;
    }

    return *this;
}
```

Must return reference  

##### Increment Operator

###### ✔ Prefix

```cpp
Vec& operator++() 
{
    x++; y++;

    return *this;
}
```

###### ✔ Postfix

```cpp
Vec operator++(int) 
{
    Vec temp = *this;
    ++(*this);
    return temp;
}
```

`int` is dummy parameter  

##### Subscript Operator

```cpp
int& operator[](int index) 
{
    if (index == 0) 
        return x;

    return y;
}
```

##### Function Call Operator

```cpp
class Functor 
{
public:
    int operator()(int x) 
    {
        return x * x;
    }
};
```

##### +Performance Considerations

###### Return by value (C++17 optimized)

```cpp
Vec operator+(const Vec& other) const 
{
    return Vec(x + other.x, y + other.y);
}
```

Copy elision / move optimization  
- Use `const&` for parameters
- Return by value (optimized)

##### ✔ DO

- Keep semantics intuitive
- Use `const` correctness
- Prefer non-member for symmetric ops
- Keep operators lightweight

##### ❌ DON'T

- Overload in confusing ways
- Hide expensive operations behind operators
- Break expected behavior

##### Bad Example: unexpected behavior

```cpp
Vec operator+(const Vec& other) 
{
    sleep(1);  // ❌ unexpected behavior
    return ...
}
```