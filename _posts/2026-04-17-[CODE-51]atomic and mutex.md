---
layout: post
title: "std::atomic and std::mutex"
date: 2026-04-17 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::atomic` and `std::mutex`</b>

---

### <b>Prerequisites</b>

---

## <b>1.  Why Do We Need Synchronization</b>

In multithreaded programs, multiple threads access the same data

#### ❌ Problem: Data Race

```cpp
int counter = 0;

void increment()
    counter++;  // not safe
```

`counter++` is NOT atomic:

```text
read → modify → write
```

Threads may interfere → **undefined behavior**

#### Two Main Solutions

| Tool | Purpose |
|------|--------|
| `std::atomic` | lock-free synchronization |
| `std::mutex` | mutual exclusion (locking) |

## <b>2. `std::atomic`</b>

`std::atomic` ensures **operations are indivisible (atomic)**.

##### Example

```cpp
#include <atomic>

std::atomic<int> counter = 0;

void increment()
    counter++;  // thread-safe
```

```text
No data race → safe concurrent access
```

##### Memory Ordering (Advanced)

```cpp
std::atomic<int> x;

x.store(10, std::memory_order_relaxed);
```

- visibility
- ordering
- CPU optimization

```cpp
std::memory_order_seq_cst
```

strongest guarantee (safe but slower)

##### Limitations of `atomic`

- only for simple types (int, pointer, etc.)
- complex logic → hard to manage
- no blocking (busy-wait)

## <b>3. `std::mutex`</b>

A mutex ensures **only one thread accesses a critical section**

#### Example

```cpp
#include <mutex>

std::mutex m;
int counter = 0;

void increment()
{
    m.lock();
    counter++;
    m.unlock();
}
```
#### Better: RAII (Resource Acquisition Is Initialization)

```cpp
#include <mutex>

std::mutex m;

void increment()
{
    std::lock_guard<std::mutex> lock(m);
    counter++;
}
```

Automatically unlocks

A mutex does not lock the code itself, but rather controls access to shared resources. When a thread acquires a mutex, it is allowed to enter the critical section and perform its operations, while other threads must wait until the mutex is released. In other words, a mutex ensures that only one thread at a time can access and modify shared data safely, preventing concurrent execution that could lead to race conditions.

##### How `mutex` Works

```text
Thread A → lock → access → unlock
Thread B → wait → lock → access
```

Guarantees **mutual exclusion**

## <b>4. Performance Comparison</b>

| Feature | `atomic` | `mutex` |
|--------|---------|--------|
| Speed | 🔥 fast | 🐢 slower |
| Blocking | ❌ no | ✅ yes |
| Complexity | low | high |
| Use case | simple data | complex logic |

The cost difference between atomic operations and mutexes comes from the level at which they operate.

A normal variable operation is just a simple CPU instruction that reads or writes a value in a register or cache, so it has almost no overhead. 

An **atomic** operation, however, must guarantee that multiple threads can safely access the same variable without corruption. To achieve this, it uses special CPU-level instructions (such as lock-prefixed operations) and enforces cache coherence across cores. This coordination introduces some overhead, making atomic operations slightly slower than regular variable access.

A **mutex**, on the other hand, is significantly more expensive because it involves the operating system. When a thread acquires a mutex, other threads trying to acquire it may have to wait, potentially being put to sleep and later woken up. This process involves context switching and thread scheduling, which are far more costly than CPU instructions.

In short, the difference in cost comes from whether synchronization is handled at the CPU level (atomic) or requires OS-level scheduling and blocking (mutex).

##### Compare process speed whether including atomic or not

```
[normal int]
result : 100000000
time   : 9.9 ms

[atomic int]
result : 100000000
time   : 64.9 ms
```

- The best is not using atomic when we don't need it

##### Use `atomic` when:

- simple counters
- flags
- lightweight synchronization
- high performance required

##### Use `mutex` when:

- multiple variables
- complex logic
- critical sections
- need blocking behavior

## <b>5. Advanced</b>

```cpp
std::scoped_lock lock(m1, m2);
```

same as:

```cpp
std::lock(m1, m2);

std::lock_guard<std::mutex> l1(m1, std::adopt_lock);
std::lock_guard<std::mutex> l2(m2, std::adopt_lock);
```

avoids deadlock automatically