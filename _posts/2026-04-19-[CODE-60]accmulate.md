---
layout: post
title: "std::accumulate"
date: 2026-04-19 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::inclusive_scan`</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `std::accumulate`?</b>

`std::accumulate` is a standard algorithm that computes a **single accumulated value** from a range.

It processes elements **sequentially from left to right**, combining them using a given operation.

```cpp
#include <numeric>
#include <vector>

std::vector<int> v = {1, 2, 3, 4};

int sum = std::accumulate(v.begin(), v.end(), 0);
```

```text
10
```

```text
(((init + v[0]) + v[1]) + v[2]) + ...
```

Always evaluated **left-to-right**

## <b>2. Function Signature</b>

```cpp
template<class InputIt, class T>
T accumulate(InputIt first, InputIt last, T init);
```

#### With Custom Operation

```cpp
template<class InputIt, class T, class BinaryOp>
T accumulate(InputIt first, InputIt last,
             T init, BinaryOp op);
```

#### Example: Multiplication

```cpp
int result = std::accumulate(v.begin(), v.end(), 1,
    [](int a, int b)
    {
        return a * b;
    });
```

```text
24
```

#### Example: String Concatenation

```cpp
#include <string>
#include <vector>

std::vector<std::string> words = {"Hello", " ", "World"};

std::string result = std::accumulate(words.begin(), words.end(), std::string(""));
```

```text
"Hello World"
```

## <b>3. Characteristics</b>

#### <b>3-1. Sequential Execution</b>

* always runs in order
* no parallelism
* deterministic result

#### <b>3-2. Order Matters</b>

```cpp
std::accumulate(v.begin(), v.end(), 0,
    [](int a, int b)
    {
        return a - b;
    });
```

Result depends on order:

```text id="yl7rxt"
(((0 - 1) - 2) - 3) - 4
```

#### <b>3-3. Initial Value (`init`)</b>

* defines starting value
* affects result type and behavior

```cpp
std::accumulate(v.begin(), v.end(), 100);
```

Result:

```text
110
```

## <b>4. Common Use Cases</b>

##### ✔️ Sum

```cpp id="x8drgk"
int sum = std::accumulate(v.begin(), v.end(), 0);
```

##### ✔️ Product

```cpp
int product = std::accumulate(v.begin(), v.end(), 1, std::multiplies<>());
```

##### ✔️ Count with condition

```cpp
int count = std::accumulate(v.begin(), v.end(), 0,
    [](int acc, int x)
    {
        return acc + (x % 2 == 0);
    });
```