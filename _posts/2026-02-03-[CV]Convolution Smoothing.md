---
layout: post
title: "Convolution for Smoothing"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Image Processing]
tags: [Computer Vision, Image Processing, Convolution, Smoothing, Uniform, Gaussian, Lowpass, Binomial, Noise]
pin: false
math: true
mermaid: true

---

> 🧠 **Core idea**  
> Smoothing convolutions reduce noise and small variations by  
> **aggregating local neighborhoods** into a more stable representation.
>
> Each smoothing kernel encodes a *different assumption* about how local pixels should contribute.

This post introduces the most common **smoothing convolution kernels**,  
with their **mathematical form and example kernel values**.

---

## 🌱 Why Smoothing Matters

Smoothing is used to:

- suppress sensor and texture noise  
- stabilize gradients and features  
- simplify response landscapes  
- improve robustness before further processing  

At its heart, smoothing answers:
> “What does this region look like *on average*?”

---

## 1️⃣ Uniform (Box) Filter

### Concept
- All pixels in the neighborhood are treated **equally**
- Simplest form of smoothing
- Strong noise averaging, but blurs edges aggressively

---

### Mathematical Form (k × k)

$$
K(i,j) = \frac{1}{k^2}
$$

for all \( i,j \in [-\frac{k}{2}, \frac{k}{2}] \)

---

### Example: 3×3 Kernel

```text id="box3x3"
1/9   1/9   1/9
1/9   1/9   1/9
1/9   1/9   1/9
```

---

### Characteristics
- ✅ Fast
- ❌ Edge-unaware
- ❌ Can introduce blocky artifacts

---

## 2️⃣ Gaussian Filter

### Concept
- Pixels closer to the center contribute more
- Assumes local structure follows a **Gaussian distribution**
- Smooths noise while preserving structure better than box filters

---

### Mathematical Form

$$
G(x,y) =
\frac{1}{2\pi\sigma^2}
\exp\left(-\frac{x^2 + y^2}{2\sigma^2}\right)
$$

---

### Example: 3×3 Gaussian (σ ≈ 1)

```text id="gaussian3x3"
1  2  1
2  4  2
1  2  1
```
Normalized by dividing by 16.

---

### Characteristics
- ✅ Good noise suppression
- ✅ Better edge behavior
- ✅ Separable (fast implementation)
- ❌ Slightly higher cost than box filter

---

## 3️⃣ Low-Pass Filter (Frequency View)

### Concept
- Suppresses high-frequency components
- Preserves low-frequency (smooth) content
- Often implemented via Gaussian or averaging filters

---

### Ideal Low-Pass (Conceptual)

In frequency domain:

$$
H(f) =
\begin{cases}
1, & |f| \le f_c \\
0, & |f| > f_c
\end{cases}
$$

⚠️ Ideal low-pass filters are **not realizable directly** in spatial domain  
(due to infinite support).

---

### Practical Approximation
- Gaussian filters
- Box filters
- Windowed sinc filters (rare in CV)

---

## 4️⃣ Binomial Filter

### Concept
- Discrete approximation of Gaussian
- Built from binomial coefficients
- Common in pyramids and fast smoothing

---

### Example: 5×5 Binomial Kernel

```text id="binomial5x5"
1  4  6  4  1
4 16 24 16  4
6 24 36 24  6
4 16 24 16  4
1  4  6  4  1
```
Normalized by dividing by 256.

---

### Characteristics
- ✅ Very fast
- ✅ Integer coefficients
- ✅ Excellent for multi-scale processing

---

## 5️⃣ Edge-Preserving (Conceptual Boundary)

Strictly speaking, these go **beyond linear convolution**,  
but they are often discussed alongside smoothing.

Examples:
- bilateral filter  
- guided filter  

They smooth **within regions**, but avoid crossing edges.

> Mentioned here to clarify the *limit* of linear smoothing kernels.

---

## 🧠 How to Choose a Smoothing Kernel

| Goal | Recommended Kernel |
|----|----|
| Fast rough smoothing | Uniform / Box |
| General-purpose smoothing | Gaussian |
| Multi-scale processing | Gaussian / Binomial |
| Frequency-domain reasoning | Gaussian (low-pass) |
| Edge preservation | Nonlinear filters |

---

## 💡 Computer Vision Takeaway

> A smoothing kernel encodes an assumption about **local similarity**.

- Box: “All neighbors are equal”  
- Gaussian: “Closer pixels matter more”  
- Low-pass: “Ignore rapid variation”  

Choosing a kernel is choosing **how you want to see the image**.

---

## ✨ Summary

- 🔹 Smoothing reduces noise via local aggregation  
- 🔹 Box, Gaussian, and binomial are the most common kernels  
- 🔹 Gaussian offers the best trade-off in practice  
- 🔹 Low-pass filtering is the frequency-domain interpretation  
- 🔹 Kernel choice reflects perceptual intent  

---

🌈 *Smoothing is not about blurring — it’s about stability.*
