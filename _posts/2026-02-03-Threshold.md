---
layout: post
title: "Thresholding in Image Processing"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Image Processing]
tags: [Computer Vision, Image Processing, Thresholding, Fixed Threshold, Adaptive Threshold, IsoData, Otsu, Maximum Entropy]
pin: false
math: true
mermaid: true
---

## 🎚️ Thresholding Overview

Thresholding converts a **grayscale image** into a **binary image** using a decision rule.

It is a fundamental step in:
- Segmentation
- ROI extraction
- Industrial inspection
- Morphology pipelines

---

## 🎯 Fixed Threshold

### Definition
A **fixed threshold value** chosen manually or empirically.

$$
I_{out}(x,y) =
\begin{cases}
255 & I(x,y) > T \\
0 & \text{otherwise}
\end{cases}
$$

### Characteristics
- Same threshold for entire image
- No statistics involved

**Pros**
- Fastest possible
- Fully deterministic

**Cons**
- Extremely sensitive to illumination
- Requires tuning

---

## 🌍 Adaptive Thresholding

### Idea
Threshold value varies **locally** depending on neighborhood statistics.

### Mean Adaptive
$$
T(x,y) = \mu_{N(x,y)} - C
$$

### Gaussian Adaptive
$$
T(x,y) = G_{N(x,y)} - C
$$

Where:
- local mean
$$
\mu
$$  
- Gaussian-weighted mean
$$
G
$$  
- bias constant  
$$
C
$$

**Pros**
- Handles uneven illumination
- Works well in real scenes

**Cons**
- Slower than global methods
- Sensitive to window size

---

## 🔁 IsoData Thresholding (Iterative Selection)

### Core Idea
Iteratively updates threshold until convergence.

### Algorithm
1. Initialize threshold $$T_0$$
2. Split pixels into two classes using $$T_k$$
3. Compute class means $$\mu_0, \mu_1$$
4. Update threshold

$$
T_{k+1} = \frac{\mu_0 + \mu_1}{2}
$$

5. Repeat until $$T_{k+1} = T_k$$

### Characteristics
- Also known as **Iterative Intermeans**
- Histogram-based

**Pros**
- Simple and intuitive
- No histogram variance computation

**Cons**
- Convergence dependent
- Fails on unimodal distributions

---

## 📊 Variance-Based Thresholding (Otsu)

### Core Idea
Select threshold maximizing **between-class variance**.

$$
\sigma_b^2(T) = w_0(T) \cdot w_1(T) \cdot \left( \mu_0(T) - \mu_1(T) \right)^2
$$

**Pros**
- Automatic
- Excellent for bimodal histograms

**Cons**
- Weak for multimodal cases

---

## 🧠 Maximum Entropy Thresholding

### Core Idea
Select threshold maximizing **total entropy**.

$$
H(T) = H_0(T) + H_1(T)
$$

Entropy definition:

$$
H = - \sum_i p(i) \log p(i)
$$

**Pros**
- Strong against histogram shape changes

**Cons**
- Computationally heavier

---

## ⚖️ Thresholding Method Comparison

| Method | Threshold Source | Speed | Robustness |
|------|-----------------|------|------------|
| Absolute | Manual | ⭐⭐⭐⭐ | ⭐ |
| IsoData | Iterative mean | ⭐⭐⭐ | ⭐⭐⭐ |
| Otsu | Variance | ⭐⭐⭐ | ⭐⭐⭐ |
| Max Entropy | Information | ⭐⭐ | ⭐⭐⭐⭐ |
| Adaptive | Local stats | ⭐⭐ | ⭐⭐⭐⭐ |

---

## 🔗 Typical CV Pipeline

```
Grayscale
   ↓
Threshold Selection
   ↓
Binary Mask
   ↓
Bitwise Logic
   ↓
Morphology
```

---

## 🧠 Practical Selection Guide

- **Stable lighting** → Absolute / Otsu
- **Histogram unclear** → IsoData / Max Entropy
- **Uneven illumination** → Adaptive
- **Real-time constraints** → Absolute / Otsu

---

## 🎯 Takeaway

Thresholding is not a single algorithm.

Understanding:
- Fixed decision rules
- Iterative statistics
- Variance and entropy

allows you to build **robust and explainable segmentation pipelines** 🚀
