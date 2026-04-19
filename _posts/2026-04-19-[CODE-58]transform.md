---
layout: post
title: "std::transform"
date: 2026-04-19 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::transform`</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `std::transform`?</b>

`std::transform` is a standard algorithm that applies a function to a range of elements and stores the result in another range. It is commonly used for **data transformation**, making it more suitable than `std::for_each` when producing output.

Use `transform` when:

* you produce new data
* you want functional-style code
* you avoid side effects

```cpp
#include <algorithm>
#include <vector>

std::vector<int> input = {1, 2, 3, 4, 5};
std::vector<int> output(input.size());

std::transform(input.begin(), input.end(),
               output.begin(),
               [](int x)
               {
                   return x * 2;
               });
```

```
{2, 4, 6, 8, 10}
```

* takes input range
* applies a function
* writes results to output range

## <b>2. Series</b>

#### <b>2-1. Unary Transform</b>

```cpp
template<class InputIt, class OutputIt, class UnaryOp>
OutputIt transform(InputIt first, InputIt last,
                   OutputIt d_first,
                   UnaryOp op);
```

```cpp
std::vector<int> v = {1, 2, 3};

std::transform(v.begin(), v.end(), v.begin(), [](int x)
{
    return x * 2;
});
```

Result:

```
{2, 4, 6}
```

#### <b>2-2. Binary Transform</b>

```cpp
template<class InputIt1, class InputIt2, class OutputIt, class BinaryOp>
OutputIt transform(InputIt1 first1, InputIt1 last1,
                   InputIt2 first2,
                   OutputIt d_first,
                   BinaryOp op);
```

```cpp
std::vector<int> a = {1, 2, 3};
std::vector<int> b = {4, 5, 6};
std::vector<int> result(3);

std::transform(a.begin(), a.end(),
               b.begin(),
               result.begin(),
               [](int x, int y)
               {
                   return x + y;
               });
```

```
{5, 7, 9}
```

#### <b>2-3. In-place Transform</b>

```cpp
std::transform(v.begin(), v.end(), v.begin(), [](int x)
{
    return x * 2;
});
```

* input and output can be the same container
* modifies original data

## <b>3. `std::for_each` vs `std::transform`</b>

| Feature      | `for_each`   | `transform`     |
| ------------ | ------------ | --------------- |
| Purpose      | apply action | transform data  |
| Return value | functor      | output iterator |
| Output       | optional     | required        |
| Style        | side-effect  | functional      |

## <b>4. Parallel Version (C++17)</b>

```cpp
#include <execution>

std::transform(std::execution::par,
               input.begin(), input.end(),
               output.begin(),
               [](int x)
               {
                   return x * 2;
               });
```

* runs in parallel
* requires independent operations
