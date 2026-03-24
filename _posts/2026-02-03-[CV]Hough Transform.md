---
layout: post
title: "Hough Transform"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Image Processing]
tags: [Computer Vision, Image Processing, Hough Transform, Line Detection, Feature Extraction, Geometry]
pin: false
math: true
mermaid: true
---

## 🎯 What Is the Hough Transform?

The **Hough Transform** is a technique to detect **parametric shapes** (most commonly lines) in images.

It is especially powerful when:
- Edges are noisy
- Lines are broken or discontinuous
- You want robustness over precision

Classic use cases:
- Lane detection
- Industrial inspection (straight edges)
- Document analysis

---

## 🧱 Problem with Line Detection in Image Space

A line in image space is:

$$
y = mx + b
$$

Problems:
- Vertical lines → infinite slope
- Noise breaks continuity
- Hard to vote consistently

---

## 🔁 Key Idea: Dual Space (Parameter Space)

Instead of detecting lines in **image space**, we detect them in **parameter space**.

Use the **normal form** of a line:

$$
\rho = x \cos\theta + y \sin\theta
$$

Where:
- distance from origin $$\rho$$
- angle of normal $$\theta$$

This avoids infinite slope issues.

---

## 🔄 Image Point → Curve in Hough Space

A **single edge point** \((x,y)\) maps to a **sinusoidal curve** in \((\rho, \theta)\) space:

$$
\rho(\theta) = x \cos\theta + y \sin\theta
$$

Key insight:
> Points lying on the same line generate curves that **intersect at one point** in Hough space.

---

## 🗳️ Step-by-Step Algorithm

### Step 1: Edge Detection
Apply an edge detector (e.g. Canny).

Only edge pixels vote.

---

### Step 2: Initialize Accumulator

Create a 2D accumulator array:

$$
A(\rho, \theta)
$$

-  discretized (e.g. 0°–180°) $$\theta$$
-  discretized based on image diagonal $$\rho$$

---

### Step 3: Voting

For each edge pixel \((x,y)\):

For each \(\theta\):

$$
\rho = x \cos\theta + y \sin\theta
$$

Increment:

$$
A(\rho, \theta) += 1
$$

---

### Step 4: Peak Detection

Local maxima in accumulator correspond to **detected lines**.

---

## 🔢 Simple Numerical Example

Edge point:
```
(x, y) = (10, 20)
```

For:
```
θ = 0° → ρ = 10
θ = 90° → ρ = 20
```

This point votes along a curve in Hough space.

Multiple points on the same line vote for the **same (ρ, θ)**.

---

## 📐 From Hough Space Back to Image Space

Detected parameters \((\rho, \theta)\) map back to line equation:

$$
x \cos\theta + y \sin\theta = \rho
$$

This line can be drawn back in image space.

---

## ⚠️ Practical Considerations

### Resolution
- Finer bins → more precision
- Coarser bins → more robustness

Trade-off required.

---

### Thresholding
Only accept accumulator values above a threshold.

---

### Complexity
If:
- number of edge points $$N$$
- number of angle bins $$T$$

Complexity:

$$
O(N \cdot T)
$$

---

## 🧠 Probabilistic Hough Transform

Optimizations:
- Randomly sample edge points
- Vote less, detect faster

Used in real-time systems.

---

## 🧩 Extensions

- **Circle Hough Transform**
- **Ellipse Hough Transform**
- Generalized Hough Transform

---

## ⚖️ Comparison with LSM

| Aspect | Hough | Least Squares |
|------|------|---------------|
| Robust to gaps | ✅ | ❌ |
| Noise tolerance | ✅ | ❌ |
| Precision | ❌ | ✅ |
| Computational cost | ❌ | ✅ |

---

## 🎯 Takeaway

Hough Transform:
- Trades precision for robustness
- Detects global structures
- Works where local fitting fails

It is a **voting-based geometric detector**, not a curve fitter.

Understanding it completes the picture with:
- Thresholding
- LSM
- Homography
- Geometry pipelines 🚀
