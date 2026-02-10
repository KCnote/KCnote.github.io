---
layout: post
title: "Support Vector Machines with Kernels"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Machince Learning, Model]
tags: [Machince Learning, Model, Mathematics, SVM, Margin, Kernel]
pin: false
math: true
mermaid: true

---

# 📘 Support Vector Machines — Kernels, Logistic Comparison, and Multi‑Class

---

## ⚔️ SVM vs Logistic Regression

### When classes are well separated

SVM tends to perform better because it **maximizes the margin**:

$$
\max \frac{2}{\|w\|}
$$

Logistic regression keeps pushing probabilities toward 0/1 and may become unstable when perfectly separable (weights → ∞).

---

### When classes overlap (noisy data)

Logistic regression often performs similarly or better because it minimizes **log loss**:

$$
\sum \log(1 + e^{-y_i f(x_i)})
$$

while SVM minimizes hinge loss:

$$
\sum \max(0, 1 - y_i f(x_i))
$$

---

### Probabilities

Logistic regression outputs:

$$
P(y=1|x) = \frac{1}{1 + e^{-f(x)}}
$$

SVM → only decision boundary (no probability unless calibrated).

---

### Nonlinear boundaries

Kernel SVM naturally handles nonlinear boundaries using kernel trick.  
Kernel logistic regression exists but is computationally heavier.

---

## 🌌 Linearly Non‑Separable Data → Kernel Trick

We map data into higher dimension:

$$
x \rightarrow \phi(x)
$$

Linear classifier becomes:

$$
f(x) = w^T \phi(x) + b
$$

Instead of explicit mapping, use kernel:

$$
K(x_i, x_j) = \phi(x_i)^T \phi(x_j)
$$

---

## 📐 Inner Products & Support Vectors

From SVM dual solution:

$$
w = \sum_{i=1}^{n} \alpha_i y_i x_i
$$

Prediction:

$$
f(x) = \text{sign} \left( \sum_{i=1}^{n} \alpha_i y_i x_i^T x + b \right)
$$

Only $\alpha_i > 0$ contribute → **Support Vectors**.

---

## 🧠 Kernelized SVM

Replace inner product:

$$
x_i^T x_j \rightarrow K(x_i, x_j)
$$

Decision function:

$$
f(x) = \text{sign} \left( \sum_{i=1}^{n} \alpha_i y_i K(x_i, x) + b \right)
$$

---

## 📊 Polynomial Kernel

$$
K(x_i, x_j) = (1 + x_i^T x_j)^d
$$

Implicitly maps data to **all monomials up to degree $d$**.

---

## 🌐 Radial Basis Function (RBF / Gaussian Kernel)

$$
K(x_i, x_j) = \exp\left(-\gamma \|x_i - x_j\|^2\right)
$$

Properties:

- Local similarity measure
- Small $\gamma$ → smooth boundary
- Large $\gamma$ → complex / wiggly boundary

---

## 🔍 Why Kernel Works (Derivation)

From dual optimization:

$$
\max_{\alpha}
\sum \alpha_i - \frac{1}{2}\sum\sum \alpha_i \alpha_j y_i y_j x_i^T x_j
$$

Replace inner product:

$$
x_i^T x_j = \phi(x_i)^T \phi(x_j) = K(x_i,x_j)
$$

Thus SVM works in high‑dim space **without explicit mapping**.

---

## 👥 SVM for Multi‑Class (K > 2)

SVM is inherently binary → need strategy.

### One‑vs‑Rest (OvR)

Train K classifiers:

$$
f_k(x) = w_k^T x + b_k
$$

Prediction:

$$
\hat{y} = \arg\max_k f_k(x)
$$

---

### One‑vs‑One (OvO)

Train pairwise classifiers:

$$
\binom{K}{2}
$$

Final prediction → majority voting.

Preferred when K is small.

---

## 🔬 Geometric Interpretation

Kernel transforms space so that nonlinear boundary in original space becomes **linear hyperplane in feature space**.

---

## 📊 Logistic vs SVM — Mathematical Difference

| Method | Objective |
|--------|----------|
| Logistic | $\sum \log(1 + e^{-y f(x)})$ |
| SVM | $\frac{1}{2}\|w\|^2 + C\sum \max(0, 1-yf(x))$ |

Logistic → probabilistic model  
SVM → margin maximization

---

## 🚀 Final Summary

- SVM maximizes margin → strong geometric classifier
- Logistic gives probabilities → probabilistic interpretation
- Kernel trick enables nonlinear boundaries
- RBF kernel → local similarity
- Multi‑class via OvR / OvO
- Support vectors define decision boundary

---

**End of Notes ✨**
