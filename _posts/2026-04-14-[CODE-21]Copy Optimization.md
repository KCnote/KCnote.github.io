---
layout: post
title: "Copy Optimization"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Copy Optimization in C++: Pointer, Swap, and Move Semantics</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Copy Matters</b>

> Avoid unnecessary copies in performance-critical code.

Copying objects can be expensive:

- Memory allocation
- Deep copy of data
- Cache inefficiency

> Minimize copying by:

1. Managing via pointer/reference  
2. Building internally and swapping  
3. Using move semantics (rvalue)

| Strategy | Use Case |
|---------|--------|
| Reference (`const&`) | Read-only access |
| Pointer | Optional / nullable |
| Swap | Replace large object |
| Move (`std::move`) | Transfer ownership |

## <b>2. Pointer / Reference Management</b>

##### ✔ Avoid Copy

```cpp
void process(const std::vector<int>& data);
```

- No copy  
- Only reference passed  

##### ✔ Pointer

```cpp
void process(const std::vector<int>* data);
```

- Same idea → no copy  
- Use when nullable or explicit ownership needed  

## <b>3. Build Then Swap</b>

##### ❌ Direct Copy

```cpp
std::vector<int> v;
v = create_large_vector();  // copy or move
```

##### ✔ Efficient Swap

```cpp
std::vector<int> temp = create_large_vector();
v.swap(temp);
```

- Constant-time swap (pointer exchange)
- No deep copy

## <b>4. Move Semantics (Rvalue)</b>

##### ✔ Move Instead of Copy

```cpp
std::vector<int> v = create_large_vector();
```

Uses move (or copy elision)

##### ✔ Explicit Move

```cpp
std::vector<int> v;
v = std::move(temp);
```

- Transfers ownership  
- Avoids deep copy  

##### Rvalue Reference

```cpp
void process(std::vector<int>&& data);
```

- Accepts temporary object  
- Enables move semantics  

## <b>5. Practice</b>

##### ❌ Inefficient

```cpp
std::vector<int> getData() 
{
    std::vector<int> v;
    // fill
    return v;  // might copy (pre-C++17)
}
```

##### ✔ Efficient (C++17)

```cpp
std::vector<int> getData() 
{
    std::vector<int> v;
    // fill
    return v;  // copy elision
}
```

##### ✔ Replace Pattern

```cpp
void update(std::vector<int>& v) 
{
    std::vector<int> temp;
    // fill temp
    v.swap(temp);
}
```

##### ✔ Return Value Optimization (RVO)

```cpp
std::vector<int> create() 
{
    return std::vector<int>(100);
}
```

No copy, no move (C++17 guaranteed)

##### ✔ DO

- Use `const T&` for input
- Use `std::move` when transferring ownership
- Use `swap()` for replacing large containers
- Rely on copy elision (C++17)

##### ❌ DON'T

- Copy large objects unnecessarily
- Overuse `std::move`
- Ignore object lifetime
