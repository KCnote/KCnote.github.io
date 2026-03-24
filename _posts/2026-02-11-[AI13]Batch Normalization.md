---
layout: post
title: "Batch Normalization"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Optimization]
tags: [Artificial Intelligenc, Batch, Normalization]
pin: false
math: true
mermaid: true
---
# 📊 Data Preprocessing & Batch Normalization

---

# 📌 1. Data Preprocessing

## 🎯 Goal

One major goal of preprocessing:

- Zero-centered data
- Unit-variance data

For input $x$:

$$
\tilde{x} = \frac{x - \mu}{\sigma}
$$

Where:

$$
\mu = \mathbb{E}[x], \quad
\sigma^2 = \text{Var}(x)
$$

---

## 🔍 Why Zero-Mean?

If data is not centered:

- Gradients become biased
- Optimization zig-zags
- Slower convergence

Zero-centered data improves gradient symmetry.

---

## ❓ What About Intermediate Layers?

Preprocessing works for input layer.

But what about hidden activations?

👉 That leads to **Batch Normalization**

---

# 🚀 2. Batch Normalization

---

# 📌 2.1 Basic Idea

We want:

> Zero-mean, unit-variance activations  
> At every layer

Given activations $x^{(k)}$ in a mini-batch:

$$
\hat{x}^{(k)} =
\frac{x^{(k)} - \mathbb{E}[x^{(k)}]}
{\sqrt{\text{Var}[x^{(k)}]}}
$$

---

# 📌 2.2 Estimating Mean and Variance

For mini-batch of size $N$:

### Mean

$$
\mu_j = \frac{1}{N} \sum_{i=1}^{N} x_{i,j}
$$

### Variance

$$
\sigma_j^2 =
\frac{1}{N} \sum_{i=1}^{N} (x_{i,j} - \mu_j)^2
$$

### Normalize

$$
\hat{x}_{i,j} =
\frac{x_{i,j} - \mu_j}
{\sqrt{\sigma_j^2 + \epsilon}}
$$

Where:

- $j$ = feature dimension
- $\epsilon$ = small constant for numerical stability

---

# 📌 2.3 Learnable Scale and Shift

Pure normalization may reduce model flexibility.

So we add:

$$
y^{(k)} = \gamma^{(k)} \hat{x}^{(k)} + \beta^{(k)}
$$

Where:

- $\gamma$ = learnable scale
- $\beta$ = learnable shift

This allows identity mapping if needed.

---

# 📌 2.4 During Inference

Problem:

- At training → use mini-batch statistics
- At test → no batch available

Solution:

Maintain running averages:

$$
\mu_{\text{running}}, \quad
\sigma^2_{\text{running}}
$$

Use them during inference.

---

# ✅ Why BatchNorm Works

✔ Easier training  
✔ Better gradient flow  
✔ Allows higher learning rate  
✔ Faster convergence  
✔ More robust to initialization  
✔ Acts as mild regularizer  

---

# ⚠ Why NOT BatchNorm?

Issues:

- Depends on mini-batch statistics
- Problem if batch size is small
- Distribution shift between train/test
- Not ideal for non-i.i.d data

---

# 🔁 Solution: Batch Renormalization

Paper:  
https://arxiv.org/pdf/1702.03275.pdf

Reduces train/test mismatch.

---

# 🧠 3. Other Normalization Methods

---

## 📦 Layer Normalization

Paper:  
https://arxiv.org/abs/1607.06450

- Normalize across features per sample
- Works well for NLP / Transformers

---

## 🖼 Instance Normalization

Paper:  
https://arxiv.org/abs/1607.08022

- Normalize per sample per channel
- Popular in style transfer

---

## 👥 Group Normalization

Paper:  
https://arxiv.org/abs/1803.08494

- Split channels into groups
- Normalize within each group
- Works better for small batch sizes

---

# 🏁 Summary Table

| Method | Depends on Batch? | Works with Small Batch? | Typical Use |
|---------|------------------|--------------------------|-------------|
| BatchNorm | Yes | ❌ Not ideal | CNN |
| LayerNorm | No | ✅ Yes | Transformers |
| InstanceNorm | No | ✅ Yes | Style transfer |
| GroupNorm | No | ✅ Yes | Small-batch CNN |

---

# 🎯 Final Takeaways

Deep networks benefit from:

- Proper preprocessing
- Stable activation distributions
- Controlled variance propagation

Normalization helps:

$$
\text{Input variance} \approx \text{Output variance}
$$
