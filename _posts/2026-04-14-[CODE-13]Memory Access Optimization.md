---
layout: post
title: "Memory Access Optimization: Contiguous vs Non-Contiguous (C++)"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Memory Access Optimization</b>

---

### <b>Prerequisites</b>
    - Compile
    - Runtime

---

## <b>1. Why Memory Access Matters</b>

Modern CPUs are **much faster than memory**.

**Performance is often limited by:**
- Cache hits vs cache misses
- Memory access patterns

> Efficient memory access can be **orders of magnitude faster** than inefficient access.

## <b>2. Contiguous Memory Access</b>

Accessing memory in **sequential order**

```cpp
int arr[1000];

for (int i = 0; i < 1000; i++) 
{
    arr[i] += 1;
}
```

##### Access pattern:

```
arr[0] → arr[1] → arr[2] → ...
```

- CPU prefetcher predicts next access
- Cache lines are fully utilized
- Fewer cache misses

##### Example:

- Cache line ≈ 64 bytes
- `int` = 4 bytes → 16 elements per cache line

## <b>2. Non-Contiguous Memory Access</b>

Accessing memory in **irregular / scattered pattern**

```cpp
int arr[1000];

for (int i = 0; i < 1000; i += 16) 
{
    arr[i] += 1;
}
```

##### Access pattern:

```
arr[0] → arr[16] → arr[32] → ...
```

- Cache lines wasted
- Frequent cache misses
- Poor prefetching

## <b>3. Real Performance Difference</b>

👉 Same number of operations, very different speed

```cpp
// ✅ Fast
for (int i = 0; i < N; i++) {
    sum += arr[i];
}

// ❌ Slow
for (int i = 0; i < N; i++) {
    sum += arr[random_index[i]];
}
```

## <b>4. 2D Array Optimization</b>

C++ uses **row-major order**

```cpp
int arr[100][100];
```

##### ✔ Fast (row-wise)

```cpp
for (int i = 0; i < 100; i++) 
{
    for (int j = 0; j < 100; j++) 
        arr[i][j] += 1;
}
```

##### ❌ Slow (column-wise)

```cpp
for (int j = 0; j < 100; j++) 
{
    for (int i = 0; i < 100; i++) 
        arr[i][j] += 1;
}
```

##### Reason:

- Row-wise = contiguous
- Column-wise = strided access

## <b>5. Pointer vs Pointer-to-Pointer</b>

##### ✔ Contiguous
```cpp
int* arr = new int[N];
```

##### ❌ Non-contiguous
```cpp
int** arr = new int*[N];

for (int i = 0; i < N; i++) 
    arr[i] = new int[M];
```

Each row is allocated separately → scattered memory

## <b>6. STL Containers</b>

##### ✔ Contiguous
- `std::vector`
- `std::array`

##### ❌ Non-contiguous
- `std::list`
- `std::map`
- `std::unordered_map`

Linked structures → pointer chasing → cache miss


- Use `std::vector` instead of `std::list`
- Row-major for 2D arrays
- Sequential access is king
- Avoid `int**` if possible