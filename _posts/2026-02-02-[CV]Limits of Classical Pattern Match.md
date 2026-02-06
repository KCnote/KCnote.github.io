---
layout: post
title: "Limits of Classical Pattern Match"
date: 2026-02-02 00:00:00 +0900
author: kang
categories: [Computer Vision, Insights]
tags: [Computer Vision, Insights, Pattern Match, NCC, Limitations, Performance]
pin: false
math: true
mermaid: true

---

> ⚠️ **Engineering Reality Check**  
> Classical pattern matching works beautifully —  
> **until it doesn’t.**
>
> This post focuses not on *how to implement* pattern matching,  
> but on *why it fundamentally struggles* in real vision systems.

---

## 🔍 What Classical Pattern Matching Assumes

Traditional pattern matching (e.g. **NCC-based matching**) operates under strong assumptions:

- 🟦 All pixels are compared in **absolute coordinates**
- 🧮 Similarity is computed via **pixel-wise correlation**
- 🧠 A high score implies *strong visual similarity*

When conditions are ideal, NCC scores are indeed meaningful.

But these assumptions come at a cost.

---

## 💥 The Core Limitation: Explosive Time Complexity

### 🧱 Problem Setup

Let:
- Template size: `w × h`
- Search image size: `W × H`

### 📍 Number of Positions

```
(W − w) × (H − h)
```

### 🧮 Cost Per Position

Each candidate requires:
```
w × h  pixel comparisons
```

### ❌ Total Cost

```
((W − w) × (H − h)) × (w × h)
```

Even **before rotation**, this quickly becomes impractical.

---

## 🔄 The Fatal Case: Rotation

Real-world objects are rarely axis-aligned.

If rotation is considered:
- Full 360° search
- Even coarse 0.1° steps → **3600 orientations**

### ☠️ Full Complexity

```
((W − w) × (H − h)) × (w × h) × 3600
```

> 🚨 This is not an optimisation problem.  
> It is a **combinatorial explosion**.

No amount of low-level tuning can save this approach.

---

## ⚠️ Secondary Limitations

### 🌫️ Noise Sensitivity

NCC assumes:
- stable illumination
- low sensor noise
- pixel consistency

In practice:
- noise decorrelates pixels
- similarity scores collapse rapidly

Small noise → large score degradation.

---

### 🌍 Environmental Dependency

Pattern matching cannot separate:
- the **object**
- its **surroundings**

Background changes directly affect similarity:
- same object, different context → lower score
- false confidence from similar backgrounds

---

### 🎭 Overconfidence in Pixel Similarity

A high NCC score only means:
> “These pixels look alike.”

It does **not guarantee**:
- geometric correctness
- semantic correctness
- functional equivalence

This limits trust in uncontrolled environments.

---

## 🧠 Why These Are Structural Problems

These failures are not bugs.

They arise because classical pattern matching:

- operates in raw pixel space
- lacks invariance (rotation, scale, illumination)
- treats all pixels as equally important

As a result:
> **Robustness and speed are fundamentally at odds.**

---

## 🚀 What High-Performance Systems Must Do Instead

Real systems introduce additional layers of reasoning.

### 🧭 Exploit Score Locality

Good matches are rare and clustered:
- reject most positions early
- evaluate expensive scores locally

---

### 🧩 Break Pixel Uniformity

Use more meaningful representations:
- edges
- gradients
- shapes

Not all pixels deserve equal weight.

---

### 🔄 Design Invariance Explicitly

Rotation and scale must be handled by design:
- canonical alignment
- geometric constraints
- multi-stage matching

Not brute-force enumeration.

---

### ⚖️ Embrace Approximation

Perfect similarity is unnecessary.

Practical systems trade:
- exactness → robustness
- exhaustiveness → determinism

This is a **system-level choice**, not a mathematical flaw.

---

## 💡 A Computer Vision Takeaway

> Classical pattern matching is not wrong —  
> it is **too literal**.

It faithfully compares pixels,  
while real-world vision requires:
- abstraction
- interpretation
- controlled approximation

Understanding *why* it fails is the first step toward building systems that work.

---

## ✅ Summary

- 🟥 NCC-based pattern matching scales poorly
- 🔄 Rotation makes brute-force search infeasible
- 🌫️ Noise and context changes undermine reliability
- 🧠 Limitations are structural, not accidental
- 🚀 Practical systems must exploit locality, abstraction, and invariance

---

✨ *Good vision systems are designed, not brute-forced.*
