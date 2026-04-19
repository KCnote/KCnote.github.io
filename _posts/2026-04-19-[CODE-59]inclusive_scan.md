---
layout: post
title: "std::inclusive_scan"
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

## <b>1. What is `std::inclusive_scan`? (C++17)</b>

`std::inclusive_scan` is a standard algorithm introduced in C++17 that computes a **prefix sum (cumulative sum)** over a range.

It transforms a sequence like this:

```text
[1, 2, 3, 4]
```

into:

```text
[1, 3, 6, 10]
```

Each element becomes the sum of all previous elements including itself.

```cpp
#include <numeric>
#include <vector>

std::vector<int> input = {1, 2, 3, 4};
std::vector<int> output(input.size());

std::inclusive_scan(input.begin(), input.end(), output.begin());
```

Result:

```text
[1, 3, 6, 10]
```

```text
output[i] = input[0] + input[1] + ... + input[i]
```

This is known as a **prefix sum**.

## <b>2. How to work</b>

#### Function Signature

```cpp
template<class InputIt, class OutputIt>
OutputIt inclusive_scan(InputIt first, InputIt last,
                        OutputIt d_first);
```

#### With Custom Operation

```cpp id="o4m6q3"
template<class InputIt, class OutputIt, class BinaryOp>
OutputIt inclusive_scan(InputIt first, InputIt last,
                        OutputIt d_first,
                        BinaryOp op);
```

```cpp
#include <numeric>
#include <vector>

std::vector<int> v = {1, 2, 3, 4};
std::vector<int> out(v.size());

std::inclusive_scan(v.begin(), v.end(), out.begin(),
    [](int a, int b)
    {
        return a * b;
    });
```

```text
[1, 2, 6, 24]
```

cumulative multiplication

#### In-place Operation

```cpp id="t9p9l3"
std::inclusive_scan(v.begin(), v.end(), v.begin());
```

* input and output can be the same container
* modifies original data

## <b>3. Parallel Version (C++17)</b>

```cpp
#include <execution>

std::inclusive_scan(std::execution::par,
                    input.begin(), input.end(),
                    output.begin());
```

* supports parallel execution
* handles dependency internally

## <b>4. Versus others</b>

This pattern:

```cpp
result[i] = result[i-1] + 1;
```

has **data dependency**.

* cannot be safely parallelized with `transform`
* requires a specialized algorithm

`inclusive_scan` is designed exactly for this case

| Algorithm        | Description                       |
| ---------------- | --------------------------------- |
| `inclusive_scan` | includes current element          |
| `exclusive_scan` | excludes current element          |
| `reduce`         | reduces entire range to one value |
| `accumulate`     | sequential reduction              |

#### inclusive vs exclusive

##### inclusive_scan

```text
input  = [1, 2, 3, 4]
output = [1, 3, 6, 10]
```

##### exclusive_scan

```text
input  = [1, 2, 3, 4]
output = [0, 1, 3, 6]
```