---
layout: post
title: "02. DevOps about Plan"
date: 2026-04-21 00:00:00 +0900
author: kang
categories: [Deploy, Deploy - DevOps]
tags: [Deploy, Deploy - DevOps]
pin: false
math: true
mermaid: true
---

# <b>DevOps about Plan</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is `Plan` in DevOps?</b>

The **Plan phase** is where everything starts.

Before writing a single line of code, you define:

* What problem you are solving
* What the system should do
* How performance will be measured
* What constraints exist

> Plan is the phase where requirements, constraints, and system goals are clearly defined before implementation begins.

## <b>2. Why Planning Matters (Especially for C++ Systems)</b>

In high-performance systems (like C++ real-time processing),
**bad planning = expensive rework**

#### ❌ Without Planning

* Wrong architecture → rewrite needed
* Latency too high → redesign pipeline
* Memory issues → major refactor
* Not scalable → system collapse under load

##### ✔ With Proper Planning

* Clear performance targets
* Predictable system behavior
* Easier optimization later
* Faster development cycle overall

##### Example

Let’s assume we are building:

> **A C++ image preprocessing pipeline for real-time industrial inspection**

## <b>4. Step-by-Step Planning Breakdown</b>

#### <b>4-1. Define the Problem</b>

What are we solving?

* Input: raw camera frames
* Output: processed images for downstream vision algorithms

#### <b>4-2. Define Requirements</b>

#### Functional Requirements

* Resize images
* Apply filtering (denoise, normalize)
* ROI extraction

#### Non-Functional Requirements (Critical)

* Latency: **< 10 ms per frame**
* Throughput: **> 100 FPS**
* Stability: **24/7 operation**
* Accuracy: preprocessing must not degrade detection quality

In C++ systems, **non-functional requirements are often more important**

#### <b>4-3. Define Constraints</b>

* Hardware:

  * CPU only? GPU allowed?

* Memory limits:

  * Can we buffer frames?

* Environment:

  * Embedded system vs server

Example:

* CPU: 8 cores
* No GPU
* Limited memory (512MB)

#### <b>4-4. Define System Architecture (High-Level)</b>

Design the pipeline before coding:

```text
Camera Input
   ↓
Frame Queue
   ↓
Preprocessing Stage (parallel)
   ↓
Output Queue
   ↓
Consumer (Vision Algorithm)
```

#### <b>4-5. Identify Performance Bottlenecks Early</b>

Ask:

* Where will time be spent?
* Where can parallelism be applied?

Example:

| Stage      | Risk                        |
| ---------- | --------------------------- |
| Image copy | memory bandwidth bottleneck |
| Filtering  | CPU-heavy                   |
| Queue sync | lock contention             |

#### <b>4-6. Choose Optimization Strategy</b>

Based on planning:

* SIMD (AVX/SSE) for pixel operations
* Multithreading (per-frame parallelism)
* Cache-friendly memory layout (SoA vs AoS)

#### <b>4-7. Define Metrics & Observability</b>

Before coding, decide how to measure success:

* Latency per frame
* Throughput (FPS)
* CPU usage
* Memory usage

#### <b>4-8. Define Build & Dependency Strategy</b>

Even in planning, DevOps thinking starts:

* Build system → CMake
* Dependencies → vcpkg
* Target platform → Linux / Docker

## <b>5. Output of the Plan Phase</b>

At the end of planning, you should have:

* Clear system requirements
* Defined architecture
* Performance targets
* Identified risks
* Tooling decisions

## <b>6. Common Mistakes</b>

##### ❌ Jumping into coding too early
##### ❌ Ignoring non-functional requirements
##### ❌ No performance targets
##### ❌ No architecture definition
