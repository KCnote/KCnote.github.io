---
layout: post
title: "Loss Function"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Machince Learning, Machince Learning - ptimization]
tags: [Machince Learning, Optimization, Mathematics, SVM, Margin]
pin: false
math: true
mermaid: true

---

# 📘 Margin-Based Loss Functions

---

## 🎯 What is a Loss Function?

A loss function measures **how wrong** a model prediction is.

Given prediction $\hat{y}$ and label $y$:

$$
\mathcal{L}(\hat{y}, y)
$$

- If $\hat{y} = y$ → small loss (≈0)
- If $\hat{y} \ne y$ → larger loss
- Larger error → larger penalty

---

## 🧠 Binary Classification Setup

Labels:

$$
y \in \{-1, +1\}
$$

Model outputs a **score**:

$$
f(x) = w^T x + b
$$

Prediction:

$$
\hat{y} = \text{sign}(f(x))
$$

Define **margin**:

$$
m = y f(x) = y(w^T x + b)
$$

- $m > 0$ → correct classification
- $m < 0$ → wrong classification

Loss depends only on **margin $m$** → Margin‑based loss

---

## 📉 0/1 Loss

Definition:

$$
\mathcal{L}_{0/1}(m) =
\begin{cases}
0 & m > 0 \\
1 & m \le 0
\end{cases}
$$

Non‑differentiable → hard to optimize.

---

## 📈 Logistic (Log) Loss — Full Derivation

Logistic model:

$$
P(y=1|x) = \sigma(f(x)) = \frac{1}{1 + e^{-f(x)}}
$$

Likelihood:

$$
P(y|x) = \sigma(f(x))^{\frac{1+y}{2}} (1-\sigma(f(x)))^{\frac{1-y}{2}}
$$

Negative log-likelihood:

$$
\mathcal{L}_{log}(m)
= \log(1 + e^{-m})
$$

Properties:

- Smooth & differentiable
- Probabilistic interpretation
- Penalizes wrong predictions smoothly

Gradient:

$$
\frac{d}{dm}\log(1+e^{-m}) = -\frac{1}{1+e^{m}}
$$

---

## 📊 Exponential Loss — AdaBoost

Used in AdaBoost:

$$
\mathcal{L}_{exp}(m) = e^{-m}
$$

Derivation (Boosting objective):

Minimize weighted classification error:

$$
\sum_i e^{-y_i f(x_i)}
$$

Properties:

- Very sensitive to outliers
- Large penalty for misclassified points

---

## 📏 Hinge Loss — SVM

Definition:

$$
\mathcal{L}_{hinge}(m) = \max(0, 1 - m)
$$

From Soft‑Margin SVM:

Primal problem:

$$
\min_w \frac{1}{2}\|w\|^2 + C\sum_i \xi_i
$$

with:

$$
\xi_i = \max(0, 1 - y_i f(x_i))
$$

Substitute →

$$
\min_w \frac{1}{2}\|w\|^2 + C\sum_i \max(0, 1 - y_i f(x_i))
$$

👉 SVM = **Hinge Loss + L2 Regularization**

---

## 📐 Geometric Interpretation

Margin:

$$
m = y f(x)
$$

- $m \ge 1$ → no loss
- $0 < m < 1$ → inside margin
- $m < 0$ → misclassified

---

## 🔬 Comparison of Loss Functions

| Loss | Formula | Smooth | Robust to Noise |
|------|--------|--------|-----------------|
| 0/1 | $\mathbf{1}(m \le 0)$ | ❌ | Medium |
| Logistic | $\log(1+e^{-m})$ | ✔ | Good |
| Exponential | $e^{-m}$ | ✔ | ❌ Sensitive |
| Hinge | $\max(0,1-m)$ | Piecewise | Good |

---

## ⚖️ Logistic vs Hinge vs Exponential

### Logistic

- Smooth optimization
- Probabilistic
- Robust

### Hinge

- Sparse solution (Support Vectors)
- Margin maximization
- Efficient

### Exponential

- Strong focus on hard samples
- Sensitive to outliers

---

## 🔥 Unified Risk Minimization

All margin-based classifiers minimize:

$$
\min_w \frac{\lambda}{2}\|w\|^2 + \sum_i \mathcal{L}(y_i f(x_i))
$$

Different loss → different algorithm.

---

## 🌈 Shapes of Loss Functions

- 0/1 → step
- Logistic → smooth S‑curve
- Exponential → steep penalty
- Hinge → piecewise linear

---

## 🚀 Summary

- Loss depends on margin $m = y f(x)$
- Logistic → probabilistic smooth loss
- Exponential → boosting loss
- Hinge → SVM loss
- All are special cases of **regularized risk minimization**
