---
layout: post
title: "Bitmask"
date: 2026-04-14 00:00:00 +0900
author: kang
categories: [CODE, CODE - Pattern]
tags: [CODE, CODE - Pattern]
pin: false
math: true
mermaid: true
---

# <b>Bitmask</b>

---

### <b>Prerequisites</b>
    - Bit

---

## <b>1. What is Bitmask?</b>

Bitmasking is a technique that uses **bits of an integer to represent multiple states**. You can manage multiple boolean values using a single integer.

Example:
```cpp
int i32Mask = 0;
```

Each bit represents a state:

```
bit:   3 2 1 0
       ↓ ↓ ↓ ↓
value: 1 0 1 1
```

## <b>2. Why use Bitmask?</b>

✔ Memory efficient  
✔ Very fast operations (bitwise)  
✔ Great for representing combinations of states  

👉 Common use cases:
- Subsets
- Visited checks (DFS/BFS)
- DP with state compression

## <b>3. How to use Bitmask</b>

#### <b>3-1. ON/OFF</b>

##### ✔ Turn ON a bit
```cpp
mask |= (1 << i);
```

##### ✔ Turn OFF a bit
```cpp
mask &= ~(1 << i);
```

##### ✔ Toggle a bit
```cpp
mask ^= (1 << i);
```

##### ✔ Check a bit
```cpp
if (mask & (1 << i)) 
{
    // i-th bit is ON
}
```

##### ✔ Reset all bits
```cpp
mask = 0;
```

#### <b>3-2. Management Enum</b>

```cpp
enum EStatus
{
    EStatus_None = 0x00,
    EStatus_Left = 0x01,    // 0b00001
    EStatus_Top = 0x02,     // 0b00010
    EStatus_Right = 0x04,   // 0b00100
    EStatus_Bottom = 0x08   // 0b01000
    EStatus_LT = EStatus_Left | EStatus_Top,    // 0b00011
    EStatus_LR = EStatus_Left | EStatus_Right,  // 0b00101
    EStatus_LB = EStatus_Left | EStatus_Bottom, // 0b01001
    ....
}
```
