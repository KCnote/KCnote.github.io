---
layout: post
title: "std::unordered_map"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b> std::unordered_map</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is `std::unordered_map`?</b>

`std::unordered_map` is an associative container that stores elements in **key-value pairs**, just like `std::map`.

```cpp
std::unordered_map<Key, Value>
```

- Keys are **unique**
- **No ordering** of elements
- Implemented using a **Hash Table**
- Provides **average O(1)** time complexity

`std::unordered_map` is implemented using a **Hash Table**

##### How it works:
1. Key → Hash function
2. Hash → Bucket index
3. Store value in that bucket

Multiple keys may land in the same bucket (**collision**)

#### Hash Collision

```cpp
key1 → hash → bucket 3
key2 → hash → bucket 3
```

This is called a **collision**

##### Handling:
- Chaining (linked list / nodes inside bucket)
- Open addressing (implementation dependent)

Use it when:
- You need **fast lookup (O(1))**
- Order does **not matter**
- You handle large datasets

##### ✔️ Declaration

```cpp
#include <unordered_map>

std::unordered_map<std::string, int> m;
```

##### ✔️ Insertion

```cpp
m["apple"] = 10;
m["banana"] = 20;
```

Or:

```cpp
m.insert({"orange", 30});
```

```cpp
m["new_key"];
```

If the key does not exist, it will be **created automatically with a default value**

##### ✔️ Access

```cpp
std::cout << m["apple"];  // 10
```

## <b>2. Functions</b>

##### find

```cpp
auto it = m.find("apple");

if (it != m.end())
    std::cout << it->second;
```

##### erase

```cpp
m.erase("apple");
```

##### size

```cpp
m.size();
```

##### clear

```cpp
m.clear();
```

##### Iteration

```cpp
for (const auto& pair : m)
    std::cout << pair.first << " : " << pair.second << "\n";
```

Order is **not guaranteed**

| Operation | Complexity |
|----------|------------|
| Insert   | O(1) average |
| Search   | O(1) average |
| Delete   | O(1) average |
| Worst Case | O(N) |

Worst case happens when **hash collisions** occur

## <b>3. `std::map` vs `std::unordered_map`</b>

| Feature | unordered_map | map |
|--------|---------------|-----|
| Structure | Hash Table | Red-Black Tree |
| Ordering | None | Sorted |
| Speed | O(1) avg | O(log N) |
| Worst Case | O(N) | O(log N) |
| Memory | More | Less |

Use `unordered_map` when **performance is critical**

## <b>4. Common Mistakes</b>

##### ❌ Overusing `operator[]`

```cpp
if (m["key"] == 0)  // ❌ Dangerous
```

Creates key if it doesn't exist

✔️ Correct:

```cpp
if (m.find("key") != m.end())
```

##### ❌ Assuming order

```cpp
for (auto& p : m)
{
    // ❌ Do NOT expect sorted order
}
```

## <b>5. Custom Hash Function</b>

```cpp
struct MyHash
{
    size_t operator()(int x) const
    {
        return x * 31;
    }
};

std::unordered_map<int, int, MyHash> m;
```
