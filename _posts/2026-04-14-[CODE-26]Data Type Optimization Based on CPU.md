---
layout: post
title: "Data Type Optimization Based on CPU Architecture"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Data Type Optimization Based on CPU Architecture (C++)</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why CPU Architecture Matters</b>

Modern CPUs are optimized for **specific data widths and alignment**.

> Choosing the right data type improves both performance and memory efficiency.

Using mismatched data types can cause:
- Slower execution
- Extra instructions
- Cache inefficiency

> Use data types that match the **native word size of the CPU**

| Architecture | Native Size |
|-------------|------------|
| 32-bit CPU  | 32 bits (4 bytes) |
| 64-bit CPU  | 64 bits (8 bytes) |

Most modern systems = **64-bit**

## <b>2. Optimal Data Types</b>

##### ✔ On 64-bit CPU

```cpp
int64_t a;   // optimal
size_t b;    // optimal
```

Matches CPU register size

##### ❌ Suboptimal

```cpp
int8_t a;
```

May require:
- Extra masking
- Additional instructions

CPUs operate on registers:
- 64-bit CPU → 64-bit registers
- Smaller types often promoted internally

##### Example

```cpp
uint8_t a, b;
uint8_t c = a + b;
```

Internally:
- Promoted to `int`
- Then truncated back

#### <b>2-1. Alignment Consideration</b>

##### ✔ Proper alignment

```cpp
int64_t a;
```

Aligned → fast access

##### ❌ Misaligned

```cpp
#pragma pack(1)
struct A 
{
    char c;
    int64_t x;
};
```

Misaligned access → slower

#### <b>2-2. SIMD & Vectorization</b>

SIMD prefers aligned, consistent data types

```cpp
float arr[8];
```

Works well with AVX (256-bit)

##### ❌ Mixed / irregular layout

```cpp
struct A 
{
    char c;
    float x;
};
```

Hard to vectorize

##### When Smaller Types Are Better

✔ Large arrays (memory-bound)  
✔ Cache-sensitive workloads  

- `uint8_t` for image processing
- `uint16_t` for compressed data

##### ✔ DO

- Use `size_t` for indexing
- Use native types (`int64_t` on 64-bit systems)
- Align data properly
- Profile performance

##### ❌ DON'T

- Use small types blindly
- Ignore alignment
- Mix data types unnecessarily


| Factor | Small Type | Native Type |
|------|-----------|------------|
| Memory | Efficient | Larger |
| Cache | Better | Worse |
| CPU ops | Slower | Faster |
| SIMD | Harder | Easier |

