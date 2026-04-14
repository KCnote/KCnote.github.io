---
layout: post
title: "Allocation Cost"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Allocation Cost</b>

---

### <b>Prerequisites</b>


---

## <b>1. Why Allocation Matters</b>

Memory allocation is **not free**.

👉 Costs include:
- Heap allocation (`new`, `malloc`)
- System calls / allocator overhead
- Cache misses
- Memory fragmentation

> Frequent allocation/deallocation can become a **major performance bottleneck**

## <b>2. Vector Allocation</b>

### ✔ Growth behavior

```cpp
std::vector<int> v;
v.push_back(1);
```

###### When capacity is exceeded:
- New memory allocated
- Old data copied/moved
- Old memory freed

##### ❌ Problem Example

```cpp
for (int i = 0; i < N; i++) 
{
    std::vector<int> v;
    v.push_back(i);
}
```

###### Every iteration:
- Allocation
- Possible reallocation
- Deallocation

❌ Huge overhead in tight loops

##### ✔ Use `reserve`

```cpp
std::vector<int> v;
v.reserve(N);

for (int i = 0; i < N; i++)
    v.push_back(i);
```

Allocates once → no reallocation

##### ✔ Reuse memory

```cpp
std::vector<int> v;
v.reserve(N);

for (int i = 0; i < N; i++) 
{
    v.clear();       // keeps capacity
    v.push_back(i);
}
```

- No allocation inside loop  
- Only logical reset

| Function | Effect |
|---------|-------|
| `clear()` | Removes elements, keeps memory |
| `shrink_to_fit()` | Requests memory release |

- Use `clear()` for reuse  
- Avoid `shrink_to_fit()` in hot paths

## <b>3. Object Pool</b>

Reuse objects instead of repeatedly allocating/deallocating them

##### ✔ Basic Idea

```cpp
class ObjectPool 
{
    std::vector<MyObject> pool;
    int index = 0;

public:
    MyObject& acquire() 
    {
        return pool[index++];
    }

    void reset() 
    {
        index = 0;
    }
};
```

###### ✔ Usage

```cpp
ObjectPool pool;

for (...) {
    auto& obj = pool.acquire();
    // use obj
}

pool.reset();  // reuse all objects
```

No allocation during runtime loop

##### ✔ DO

- Reserve memory upfront
- Reuse containers (`clear()` instead of recreate)
- Use object pool for repeated allocation
- Minimize heap operations in loops

##### ❌ DON'T

- Allocate inside tight loops
- Frequently call `new/delete`
- Use `shrink_to_fit()` repeatedly
