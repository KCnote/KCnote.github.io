---
layout: post
title: "Move Constructor & Move Assignment"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b> Move Constructor & Move Assignment in C++</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Move Semantics Exist</b>

Copying objects can be expensive:

> Move semantics allows transferring ownership instead of copying data.

- Memory allocation
- Deep copy of data
- Performance overhead

> Instead of copying resources, **move them**

#### Copy vs Move

##### ✔ Copy (expensive)

```cpp
std::vector<int> a = {1,2,3};
std::vector<int> b = a;  // copy
```

- New memory allocated  
- Data copied  

##### ✔ Move (cheap)

```cpp
std::vector<int> a = {1,2,3};
std::vector<int> b = std::move(a);  // move
```

- Ownership transferred  
- No deep copy  

## <b>2. Move Constructor</b>

```cpp
ClassName(ClassName&& other);
```

```cpp
class Buffer 
{
public:
    Buffer(size_t s) : size(s) 
    {
        data = new int[s];
    }

    // Move constructor
    Buffer(Buffer&& other) noexcept 
    {
        data = other.data;
        size = other.size;

        other.data = nullptr;
        other.size = 0;
    }

public:
    int* data;
    size_t size;
};
```

- `&&` = rvalue reference
- Transfer ownership
- Leave source in safe state

## <b>3. Move Assignment Operator</b>

```cpp
ClassName& operator=(ClassName&& other);
```

```cpp
Buffer& operator=(Buffer&& other) noexcept 
{
    if (this != &other) 
    {
        delete[] data;  // release current

        data = other.data;
        size = other.size;

        other.data = nullptr;
        other.size = 0;
    }

    return *this;
}
```

#### Why `noexcept` is Important

STL containers rely on it

```cpp
Buffer(Buffer&& other) noexcept;
```

- Enables move instead of copy  
- Improves performance  

#### `std::move` Meaning

```cpp
std::move(obj);
```

- Does NOT move by itself  
- Just casts to rvalue

##### ✔ Real meaning

```cpp
static_cast<T&&>(obj);
```

## <b>4. Common Mistakes</b>

#### ❌ Forget to nullify source

```cpp
data = other.data;  // but not resetting other
```

Double free risk

#### ❌ Overusing `std::move`

```cpp
return std::move(obj);  // may break copy elision
```

###### ✔ Correct

```cpp
return obj;
```

```cpp
std::vector<int> create() 
{
    std::vector<int> v(1000);
    return v;  // move or elision
}
```

##### When to Use

✔ Large objects  
✔ Resource ownership (heap, file, socket)  
✔ Performance-critical code  

##### When NOT Needed

✔ Small objects (int, double)  
✔ Trivially copyable types  
