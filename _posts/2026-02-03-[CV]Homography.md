---
layout: post
title: "Homography in Computer Vision (4-Point Solution and N-Point Least Squares)"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Geometry]
tags: [Homography, Projective Geometry, DLT, Pseudo Inverse, Least Squares]
pin: false
math: true
mermaid: true
---

## 📐 What Is Homography?

A **homography** is a **projective transformation** that maps points from one plane to another.

It is widely used for:
- Image stitching
- Perspective correction
- Planar object tracking
- Camera pose estimation (planar scenes)

A homography is represented by a **3×3 matrix**:

$$
\mathbf{H} =
\begin{bmatrix}
h_{11} & h_{12} & h_{13} \\
h_{21} & h_{22} & h_{23} \\
h_{31} & h_{32} & h_{33}
\end{bmatrix}
$$

Defined up to scale:
$$
\mathbf{H} \sim \lambda \mathbf{H}
$$

So it has **8 degrees of freedom**.

---

## 🔗 Point Mapping Equation

Using homogeneous coordinates:

$$
\begin{bmatrix}
x' \\
y' \\
1
\end{bmatrix}
\sim
\mathbf{H}
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
$$

Which expands to:

$$
x' = \frac{h_{11}x + h_{12}y + h_{13}}{h_{31}x + h_{32}y + h_{33}}
$$

$$
y' = \frac{h_{21}x + h_{22}y + h_{23}}{h_{31}x + h_{32}y + h_{33}}
$$

---

## 🟦 Case 1: Homography from 4 Point Correspondences

### Why 4 Points?

- Each point correspondence gives **2 independent equations**
- Homography has **8 unknowns**
- Minimum: **4 point pairs**

---

### Linear Equation per Point

For correspondence \((x,y) \leftrightarrow (x',y')\):

$$
\begin{aligned}
x'(h_{31}x + h_{32}y + h_{33}) &= h_{11}x + h_{12}y + h_{13} \\
y'(h_{31}x + h_{32}y + h_{33}) &= h_{21}x + h_{22}y + h_{23}
\end{aligned}
$$

Rearranged into linear form:

$$
\mathbf{A} \mathbf{h} = \mathbf{0}
$$

Where:

$$
\mathbf{h} =
[h_{11}, h_{12}, h_{13}, h_{21}, h_{22}, h_{23}, h_{31}, h_{32}, h_{33}]^T
$$

Matrix \(\mathbf{A}\) has size **8×9**.

---

### Solution

- Solve using **SVD**
- Solution = right singular vector corresponding to smallest singular value
- Reshape into 3×3 matrix

---

## 🟩 Case 2: Homography from N Points (N ≥ 4)

When **more than 4 correspondences** are available, the system becomes **overdetermined**.

This improves:
- Noise robustness
- Numerical stability

---

## 📊 Linear System (DLT Form)

For N point pairs:

$$
\mathbf{A} \mathbf{h} = \mathbf{0}
$$

Where:
- \(\mathbf{A}\) is **(2N)×9**
- \(N > 4\)

---

## 🧮 Pseudo-Inverse / Least Squares View

We solve:

$$
\min_{\mathbf{h}} \| \mathbf{A} \mathbf{h} \|^2
$$

Subject to:

$$
\| \mathbf{h} \| = 1
$$

### Solution

- Compute **SVD** of \(\mathbf{A}\)
- Take eigenvector corresponding to smallest singular value
- Equivalent to solving with **Moore–Penrose pseudo-inverse**

---

## 🔧 Normalized DLT (Important in Practice)

Before solving:
1. Normalize points (zero mean, unit variance)
2. Solve homography
3. Denormalize result

This greatly improves numerical stability.

---

## ⚠️ Degenerate Configurations

Homography estimation fails if:
- Points are collinear
- All points lie on a line
- Insufficient spatial spread

---

## ⚖️ 4-Point vs N-Point Comparison

| Aspect | 4 Points | N Points |
|------|---------|---------|
| Minimum | Yes | No |
| Noise robustness | ❌ | ✅ |
| Uses least squares | ❌ | ✅ |
| Typical use | Theory, exact | Real systems |

---

## 🧠 Practical Pipeline

```
Feature Match
   ↓
RANSAC (inliers)
   ↓
Normalized DLT
   ↓
Homography
```

---

## 🎯 Takeaway

- Homography has **8 DOF**
- **4 points** give a minimal exact solution
- **N points** → least squares via SVD / pseudo-inverse
- Normalization is critical in real systems

Understanding this math is essential for **stitching, tracking, and registration** 🚀
