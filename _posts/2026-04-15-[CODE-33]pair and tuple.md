---
layout: post
title: "std::pair and std::tuple"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b> `std::pair` and `std::tuple`</b>

---

### <b>Prerequisites</b>


---

## <b>1. `std::pair`?</b>

`std::pair` is a simple container that holds **exactly two values**.

```cpp
std::pair<T1, T2>
```

Useful for grouping two related values together.

- Return **two values** from a function
- Store simple key-value pairs
- Use in STL containers (e.g., `map`, `priority_queue`)

##### ✔️ Declaration

```cpp
#include <utility>

std::pair<int, std::string> p;
```

##### ✔️ Initialization

```cpp
std::pair<int, std::string> p = {1, "apple"};
```

Or:

```cpp
auto p = std::make_pair(1, "apple");
```

##### ✔️ Access

```cpp
std::cout << p.first;   // 1
std::cout << p.second;  // apple
```

#### ✔️ Comparison

```cpp
std::pair<int, int> a = {1, 2};
std::pair<int, int> b = {1, 3};

if (a < b)
{
    // true (lexicographical comparison)
}
```

- Compare `.first`
- If equal → compare `.second`

## <b>2. `std::tuple`?</b>

`std::tuple` is a container that can hold **multiple values (more than two)**.

```cpp
std::tuple<T1, T2, T3, ...>
```

- Return **multiple values** from a function
- Temporary grouping of heterogeneous data
- Avoid creating a struct for simple cases

##### ✔️ Declaration

```cpp
#include <tuple>

std::tuple<int, std::string, double> t;
```

##### ✔️ Initialization

```cpp
auto t = std::make_tuple(1, "apple", 3.14);
```

##### ✔️ Access

```cpp
std::cout << std::get<0>(t);  // 1
std::cout << std::get<1>(t);  // apple
std::cout << std::get<2>(t);  // 3.14
```

##### ✔️ Structured Binding (C++17)

```cpp
auto [id, name, value] = t;

std::cout << id << " " << name << " " << value;
```

Much cleaner and readable

##### ✔️ Modify Value

```cpp
std::get<0>(t) = 10;
```

##### ✔️ Comparison

```cpp
std::tuple<int, int, int> a = {1, 2, 3};
std::tuple<int, int, int> b = {1, 2, 4};

if (a < b)
{
    // true
}
```

Also **lexicographical comparison**

## <b>3. `std::pair` vs `std::tuple`</b>

| Feature | pair | tuple |
|--------|------|-------|
| Number of elements | 2 | 2 or more |
| Access | `.first`, `.second` | `std::get<N>` |
| Readability | High | Lower (index-based) |
| Flexibility | Low | High |

```cpp
// pair
std::pair<int, int> p = {1, 2};

// tuple
std::tuple<int, int, int> t = {1, 2, 3};
```

## <b>4. Common Mistakes</b>

##### ❌ Forgetting index type

```cpp
std::get<i>(t);  // ❌ wrong
```

✔️ Must be compile-time constant:

```cpp
std::get<0>(t);
```

##### ❌ Overusing tuple

If structure becomes complex:

```cpp
std::tuple<int, std::string, double, int, float>
```

Better to use:

```cpp
struct Data
{
    int id;
    std::string name;
    double value;
};
```
