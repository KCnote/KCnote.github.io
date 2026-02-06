---
layout: post
title: "Process Synchronization"
date: 2026-01-29 00:00:00 +0900
author: kang
categories: [OS, Parallel]
tags: [OS, Thread, MultiThread, MultiProcessing, Parallel, CPU, Process, Synchronization,Mutex, Semaphores, Monitors, Deadlock]
pin: false
math: true
mermaid: true

---

# 🔒 Process Synchronization — Critical Sections, Locks, Semaphores, Monitors, and Deadlocks

> A structured guide to **process/thread synchronization**,  
> covering **race conditions, critical sections, mutexes, semaphores, monitors, and deadlock handling**.

---

# 1️⃣ Why Process Synchronization Matters

Synchronization ensures:

✔ Correct execution order  
✔ Safe access to shared resources  
✔ Consistent data  
✔ Prevention of race conditions  

---

# 2️⃣ Execution Order Control

### Example: Reader–Writer Problem
> Writing must complete **before reading** to avoid inconsistent data.

Used when **task order matters**, not just mutual exclusion.

---

# 3️⃣ Mutual Exclusion (Preventing Concurrent Access)

Only **one process/thread** may access a sensitive resource at a time.

### Example: Bank Account Problem
> Prevent two withdrawals from corrupting account balance.

### Example: Producer–Consumer Problem
> Prevent buffer overflow / underflow when sharing a queue.

---

# 4️⃣ Shared Resources

Shared resources include:
- Global variables
- Heap memory
- Files
- I/O devices
- Secondary storage

---

# 5️⃣ Critical Section

A **critical section** is a code region that accesses shared data.

### Rule
> If one process enters the critical section, others **must wait**.

### Race Condition
Occurs when **multiple processes enter simultaneously**, breaking consistency.

---

# 6️⃣ Three Principles of Correct Critical Section Solutions

| Principle | Meaning |
|---------|--------|
| **Mutual Exclusion** | Only one process enters |
| **Progress** | If free, waiting process may enter |
| **Bounded Waiting** | No process waits forever |

---

# 7️⃣ Synchronization Techniques

---

## 🔹 Mutex Lock

Used to **enforce mutual exclusion**.

### Mechanism
- `acquire()` → lock resource  
- `release()` → unlock resource  

### Problem: Busy Waiting
```cpp
while(lock == true); // wastes CPU
```
❌ Not recommended

---

## 🔹 Semaphore

A **generalized lock** that supports **multiple shared resources**.

### Components
- Integer counter = number of available resources
- `wait()` (P) → decrement or block  
- `signal()` (V) → increment or wake  

### Behavior
```
wait()   → N--
signal() → N++
```

If `N == 0` → process **blocks**  
When resource freed → process **moves to Ready queue**

---

## 🔹 Monitor

A high‑level synchronization construct.

### Properties
✔ Only one process executes inside monitor at a time  
✔ Automatically enforces mutual exclusion  

### Execution Order Control
Uses **Condition Variables**:
- `wait()` → sleep  
- `signal()` → wake  

---

# 8️⃣ Deadlock (교착 상태)

> A deadlock occurs when processes **wait on each other forever**.

### Visual Pattern
Deadlocks form a **cycle of waiting**.

---

# 9️⃣ Deadlock Necessary Conditions (Coffman Conditions)

All four must hold:

1️⃣ **Mutual Exclusion** — resource cannot be shared  
2️⃣ **Hold and Wait** — hold one resource while waiting for another  
3️⃣ **No Preemption** — resources cannot be forcibly taken  
4️⃣ **Circular Wait** — cycle exists among waiting processes  

---

# 🔟 Deadlock Handling Strategies

---

## 🔹 Prevention (Remove One Condition)

| Condition Removed | Strategy | Tradeoff |
|------------------|---------|---------|
| Mutual Exclusion | Share resources | Often impossible |
| Hold & Wait | Allocate all at once | Low utilization |
| No Preemption | Allow forced release | Limited |
| Circular Wait | Number resources | Complex |

---

## 🔹 Avoidance

Allocate resources **carefully** to stay in a **safe state**.

### Key Concepts
✔ Safe Sequence  
✔ Safe State  
❌ Unsafe State  

> Example algorithm: **Banker’s Algorithm**

---

## 🔹 Detection & Recovery

### Detection
- Track allocated & requested resources
- Detect cycles

### Recovery
✔ Resource preemption  
✔ Kill deadlocked processes  
✔ Rollback states  

---

# 🎯 Developer Takeaways

✔ Race conditions break correctness  
✔ Mutex protects single resources  
✔ Semaphores manage multiple units  
✔ Monitors simplify synchronization  
✔ Deadlocks require prevention or detection  
✔ Correct synchronization = correctness + performance  
