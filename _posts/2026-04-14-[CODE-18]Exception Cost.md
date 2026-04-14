---
layout: post
title: "Exception Cost"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Exception Cost in C++: `try-catch` vs `malloc`</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Exception Matters</b>

C++ exceptions (`try-catch`) provide **safe error handling**, but they come with **performance considerations**.

Especially in **performance-critical code**, exception usage must be carefully evaluated.

> Exceptions are **zero-cost when not thrown**, but **very expensive when thrown**

##### ✔ When NOT thrown

```cpp
try 
{
    foo();
} 
catch (...) 
{
}
```

Almost no runtime cost (modern compilers)

##### ❌ When thrown

```cpp
throw std::bad_alloc();
```

###### Heavy operations:
- Stack unwinding
- Destructor calls
- RTTI lookup
- Control transfer

❌ Can be **orders of magnitude slower**

## <b>2. Instead of Exception, Another</b>

#### <b>2-1. `new` vs `malloc`</b>

##### ✔ `new`

```cpp
int* p = new int;
```

- Calls constructor
- Throws `std::bad_alloc` on failure

##### ✔ `malloc`

```cpp
int* p = (int*)malloc(sizeof(int));
```

- No constructor
- Returns `nullptr` on failure
- No exception

| Feature | `new` | `malloc` |
|--------|------|---------|
| Failure handling | Exception | `nullptr` |
| Constructor call | Yes | No |
| Overhead | Higher | Lower |
| Safety | Higher | Lower |

#### <b>2-1. When to Use `malloc`</b>

✔ Plain data (POD / primitive types)  
✔ Performance-critical paths  
✔ When exception handling is undesirable  

```cpp
int* p = (int*)malloc(sizeof(int));
if (!p) 
{
    // handle error manually
}
```

#### <b>2-2. When to Use `new`</b>

✔ Complex objects  
✔ RAII / constructor required  
✔ Safety more important than raw speed  

#### <b>2-3. Exception-Free Design</b>

👉 In high-performance systems:

- Avoid exceptions in hot paths
- Use error codes or flags
- Use pre-allocation / object pools

##### ✔ DO

- Use `malloc` for simple data in critical loops
- Check `nullptr` manually
- Avoid exception-heavy flows

##### ❌ DON'T

- Rely on exceptions for normal control flow
- Use `malloc` for complex objects
- Ignore memory initialization

#### <b>2-4. Advanced Tip</b>

##### ✔ `new (std::nothrow)`

```cpp
int* p = new (std::nothrow) int;
if (!p) 
{
    // handle error
}
```

- Avoids exception  
- Returns `nullptr` like `malloc`