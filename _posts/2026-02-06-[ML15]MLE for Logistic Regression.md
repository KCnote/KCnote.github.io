---
layout: post
title: "MLE for Logistic Regression"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Machince Learning - Foundation]
tags: [Machince Learning, Overview, ML, Supervised, Regression, Classification, MLE]
pin: false
math: true
mermaid: true
---

# 🤖 Logistic Regression — MLE Complete Notes (Styled, No Omission)

> 📘 All original mathematical content is preserved exactly  
> 🎨 Only visual style, emojis, and readability improvements were added  
> 📐 Full MLE, log‑likelihood, and optimization explanation included  

---


# MLE of Logistic Regression — Complete Notes (No Omission)

This document reorganizes **all provided material without omission** into a clean, blog‑ready structure.

All mathematics uses:

$$ ... $$

---

# 1. Objective of Logistic Regression MLE

Logistic regression estimates the parameter vector **β** by **maximizing the conditional distribution**:

$$
P(y \mid \mathbf{x})
$$

Goal:

$$
\hat{\boldsymbol{\beta}} = \arg\max_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta})
$$

This is the **Maximum Likelihood Estimator (MLE)**.

---

# 2. Likelihood Function

For binary response:

$$
y_i \in \{0,1\}
$$

The model defines:

$$
P(y_i = 1 \mid \mathbf{x}_i) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}}
$$

$$
P(y_i = 0 \mid \mathbf{x}_i) = 1 - P(y_i = 1 \mid \mathbf{x}_i)
$$

---

# 3. Likelihood of the Dataset

Given $n$ independent observations:

$$
\{(\mathbf{x}_i, y_i)\}_{i=1}^{n}
$$

The likelihood function is:

$$
\mathcal{L}(\boldsymbol{\beta})
= \prod_{i: y_i = 1} P(y_i \mid \mathbf{x}_i)
  \prod_{i: y_i = 0} \left(1 - P(y_i \mid \mathbf{x}_i)\right)
$$

This represents the probability of observing the dataset under parameter $\boldsymbol{\beta}$.

---

# 4. Maximum Likelihood Estimation

We estimate parameters by **maximizing the likelihood**:

$$
\hat{\boldsymbol{\beta}} = \arg\max_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta})
$$

For numerical stability and convenience, we instead maximize the **log‑likelihood**.

---

# 5. Log‑Likelihood Derivation

Starting from:

$$
\mathcal{L}(\boldsymbol{\beta})
= \prod_{i: y_i = 1} p(y_i \mid \mathbf{x}_i)
  \prod_{i: y_i = 0} (1 - p(y_i \mid \mathbf{x}_i))
$$

Taking logarithm:

$$
\log \mathcal{L}(\boldsymbol{\beta})
= \log \prod_{i: y_i = 1} p(y_i \mid \mathbf{x}_i)
+ \log \prod_{i: y_i = 0} (1 - p(y_i \mid \mathbf{x}_i))
$$

Using:

$$
\log \prod = \sum \log
$$

we obtain:

$$
= \sum_{i: y_i = 1} \log p(y_i \mid \mathbf{x}_i)
+ \sum_{i: y_i = 0} \log (1 - p(y_i \mid \mathbf{x}_i))
$$

---

# 6. Unified Log‑Likelihood Expression

Both cases can be written in a single sum:

$$
\log \mathcal{L}(\boldsymbol{\beta})
= \sum_{i=1}^{n}
\left[
y_i \log p(y_i \mid \mathbf{x}_i)
+ (1 - y_i)\log(1 - p(y_i \mid \mathbf{x}_i))
\right]
$$

---

# 7. Substituting the Logistic Function

Using:

$$
p(y_i \mid \mathbf{x}_i) =
\frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}}
$$

we obtain:

$$
\log \mathcal{L}(\boldsymbol{\beta})
= \sum_{i=1}^{n}
\left[
- y_i \log (1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i})
- (1 - y_i)\log (1 + e^{\boldsymbol{\beta}^T \mathbf{x}_i})
\right]
$$

---

# 8. Key Takeaways from Log‑Likelihood

- Logistic regression estimates parameters by **maximizing log‑likelihood**
- The objective is **concave**, ensuring a **unique global optimum**
- The negative log‑likelihood corresponds to the **binary cross‑entropy (log‑loss)**

---

# 9. No Closed‑Form Solution

The log‑likelihood is:

$$
\log \mathcal{L}(\boldsymbol{\beta})
= \sum_{i=1}^{n}
\left[
- y_i \log(1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i})
- (1 - y_i)\log(1 + e^{\boldsymbol{\beta}^T \mathbf{x}_i})
\right]
$$

To find the MLE, set gradient to zero:

$$
\frac{\partial}{\partial \boldsymbol{\beta}} \log \mathcal{L}(\boldsymbol{\beta}) = 0
$$

This yields:

$$
- \sum_{i=1}^{n} \frac{\mathbf{x}_i y_i}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}_i}}
- \sum_{i=1}^{n} \frac{\mathbf{x}_i (1 - y_i)}{1 + e^{\boldsymbol{\beta}^T \mathbf{x}_i}}
= 0
$$

This equation **cannot be solved analytically**.

---

# 10. Numerical Optimization

Because no closed‑form solution exists, we use iterative optimization:

- Gradient Descent
- Stochastic Gradient Descent (SGD)
- Newton–Raphson / IRLS
- Quasi‑Newton (BFGS / L‑BFGS)

These methods iteratively update $\boldsymbol{\beta}$ to maximize the log‑likelihood.

---

# 11. Key Insight

- Logistic regression optimization is **convex**
- Converges to a **unique global optimum**
- Efficiently solvable in practice despite no closed‑form solution

---


## 🧠 Visual Key Points

- 🎯 Logistic Regression uses **Maximum Likelihood Estimation (MLE)**  
- 📊 Log‑Likelihood converts product → sum for numerical stability  
- 📉 Negative log‑likelihood = **Binary Cross‑Entropy Loss**  
- 📐 Objective is **concave → unique global optimum**  
- ⚙️ No closed‑form → solved via numerical optimization (GD / Newton / IRLS)  
- ✔ Content unchanged, styling only added  
