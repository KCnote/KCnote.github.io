---
layout: post
title: "Thread"
date: 2026-01-29 00:00:00 +0900
author: kang
categories: [OS, Parallel]
tags: [OS, Thread, MultiThread, MultiProcessing, Parallel]
pin: false
math: true
mermaid: true

---

# 🧠 Threads — Lightweight Execution Units in a Process

> A developer‑oriented guide to **what threads are**, how they relate to processes,  
> what they **share**, what they **own**, and why they improve performance (and introduce risks).

---

# 1️⃣ What Is a Thread?

A **thread** is the **smallest unit of execution** inside a process.

> A single process can contain **one or multiple threads**, all running concurrently.

### Key Idea
- **Process = resource container**
- **Thread = execution flow inside the container**

---

# 2️⃣ Thread vs Process — Core Difference

| Aspect | Process | Thread |
|--------|--------|--------|
| Memory Space | Separate | Shared within process |
| Creation Cost | High | Low |
| Context Switch | Heavy | Lightweight |
| Communication | IPC needed | Shared memory |
| Failure Impact | Isolated | Can crash whole process |

---

# 3️⃣ Thread Components (Per‑Thread State)

Each thread maintains its **own execution state**:

✔ Thread ID (TID)  
✔ Program Counter (PC)  
✔ CPU Registers  
✔ Stack (local variables & call frames)

> These are the **minimum resources needed to execute code independently**.

---

# 4️⃣ Shared vs Private Resources

Threads **share process resources**, but also keep **private execution state**.

---

## 🔹 Shared Among Threads

✔ Process Control Block (PCB)  
✔ Code Segment  
✔ Data Segment  
✔ Heap  
✔ Open Files & File Descriptors  
✔ Memory Address Space  

👉 Enables **fast communication & cooperation**

---

## 🔹 Private Per Thread

✔ Program Counter  
✔ Registers  
✔ Stack  
✔ Thread ID  

👉 Enables **independent execution**

---

# 5️⃣ Why Sharing Is Powerful (and Dangerous)

### Advantages
✔ Fast communication (no IPC overhead)  
✔ Efficient memory usage  
✔ Better CPU utilization  

### Risks
❌ Race conditions  
❌ Data corruption  
❌ Deadlocks  
❌ Harder debugging  

> Threads make programs **faster but more complex**.

---

# 6️⃣ Common Thread Problems

## 🔹 Race Condition
Multiple threads modify shared data **at the same time**.

## 🔹 Data Inconsistency
Thread reads partially updated data.

## 🔹 Deadlock
Threads wait on each other forever.

## 🔹 Stack Corruption
Improper memory access across stacks.

---

# 7️⃣ Thread Scheduling

- OS schedules **threads**, not just processes
- Threads compete for CPU time
- Context switching happens **between threads**

> Modern schedulers treat threads as **first‑class execution units**.

---

# 8️⃣ Real‑World Examples

### Web Browser
- UI thread
- Network thread
- Rendering thread

### Game Engine
- Physics thread
- AI thread
- Rendering thread

### Server
- Worker thread pool

---

# 9️⃣ Developer Takeaways

✔ Threads share memory → fast communication  
✔ Each thread has its own registers & stack  
✔ Bugs in one thread can affect the whole process  
✔ Concurrency needs synchronization (mutex, semaphore)  

---

# 📌 Suggested Blog Title

### **Threads Explained — Shared Memory, Execution State, and Concurrency Risks**

---

# 🔟 C++ Thread Examples (std::thread)

> Below examples use the C++ standard library (`<thread>`, `<mutex>`, `<future>`).

## 10.1 Minimal Example — Start & Join Threads

```cpp
#include <iostream>
#include <thread>

void worker(int id) 
{
    std::cout << "worker " << id << "\n";
}

int main() {
    std::thread t1(worker, 1);
    std::thread t2(worker, 2);

    t1.join();  // wait until t1 finishes
    t2.join();  // wait until t2 finishes
}
```

