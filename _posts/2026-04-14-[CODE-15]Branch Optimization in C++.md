---
layout: post
title: "Branch Optimization"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Optimization]
tags: [CODE, CODE - Optimization]
pin: false
math: true
mermaid: true
---

# <b>Branch Optimization</b>

---

### <b>Prerequisites</b>


---

## <b>1. Branch Optimization</b>

Modern CPUs use **branch prediction** to guess the next execution path.

👉 If prediction is correct → fast  
👉 If prediction fails → pipeline flush → **huge performance penalty**

> A mispredicted branch can cost **10~20+ cycles**

1. Can this branch be removed?
2. If not, can it be made predictable?
3. Can it be converted to branchless logic?

## <b>2. Methods of Branch Optimization</b>

#### <b>2-1. Branch Minimization</b>

##### ❌ Branch-heavy code

```cpp
if (x > 0) 
    sum += x;
```

```cpp
if (x > y) 
    result = a;
else 
    result = b;
```

- Branch may be unpredictable
- Causes pipeline stalls

##### ✔ Branchless version

```cpp
sum += (x > 0) * x;
```

- `(x > 0)` → 0 or 1  
- Eliminates branch

```cpp
int flag = (x > y);  // 0 or 1
result = flag * a + (1 - flag) * b;
```

- No branch misprediction
- Better SIMD/vectorization
- Stable performance

| Condition | Recommendation |
|----------|---------------|
| Unpredictable branch | Use branchless |
| Tight loop | Prefer branchless |
| SIMD/vectorization | Required |

#### <b>2-2. Make it predictable on Branch</b>

```cpp
if (likely(x > 0))
{
    // hot path
}
```

Compilers may optimize based on probability

```cpp
// ✔ Better
if (x == 0) 
{
    // common case
} 
else {
    // rare case
}
```

##### ❌ Bad ordering

```cpp
if (x != 0) 
{
    // rare case
} 
else 
{
    // common case
}
```