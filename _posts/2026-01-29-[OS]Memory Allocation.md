---
layout: post
title: "Contiguous Memory Allocation"
date: 2026-01-29 00:00:00 +0900
author: kang
categories: [OS, Memory]
tags: [OS, Contiguous, Memory, Allocation,Swapping, Fragmentation, Virtual]
pin: false
math: true
mermaid: true

---

# 🧠 Contiguous Memory Allocation — Swapping, Placement Strategies, and Fragmentation

> How operating systems manage **contiguous memory**,  
> including **swapping, allocation strategies, fragmentation problems, and solutions**.

---

# 1️⃣ Contiguous Memory Allocation

**Contiguous memory allocation** assigns each process a **single continuous block** of physical memory.

### Key Rule
> A process must fit **entirely in one continuous region**.

---

# 2️⃣ Swapping (Memory Overcommit Handling)

When **memory is insufficient**, the OS temporarily moves processes to secondary storage.

## 🔹 Swap Out
> Move process **RAM → Disk**

## 🔹 Swap In
> Move process **Disk → RAM**

### Why Needed?
- Total process memory demand **> physical RAM**
- Frees memory for active processes

---

# 3️⃣ Memory Allocation Strategies

Processes must be placed into **available free memory regions**.

---

## 🔹 First‑Fit
> Assign the **first block large enough**

✔ Fast allocation  
❌ May cause fragmentation near front

---

## 🔹 Best‑Fit
> Assign the **smallest block that fits**

✔ Minimizes leftover space  
❌ Slow (must scan all blocks)  
❌ Creates tiny unusable holes

---

## 🔹 Worst‑Fit
> Assign the **largest available block**

✔ Leaves large remaining holes  
❌ Often wastes big memory chunks

---

# 4️⃣ Problems of Contiguous Allocation

---

## ❌ External Fragmentation

> Free memory becomes **split into many small gaps**,  
> even if total free memory is sufficient.

### Example
```
[ Used ][ Free ][ Used ][ Free ][ Used ]
```
Large process **cannot fit** due to split holes.

---

## ❌ Cannot Run Large Processes

> Process size **must not exceed physical memory**

---

# 5️⃣ Solutions to External Fragmentation

---

## 🔹 Compaction (Memory Compression)

> Move processes to **merge scattered free spaces**

✔ Creates large contiguous free block  
❌ CPU overhead  
❌ Pauses processes temporarily  

---

## 🔹 Virtual Memory (Paging)

> Break memory into **fixed‑size pages**, eliminating need for contiguous space

✔ No external fragmentation  
✔ Allows processes larger than RAM  
✔ Basis of modern OS memory management  

---

# 6️⃣ Why Contiguous Allocation Is Inefficient Today

| Limitation | Impact |
|-----------|--------|
| Requires contiguous space | Poor flexibility |
| External fragmentation | Wasted RAM |
| Limited process size | Not scalable |
| Compaction overhead | Performance cost |

> Modern OS prefer **paging and virtual memory**.

---

# 🎯 Developer Takeaways

✔ First‑fit = fast and practical  
✔ Best‑fit = space‑efficient but slower  
✔ External fragmentation wastes memory  
✔ Compaction fixes gaps but costs CPU  
✔ Paging solves fragmentation fundamentally  
