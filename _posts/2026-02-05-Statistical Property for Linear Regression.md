---
layout: post
title: "Statistical Properties for Linear Regression"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Linear Regression]
tags: [Machince Learning, Mathematics, Likelihood, MLE, Linear Regression, Statistics]
pin: false
math: true
mermaid: true
---

# 📊 Statistical Properties

---

## Model Assumption

Assume the linear model:

$$
Y = X\beta + \epsilon, \quad \epsilon \sim \mathcal{N}(0, \sigma^2 I)
$$

Then:

$$
Y \mid X \sim \mathcal{N}(X\beta, \sigma^2 I)
$$

---

## 1. Expectation of $$ \hat{\beta} $$

$$
\hat{\beta} = (X^T X)^{-1} X^T Y
$$

$$
\mathbb{E}[\hat{\beta} \mid X] 
= \mathbb{E}[(X^T X)^{-1} X^T Y \mid X]
$$

$$
= (X^T X)^{-1} X^T \mathbb{E}[Y \mid X]
$$

$$
= (X^T X)^{-1} X^T (X\beta)
$$

$$
= (X^T X)^{-1} (X^T X)\beta
$$

$$
\boxed{\mathbb{E}[\hat{\beta} \mid X] = \beta}
$$

---

## 2. Variance of $$ \hat{\beta} $$

$$
\mathrm{Var}[\hat{\beta} \mid X]
= \mathrm{Var}[(X^T X)^{-1} X^T Y \mid X]
$$

Using:

$$
\mathrm{Var}[AX] = A \mathrm{Var}[X] A^T,
\qquad
\mathrm{Var}[Y \mid X] = \sigma^2 I
$$

$$
= (X^T X)^{-1} X^T \mathrm{Var}[Y \mid X] ((X^T X)^{-1} X^T)^T
$$

$$
= (X^T X)^{-1} X^T (\sigma^2 I) X (X^T X)^{-1}
$$

$$
= \sigma^2 (X^T X)^{-1} X^T X (X^T X)^{-1}
$$

$$
\boxed{\mathrm{Var}[\hat{\beta} \mid X] = \sigma^2 (X^T X)^{-1}}
$$

---

## Interpretation

- **unbiased**: $$ \hat{\beta} $$
  
- Variance decreases as $$ X^T X $$ increases
- More data ⇒ **smaller variance ⇒ more stable estimator**

---

## Summary

| Property | Result |
|----------|--------|
| Expectation | $$ \mathbb{E}[\hat{\beta} \mid X] = \beta $$ |
| Bias | 0 (Unbiased) |
| Variance | $$ \sigma^2 (X^T X)^{-1} $$ |
| Effect of More Data | Smaller variance |
