---
layout: post
title: "Parameter Optimization"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Parameter Optimization</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Parameter Passing Matters</b>

Passing parameters incorrectly can introduce **hidden costs**:

- Temporary object creation
- Unnecessary copies
- Cache inefficiency

> Even small overhead becomes significant in tight loops or large-scale systems.

1. Does this create a copy?
2. Is this object large?
3. Can I avoid unnecessary construction?

## <b>2. Call by Value vs Call by Reference</b>

##### ❌ Expensive (Copy)

```cpp
void process(std::string s) 
{
    // copy occurs
}
```

- `std::string` copy = allocation + memory copy

##### ✔ Efficient (Reference)

```cpp
void process(const std::string& s) 
{
    // no copy
}
```

- No temporary object  
- No memory allocation  

#### <b>2-1. When Call by Value is OK</b>

✔ Small types (int, float, pointer)  
✔ When copy is cheap  
✔ When you need a local copy anyway  

```cpp
void foo(int x);  // OK
```

#### <b>2-2. Minimizing Copies</b>

##### ✔ Pass only what is needed

```cpp
// ❌ Bad
void process(User user);

// ✔ Better
void process(const User& user);
```

##### ✔ Avoid unnecessary temporaries

```cpp
process(std::string("hello"));  // temporary object created
```

##### `std::string_view` Optimization

```cpp
void process(std::string_view s);
```

- No allocation  
- No copy  

## <b>3. Copy Elision (C++17)</b>

Compiler can **eliminate copies automatically**

```cpp
std::string create() 
    return std::string("hello");
```

- No copy  
- No move  
- Direct construction in return location

##### ❌ Wrong Optimization

```cpp
std::string create() 
{
    std::string s = "hello";
    return std::move(s);  // ❌ prevents copy elision
}
```

- Forces move → may be slower

#### Move Semantics (Caution)

##### ✔ Good usage

```cpp
std::string s = std::move(temp);
```

##### ❗ Overuse problem

Unnecessary `std::move` can:

- Disable copy elision
- Introduce extra move operation

> “Faster in theory” ≠ “Faster in practice”

## <b>4. Best Practice</b>

##### ✔ Measure, don’t guess

- Benchmark with real data
- Use compiler-specific optimization flags
- Profile before and after

##### ❌ Over-optimized (wrong)

```cpp
return std::move(obj);
```

##### ✔ Better

```cpp
return obj;  // let compiler optimize
```
