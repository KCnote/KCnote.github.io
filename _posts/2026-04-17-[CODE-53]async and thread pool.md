---
layout: post
title: "std::async vs Thread Pool"
date: 2026-04-17 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>`std::async` and Thread Pool</b>

---

### <b>Prerequisites</b>

---

## <b>1. What are `std::async` and Thread Pool</b>

In C++, there are multiple ways to execute tasks concurrently:

- `std::async` → simple, task-based concurrency
- Thread Pool → reusable threads for high-performance workloads

```text
std::async = create a thread per task
thread pool = reuse a fixed set of threads
```

## <b>2. `std::async`</b>

```cpp
#include <future>
#include <iostream>

int work(int x)
{
    return x * x;
}

int main()
{
    auto f1 = std::async(std::launch::async, work, 10);
    auto f2 = std::async(std::launch::async, work, 20);

    std::cout << f1.get() << "\n";
    std::cout << f2.get() << "\n";
}
```

```text
- Each async call may create a new thread
- Task runs independently
- Result is returned via std::future
```

Use std::async when:

```text
- Few tasks
- Simplicity matters
- No need for high performance
```

##### Pros

- Easy to use
- Automatic thread management
- Built-in result handling (future)

##### Cons

- Thread creation overhead
- No reuse of threads
- Poor scalability for many tasks

## <b>3. Thread Pool</b>

```cpp
#include <iostream>
#include <thread>
#include <queue>
#include <vector>
#include <functional>
#include <mutex>
#include <condition_variable>

class ThreadPool
{
public:
    ThreadPool(size_t n) : stop(false)
    {
        auto lmdThread = [this]()
        {
            auto lmdcondition = [this]() -> bool
            {
                return stop || !tasks.empty();
            };

            while (true)
            {
                bool bFinish = false;

                do
                {
                    std::function<void()> task;

                    {
                        std::unique_lock<std::mutex> lock(m);
                        cv.wait(lock, lmdcondition);

                        if (stop && tasks.empty())
                        {
                            bFinish = true;
                            break;
                        }

                        task = std::move(tasks.front());
                        tasks.pop();
                    }

                    task();
                } 
                while (false);

                if(bFinish)
                    break;
            }
        };

        for (size_t i = 0; i < n; i++)
            workers.emplace_back(lmdThread);
    }

    ~ThreadPool()
    {
        {
            std::lock_guard<std::mutex> lock(m);
            stop = true;
        }

        cv.notify_all();

        for (auto& worker : workers)
        {
            if (worker.joinable())
                worker.join();
        }
    }

    void enqueue(std::function<void()> task)
    {
        {
            std::lock_guard<std::mutex> lock(m);
            tasks.push(std::move(task));
        }

        cv.notify_one();
    }

private:
    std::vector<std::thread> workers;
    std::queue<std::function<void()>> tasks;
    std::mutex m;
    std::condition_variable cv;
    bool stop;
};

int main()
{
    ThreadPool pool(2);

    pool.enqueue([] { std::cout << "Task 1\n"; });
    pool.enqueue([] { std::cout << "Task 2\n"; });
    pool.enqueue([] { std::cout << "Task 3\n"; });

    std::this_thread::sleep_for(std::chrono::seconds(1));
}
```

```text
- Threads are created once
- Tasks are queued
- Threads continuously fetch and execute tasks
```

Use Thread Pool when:

```text
- Many small tasks
- Performance is critical
- Reusing threads is important
```

##### Pros

- No repeated thread creation
- Better performance for many tasks
- Scales well

##### Cons

- More complex to implement
- Manual management required
- No built-in result handling (unless extended)

## <b>4. Performance</b>

| Aspect | std::async | Thread Pool |
|--------|------------|------------|
| Thread creation | Per task | Once |
| Reuse | ❌ | ✔️ |
| Overhead | High (many tasks) | Low |
| Scalability | Poor | Good |
| Ease of use | Easy | Moderate |

## <b>5. thread_local</b>

```text
std::async → thread recreated → thread_local reset
thread pool → thread reused → thread_local persists
```
