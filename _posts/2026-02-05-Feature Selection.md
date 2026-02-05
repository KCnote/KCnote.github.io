---
layout: post
title: "Feature Selection"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Mathematics]
tags: [Machince Learning, Mathematics, Variable, Linear Regression, Statistics, Model]
pin: false
math: true
mermaid: true
---

# 📊 Feature Selection and Stepwise Methods

---

# Feature Selection

## Motivation

- Sometimes the inverse matrix does not exist.
- Sometimes the inverse matrix becomes numerically unstable.

Example:

$$
x + y = 3
$$

$$
2x + 2y \approx 6.0000000000000001
$$

This leads to a near-singular matrix and unstable coefficient estimates.

In machine learning, instead of manually choosing variables, we can let the model evaluate which variables are useful.

---

# Best Subset Selection

## Idea

Compute least squares for **all possible subsets of predictors**, then choose the best model based on a criterion balancing **training error and model size**.

---

## Procedure

### 1. Null Model

- Let $$M_0$$ be the model with **no predictors**.
- It predicts the **sample mean**.

---

### 2. For $$k = 1,2,\dots,p$$

- Fit all models containing **exactly $$k$$ predictors**.

Number of models:

$$
\binom{p}{k} = \frac{p!}{k!(p-k)!}
$$

- Choose the best $$M_k$$ using:
  - Smallest **RSS**
  - Largest **$$R^2$$**

---

### 3. Final Model Selection

Choose the best model among $$M_0, M_1, \dots, M_p$$ using:

- Prediction error
- $$R^2$$
- Adjusted $$R^2$$
- AIC / BIC / $$C_p$$

---

## Key Takeaway

- Searches **all possible subsets**
- Finds globally best model
- Computationally expensive

---

# Issues with Best Subset Selection

## 1. Computational Limitation

Total number of models:

$$
2^p
$$

Example: $$p = 40$$ → over **1 billion models**.

---

## 2. Statistical Problems

- Large search space → **overfitting**
- High variance estimates
- Curse of dimensionality

---

## 3. Practical Alternative

**Stepwise methods** explore fewer models while keeping good performance.

---

# Forward Stepwise Selection

## Idea

Start with no predictors and add **one variable at a time** that gives the largest improvement.

---

## Procedure

### 1. Null Model

Start with $$M_0$$.

---

### 2. For $$k = 0,1,\dots,p-1$$

- Consider $$p-k$$ models by adding one new predictor.
- Choose best model $$M_{k+1}$$ using:
  - Smallest RSS
  - Largest $$R^2$$

---

### 3. Final Selection

Choose best among $$M_0,\dots,M_p$$ using:

- Prediction error
- Adjusted $$R^2$$
- AIC / BIC / $$C_p$$

---

## Key Takeaway

- Much faster than best subset
- Searches restricted model space
- May miss global optimum

---

## Complexity

Stepwise:

$$
O(p^2)
$$

Best subset:

$$
O(2^p)
$$

---

# Backward Stepwise Selection

## Idea

Start from the **full model** and remove the least useful predictor iteratively.

---

## Procedure

### 1. Full Model

Start with $$M_p$$ (all predictors).

---

### 2. For $$k = p,p-1,\dots,1$$

- Remove one predictor from $$M_k$$
- Choose best $$M_{k-1}$$ using:
  - Smallest RSS
  - Largest $$R^2$$

---

### 3. Final Selection

Choose best among $$M_0,\dots,M_p$$ using:

- Prediction error
- Adjusted $$R^2$$
- AIC / BIC / $$C_p$$

---

## Key Takeaway

- Starts with full model
- Efficient vs best subset
- Requires $$n > p$$

---

# Forward Stagewise Selection

## Idea

Add predictors gradually **without refitting previous coefficients**.  
A predictor may be selected multiple times.

---

## Procedure

### 1. Null Model

Start with $$M_0$$.

---

### 2. For $$k = 1,2,\dots,K$$

- For each predictor $$j$$:
  - Take a small step in coefficient $$\beta_j$$
- Choose best $$M_k$$ using:
  - Smallest RSS
  - Largest $$R^2$$

---

### 3. Final Selection

Choose best among $$M_0,\dots,M_K$$ using:

- Prediction error
- Adjusted $$R^2$$
- AIC / BIC / $$C_p$$

---

## Key Takeaway

- Coefficients updated gradually
- Predictor may be selected multiple times
- Related to **Lasso and boosting**
- More stable than forward stepwise

---

# Comparison

| Method    | Coefficient Update                | Flexibility | Greediness |
|-----------|----------------------------------|-------------|-----------|
| Stepwise  | Refit all coefficients each step | Higher      | Greedy    |
| Stagewise | Update only one coefficient step | Lower       | More greedy |

