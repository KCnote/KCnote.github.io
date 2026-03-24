---
layout: post
title: "Hough Line Transform with example"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Image Processing]
tags: [Computer Vision, Image Processing, Hough Transform, Line Detection, Accumulator, Voting]
pin: false
math: true
mermaid: true
---

## 🎯 Goal

This post shows a **real Hough voting case where votes actually accumulate**  
and a line is detected — not just theoretical curves.

We will:
1. Choose points that lie **exactly on one line**
2. Use a **specific θ**
3. Show **accumulator peak formation**

---

## 📐 Target Line (Ground Truth)

Assume the true line is:

$$
y = x
$$

This line can be written in Hough normal form.

For a line with slope 1:
- Normal angle $$\theta = 135^\circ$$
- Because the normal is perpendicular to the line

Recall:
$$
\rho = x \cos\theta + y \sin\theta
$$

---

## 🧱 Step 1: Edge Points on the Line

Choose 3 exact edge points:

| Point | x | y |
|-----|---|---|
| P1 | 1 | 1 |
| P2 | 2 | 2 |
| P3 | 3 | 3 |

All points lie on $$y=x$$.

---

## 🧮 Step 2: Fix θ = 135°

Compute constants:

$$
\cos 135^\circ = -0.707
$$

$$
\sin 135^\circ = 0.707
$$

---

## 🗳️ Step 3: Compute ρ for Each Point

### ▶ Point P1 = (1,1)

$$
\rho_1 = 1(-0.707) + 1(0.707) = 0
$$

---

### ▶ Point P2 = (2,2)

$$
\rho_2 = 2(-0.707) + 2(0.707) = 0
$$

---

### ▶ Point P3 = (3,3)

$$
\rho_3 = 3(-0.707) + 3(0.707) = 0
$$

---

## ✅ Observation

All points produce **the same (ρ, θ)**:

$$
(\rho, \theta) = (0, 135^\circ)
$$

This is the **accumulator peak**.

---

## 📊 Step 4: Accumulator Table

| θ (deg) | ρ | Votes |
|--------|---|------|
| 135 | 0 | 3 ✅ |
| others | * | ≤1 |

Define detection threshold:
```
votes ≥ 3
```

→ **Line detected**.

---

## 🔄 Step 5: Recover the Line

Detected parameters:

$$
x \cos\theta + y \sin\theta = \rho
$$

Substitute values:

$$
-0.707x + 0.707y = 0
$$

Divide both sides by 0.707:

$$
y = x
$$

✔ Original line recovered.

---

## 🧠 Why Voting Works (Key Insight)

- Each point votes for all possible lines through it
- Collinear points vote for the **same parameter**
- Noise-free case → perfect intersection
- Noisy case → cluster of votes

This is why **Hough is robust to gaps and noise**.

---

## ⚠️ What Happens with Discretization

If:
- resolution is coarse $$\theta$$
- bins are wide $$\rho$$

Then:
- Votes still accumulate nearby
- Peak becomes a **cluster**, not a single bin

---

## ⚖️ Compare with LSM

| Aspect | Hough | Least Squares |
|------|------|---------------|
| Needs full line | ❌ | ✅ |
| Robust to gaps | ✅ | ❌ |
| Uses voting | ✅ | ❌ |
| Uses fitting | ❌ | ✅ |

---

## 🎯 Final Takeaway

This example shows **real voting**:

- Same $$\theta$$
- Same $$\rho$$
- Accumulator peak appears

Once you see this numerically,  
**Hough Transform stops being abstract** 🚀
