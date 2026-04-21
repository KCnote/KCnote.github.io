---
layout: post
title: "03. DevOps about Test"
date: 2026-04-21 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - DevOps]
tags: [Deploy, Deploy - DevOps]
pin: false
math: true
mermaid: true
---

# <b>DevOps about Test</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `Test` in DevOps?</b>

The **Test phase** ensures that the built system behaves correctly, performs as expected, and is safe to deploy.

In DevOps, testing is not a single step.
It is a **continuous and automated validation process** integrated into the pipeline.

> Test is the phase where software is validated for correctness, performance, and reliability before deployment.

## <b>2. Why Testing is Critical in C++</b>

For C++ systems, testing is especially important because:

* no runtime safety (compared to managed languages)
* memory issues can crash the system
* undefined behavior can silently break logic
* performance regressions are common

#### ❌ Without proper testing

* crashes in production
* memory leaks
* incorrect results
* unstable long-running systems
* hidden performance degradation

#### ✔ With proper testing

* predictable behavior
* stable performance
* safe deployment
* easier debugging

## <b>3. Testing in System</b>

We are testing:

> **A C++ real-time pipeline**

This means testing must cover:

* correctness (output is valid)
* performance (latency, throughput)
* stability (long-running behavior)

## <b>4. Types of Testing</b>

#### <b>4-1. Unit Testing</b>

Test individual components in isolation

#### Example: Resize Function

```cpp
Frame Resize(const Frame& input);
```

Test:

* output dimensions correct
* no data corruption
* edge cases handled

##### ✔ Characteristics

* fast
* isolated
* deterministic

#### <b>4-2. Integration Testing</b>

Test interaction between modules

##### Example

```text
FrameSource → Preprocessor → Output Queue
```

Test:

* correct data flow
* no deadlocks
* proper synchronization

#### <b>4-3. Performance Testing (Critical for C++)</b>

Measure:

* latency per frame
* throughput (FPS)
* CPU usage

##### Example

```text
Target:
Latency < 10 ms
Throughput > 100 FPS
```

If performance fails, the system is not acceptable

#### <b>4-4. Stress / Load Testing</b>

Test system under heavy load

Examples:

* high frame rate input
* burst traffic
* large image sizes

Test for:

* system slowdown
* crashes
* queue overflow

#### <b>4-5. Stability / Soak Testing</b>

Run system for long periods

Example:

```text
Run for 24–72 hours continuously
```

Check:

* memory leaks
* performance degradation
* resource exhaustion

Critical for production systems

## <b>5. Test Automation in DevOps</b>

Testing must be automated.

#### CI Pipeline Example

```text
Code Push
   ↓
Build
   ↓
Run Unit Tests
   ↓
Run Integration Tests
   ↓
Report Results
```

If tests fail → pipeline stops

## <b>6. Test Examples</b>

#### Example: Google Test

```cpp
TEST(ResizeTest, OutputSize)
{
    Frame input{...};
    Frame output = Resize(input);

    EXPECT_EQ(output.width, expected_width);
    EXPECT_EQ(output.height, expected_height);
}
```

---

Easy to integrate into CI

#### Example: Performance Test

```cpp
auto start = std::chrono::high_resolution_clock::now();

for (int i = 0; i < 1000; i++)
{
    preprocessor.Process(input, output);
}

auto end = std::chrono::high_resolution_clock::now();
```

## <b>7. Common Mistakes</b>

#### ❌ Only testing correctness

→ ignores performance regression

#### ❌ Testing manually

→ not scalable, not reliable

#### ❌ No edge case testing

→ crashes in production

#### ❌ Ignoring long-running behavior

→ memory leaks appear later

#### ❌ Not integrating tests into CI

→ bugs slip into production
