---
layout: post
title: "std::find, std::search, std::merge, std::erase"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::find`, `std::search`, `std::merge`, `std::erase`</b>

---

### <b>Prerequisites</b>

---

## <b>1. What are `std::find`, `std::search`, `std::merge`, `std::erase`</b>

These algorithms are part of the C++ Standard Library and work with **iterators**, not containers directly.

- `find` → find element  
- `search` → find subsequence  
- `merge` → merge sorted ranges  
- `erase` → remove elements  

| Algorithm | Purpose | Complexity |
|----------|--------|------------|
| `find` | find value | O(N) |
| `search` | find subsequence | O(N×M) |
| `merge` | merge sorted ranges | O(N+M) |
| `erase` | remove elements | O(N) |

###### STL algorithms are:

- iterator-based
- container-independent
- highly optimized

## <b>2. `std::find`</b>

Finds the **first occurrence** of a value.

```cpp
std::find(begin, end, value);
```

#### Example

```cpp
#include <algorithm>
#include <vector>
#include <iostream>

int main()
{
    std::vector<int> v = {1, 2, 3, 4};

    auto it = std::find(v.begin(), v.end(), 3);

    if (it != v.end())
        std::cout << "Found: " << *it;
}
```

Complexity: O(N)

## <b>3. `std::search`</b>

Finds a **subsequence (range inside range)**

```cpp
std::search(begin1, end1, begin2, end2);
```

#### Example

```cpp
#include <algorithm>
#include <vector>
#include <iostream>

int main()
{
    std::vector<int> v = {1, 2, 3, 4, 5};
    std::vector<int> sub = {3, 4};

    auto it = std::search(v.begin(), v.end(), sub.begin(), sub.end());

    if (it != v.end())
        std::cout << "Subsequence found";
}
```

Complexity: O(N × M)

- pattern matching
- substring-like search
- sequence detection

## <b>4. `std::merge`</b>

Merges **two sorted ranges** into one sorted output.

```cpp
std::merge(begin1, end1, begin2, end2, output);
```

#### Example

- Inputs must be **sorted**

```cpp
#include <algorithm>
#include <vector>
#include <iostream>

int main()
{
    std::vector<int> a = {1, 3, 5};
    std::vector<int> b = {2, 4, 6};
    std::vector<int> result;

    std::merge(a.begin(), a.end(),
               b.begin(), b.end(),
               std::back_inserter(result));

    for (int x : result)
        std::cout << x << " ";
}
```

Complexity: O(N + M)

## <b>5. `std::erase`</b>

Removes elements from containers.

```cpp
v.erase(v.begin());
```

---

## 🔥 Erase-Remove Idiom (Important)

```cpp
#include <algorithm>

v.erase(std::remove(v.begin(), v.end(), 3), v.end());
```

```cppp
v = {1, 2, 3, 4, 3, 5}

std::remove(v.begin(), v.end(), 3)
v = {1, 2, 4, 5, ?, ?}
            ↑ it

v.erase(it, v.end());
v = {1, 2, 4, 5}
```

1. `remove` shifts elements
2. returns new logical end
3. `erase` actually deletes

Complexity: O(N)