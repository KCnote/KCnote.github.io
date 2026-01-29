---
layout: post
title: "Secondary Storage"
date: 2026-01-28 00:00:00 +0900
author: kang
categories: [Computer Structure, Components]
tags: [Computer Structure, Components, Secondary Storage, HDD, SSD, Flash Memory]
pin: false
math: true
mermaid: true

---

# 💾 Secondary Storage — HDD & Flash Memory

> A structured overview of **secondary storage technologies**, focusing on **how data is physically stored**, **how performance is determined**, and **what developers should understand**.

---

# 1️⃣ Hard Disk Drive (HDD) — Magnetic Storage

## How HDD Stores Data
HDDs store data **magnetically on spinning platters**.

### Physical Layout
- **Track** → circular data path
- **Sector** → smallest addressable unit
- **Cylinder** → same track across multiple platters
- Sequential data is often placed **within the same cylinder** for performance

---

## HDD Access Time Components

| Component | Meaning |
|---------|--------|
| **Seek Time** | Time to move head to correct track |
| **Rotational Latency** | Time for platter to rotate to target sector |
| **Transfer Time** | Time to move data between disk and computer |

### Total HDD Access Time
```
Access Time = Seek Time + Rotational Latency + Transfer Time
```

---

## Why HDDs Are Slow
- Mechanical movement required
- Head movement latency dominates access time
- Random access is much slower than sequential access

---

# 2️⃣ Flash Memory — Solid-State Storage

> Flash memory stores data **electronically**, with **no moving parts**, enabling much faster access than HDDs.

---

## NAND vs NOR Flash

| Type | Primary Use | Characteristics |
|------|-------------|----------------|
| **NAND Flash** | SSD, USB, SD cards | High density, low cost |
| **NOR Flash** | Firmware, embedded systems | Fast random reads, low density |

---

## Flash Memory Storage Unit Hierarchy

```
Cell → Page → Block → Plane → Die
```

### Cell
- Smallest storage unit
- Stores **1–4 bits per cell**

| Type | Bits per Cell | Name |
|------|---------------|------|
| **SLC** | 1 bit | Fastest, longest lifespan |
| **MLC** | 2 bits | Balanced |
| **TLC** | 3 bits | Consumer SSD |
| **QLC** | 4 bits | High density, lower endurance |

> More bits per cell = **higher capacity but lower speed & lifespan**

---

## Page & Block Operations

| Operation | Unit |
|---------|------|
| Read | Page |
| Write | Page |
| Erase | Block |

📌 Flash cannot overwrite in place — it must **erase before rewriting**.

---

## Page States

| State | Meaning |
|------|--------|
| **Free** | Never written |
| **Valid** | Contains active data |
| **Invalid** | Contains stale (garbage) data |

---

# 3️⃣ Garbage Collection — Cleaning Flash Storage

> Garbage Collection (GC) reclaims storage by **moving valid pages to a new block and erasing old blocks**.

### Steps
1. Copy **valid pages** to a new block
2. Mark old block as **invalid**
3. Erase old block → becomes reusable

GC is required because flash **cannot overwrite pages directly**.

---

# 4️⃣ Write Amplification & Wear Leveling

## Write Amplification
- Actual writes > requested writes
- Caused by page copying during GC

## Wear Leveling
- Distributes writes evenly across blocks
- Prevents early cell wear-out

---

# 5️⃣ SSD Performance Characteristics

| Factor | Impact |
|------|--------|
| Sequential Access | Very fast |
| Random Access | Faster than HDD |
| Queue Depth | Improves throughput |
| Write Amplification | Affects endurance |
| Cell Type (SLC–QLC) | Affects speed & lifespan |

---

# 6️⃣ Developer-Relevant Takeaways

✔ HDD is cheap but slow due to mechanical latency  
✔ SSD/Flash is faster but has limited write endurance  
✔ Sequential access patterns improve performance  
✔ Garbage collection explains SSD slowdowns  
✔ Wear leveling protects SSD lifespan  

---

# 🧠 One-Line Mental Model

> **HDD = spinning disk (slow mechanics)**  
> **SSD = electronic memory (fast, but wears out)**
