---
layout: post
title: "std::execution::par"
date: 2026-04-19 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::for_each`</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `std::execution::par`? (C++17 Parallel Execution)</b>

C++17 introduced **parallel algorithms** through the `<execution>` header.
`std::execution::par` is a policy that enables **multi-threaded execution** for standard algorithms.

It allows developers to write code in a familiar STL style while letting the standard library handle parallelization internally.

```cpp
#include <algorithm>
#include <execution>
#include <vector>

std::vector<int> v = {1, 2, 3, 4, 5};

std::for_each(std::execution::par, v.begin(), v.end(), [](int& x)
{
    x *= 2;
});
```

```
{2, 4, 6, 8, 10}
```

## <b>2. How to work</b>

```cpp
std::execution::par
```

This single argument changes the execution model:

* Sequential → Parallel
* Single thread → Multiple threads
* Manual thread management → Automatic

##### Conceptual Model

Internally, the algorithm may split work like this:

```
Thread 1 → v[0 ~ 249]
Thread 2 → v[250 ~ 499]
Thread 3 → v[500 ~ 749]
Thread 4 → v[750 ~ 999]
```

Each thread processes a different chunk independently.

> Note: The actual implementation is library-dependent.

##### Supported Algorithms

Common algorithms that support `par`:

* `std::for_each`
* `std::transform`
* `std::sort`
* `std::reduce`
* `std::transform_reduce`
* `std::inclusive_scan`
* `std::exclusive_scan`

##### Example: transform

```cpp
#include <algorithm>
#include <execution>
#include <vector>

std::vector<int> input = {1, 2, 3, 4, 5};
std::vector<int> output(input.size());

std::transform(std::execution::par,
               input.begin(), input.end(),
               output.begin(),
               [](int x)
               {
                   return x * x;
               });
```

##### Critical Rule: No Data Dependency

Parallel execution requires **independent work per element**.

## <b>3. Features</b>

##### Safe

```cpp
std::for_each(std::execution::par, v.begin(), v.end(), [](int& x)
{
    x *= 2;
});
```

* No shared state
* No dependency between elements

##### Unsafe (Race Condition)

```cpp
int sum = 0;

std::for_each(std::execution::par, v.begin(), v.end(), [&](int x)
{
    sum += x;   // race condition
});
```

* Multiple threads modify `sum`
* Undefined behavior

#### Order is NOT Guaranteed

```cpp
std::for_each(std::execution::par, v.begin(), v.end(), [](int x)
{
    std::cout << x << " ";
});
```

Output order may vary.

