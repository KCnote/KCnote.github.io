---
layout: post
title: "Int vs Float in C++"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Int vs Float in C++: Performance-Oriented Data Type Choice</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Data Type Choice Matters</b>

> Not all numeric types are equal in performance.

The type you choose affects:

- CPU execution speed
- Precision
- Cache efficiency
- Overall performance

> If exact floating-point values are NOT required, prefer **integer-based computation**

| Feature | `int` | `float` |
|--------|------|--------|
| Precision | Exact | Approximate |
| Speed | Faster (generally) | Slower (depends on hardware) |
| Determinism | High | Low (rounding errors) |
| Comparison | Reliable | Risky |

## <b>2. Int vs Float</b>

##### ✔ Reasons of using Int

- Simpler CPU instructions
- No rounding / normalization
- Better predictability
- Often lower latency

##### Comparison Problem with Float

### ❌ Unsafe

```cpp
if (a == b) 
{
    // may fail due to precision error
}
```

### ✔ Safer

```cpp
if (fabs(a - b) < 1e-6) 
{
    // approximate comparison
}
```

Extra computation required → slower

## <b>3. Replace Float with Int (Scaling Technique)</b>

Convert float to scaled integer

```cpp
float value = 3.14f;
int scaled = (int)(value * 100);  // 314
```

Now compare as integer:

```cpp
if (scaled > 300) 
{
    // fast integer comparison
}
```

##### ❌ Float-based

```cpp
if (distance < 1.5f) 
{
    // ...
}
```

##### ✔ Int-based

```cpp
int dist_scaled = distance * 100;
if (dist_scaled < 150) 
{
    // ...
}
```

Faster + deterministic

#### <b>3-1. When to Prefer `int`</b>

✔ Only comparison matters  
✔ Fixed precision acceptable  
✔ Performance-critical loops  
✔ Large-scale data processing  

#### <b>3-2. When to Prefer `float`</b>

✔ Real-world measurement  
✔ Continuous values  
✔ Scientific / graphics computation  
✔ High precision required  

| Approach | Pros | Cons |
|--------|------|------|
| `int` | Fast, deterministic | Limited range/precision |
| `float` | Flexible, expressive | Slower, precision issues 