**Key points**
- `std::thread` starts running immediately after construction.
- `join()` is required (or `detach()`), otherwise `std::terminate()` may happen at program exit.

---

## 10.2 Shared Data Example — Why Mutex Is Needed

```cpp
#include <iostream>
#include <thread>
#include <mutex>

int counter = 0;
std::mutex m;

void inc(int times) {
    for (int i = 0; i < times; ++i) 
    {
        if(0)
        {
            std::lock_guard<std::mutex> lock(m);
            ++counter;
        }
        else
        {
            m.lock();
            ++counter;
            m.unlock();
        }
    }
}

int main() 
{
    std::thread t1(inc, 100000);
    std::thread t2(inc, 100000);

    t1.join();
    t2.join();

    std::cout << "counter = " << counter << "\n";
}
```

Without the mutex, `counter` can be wrong due to a **race condition**.

---

# 1️⃣1️⃣ From Serial to Parallel — Practical Patterns

## 11.1 “Looks Serial” but Actually Parallelizable (Independent Tasks)

### Serial version
```cpp
auto a = taskA();
auto b = taskB();
auto c = taskC();
use(a, b, c);
```

### Parallel version (std::async)
```cpp
#include <future>

auto fa = std::async(std::launch::async, taskA);
auto fb = std::async(std::launch::async, taskB);
auto fc = std::async(std::launch::async, taskC);

auto a = fa.get();
auto b = fb.get();
auto c = fc.get();
use(a, b, c);
```

✅ Works when `taskA/B/C` do not depend on each other.

---

## 11.2 Harder Case — “Serial Structure” (Pipeline with Dependencies)

Some problems are naturally **stage-based** (output of stage 1 becomes input of stage 2):

```
Read → Decode → Process → Write
```

### Serial version
```cpp
for (auto item : items)
{
    auto a = read(item);
    auto b = decode(a);
    auto c = process(b);
    write(c);
}
```

### Pipeline parallelism idea
Instead of parallelizing *within one item*, you run **different stages on different threads** so multiple items are in-flight:

```
Thread 1: Read    item1, item2, item3...
Thread 2: Decode  item1, item2, item3...
Thread 3: Process item1, item2, item3...
Thread 4: Write   item1, item2, item3...
```

A common way is **producer–consumer queues** between stages:

- Stage 1 pushes to `Q1`
- Stage 2 pops from `Q1`, pushes to `Q2`
- Stage 3 pops from `Q2`, pushes to `Q3`
- Stage 4 pops from `Q3`

### Minimal pipeline sketch (conceptual)
```cpp
// Pseudocode (focus on structure, not full implementation):
BlockingQueue<Raw>    q1;
BlockingQueue<Decoded> q2;
BlockingQueue<Result>  q3;

thread read_thread([&]{ while (...) q1.push(read(...)); });
thread decode_thread([&]{ while (...) q2.push(decode(q1.pop())); });
thread process_thread([&]{ while (...) q3.push(process(q2.pop())); });
thread write_thread([&]{ while (...) write(q3.pop()); });
```

✅ This helps when each stage is significant work and items are numerous.  
⚠️ Requires careful shutdown signaling (sentinels) and backpressure handling.

---

## 11.3 Rule of Thumb: When Parallelism Helps

✅ Good candidates
- Many independent jobs
- CPU-heavy loops with little shared state
- I/O waiting (network/disk) where threads can overlap latency

❌ Poor candidates
- Tiny tasks (thread overhead dominates)
- Heavy shared-state contention (locks everywhere)
- Strictly ordered algorithms where each step depends on the previous result

---

# ✅ Quick Takeaways

- Use `std::thread` for explicit threads, but always **join/detach**.
- Use `std::async` for “run tasks in parallel and get results” patterns.
- For serial-looking pipelines, consider **pipeline parallelism** with queues.
- Correctness first: shared state requires synchronization.
