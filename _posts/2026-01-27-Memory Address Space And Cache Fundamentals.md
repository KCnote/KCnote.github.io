---
layout: post
title: "Memory Address Space & Cache"
date: 2026-01-27 00:00:00 +0900
author: kang
categories: [Computer Structure, Memory]
tags: [Computer Structure, Components, FIFO, LRU, TLB, Cache, Memory, Virtual Address, Physical Address]
pin: false
math: true
mermaid: true

---

# 🧠 Memory Address Space & Cache

> A clean, **readable, blog‑friendly explanation** of how CPUs handle
> memory addressing, protection, and cache performance.

## 📌 Quick Overview

**This post explains:** - How **virtual memory maps to physical
memory** - How the **MMU protects processes** - Why **cache hierarchy
improves performance** - How **locality makes cache effective**

------------------------------------------------------------------------

# 🏗 PART 1 --- Memory Addressing (Concept Flow)

## 1️⃣ Physical vs Virtual Address

### 🧱 Physical Address

-   Real **hardware memory location**
-   Used internally by **RAM**
-   **Not visible** to programs

### 🧭 Virtual (Logical) Address

-   Address seen by **CPU & running processes**
-   Each process has **its own private address space**
-   Multiple programs can use the **same address safely**

------------------------------------------------------------------------

## 2️⃣ Address Translation --- MMU

The **MMU (Memory Management Unit)** converts **virtual → physical**
addresses.

### 🔄 Translation Model

    Physical Address = Virtual Address + Base Register

### ✅ Why this matters

-   Process isolation
-   Memory safety
-   Prevents illegal access

------------------------------------------------------------------------

## 3️⃣ Memory Protection

The CPU ensures a process **cannot access memory outside its region**.

### 🛑 Limit Register Check

    if address ≥ limit → trap (memory fault)

### 🛡 Benefits

✔ Process isolation\
✔ System stability\
✔ Security enforcement

------------------------------------------------------------------------

# 🧱 PART 2 --- Memory Hierarchy & Cache

## 4️⃣ Storage Hierarchy Pyramid

> The closer memory is to the CPU, the **faster**, **smaller**, and
> **more expensive** it becomes.

    Registers
    ↓
    L1 Cache
    ↓
    L2 Cache
    ↓
    L3 Cache
    ↓
    RAM
    ↓
    SSD / HDD

------------------------------------------------------------------------

## 5️⃣ Cache Memory --- Why It Exists

Cache is a **small, fast SRAM layer** between CPU and RAM.

### 🎯 Purpose

-   Reduce memory latency
-   Store **frequently used data**
-   Prevent CPU stalls

------------------------------------------------------------------------

## 6️⃣ Cache Levels & Multi‑Core Design

  Level       Location      Purpose
  ----------- ------------- ------------------
  🟦 **L1**   Inside Core   Fastest access
  🟩 **L2**   Inside Core   Larger buffer
  🟨 **L3**   Shared        Cross‑core cache

### Split Cache

-   **L1I** → Instruction Cache\
-   **L1D** → Data Cache

------------------------------------------------------------------------

## 7️⃣ Cache Coherency (Multi‑Core Problem)

Each CPU core has **its own cache copy**.

### ⚠ Problem

> Cores may hold **different values**

### ✅ Solution

-   MESI / MOESI coherency protocols
-   Shared L3 cache synchronization

------------------------------------------------------------------------

# 🧠 PART 3 --- Why Cache Works (Locality)

## 8️⃣ Temporal Locality

> Recently used data is likely used again

Examples: - Loop counters - Function stack data

------------------------------------------------------------------------

## 9️⃣ Spatial Locality

> Nearby memory addresses tend to be accessed

Examples: - Arrays - Sequential instructions

------------------------------------------------------------------------

## 🔁 PART 4 --- Cache Performance

## 🔹 Cache Hit vs Miss

  Type          Meaning                 Performance
  ------------- ----------------------- -------------
  ✅ **Hit**    Data found in cache     Fast
  ❌ **Miss**   Data fetched from RAM   Slow

### 📊 Hit Rate Formula

    Hit Rate = Hits / (Hits + Misses)

### 📈 Measuring Cache Performance

Tools: - `perf` - Intel VTune - AMD uProf - ARM PMU

------------------------------------------------------------------------

# 🚀 PART 5 --- Advanced Concepts

-   Cache mapping (Direct / Set‑Associative / Fully Associative)
-   Replacement policies (LRU / FIFO)
-   Write policies (Write‑Back / Write‑Through)
-   **TLB** --- Address translation cache

------------------------------------------------------------------------
