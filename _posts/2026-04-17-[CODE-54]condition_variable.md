---
layout: post
title: "std::condition_variable"
date: 2026-04-18 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::condition_variable`</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `condition_variable`?</b>

A `std::condition_variable` is a synchronization primitive that allows threads to:

- wait until a condition becomes true
- be notified by another thread

**condition_variable lets threads sleep efficiently and wake up when work is available.**

## <b>2. Why Do We Need It?</b>

#### ❌ Bad Approach (Busy Waiting)

```cpp
while (queue.empty()) {}
```

```text
- CPU usage is high
- Wasteful spinning
```

#### ✔️ Better Approach

```cpp
cv.wait(lock, condition);
```

```text
- Thread sleeps (no CPU usage)
- Wakes only when needed
```

#### Basic Concept

```text
Producer → produces data
Consumer → waits for data
condition_variable → notifies consumer
```

```text
cv.wait() → sleep
while loop → spin
```

## <b>3. Example</b>

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>

std::mutex m;
std::condition_variable cv;
std::queue<int> q;

void producer()
{
    {
        std::lock_guard<std::mutex> lock(m);
        q.push(42);
    }

    cv.notify_one();  // wake one waiting thread
}

void consumer()
{
    std::unique_lock<std::mutex> lock(m);

    auto lmdCondition = []() { return !q.empty(); };

    cv.wait(lock, lmdCondition);

    int value = q.front();
    q.pop();

    std::cout << value << std::endl;
}

int main()
{
    std::thread t1(consumer);
    std::thread t2(producer);

    t1.join();
    t2.join();
}
```

#### How wait() Works Internally

```text
1. lock is released
2. thread goes to sleep
3. notify_one() wakes it
4. lock is reacquired
5. condition is checked again
```

#### Important Rule (Predicate)

Always use a condition:

```cpp
cv.wait(lock, [] { return !q.empty(); });
```

```text
- Avoid spurious wakeups
- Ensure condition is actually satisfied
```

#### notify_one vs notify_all

| Function | Description |
|----------|------------|
| notify_one | wakes one thread |
| notify_all | wakes all waiting threads |

```cpp
cv.notify_one(); // single worker
cv.notify_all(); // broadcast
```

#### ✔️ Not a Lock

```text
condition_variable does NOT protect data
→ mutex is required
```

#### ✔️ Used with mutex

```cpp
std::unique_lock<std::mutex> lock(m);
cv.wait(lock, condition);
```

#### ✔️ Event-driven

```text
Thread sleeps → wakes on notification
```

## <b>4. Common Mistakes</b>

#### ❌ Missing predicate

```cpp
cv.wait(lock);  // dangerous
```

#### ❌ Holding lock too long

```cpp
lock → long computation → unlock ❌
```

##### Bad Example

```cpp
std::lock_guard<std::mutex> lock(m);
shared_data = prepare_input();
long_computation();   // too long
shared_result = finalize();
```

##### Better Example

```cpp
Better Example
int local_copy;
{
    std::lock_guard<std::mutex> lock(m);
    local_copy = shared_data;
}

long_computation(local_copy); // No lock

{
    std::lock_guard<std::mutex> lock(m);
    shared_result = local_copy;
}
```

## ❌ Forget notify

```text
thread sleeps forever
```


