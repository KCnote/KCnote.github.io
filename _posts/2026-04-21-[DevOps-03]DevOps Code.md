---
layout: post
title: "03. DevOps about Code"
date: 2026-04-21 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - DevOps]
tags: [Deploy, Deploy - DevOps]
pin: false
math: true
mermaid: true
---

# <b>DevOps about Code</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `Code` in DevOps?</b>

The **Code phase** is where the planned system starts becoming real.

This is not just about writing something that works.
It is about writing software that is:

* correct
* maintainable
* testable
* performant
* ready to move through the next stages of the pipeline

> Code is the phase where requirements are translated into reliable, maintainable, and production-ready source code.

## <b>2. Why the Code Phase Matters</b>

In DevOps, code is not isolated from the rest of the lifecycle.

Bad code does not only create bugs.
It also creates problems for:

* build stability
* testing
* deployment
* operations
* monitoring

##### ❌ Poor coding leads to:

* unclear ownership
* hard-to-test modules
* fragile builds
* hidden performance issues
* difficult debugging in production

##### ✔ Good coding leads to:

* cleaner builds
* easier testing
* safer refactoring
* predictable performance
* faster delivery

## <b>3. Goals of the Code Phase</b>

When writing code for this project, the goals are:

1. implement the required functionality
2. preserve low latency
3. minimize unnecessary memory operations
4. keep modules testable and replaceable
5. make the system safe for long-running production use

## <b>4. Core Principles in the Code Phase</b>

#### <b>4-1. Write code that matches the architecture</b>

Do not start by writing random helper functions.
Follow the pipeline defined in the planning phase.

For example:

```text
Input Frame
   ↓
Preprocessor
   ├─ Resize
   ├─ Normalize
   ├─ Filter
   └─ ROI Extract
   ↓
Output Frame
```

Each stage should have a clear responsibility.

#### <b>4-2. Separate responsibilities clearly</b>

A preprocessing system becomes difficult to maintain when one class does everything.

For example:

* frame acquisition → separate module
* preprocessing logic → separate module
* queue handling → separate module
* metrics/logging → separate module

##### ❌ Bad idea

```cpp
class ImageProcessor
{
public:
    void CaptureResizeFilterLogQueueAndSend();
};
```

Too many responsibilities are mixed together.

##### ✔ Better idea

```cpp
class FrameSource;
class Preprocessor;
class FrameQueue;
class MetricsCollector;
```

This makes the system easier to test, optimize, and replace.

#### <b>4-3. Prefer explicit data flow</b>

In performance-sensitive C++ systems, hidden behavior is dangerous.

Avoid:

* unnecessary copies
* unclear ownership
* implicit global state
* surprise allocations

##### ✔ Good practice

* pass large buffers by reference or view
* define ownership clearly
* avoid global mutable state
* keep memory lifetime predictable

## <b>6. Example Design for the Preprocessing Module</b>

A simple interface might look like this:

```cpp
struct Frame
{
    std::vector<uint8_t> pixels;
    int width;
    int height;
    int channels;
};

class Preprocessor
{
public:
    Frame Process(const Frame& input);

private:
    Frame Resize(const Frame& input);
    void Normalize(Frame& frame);
    void Filter(Frame& frame);
    Frame ExtractROI(const Frame& input);
};
```

This is easy to understand, but it may not yet be optimal.

## <b>7. Coding with Performance in Mind</b>

For real-time C++ systems, the code phase must already consider optimization.

Not premature micro-optimization,
but **structural performance awareness**.

#### <b>7-1. Minimize copies</b>

Image data is large.
Careless copying can destroy throughput.

##### Risky pattern

```cpp
Frame Resize(Frame input);
```

This may copy the whole frame.

##### ✔ Better pattern

```cpp
Frame Resize(const Frame& input);
```

Or use preallocated output buffers when possible.

#### <b>7-2. Avoid repeated allocations</b>

Allocating memory every frame can become expensive in real-time systems.

Instead of:

* allocating temporary buffers inside tight loops
* resizing vectors repeatedly
* constructing heavy objects per frame

Prefer:

* preallocated buffers
* object reuse
* fixed-capacity structures when possible

#### <b>7-3. Keep hot paths simple</b>

The critical frame-processing path should be easy for both the CPU and the engineer.

Avoid putting too much into the hot loop:

* logging
* dynamic dispatch if unnecessary
* complex branching
* unrelated bookkeeping

#### <b>7-4. Write code that can be optimized later</b>

If SIMD or multithreading may be added later, the code structure should allow it.

For example:

* isolate pixel operations
* keep data layout consistent
* avoid tightly coupling stages
* make work units parallelizable

##### Example: A More Optimization-Friendly Structure

```cpp
class Preprocessor
{
public:
    bool Process(const Frame& input, Frame& output);

private:
    bool Resize(const Frame& input, Frame& output);
    bool Normalize(Frame& frame);
    bool Filter(Frame& frame);
};
```

Why is this better?

* avoids unnecessary return-value objects
* allows buffer reuse
* makes ownership clearer
* fits better with real-time pipelines

#### <b>8. Code Quality in DevOps Means More Than Syntax</b>

In DevOps, “good code” includes more than clean formatting.

It should also support:

* automated builds
* unit testing
* code review
* CI pipelines
* production debugging

#### <b>9. Common Mistakes</b>

##### ❌ Writing without module boundaries

Leads to tightly coupled code

##### ❌ Mixing business logic with infrastructure logic

Makes testing and maintenance difficult

##### ❌ Ignoring memory behavior

Causes performance regression

##### ❌ Overengineering too early

Makes the code hard to deliver and maintain

##### ❌ Optimizing blindly

Can waste time without solving the real bottleneck
