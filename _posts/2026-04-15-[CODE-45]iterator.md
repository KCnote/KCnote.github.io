---
layout: post
title: "Iterators"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>Iterators</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is an Iterator</b>

An iterator is a **generalized pointer** that allows you to access elements inside a container.

## <b>2. Iterators</b>

##### Iteration

```cpp
#include <vector>
#include <iostream>

int main()
{
    std::vector<int> v = {1, 2, 3};

    for (auto it = v.begin(); it != v.end(); ++it)
        std::cout << *it << " ";
}
```

###### `begin()` and `end()`

```cpp
v.begin();  // first element
v.end();    // one past the last element
```

- `end()` is **not valid to dereference**
- it marks the stopping condition

##### Reverse Iteration

```cpp
for (auto it = v.rbegin(); it != v.rend(); ++it)
    std::cout << *it << " ";
```

###### `rbegin()` and `rend()`

```cpp
v.rbegin();  // last element
v.rend();    // before first element
```

Iterators unify access across containers:

- `vector`
- `list`
- `set`
- `map`

##### Iterator Adapters

**Adapters modify iterator behavior**

###### `back_insert_iterator`

```cpp
#include <iterator>

std::vector<int> v;

auto it = std::back_inserter(v);
*it = 10;  // v.push_back(10)
```

###### `front_insert_iterator`

```cpp
#include <list>

std::list<int> l;

auto it = std::front_inserter(l);
*it = 10;  // l.push_front(10)
```

###### `insert_iterator`

```cpp
auto it = std::inserter(v, v.begin());
*it = 5;  // insert at position
```

###### `reverse_iterator`

```cpp
for (auto it = v.rbegin(); it != v.rend(); ++it)
    std::cout << *it;
```

Traverses container in reverse

###### `move_iterator`

```cpp
#include <iterator>

std::vector<std::string> src = {"a", "b"};
std::vector<std::string> dst;

std::copy(
    std::make_move_iterator(src.begin()),
    std::make_move_iterator(src.end()),
    std::back_inserter(dst)
);
```

Moves elements instead of copying

## <b>3. Iterator Utilities</b>

**Utilities simplify iterator movement**

###### `next`

```cpp
auto it = std::next(v.begin(), 2);
```

Returns a new iterator moved forward

###### `prev`

```cpp
auto it = std::prev(v.end());
```

Returns a new iterator moved backward

###### `advance`

```cpp
auto it = v.begin();
std::advance(it, 2);
```

Moves iterator in-place

| Function | Behavior |
|---------|---------|
| `next` | returns new iterator |
| `prev` | returns new iterator |
| `advance` | modifies existing iterator |

## <b>4. Check</b>

##### ❌ Not all iterators support backward movement

```cpp
std::forward_list → no prev()
```

##### ❌ `advance` depends on iterator type

- Random access → fast
- Forward → linear time

##### Iterator categories matter

| Type | Complexity |
|------|-----------|
| Random access (`vector`) | O(1) |
| Bidirectional (`list`) | O(n) |
| Forward (`forward_list`) | O(n) |