---
layout: post
title: "Extension of Logistic Regression"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Classification]
tags: [Machince Learning, Mathematics, ML, Supervised, Regression, Classification]
pin: false
math: true
mermaid: true
---

# 📘 Revised Supervised Learning & Logistic Regression

> 🎯 Complete version — **no omission**, full equations, clean blog styling with emojis  
> 🧠 Covers: supervised learning framework → logistic regression → softmax → MLE → log‑loss  

---

# 🤖 Revised Supervised Learning Framework

## Overview

Machine learning is a **data‑driven approach**.  
Instead of manually designing rules, we design a **model form**, and data determines the optimal parameters.

---

## 🧭 Step‑by‑Step Framework

### 1️⃣ Model Design

Humans specify the **structure of the model**.

Example — Logistic Regression:

$$
P(y = 1 \mid \mathbf{x}) =
\frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
$$

---

### 2️⃣ Define the Learning Goal

We want the model to approximate:

$$
y \approx f(\mathbf{x})
$$

as accurately as possible.

---

### 3️⃣ Learn Parameters from Data

Given training data:

$$
\{(\mathbf{x}_i, y_i)\}_{i=1}^{n}
$$

we estimate optimal parameters $\boldsymbol{\beta}$.

---

#### 🔹 Step 3‑1. Prediction

Compute prediction:

$$
\hat{y} = f(\mathbf{x})
$$

---

#### 🔹 Step 3‑2. Loss Evaluation

Measure prediction quality using a **loss function**.

For logistic regression:

$$
\ell(y,\hat{y})
=
- y \log \hat{y}
- (1-y)\log(1-\hat{y})
$$

This is **log‑loss / cross‑entropy**.

---

#### 🔹 Step 3‑3. Parameter Update

Update parameters to reduce loss:

$$
\boldsymbol{\beta} \leftarrow \boldsymbol{\beta}
- \eta \nabla_{\boldsymbol{\beta}} \ell
$$

---

#### 🔹 Step 3‑4. Iteration

Repeat:

Prediction → Loss → Update → Convergence

Goal:

$$
\hat{y} \approx y
$$

for most samples.

---

### 4️⃣ Inference

For new input $\mathbf{x}$:

$$
\hat{y} = \arg\max_y P(y \mid \mathbf{x})
$$

---

## 💡 Key Insight

Training = **minimizing loss = learning parameters from data**.

---

# 📈 Logistic Regression — Model & Extensions

## Binary Logistic Model

$$
P(y=1 \mid \mathbf{x})
=
\frac{1}{1+e^{-\boldsymbol{\beta}^T\mathbf{x}}}
$$

For single feature:

$$
P(y=1 \mid x)
=
\frac{1}{1+e^{-(\beta_0+\beta_1x)}}
$$

---

## Multi‑Feature Extension

For $p$ features:

$$
\log\frac{p}{1-p}
=
\beta_0+\beta_1x_1+\cdots+\beta_px_p
$$

Parameters:

- $\beta_0$ : intercept  
- $\beta_1 \ldots \beta_p$ : feature weights  

Vector form:

$$
P(y=1 \mid \mathbf{x})
=
\frac{1}{1+e^{-\boldsymbol{\beta}^T\mathbf{x}}}
$$

---

## Multi‑Class Extension — Softmax

Define class score:

$$
s_k = \boldsymbol{\beta}_k^T \mathbf{x}
$$

Probability:

$$
P(y=k \mid \mathbf{x})
=
\frac{e^{s_k}}{\sum_{j=1}^{K}e^{s_j}}
$$

Properties:

- $0 \le P \le 1$
- $\sum_k P = 1$

Sigmoid = special case of softmax with $K=2$.

---

# 📐 Logistic Regression — Maximum Likelihood

## Conditional Log‑Likelihood

$$
\hat{\boldsymbol{\beta}}
=
\arg\max_{\boldsymbol{\beta}}
\sum_{i=1}^{n}\log P(y_i \mid \mathbf{x}_i)
$$

---

## Expand Using Logistic Model

$$
p(y=1 \mid \mathbf{x})
=
\frac{1}{1+e^{-\boldsymbol{\beta}^T\mathbf{x}}}
$$

$$
p(y=0 \mid \mathbf{x})
=
\frac{1}{1+e^{\boldsymbol{\beta}^T\mathbf{x}}}
$$

---

## Log‑Likelihood Form

$$
\sum_{i=1}^{N}
\left[
y_i \log \frac{1}{1+e^{-\boldsymbol{\beta}^T\mathbf{x}_i}}
+
(1-y_i)\log\frac{1}{1+e^{\boldsymbol{\beta}^T\mathbf{x}_i}}
\right]
$$

---

## Equivalent Minimization (Negative Log‑Likelihood)

$$
\hat{\boldsymbol{\beta}}
=
\arg\min_{\boldsymbol{\beta}}
\sum_{i=1}^{N}
\left[
- y_i \log(1+e^{-\boldsymbol{\beta}^T\mathbf{x}_i})
-
(1-y_i)\log(1+e^{\boldsymbol{\beta}^T\mathbf{x}_i})
\right]
$$

---

## Log‑Loss / Binary Cross‑Entropy

$$
\ell(\boldsymbol{\beta})
=
\sum_{i=1}^{N}
\left[
- y_i \log(1+e^{-\boldsymbol{\beta}^T\mathbf{x}_i})
-
(1-y_i)\log(1+e^{\boldsymbol{\beta}^T\mathbf{x}_i})
\right]
$$

---

## 🧠 Final Insights

- Logistic regression **maximizes conditional likelihood**
- Equivalent to **minimizing log‑loss**
- Convex → unique global optimum
- Optimized using:
  - Gradient Descent  
  - SGD / Mini‑batch  
  - Newton / Quasi‑Newton  
