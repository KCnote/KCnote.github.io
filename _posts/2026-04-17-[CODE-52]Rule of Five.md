---
layout: post
title: "Rule of Five"
date: 2026-04-17 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b>Rule of Five</b>

---

### <b>Prerequisites</b>

---

## <b>1.  What is Rule of Five</b>

The **Rule of Five** states:

If a class manages resources and defines one of these:

- destructor
- copy constructor
- copy assignment operator

Then it should also define:

- move constructor
- move assignment operator

**If your class manages resources, you should explicitly define all five special member functions.**

## <b>2.  Why Do We Need It?</b>

C++ classes often manage:

- dynamic memory (`new/delete`)
- file handles
- sockets
- mutexes

Use when:

- managing raw resources
- using `new/delete`
- handling ownership manually

#### ❌ Default Behavior Problem

```cpp
class A
{
    int* data;

public:
    A(int v)
    {
        data = new int(v);
    }

    ~A()
    {
        delete data;
    }
};
```

#### Issue

```cpp
A a1(10);
A a2 = a1;  // shallow copy
```

- Both objects point to the same memory  
- **double delete → crash**

## <b>3.  The Five Functions</b>

#### 3-1. Destructor

```cpp
~A()
{
    delete data;
}
```

releases resource

#### 3-2. Copy Constructor

```cpp
A(const A& other)
{
    data = new int(*other.data);
}
```

deep copy

#### 3-3. Copy Assignment Operator

```cpp
A& operator=(const A& other)
{
    if (this != &other)
    {
        delete data;
        data = new int(*other.data);
    }
    return *this;
}
```

#### 3-4. Move Constructor

```cpp
A(A&& other) noexcept
{
    data = other.data;
    other.data = nullptr;
}
```

transfers ownership (no copy)

#### 3-5. Move Assignment Operator

```cpp
A& operator=(A&& other) noexcept
{
    if (this != &other)
    {
        delete data;

        data = other.data;
        other.data = nullptr;
    }
    return *this;
}
```

## <b>4.  Performance Perspective</b>

#### ❌ Copy

```text
alloc → copy → slow
```

#### ✔️ Move

```text
pointer transfer → fast
```

Move avoids:

- allocation
- deep copy
- expensive operations

#### <b>4-1. Stable Design</b>

##### ❌ Missing self-assignment check

```cpp
if (this != &other)
```

##### ❌ Not using `noexcept`

```cpp
A(A&& other) noexcept;
```

required for STL optimizations

## <b>5.  Full Example</b>

```cpp
class A
{
    int* data;

public:
    A(int v) : data(new int(v)) {}

    ~A()
    {
        delete data;
    }

    A(const A& other)
    {
        data = new int(*other.data);
    }

    A& operator=(const A& other)
    {
        if (this != &other)
        {
            delete data;
            data = new int(*other.data);
        }
        return *this;
    }

    A(A&& other) noexcept
    {
        data = other.data;
        other.data = nullptr;
    }

    A& operator=(A&& other) noexcept
    {
        if (this != &other)
        {
            delete data;

            data = other.data;
            other.data = nullptr;
        }
        return *this;
    }
};
```

## <b>6. Rule of Zero / One / Three / Five</b>

| Rule | Meaning | Focus | When to Use | Key Functions |
|------|--------|------|------------|--------------|
| Rule of Zero | Do not define any special member functions | No manual resource management | Use STL containers / smart pointers | None |
| Rule of One | One class manages one resource (RAII principle) | Single responsibility for a resource | Designing simple resource wrappers | Typically destructor (and possibly others) |
| Rule of Three | If one is defined, define all three | Safe copying of resources | When managing raw resources (C++98 style) | Destructor, Copy Constructor, Copy Assignment |
| Rule of Five | Rule of Three + move semantics | Efficient resource transfer | When performance matters (C++11+) | + Move Constructor, Move Assignment |
