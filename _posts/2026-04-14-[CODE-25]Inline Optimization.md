---
layout: post
title: "Inline Optimization"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Inline Optimization in C++: Using Lambda for Small Operations</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Function Calls Matter</b>

Function calls are not free.

> In tight loops or small operations, this overhead becomes significant.

Each call may involve:
- Stack frame setup
- Parameter passing
- Jump instruction (branch)

> For very small operations, avoid function call overhead by using **inline functions or lambdas**

## <b>2. Inline Function</b>

##### Example

```cpp
inline int add(int a, int b) 
{
    return a + b;
}
```

Compiler may replace function call with actual code

- `inline` is a hint, not a guarantee
- Compiler decides based on optimization level

## <b>3. Lambda for Inline Behavior</b>

##### ✔ Example

```cpp
auto add = [](int a, int b) 
{
    return a + b;
};

int result = add(1, 2);
```

Often inlined by compiler. No function call overhead in optimized builds  

## <b>4. How to use</b>

#### <b>4-1. Small Operations</b>

##### ❌ Function Call Overhead

```cpp
int square(int x) 
{
    return x * x;
}

for (int i = 0; i < N; i++) 
{
    sum += square(i);
}
```

##### Inline / Lambda

```cpp
auto square = [](int x) 
{
    return x * x;
};

for (int i = 0; i < N; i++) 
{
    sum += square(i);
}
```

Compiler likely inlines → no call overhead  

#### <b>4-2. When NOT to Use</b>

❌ Large functions  
❌ Complex logic  
❌ Recursion  

**Inlining large code may:**
- Increase binary size
- Hurt instruction cache