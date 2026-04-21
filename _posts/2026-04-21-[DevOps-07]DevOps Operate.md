---
layout: post
title: "07. DevOps about Operate"
date: 2026-04-21 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - DevOps]
tags: [Deploy, Deploy - DevOps]
pin: false
math: true
mermaid: true
---

# <b>DevOps about Operate</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `Operate` in DevOps?</b>

The **Operate phase** is where the deployed system is actually run, maintained, and kept stable in production.

Deployment is not the end. Operate is about what happens **after the system is live**.

> Operate is the phase where a deployed system is continuously run, managed, and maintained to ensure stability, performance, and reliability.

## <b>2. Why Operate Matters</b>

A system can:

* build successfully
* pass all tests
* deploy correctly

and still fail in production

#### ❌ Without proper operation

* system crashes remain unnoticed
* performance gradually degrades
* memory leaks accumulate
* queues overflow silently
* users experience failures

#### ✔ With proper operation

* issues are detected early
* performance remains stable
* failures are handled quickly
* system runs continuously (24/7)

## <b>3. Goals of the Operate Phase</b>

The Operate phase ensures:

1. the system keeps running without interruption
2. performance remains within target limits
3. failures are detected and handled quickly
4. resources are used efficiently
5. the system can recover when issues occur

## <b>4. What Happens During Operation?</b>

#### <b>4-1. Process Management</b>

The system must:

* start correctly
* restart automatically if it crashes
* run continuously

##### Example

```text
Process starts → runs loop → crash → auto-restart
```

Tools (examples):

* systemd (Linux)
* Docker restart policies

#### <b>4-2. Runtime Monitoring (Basic Level)</b>

Even before full monitoring systems, the application should:

* log important events
* report errors
* expose basic metrics

Example logs:

```text
[INFO] Frame processed in 5ms
[WARN] Queue size increasing
[ERROR] Frame dropped
```

#### <b>4-3. Resource Management</b>

The system must not exhaust resources.

Monitor:

* CPU usage
* memory usage
* thread count
* queue size

##### ❌ Example problem

```text
Queue size keeps increasing → memory grows → crash
```

#### <b>4-4. Failure Handling</b>

Failures will happen.

The system must:

* handle errors gracefully
* avoid crashing when possible
* recover automatically

##### Example

```text
Frame processing fails → skip frame → continue
```

Never stop the whole system for one bad frame

#### <b>4-5. Continuous Operation (24/7)</b>

Unlike test environments, production systems:

* run indefinitely
* must handle long-term stability

This connects directly to:

* soak testing
* memory leak prevention
* resource cleanup

#### <b>4-6. Key Concepts</b>

##### ✔ Idempotency (important)

Running the same operation multiple times should not break the system.

##### ✔ Fault tolerance

The system should continue running even when parts fail.

##### ✔ Backpressure

If input is faster than processing:

* slow down input
* drop frames
* limit queue size

## <b>5. Real Example: Image Pipeline Operation</b>

```text
Camera
   ↓
Frame Queue
   ↓
Preprocessor (C++)
   ↓
Output
```

#### During operation, you must ensure:

* queue does not overflow
* processing stays within latency limits
* system recovers from temporary failures
* CPU usage remains stable

## <b>6. Common Problems in Operation</b>

#### ❌ Memory leaks

→ system crashes after hours or days

#### ❌ Performance drift

→ latency increases over time

##### ❌ Deadlocks

→ system freezes

##### ❌ Resource exhaustion

→ no memory / threads left

##### ❌ Silent failures

→ system runs but produces wrong output

## <b>7. Restart & Recovery Strategy</b>

Operation must include recovery.

#### Example strategies

* auto-restart process
* watchdog monitoring
* fallback modes
* restart pipeline stage only

```text
Crash → restart → resume processing
```

No manual intervention required

## <b>8. Automation in Operation</b>

Manual operation is risky.
Automation should handle:

* process restart
* log collection
* health checks
* scaling (if needed)

Example:

```text
Container crashes → auto-restart
```