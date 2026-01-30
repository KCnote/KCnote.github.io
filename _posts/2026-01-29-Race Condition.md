---
layout: post
title: "Race Condition"
date: 2026-01-29 00:00:00 +0900
author: kang
categories: [OS, Parallel]
tags: [OS, Thread, MultiThread, MultiProcessing, Parallel, Race Condition]
pin: false
math: true
mermaid: true

---

# 🧨 Race Condition — When Threads Corrupt Shared Data

> A **race condition** happens when **multiple threads access and modify shared data at the same time**,  
> and the program’s behavior **depends on timing or execution order**.

In short:
> **Threads “race” to update shared memory — causing unpredictable results.**

---

# 1️⃣ Why Race Conditions Happen

Race conditions occur when:

✔ Shared data exists  
✔ Multiple threads modify it  
✔ No synchronization is used  

Example shared resources:
- Global variables
- Heap memory
- Files
- Shared containers (vectors, maps)

---

# 2️⃣ Real‑World Analogy

> Two people editing the **same Google Doc at the same time without autosync** → data becomes inconsistent.

---

# 3️⃣ Broken Example — Race Condition in C++

```cpp
#include <iostream>
#include <thread>

int counter = 0;

void increment() {
    for (int i = 0; i < 100000; i++)
    {
        counter++; // ❌ Not thread‑safe
    }
}

int main() 
{
    std::thread t1(increment);
    std::thread t2(increment);

    t1.join();
    t2.join();

    std::cout << "Counter = " << counter << "\n"; 
}
```

### Expected output:
```
Counter = 200000
```

### Actual output (varies):
```
Counter = 143982
Counter = 178221
```
❌ Because both threads modify `counter` simultaneously.

---

# 4️⃣ Why It Breaks — CPU Instruction Level

`counter++` is **not atomic**:

```
Load counter
Add 1
Store counter
```

If threads interleave:
```
Thread A loads counter = 10
Thread B loads counter = 10
Thread A stores 11
Thread B stores 11   ❌ lost update
```

---

# 5️⃣ Fix Example — Using Mutex

```cpp
#include <mutex>

int counter = 0;
std::mutex m;

void increment() {
    for (int i = 0; i < 100000; i++) 
    {
        std::lock_guard<std::mutex> lock(m);
        counter++; // ✅ protected
    }
}
```

✔ Prevents simultaneous access  
✔ Ensures correct result  

---

# 6️⃣ Faster Fix — Atomic Variables

```cpp
#include <atomic>

std::atomic<int> counter(0);

void increment() 
{
    for (int i = 0; i < 100000; i++) 
    {
        counter++; // ✅ atomic
    }
}
```

✔ Lock‑free  
✔ Faster than mutex in many cases  

---

# 7️⃣ Common Race Condition Scenarios

| Scenario | Risk |
|--------|------|
| Updating shared counters | Wrong values |
| Writing logs | Mixed/corrupted logs |
| Modifying STL containers | Crashes |
| Banking/account balance | Financial errors |
| Game state updates | Broken logic |

---

# 8️⃣ How Developers Prevent Race Conditions

✔ Mutex / lock_guard  
✔ Atomic variables  
✔ Read‑write locks  
✔ Thread‑safe data structures  
✔ Minimize shared state  
✔ Immutable data patterns  
