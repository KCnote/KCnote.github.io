---
layout: post
title: "std::multiplies"
date: 2026-04-20 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::multiplies`</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `std::multiplies`?</b>

`std::multiplies` is a function object (functor) defined in `<functional>` that performs **multiplication**.

It is part of the standard functional utilities used with STL algorithms.

```cpp
#include <functional>

std::multiplies<int> mul;

int result = mul(3, 4);  // 12
```

#### Key Idea

```text
std::multiplies<T>()(a, b) == a * b
```

behaves like a function:

```cpp
int x = std::multiplies<int>()(3, 4);  // 12
```

## <b>2. Function Signature</b>

```cpp
template<class T>
struct multiplies
{
    constexpr T operator()(const T& lhs, const T& rhs) const;
};
```

#### Example with `std::accumulate`

```cpp
#include <numeric>
#include <functional>
#include <vector>

std::vector<int> v = {1, 2, 3, 4};

int product = std::accumulate(v.begin(), v.end(),
                              1,
                              std::multiplies<>());
```

```text
24
```

#### Example with `std::reduce`

```cpp
#include <numeric>
#include <execution>
#include <functional>

int product = std::reduce(std::execution::par,
                          v.begin(), v.end(),
                          1,
                          std::multiplies<>());
```

## <b>3. Why Use `std::multiplies`</b>

#### Cleaner Code

```cpp
std::accumulate(v.begin(), v.end(), 1,
    [](int a, int b)
    {
        return a * b;
    });
```

vs

```cpp
std::accumulate(v.begin(), v.end(), 1, std::multiplies<>());
```

shorter and clearer

#### Generic Programming

Works well in templates:

```cpp
template<typename T>
T product(const std::vector<T>& v)
{
    return std::accumulate(v.begin(), v.end(), T{1}, std::multiplies<>());
}
```

#### Works with Parallel Algorithms

```cpp id="ck8u0p"
std::reduce(std::execution::par,
            v.begin(), v.end(),
            1,
            std::multiplies<>());
```

associative operation → safe for parallel

#### Related Function Objects

| Functor           | Operation |
| ----------------- | --------- |
| `std::plus`       | `a + b`   |
| `std::minus`      | `a - b`   |
| `std::multiplies` | `a * b`   |
| `std::divides`    | `a / b`   |
| `std::modulus`    | `a % b`   |
