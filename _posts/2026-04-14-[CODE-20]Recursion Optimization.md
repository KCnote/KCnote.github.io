---
layout: post
title: "Recursion Optimization"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Recursion Optimization in C++: Tail Call Optimization (TCO)</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Recursion Can Be Expensive</b>

Recursive functions use the **call stack**.

- Pushes stack frame
- Stores local variables
- Adds overhead

❌ Deep recursion → stack overflow + performance cost

## <b>2. Tail Call Optimization</b>

> A recursion where the **last operation is a function call**

##### Compiler can optimize:
- Reuse current stack frame
- Avoid additional stack growth

> For TCO to work, the recursive call must be the **final operation**

##### Non-Tail Recursion (❌ Not Optimized)

```cpp
int sum(int n) 
{
    if (n == 0) 
        return 0;

    return n + sum(n - 1);
}
```

- `+ n` happens **after** recursive call
- Requires stack frame → no TCO

##### Tail Recursion (✔ Optimizable)

```cpp
int sum_helper(int n, int acc) 
{
    if (n == 0) 
        return acc;
    
    return sum_helper(n - 1, acc + n);
}
```

Recursive call is the **last operation**

#### <b>2-1. Why `+`, `-`, `*` Break Optimization</b>

```cpp
return sum(n - 1) + n;  // ❌ breaks TCO
```

###### Compiler must:

- Wait for recursive result
- Then perform addition

❌ Cannot reuse stack frame

##### ✔ Typical requirements

- No extra operations after call
- No need for current stack frame

#### <b>2-2. Another example</b>

##### ❌ Bad (no TCO)

```cpp
int fact(int n) 
{
    if (n <= 1) 
        return 1;
    
    return n * fact(n - 1);
}
```

##### ✔ Good (TCO possible)

```cpp
int fact_helper(int n, int acc) 
{
    if (n <= 1) 
        return acc;
    
    return fact_helper(n - 1, acc * n);
}
```

##### Even better:

```cpp
int sum(int n) 
{
    int acc = 0;

    while (n > 0) 
        acc += n--;

    return acc;
}
```

✔ No recursion  
✔ No stack overhead  

##### ✔ DO

- Ensure recursive call is last
- Move operations into parameters (accumulator)
- Use compiler optimization flags

##### ❌ DON'T

- Add operations after recursive call
- Assume TCO always works
- Ignore stack usage
