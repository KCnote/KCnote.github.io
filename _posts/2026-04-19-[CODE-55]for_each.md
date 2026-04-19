---
layout: post
title: "std::for_each"
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

## <b>1. What is `std::for_each`?</b>

`std::for_each` is an algorithm that applies a function to each element in a range.

```cpp
#include <algorithm>
#include <vector>
#include <iostream>

int main()
{
    std::vector<int> v = {1, 2, 3, 4, 5};

    std::for_each(v.begin(), v.end(), [](int x)
    {
        std::cout << x << " ";
    });
}
```

```text
1 2 3 4 5
```

## <b>2. Example</b>

#### <b>2-1. Lambda</b>

```cpp
std::for_each(v.begin(), v.end(), [](int& x)
{
    x *= 2;
});
```

#### <b>2-2. Function Pointer</b>

```cpp
void print(int x)
{
    std::cout << x << " ";
}

std::for_each(v.begin(), v.end(), print);
```

#### <b>2-3. Functor</b>

```cpp
struct Multiply
{
    void operator()(int& x)
    {
        x *= 2;
    }
};

std::for_each(v.begin(), v.end(), Multiply());
```

#### <b>2-4. Return Value</b>

```cpp
auto f = std::for_each(...);
```

Returns the function object used

```cpp
struct Counter
{
    int count = 0;

    void operator()(int)
    {
        count++;
    }
};

Counter c = std::for_each(v.begin(), v.end(), Counter());
std::cout << c.count;
```

#### <b>2-5. Parallel Execution(C++17)</b>

```cpp
#include <execution>

std::for_each(std::execution::par, v.begin(), v.end(), [](int& x)
{
    x *= 2;
});
```

```text
Parallel for_each requires thread-safe operations
```