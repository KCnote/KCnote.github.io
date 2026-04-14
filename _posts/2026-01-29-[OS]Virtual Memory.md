---
layout: post
title: "Memory - Virtual Memory & Paging"
date: 2026-01-29 00:00:00 +0900
author: kang
categories: [CS Fundamentals, CS Fundamentals - OS]
tags: [OS, Pages, TLB, Thrashing, Virtual]
pin: false
math: true
mermaid: true

---

# 🧠 Virtual Memory & Paging — Pages, TLB, Page Faults, Replacement, and Thrashing

> A comprehensive guide to **virtual memory**,  
> covering **paging, page tables, TLB, page faults, replacement algorithms, frame allocation, and thrashing**.

---

# 1️⃣ Virtual Memory — Core Idea

**Virtual Memory** allows programs to run even when they are **larger than physical RAM**,  
by loading **only the needed parts** into memory.

> Program runs with the illusion of having **large continuous memory**.

---

# 2️⃣ Paging — Fixed‑Size Memory Division

Instead of allocating contiguous memory:

- Process memory is split into **Pages**
- Physical memory is split into **Frames**
- Pages are mapped to frames **non‑contiguously**

### Benefit
✔ Eliminates external fragmentation  
✔ Allows flexible memory placement  

---

# 3️⃣ Page In / Page Out (Swapping at Page Level)

- **Page In** → Disk → RAM  
- **Page Out** → RAM → Disk  

> Only required pages are loaded → supports **large processes**

---

# 4️⃣ Page Table — Logical → Physical Mapping

Because pages are scattered, the CPU needs a **map**:

> **Page Table** maps `Page Number → Frame Number`

Ensures logical memory appears **continuous** to processes.

---

# 5️⃣ Internal Fragmentation

Occurs when a process does not fully use its last page.

### Example
```
Page Size = 10 KB
Process Size = 108 KB
Unused = 2 KB
```

---

# 6️⃣ PTBR — Page Table Base Register

Each process has its own page table.  
The **PTBR** stores the memory address of the **current process's page table**.

---

# 7️⃣ Page Table Lookup Cost & TLB

Accessing page table in memory doubles access time.

### TLB (Translation Lookaside Buffer)
A **cache of page table entries** stored inside CPU.

| Case | Memory Access Count |
|------|----------------------|
| **TLB Hit** | 1 |
| **TLB Miss** | 2 |

---

# 8️⃣ Paging Address Translation

A logical address consists of:

```
[ Page Number | Offset ]
```

- Page Number → find frame
- Offset → exact byte inside frame

Offset stays the **same** after translation.

---

# 9️⃣ Page Table Entry (PTE)

Each row in a page table contains metadata:

| Bit | Meaning |
|-----|--------|
| **Valid Bit** | Page is in memory |
| **Protection Bits** | R / W / X permissions |
| **Reference Bit** | Recently accessed |
| **Dirty Bit** | Modified since load |

---

# 🔟 Page Fault

Occurs when a page is **not in RAM**.

### Handling Flow
1. CPU detects **valid bit = 0**
2. Page Fault interrupt triggers
3. OS loads page from disk
4. Valid bit set = 1
5. Execution resumes

---

# 1️⃣1️⃣ Demand Paging

Only pages that are **actually accessed** are loaded.

### Pure Demand Paging
> Load nothing at start — pages load **on first access**

---

# 1️⃣2️⃣ Page Replacement Algorithms

When memory is full, OS must **evict a page**.

---

## 🔹 FIFO (First In First Out)
- Oldest page removed  
❌ May evict frequently used pages  

---

## 🔹 Second Chance
- FIFO + Reference Bit  
- Gives recently used pages another chance  

---

## 🔹 Optimal Replacement
- Evict page **used farthest in future**  
✔ Lowest fault rate  
❌ Impossible to implement (requires future knowledge)

---

## 🔹 LRU (Least Recently Used)
- Evict page **not used for longest time**  
✔ Practical & effective  

---

# 1️⃣3️⃣ Thrashing — When Paging Kills Performance

**Thrashing** occurs when system spends **more time paging than executing**.

### Cause
> Too few frames per process

### Effect
❌ High page fault rate  
❌ Low CPU utilization  

---

# 1️⃣4️⃣ Frame Allocation Strategies

---

## 🔹 Static Allocation

### Equal Allocation
All processes get same number of frames  
❌ Ignores real needs  

### Proportional Allocation
Frames assigned proportional to process size  
❌ Usage behavior ignored  

---

## 🔹 Dynamic Allocation

### Working Set Model
Allocate frames based on **recently used pages**

### Page Fault Frequency Control For Performance
- Too many faults → give more frames  
- Too few faults → take frames away  

---

# 1️⃣5️⃣ Copy‑On‑Write (COW)

After `fork()`:
- Parent & child **share memory**
- Copy occurs **only when writing happens**

✔ Saves memory  
✔ Fast process creation  

---

# 1️⃣6️⃣ Multilevel Paging

Large page tables are split into **multiple levels**.

### Logical Address Format
```
[ Outer Page # | Inner Page # | Offset ]
```

✔ Saves memory  
❌ More memory lookups on page faults  

---

# 🎯 Developer Takeaways

✔ Virtual memory enables large apps  
✔ TLB is critical for performance  
✔ Page replacement impacts speed  
✔ Thrashing indicates frame starvation  
✔ COW improves fork efficiency  
✔ Multilevel paging scales page tables  

---

# 📌 Suggested Blog Title

### **Virtual Memory & Paging — TLB, Page Faults, Replacement Algorithms, and Thrashing**
