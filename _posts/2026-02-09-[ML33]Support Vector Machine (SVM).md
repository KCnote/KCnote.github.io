---
layout: post
title: "Support Vector Machine (SVM)"
date: 2026-02-09 00:00:00 +0900
author: kang
categories: [Machince Learning, Model]
tags: [Machince Learning, Model, Mathematics, SVM]
pin: false
math: true
mermaid: true

---



# 🧠✨ Support Vector Machine (SVM)

> 🎯 Goal: Find the **maximum-margin hyperplane** that separates two classes.

---

## 📦 1. Problem Setup

Training data:

$$
\{(x_i, y_i)\}_{i=1}^n,\quad x_i \in \mathbb{R}^p,\quad y_i \in \{-1, +1\}
$$

Hyperplane:

$$
\beta^T x + \beta_0 = 0
$$

Classifier:

$$
\hat y = \mathrm{sign}(\beta^T x + \beta_0)
$$

---

## 📐 2. Distance from a Point to a Hyperplane (Full Derivation)

We compute the signed distance from a point $x$ to the hyperplane.

### 🔹 Step 1 — Closest point on the hyperplane

The closest point must lie along the normal vector $\beta$:

$$
x_\perp = x - t\beta
$$

for some scalar $t$.

### 🔹 Step 2 — Enforce hyperplane constraint

$$
\beta^T x_\perp + \beta_0 = 0
$$

Substitute:

$$
\beta^T (x - t\beta) + \beta_0 = 0
$$

$$
\beta^T x - t\beta^T\beta + \beta_0 = 0
$$

Solve for $t$:

$$
t = \frac{\beta^T x + \beta_0}{\|\beta\|^2}
$$

### 🔹 Step 3 — Distance

$$
\|x - x_\perp\| = \|t\beta\| = |t| \|\beta\|
$$

$$
\|x - x_\perp\| = \frac{|\beta^T x + \beta_0|}{\|\beta\|}
$$

Signed distance:

$$
\boxed{
\mathrm{dist}(x,H) = \frac{\beta^T x + \beta_0}{\|\beta\|}
}
$$

---

## 🚀 3. Functional Margin and Geometric Margin

Functional margin:

$$
y_i(\beta^T x_i + \beta_0)
$$

Geometric margin:

$$
\gamma_i = \frac{y_i(\beta^T x_i + \beta_0)}{\|\beta\|}
$$

Dataset margin:

$$
\gamma = \min_i \gamma_i
= \min_i \frac{y_i(\beta^T x_i + \beta_0)}{\|\beta\|}
$$

🎯 Goal: **maximize margin**.

---

## 🔁 4. Max-Margin Optimization (Full Transformation)

We want:

$$
\max_{\beta,\beta_0} \gamma
$$

### 🔹 Scaling invariance

$$
\gamma(\beta,\beta_0) = \gamma(c\beta, c\beta_0)
$$

So enforce normalization:

$$
y_i(\beta^T x_i + \beta_0) \ge 1
$$

Then:

$$
\gamma = \frac{1}{\|\beta\|}
$$

Thus:

$$
\max \gamma \Longleftrightarrow \min \|\beta\|
$$

Smooth objective:

$$
\boxed{
\min_{\beta,\beta_0} \frac12 \|\beta\|^2
\quad \text{s.t.} \quad
y_i(\beta^T x_i + \beta_0) \ge 1
}
$$

👉 This is the **Hard-Margin SVM Primal Problem**.

---

## 🧮 5. Lagrangian Formulation

Introduce multipliers $\alpha_i \ge 0$:

$$
\mathcal{L}(\beta,\beta_0,\alpha)
=
\frac12 \|\beta\|^2
+
\sum_{i=1}^n \alpha_i
\left(1 - y_i(\beta^T x_i + \beta_0)\right)
$$

---

## 📉 6. Stationarity Conditions

### 🔹 Derivative w.r.t. $\beta$

$$
\frac{\partial \mathcal{L}}{\partial \beta}
= \beta - \sum_{i=1}^n \alpha_i y_i x_i = 0
$$

$$
\boxed{
\beta = \sum_{i=1}^n \alpha_i y_i x_i
}
$$

### 🔹 Derivative w.r.t. $\beta_0$

$$
\frac{\partial \mathcal{L}}{\partial \beta_0}
= -\sum_{i=1}^n \alpha_i y_i = 0
$$

$$
\boxed{
\sum_{i=1}^n \alpha_i y_i = 0
}
$$

---

## 🔄 7. Dual Derivation (No Steps Skipped)

Substitute

$$
\beta = \sum_{i=1}^n \alpha_i y_i x_i
$$

into the Lagrangian.

### 🔹 Expand $\|\beta\|^2$

$$
\|\beta\|^2
=
\left(\sum_{i=1}^n \alpha_i y_i x_i\right)^T
\left(\sum_{j=1}^n \alpha_j y_j x_j\right)
$$

$$
=
\sum_{i=1}^n \sum_{j=1}^n
\alpha_i \alpha_j y_i y_j x_i^T x_j
$$

Thus:

$$
\frac12 \|\beta\|^2
=
\frac12 \sum_{i,j}
\alpha_i \alpha_j y_i y_j x_i^T x_j
$$

### 🔹 Expand middle term

$$
\sum_{i=1}^n \alpha_i y_i \beta^T x_i
=
\sum_{i=1}^n \sum_{j=1}^n
\alpha_i \alpha_j y_i y_j x_i^T x_j
$$

### 🔹 Combine

$$
\mathcal{L}
=
\sum_{i=1}^n \alpha_i
-
\frac12 \sum_{i,j}
\alpha_i \alpha_j y_i y_j x_i^T x_j
$$

---

## 🎯 8. Dual Optimization

$$
\boxed{
\max_{\alpha}
\sum_{i=1}^n \alpha_i
-
\frac12 \sum_{i,j}
\alpha_i \alpha_j y_i y_j x_i^T x_j
}
$$

subject to:

$$
\alpha_i \ge 0,
\quad
\sum_{i=1}^n \alpha_i y_i = 0
$$

---

## 🧠 9. Final Decision Function

Optimal weight:

$$
\beta = \sum_{i=1}^n \alpha_i y_i x_i
$$

Decision function:

$$
f(x) =
\sum_{i=1}^n \alpha_i y_i x_i^T x + \beta_0
$$

Prediction:

$$
\boxed{\hat y = \mathrm{sign}(f(x))}
$$

Only $\alpha_i > 0$ matter → **Support Vectors**.

---

## 📌 10. Complementary Slackness

$$
\alpha_i \left(1 - y_i(\beta^T x_i + \beta_0)\right) = 0
$$

- If $\alpha_i > 0$ ⇒ point lies on margin  
- If point outside margin ⇒ $\alpha_i = 0$

Only support vectors determine the boundary.

---

## 🔗 11. Kernel Form

Replace inner product:

$$
x_i^T x \rightarrow K(x_i, x)
$$

Decision function:

$$
f(x)
=
\sum_{i=1}^n \alpha_i y_i K(x_i, x) + \beta_0
$$

---

# ✨ Key Insights

- 🧠 SVM maximizes geometric margin  
- 📐 Primal minimizes weight norm  
- 🔎 Dual depends only on inner products  
- 🧷 Solution is sparse (support vectors)  
- 🔥 Kernel trick enables nonlinear boundaries
