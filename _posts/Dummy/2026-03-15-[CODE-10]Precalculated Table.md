---
layout: post
title: "Loop Unrolling in 5x5 Convolution: Why It Matters"
date: 2026-03-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# Precalculated Table (Lookup Table)
### Reference Cost vs Calculation Cost

In performance-critical systems such as **computer vision, real-time systems, and game engines**, it is common to replace expensive calculations with **precalculated tables**.

This technique is often called a **Lookup Table (LUT)**.

The core idea is simple:

> Instead of computing a value repeatedly, compute it once in advance and store it in memory.

Later, the program only performs a **memory reference**.

---

# Core Concept

Suppose a function:

$$
f(x)
$$

is expensive to compute.

Instead of repeatedly computing:

```
result = f(x)
```

we precompute:

```
table[x] = f(x)
```

Then runtime becomes:

```
result = table[x]
```

This replaces **calculation cost** with **memory reference cost**.

---

# Basic Example

Suppose we repeatedly compute a square value.

## Without Precalculation

```cpp
int Square(int x)
{
    return x * x;
}

int value = Square(a);
```

If this is executed millions of times, multiplication cost accumulates.

---

## With Precalculated Table

```cpp
int table[256];

void InitTable()
{
    for(int i = 0; i < 256; i++)
        table[i] = i * i;
}

int value = table[a];
```

Now the runtime operation becomes a simple **array access**.

---

# Performance Perspective

Let us compare the two approaches.

| Operation | Cost Type |
|---|---|
| Mathematical computation | CPU calculation cost |
| Lookup table access | Memory reference cost |

Which one is faster depends on the system.

But generally:

- **simple memory access is cheaper than complex computation**

especially when:

- computation is heavy
- the function is called many times
- the domain of input values is limited

---

# Cost Comparison

### Calculation Cost

When computing directly:

```
value = expensive_function(x)
```

Possible costs include:

- multiplication
- division
- trigonometric functions
- exponentiation
- branching logic

These can cost **multiple CPU cycles**.

---

### Reference Cost

Lookup table approach:

```
value = table[x]
```

This becomes:

- one memory load
- one array indexing

If the table fits in **L1/L2 cache**, it is extremely fast.

---

# Example: Trigonometric Functions

A classic example is **sin()** or **cos()**.

Instead of computing:

```cpp
double v = sin(angle);
```

we create a table:

```cpp
double sinTable[360];

for(int i=0;i<360;i++)
    sinTable[i] = sin(i * PI / 180.0);
```

Runtime becomes:

```cpp
value = sinTable[angle];
```

This was widely used in:

- game engines
- embedded systems
- early graphics pipelines

---

# Example in Computer Vision

In computer vision systems, lookup tables are frequently used for:

### Gamma correction

Instead of computing

$$
I_{out} = I_{in}^{\gamma}
$$

we build a table:

```
gammaLUT[input_value]
```

Runtime becomes a simple lookup.

---

### Distance Transform

Precompute

```
sqrt(dx^2 + dy^2)
```

for small ranges.

---

### Color conversion

Precompute mapping tables such as:

```
RGB → Gray
RGB → YUV
```

---

# Time vs Memory Trade-off

Precalculated tables introduce a classic trade-off.

| Factor | Calculation | Lookup Table |
|---|---|---|
| CPU cost | Higher | Lower |
| Memory usage | Lower | Higher |
| Initialization time | None | Required |
| Runtime speed | Slower | Faster |

This is known as:

> **Time–Memory Trade-off**

---

# When Lookup Tables Are Useful

Lookup tables work best when:

- input domain is **small**
- the same calculation is repeated **many times**
- computation is **expensive**
- memory is **relatively cheap**

Examples:

- image processing
- shader pipelines
- physics engines
- signal processing
- cryptography

---

# When Lookup Tables Are NOT Good

They may be inefficient when:

- input domain is very large
- memory bandwidth is limited
- cache misses occur frequently
- calculation is already very cheap

For example, replacing a simple addition with a lookup table is usually pointless.

---

# Cache Considerations

The effectiveness of lookup tables depends heavily on **CPU cache behavior**.

If the table fits inside:

- L1 cache
- L2 cache

then lookup is extremely fast.

But if the table is too large and causes:

- cache misses
- memory stalls

then the benefit may disappear.

---

# Practical Guideline

Use a precalculated table when:

- computation is expensive
- input domain is limited
- the operation occurs frequently

Avoid it when:

- memory usage explodes
- cache locality is poor
- computation is trivial

---

# Summary

The lookup table technique replaces **calculation cost** with **reference cost**.

Instead of computing values repeatedly, we compute them once and store them in memory.

This optimization is widely used in performance-critical systems such as:

- computer vision
- graphics engines
- embedded systems
- real-time processing

---

# One-Line Summary

> A **precalculated table (lookup table)** trades CPU computation for memory access, often improving performance when expensive calculations are repeatedly executed over a limited input domain.
```