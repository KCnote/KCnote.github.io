---
layout: post
title: "Support Vector Machine (SVM) — Soft Margin with Slack Variables"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Machince Learning, Model]
tags: [Machince Learning, Model, Mathematics, SVM, Margin]
pin: false
math: true
mermaid: true

---


# 📘 Support Vector Machine (SVM) — Soft Margin with Slack Variables

---

## 🌪️ Noisy & Non‑Separable Data

Real-world data are often **not perfectly separable** due to noise, overlap, or outliers.  
The classical **hard‑margin SVM** fails in this case because it enforces:

$$
y_i (w^T x_i + b) \ge 1
$$

for **all samples**, which may be impossible.

👉 Solution: introduce **Slack Variables** → Soft Margin SVM

---

## 🧠 Idea of Slack Variables

We allow violations of the margin constraint by introducing:

$$
\xi_i \ge 0
$$

Modified constraint:

$$
y_i (w^T x_i + b) \ge 1 - \xi_i
$$

Interpretation:

| Case | Meaning |
|------|---------|
| $\xi_i = 0$ | Correct & outside margin |
| $0 < \xi_i < 1$ | Inside margin |
| $\xi_i = 1$ | On wrong side of boundary |
| $\xi_i > 1$ | Misclassified |

Slack definition:

$$
\xi_i = \max(0, 1 - y_i(w^T x_i + b))
$$

This becomes **Hinge Loss**.

---

## 🎯 Soft Margin Primal Optimization

Hard margin objective:

$$
\min_w \frac{1}{2} \|w\|^2
$$

Soft margin adds penalty for violations:

$$
\min_{w,b,\xi} \quad \frac{1}{2} \|w\|^2 + C \sum_{i=1}^{n} \xi_i
$$

subject to:

$$
y_i(w^T x_i + b) \ge 1 - \xi_i,\quad \xi_i \ge 0
$$

---

## ⚖️ Meaning of Hyperparameter $C$

$$
C \uparrow \Rightarrow \text{penalize errors more → smaller margin}
$$

$$
C \downarrow \Rightarrow \text{allow more errors → larger margin}
$$

---

## 🧮 Lagrangian Formulation

We introduce multipliers:

- $\alpha_i \ge 0$ for margin constraint  
- $\mu_i \ge 0$ for $\xi_i \ge 0$

Lagrangian:

$$
\mathcal{L}(w,b,\xi,\alpha,\mu)
= \frac{1}{2} \|w\|^2 + C\sum_{i=1}^n \xi_i
- \sum_{i=1}^n \alpha_i \big(y_i(w^T x_i + b) - 1 + \xi_i\big)
- \sum_{i=1}^n \mu_i \xi_i
$$

---

## 📐 KKT Conditions

### Derivative w.r.t $w$

$$
\frac{\partial \mathcal{L}}{\partial w}
= w - \sum_{i=1}^n \alpha_i y_i x_i = 0
$$

$$
\Rightarrow
w = \sum_{i=1}^n \alpha_i y_i x_i
$$

---

### Derivative w.r.t $b$

$$
\frac{\partial \mathcal{L}}{\partial b}
= -\sum_{i=1}^n \alpha_i y_i = 0
$$

$$
\Rightarrow
\sum_{i=1}^n \alpha_i y_i = 0
$$

---

### Derivative w.r.t $\xi_i$

$$
\frac{\partial \mathcal{L}}{\partial \xi_i}
= C - \alpha_i - \mu_i = 0
$$

Since $\mu_i \ge 0$:

$$
0 \le \alpha_i \le C
$$

---

## 🎯 Dual Optimization Problem

Substitute $w$ into Lagrangian:

$$
\max_{\alpha}
\sum_{i=1}^n \alpha_i
- \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n
\alpha_i \alpha_j y_i y_j x_i^T x_j
$$

subject to:

$$
0 \le \alpha_i \le C
$$

$$
\sum_{i=1}^n \alpha_i y_i = 0
$$

---

## ✨ Final Decision Function

After solving $\alpha$:

$$
w = \sum_{i=1}^n \alpha_i y_i x_i
$$

Prediction:

$$
f(x) = \text{sign}\left( \sum_{i=1}^n \alpha_i y_i x_i^T x + b \right)
$$

Only points with $\alpha_i > 0$ → **Support Vectors**

---

## 🌈 Geometric Interpretation

Margin width:

$$
\text{Margin} = \frac{2}{\|w\|}
$$

Slack allows some points inside margin or misclassified while still maximizing margin.

---

## 📊 Effect of $C$

| Small $C$ | Large Margin | More tolerant |
|----------|--------------|--------------|
| Large $C$ | Small Margin | Less tolerant |

---

## 🔥 Connection to Hinge Loss

Soft‑margin SVM equals minimizing:

$$
\frac{1}{2} \|w\|^2
+ C \sum_{i=1}^n \max(0, 1 - y_i(w^T x_i + b))
$$

Regularization + Hinge Loss

---

## 🚀 Kernel Extension (Optional)

Replace inner product:

$$
x_i^T x_j \rightarrow K(x_i, x_j)
$$

Dual becomes:

$$
\max_{\alpha}
\sum \alpha_i - \frac{1}{2}\sum\sum \alpha_i \alpha_j y_i y_j K(x_i,x_j)
$$

---

## 📌 Summary

- Hard margin → requires separable data  
- Slack variables → allow violations  
- $C$ controls margin vs errors  
- Dual → depends only on inner products  
- Support vectors define the boundary  

