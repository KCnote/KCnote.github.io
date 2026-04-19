---
layout: post
title: "std::execution"
date: 2026-04-19 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::execution`</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `std::execution`? (C++17)</b>

C++17 introduced **execution policies** through the `<execution>` header.

They allow standard algorithms (such as `std::for_each`, `std::transform`, `std::sort`)
to control **how** the algorithm is executed:

* sequentially
* in parallel
* or in a more aggressively optimized manner (parallel + vectorized)

## <b>2. Why `std::execution` exists?</b>

Traditional STL algorithms define **what** to do, but not **how** to execute it.

```cpp
std::for_each(v.begin(), v.end(), f);
```

* Always sequential
* Single thread
* No control over execution strategy

With execution policies:

```cpp
std::for_each(std::execution::par, v.begin(), v.end(), f);
```

You explicitly tell the algorithm:

> “You are allowed to run this in parallel.”

## <b>3. Available Execution Policies</b>

```cpp
#include <execution>
```

| Policy                      | Description                                        |
| --------------------------- | -------------------------------------------------- |
| `std::execution::seq`       | Sequential execution                               |
| `std::execution::par`       | Parallel (multi-threaded) execution                |
| `std::execution::par_unseq` | Parallel + unsequenced (SIMD + reordering allowed) |

| Feature         | `seq` | `par`    | `par_unseq`   |
| --------------- | ----- | -------- | ------------- |
| Threads         | 1     | Multiple | Multiple      |
| SIMD            | No    | Optional | Yes (allowed) |
| Order Guarantee | Yes   | No       | No            |
| Reordering      | No    | Limited  | Fully allowed |
| Determinism     | Yes   | No       | No            |

#### <b>3-1. `std::execution::seq`</b>

```cpp
std::for_each(std::execution::seq, v.begin(), v.end(), f);
```

* Executes in order
* Single thread
* Same as default behavior

##### Characteristics

* deterministic
* easiest to debug
* no concurrency

#### <b>3-2. `std::execution::par`</b>

```cpp
std::for_each(std::execution::par, v.begin(), v.end(), f);
```

* Executes using multiple threads
* Work is split into chunks
* Order is not guaranteed

##### Characteristics

* multi-threaded
* faster for large workloads
* requires thread-safe operations

#### <b>3-3. `std::execution::par_unseq`</b>

```cpp
std::for_each(std::execution::par_unseq, v.begin(), v.end(), f);
```

* Parallel + vectorized execution
* Allows compiler to reorder operations
* Enables SIMD optimizations (automatically create SIMD operation on compiler)

##### Characteristics

* maximum performance potential
* no ordering guarantees at all
* strongest constraints on safety

#### <b>3-4. `std::execution::par` vs `std::execution::par_unseq`</b>

```cpp
std::for_each(std::execution::par, v.begin(), v.end(), [](int x) {
    work(x);
});
```

##### `std::execution::par`

thread A: work(v[0])
thread B: work(v[5])
thread C: work(v[2])

##### `std::execution::par_unseq`

thread A: create SIMD - < v[0], v[1], v[2], v[3] >

## <b>4. Critical Rules</b>

##### No Data Races

```cpp
int sum = 0;

std::for_each(std::execution::par, v.begin(), v.end(), [&](int x)
{
    sum += x;   // ❌ race condition
});
```

##### No Dependency Between Elements

```cpp
// ❌ depends on previous element
v[i] = v[i - 1] + 1;
```

##### Avoid Shared Mutable State

```cpp
// ❌ unsafe shared state
static int counter = 0;
counter++;
```