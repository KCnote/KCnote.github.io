---
layout: post
title: "std::lower_bound and std::upper_bound"
date: 2026-04-16 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`lower_bound` and `upper_bound`</b>

---

### <b>Prerequisites</b>

---

## <b>1. What are `lower_bound` and `upper_bound`?</b>

They are STL algorithms used for **binary search on sorted ranges**. They work in **O(log N)** time

- `lower_bound` → first element **≥ value**
- `upper_bound` → first element **> value**

```cpp
#include <algorithm>

std::lower_bound(begin, end, value);
std::upper_bound(begin, end, value);
```

#### Example

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

int main()
{
    std::vector<int> v = {1, 2, 2, 2, 3, 4};

    auto lb = std::lower_bound(v.begin(), v.end(), 2);
    auto ub = std::upper_bound(v.begin(), v.end(), 2);

    std::cout << "lower_bound: " << *lb << "\n";
    std::cout << "upper_bound: " << *ub << "\n";
}
```

```text
lower_bound → first 2
upper_bound → first 3
```

```text
value = 2

[1, 2, 2, 2, 3, 4]
     ↑        ↑
     lb       ub
```

| Function | Meaning |
|----------|--------|
| `lower_bound` | first ≥ value |
| `upper_bound` | first > value |

## <b>1-1. Counting Duplicates</b>

```cpp
int count = ub - lb;
```

Number of occurrences of `value`

```cpp
int count = std::upper_bound(v.begin(), v.end(), 2)
          - std::lower_bound(v.begin(), v.end(), 2);
```

## <b>1-2. Custom Comparator</b>

```cpp
std::lower_bound(v.begin(), v.end(), value, std::greater<int>());
```

Works with custom ordering

## <b>1-3. with Struct</b>

```cpp
struct Item
{
    int key;
};

std::vector<Item> v;

auto it = std::lower_bound(v.begin(), v.end(), 10,
    [](const Item& a, int value)
    {
        return a.key < value;
    });
```

## <b>1-4. Performance</b>

- Time complexity: **O(log N)**
- Requires random access iterator for best performance

## 🧠 Insight

- On `std::vector` → fast  
- On `std::set` → use member function instead

## <b>2. STL Containers Version</b>

```cpp
std::set<int> s;

auto it = s.lower_bound(10);
```

Better than `std::lower_bound` for tree containers

## <b>3. Real-World Patterns</b>

#### ✔️ Find insertion position

```cpp
v.insert(std::lower_bound(v.begin(), v.end(), x), x);
```

#### ✔️ Binary search existence

```cpp
auto it = std::lower_bound(v.begin(), v.end(), x);

if (it != v.end() && *it == x)
{
    // found
}
```

#### ✔️ Range query

```cpp
auto start = std::lower_bound(v.begin(), v.end(), L);
auto end   = std::upper_bound(v.begin(), v.end(), R);
```

