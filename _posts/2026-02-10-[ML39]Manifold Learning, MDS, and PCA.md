---
layout: post
title: "Manifold Learning, MDS, and PCA"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Machince Learning, Machince Learning - Foundation]
tags: [Machince Learning, Unsuperviseding, Clustering, PCA, MDS, Manifold]
pin: false
math: true
mermaid: true
---

# 🌌 Manifold Learning, MDS, and PCA

> Unsupervised Learning – Dimensionality Reduction  
> Lecture-style notes with full mathematical derivations ✨

---

## 🌌 Data Manifold

### What is a Manifold?
- A **manifold** is a low-dimensional surface embedded in a high-dimensional space.
- Although data lives in a high-dimensional space, it often occupies only a **small subspace**.

### Example: MNIST
- Each image: 28 × 28 = **784-dimensional**
- Total possible images: $$256^{28 \times 28}$$
- In reality:
  - Digits concentrate around **10 semantic clusters (0–9)**
  - Data lies on **low-dimensional manifolds**

➡️ High dimension ≠ high intrinsic dimension

---

## 🧠 Manifold Learning

### Key Idea
Machine learning often tries to **rediscover the hidden low-dimensional manifold**.

Formally:
- Given data:
$$
x^{(1)}, x^{(2)}, \dots, x^{(N)} \in \mathbb{R}^D
$$

- Find an embedding:
$$
f(x^{(i)}) \in \mathbb{R}^d, \quad d \ll D
$$

such that **spatial relationships are approximately preserved**.

---

## 📐 Multidimensional Scaling (MDS)

### Objective
Given a dissimilarity measure $$\rho(x^{(i)}, x^{(j)})$$,
find embedded points $$f(x^{(i)})$$ minimizing distortion:

$$
\sum_{i < j}
R\Big(
\| f(x^{(i)}) - f(x^{(j)}) \|,
\rho(x^{(i)}, x^{(j)})
\Big)
$$

---

## 📊 Principal Component Analysis (PCA)

### PCA as a Special Case of MDS

- $$\rho(x^{(i)}, x^{(j)}) = \|x^{(i)} - x^{(j)}\|_2$$
- $$R(a,b) = (a - b)^2$$

➡️ PCA maximally preserves **variance**

---

## ⚙️ PCA Algorithm

### Step 1: Zero-centering
$$
\bar{x} = \frac{1}{N}\sum_{i=1}^N x^{(i)}, \quad \tilde{x}^{(i)} = x^{(i)} - \bar{x}
$$

### Step 2: Covariance
$$
\hat{\Sigma} = \frac{1}{N}\sum_{i=1}^N \tilde{x}^{(i)}\tilde{x}^{(i)T}
$$

### Step 3: Eigen Decomposition
$$
\hat{\Sigma} = U\Lambda U^T
$$

### Step 4: Projection
$$
z^{(i)} = U_k^T \tilde{x}^{(i)}
$$

---

## 🧠 Summary
- Manifold learning uncovers intrinsic structure
- MDS preserves pairwise distances
- PCA preserves variance optimally
