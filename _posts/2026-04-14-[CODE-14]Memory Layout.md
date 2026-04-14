---
layout: post
title: "Memory Layout in C++"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Foundation]
tags: [CODE, CODE - Foundation]
pin: false
math: true
mermaid: true
---

# <b>Memory Layout in C++</b>

---

### <b>Prerequisites</b>
    - Struct
    - Class

---

## <b>1. Memory Layout</b>

Memory layout defines **how data is physically arranged in memory**.

👉 Performance depends heavily on:
- Cache line usage
- Data locality
- Access pattern

## <b>2. Struct</b>

```cpp
struct A 
{
    char c;
    int i;
};
```

Memory (with padding):
```
[c][pad][pad][pad][i][i][i][i]
```

- CPU prefers aligned access
- Compiler inserts padding automatically

##### Why important?

- Affects size
- Affects cache efficiency

##### ✔ Optimization by Reordering

```cpp
// ❌ Inefficient
struct A 
{
    char c;
    int i;
    char d;
};

// ✔ Optimized
struct A 
{
    int i;
    char c;
    char d;
};
```

Less padding → better cache usage

## <b>3. Class</b>

##### ✔ Same as struct (mostly)

```cpp
class A 
{
    int x;
    float y;
};
```

Same layout as struct

##### ✔ With Virtual Function

```cpp
class Base 
{
public:
    virtual void foo() {}
    int x;
};
```

Memory:
```
[vptr][x]
```

- vptr = pointer to virtual table  
- Adds memory + runtime overhead

## <b>4. Optimization</b>

##### AoS (Array of Structures)

```cpp
struct Particle 
{
    float x, y, z;
    float vx, vy, vz;
};

Particle particles[N];
```

Memory:
```
[x y z vx vy vz][x y z vx vy vz][x y z vx vy vz]...
```

##### ✔ Problem

```cpp
for (int i = 0; i < N; i++) 
    particles[i].x += 1.0f;
```

- Loads entire struct  
- Uses only `x`

❌ Wasted memory bandwidth  
❌ Poor cache efficiency  

#####  5. SoA (Structure of Arrays)

### ✔ Definition

```cpp
struct Particles 
{
    float x[N], y[N], z[N];
    float vx[N], vy[N], vz[N];
};
```

Memory:
```
[x x x x x ...]
[y y y y y ...]
[z z z z z ...]
```

##### ✔ Advantage

```cpp
for (int i = 0; i < N; i++) 
    particles.x[i] += 1.0f;
```

- Only loads required data  
- Perfect sequential access  

✔ Cache-friendly  
✔ SIMD-friendly  

##### Cache Perspective
    CPU reads memory in **cache lines (~64 bytes)**

##### AoS

```
[x y z vx vy vz] → unnecessary data loaded
```

- AoS → wasted bandwidth

##### SoA

```
[x x x x x] → only needed data loaded
```

- SoA → efficient bandwidth