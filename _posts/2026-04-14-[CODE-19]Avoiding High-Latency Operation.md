---
layout: post
title: "Avoiding High-Latency Operations"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Avoiding High-Latency Operations in C++ (Division Optimization)</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Division is Expensive</b>

Not all CPU operations cost the same.

##### Approximate latency (cycles):

| Operation | Latency |
|----------|--------|
| Add / Sub | ~1 cycle |
| Multiply  | ~3–5 cycles |
| Divide    | ~10–30+ cycles |

> Division is significantly slower than other arithmetic operations.
> Avoid division (`/`) in performance-critical paths whenever possible.

## <b>2. Replace Division with Multiplication</b>

#### Loop Optimization

##### ❌ Bad

```cpp
for (int i = 0; i < N; i++) 
{
    arr[i] = data[i] / scale;
}
```

##### ✔ Better

```cpp
float inv_scale = 1.0f / scale;

for (int i = 0; i < N; i++) 
    arr[i] = data[i] * inv_scale;
```

#### Integer Division Optimization

##### ✔ Division by constant

```cpp
int x = value / 8;
```

Replace with bit shift:

```cpp
int x = value >> 3;
```

Actually compiler may optimize automatically, but:

- Only for compile-time constants
- Not always optimal

#### Approximation Techniques

### ✔ Fast inverse (approximate)

```cpp
float inv = 1.0f / x;
```

- Lookup table
- Newton-Raphson iteration
- SIMD intrinsic (e.g., `_mm_rcp_ps`)

```cpp
float inv = approx_inverse(x);
inv = inv * (2.0f - x * inv);  // refine
```

Faster than division in some cases

#### Trade-offs

| Approach | Pros | Cons |
|--------|------|------|
| Division | Accurate | Slow |
| Multiply by inverse | Faster | Slight precision loss |
| Approximation | Very fast | Less accurate |

#### ❗ Precision matters:

- Financial / critical systems → avoid approximation  
- Graphics / simulation → approximation acceptable  

### ✔ DO

- Precompute reciprocal values
- Use multiplication inside loops
- Replace division by constant with shift

### ❌ DON'T

- Use division inside tight loops
- Ignore precision requirements
- Assume compiler always optimizes
