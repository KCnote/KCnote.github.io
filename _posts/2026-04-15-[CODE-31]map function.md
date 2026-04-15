---
layout: post
title: "std::map"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b> std::map</b>

---

### <b>Prerequisites</b>


---

## <b>1. What is `std::map`?</b>

`std::map` is an associative container that stores elements in **key-value pairs**.

```cpp
std::map<Key, Value>
```

- Keys are **unique** (no duplicates allowed)
- Automatically **sorted by key**
- Implemented using a **Red-Black Tree (self-balancing BST)**

`std::map` is implemented using a **Red-Black Tree**

Use it when:
- You need **sorted data**
- You need **logarithmic lookup**
- You need **range queries** (`lower_bound`, `upper_bound`)

##### Characteristics:
- Self-balancing
- Guarantees O(log N) operations
- Maintains sorted order

##### ✔️ Declaration

```cpp
#include <map>

std::map<std::string, int> m;
```

##### ✔️ Insertion

```cpp
m["apple"] = 10;
m["banana"] = 20;
```

```cpp
m.insert({"orange", 30});
```

```cpp
m["new_key"];
```

If the key does not exist, it will be **created automatically with a default value**.

##### ✔️ Access

```cpp
std::cout << m["apple"];  // 10
```

## <b>2. Functions</b>

##### ✔️ find

```cpp
auto it = m.find("apple");

if (it != m.end())
    std::cout << it->second;
```

Returns `m.end()` if not found

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

##### Order

```cpp
std::map<int, std::string, std::greater<int>> m;
```

Sorts keys in **descending order**

Always iterates in **sorted order (by key)**

| Operation | Complexity |
|----------|------------|
| Insert   | O(log N)   |
| Search   | O(log N)   |
| Delete   | O(log N)   |

## <b>3. `std::map` vs `std::unordered_map`</b>

| Feature | map | unordered_map |
|--------|-----|---------------|
| Structure | Red-Black Tree | Hash Table |
| Ordering | Sorted | Not sorted |
| Speed | O(log N) | Avg O(1) |
| Key Order | Maintained | Not maintained |

Use `map` when **ordering matters**  
Use `unordered_map` when **performance is critical**

## <b>4. Common Mistakes</b>

#### ❌ Overusing `operator[]`

```cpp
if (m["key"] == 0)  // ❌ Dangerous
```

This creates the key if it doesn't exist

✔️ Correct way:

```cpp
if (m.find("key") != m.end())
```

#### ❌ Unnecessary Copy

```cpp
for (auto pair : m)  // ❌ Copy occurs
```

✔️ Better:

```cpp
for (const auto& pair : m)
```
