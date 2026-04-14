---
layout: post
title: "Data Type Size Optimization"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Data Type Size Optimization in C++ (Use Only What You Need)</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Data Type Size Matters</b>

Choosing unnecessarily large data types can lead to:

> Memory efficiency directly impacts CPU performance.

- Increased memory usage
- Poor cache utilization
- Lower performance

> Use the **smallest data type that satisfies your requirements**

#### <b>1-1. Array Optimization</b>

##### ❌ Inefficient

```cpp
int arr[1000000];  // 4MB
```
```
64 / 4 = 16 elements per cache line
```

##### ✔ Optimized

```cpp
uint8_t arr[1000000];  // 1MB
```
```
64 / 1 = 64 elements per cache line
```

- 4x less memory  
- Better cache performance  

##### Cache Impact

CPU cache line ≈ 64 bytes
More data per cache line → fewer cache misses

#### <b>1-2. Struct Optimization</b>

##### ❌ Wasteful

```cpp
struct Data {
    int a;
    int b;
    int c;
};
```

##### ✔ Optimized

```cpp
struct Data {
    uint8_t a;
    uint8_t b;
    uint8_t c;
};
```

👉 Significant memory reduction when repeated many times

##### ❗ Alignment & Padding

```cpp
struct A 
{
    uint8_t a;
    int b;
};
```

Padding added → may reduce benefit

##### ❗ CPU Efficiency

- Some CPUs are optimized for 32-bit or 64-bit operations  
- Too small types may require extra instructions

##### When Optimization Matters

✔ Large arrays / datasets  
✔ Performance-critical loops  
✔ Embedded systems  
✔ Game / simulation engines  


## <b>2. Trade-offs</b>

#### ✔ DO

- Choose smallest sufficient type
- Use fixed-width types (`uint8_t`, `uint32_t`)
- Profile memory-heavy code

#### ❌ DON'T

- Use `int` by default everywhere
- Over-optimize prematurely
- Ignore alignment and padding

| Factor | Small Type | Large Type |
|------|-----------|-----------|
| Memory | Efficient | Wasteful |
| Cache | Better | Worse |
| Range | Limited | Large |
| CPU ops | Sometimes slower | Often faster |