---
layout: post
title: "Statistical Interpret for MLE"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Mathematics]
tags: [Machince Learning, Mathematics, Bias, Variance, Likelihood, MLE, Linear Regression, Statistics]
pin: false
math: true
mermaid: true
---

# 📊 Bias, Variance, and Mean Squared Error (MSE)

---

## 1. Definitions

### Bias
Bias is the systematic error of an estimator.

$$
\mathrm{Bias}(\hat{\theta}) = \mathbb{E}[\hat{\theta}] - \theta
$$

---

### Variance
Variance measures the expected squared deviation from the mean.

$$
\mathrm{Var}(\hat{\theta}) = \mathbb{E}\left[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2\right]
$$

---

### Mean Squared Error (MSE)

Mean Squared Error measures the expected squared difference between the estimator and the true value.

$$
\mathrm{MSE}(\hat{\theta}) = \mathbb{E}\left[\|\hat{\theta} - \theta\|^2\right]
= \mathbb{E}\left[\sum_{j=1}^{d} (\hat{\theta}_j - \theta_j)^2 \right]
$$

---

## 2. Bias–Variance Decomposition Theorem

**Theorem:** The MSE of an estimator equals the sum of its variance and squared bias.

$$
\mathrm{MSE}(\hat{\theta}) = \mathrm{tr}(\mathrm{Var}(\hat{\theta})) + \|\mathrm{Bias}(\hat{\theta})\|^2
$$

---

## 3. Proof (Scalar Case)

$$
\mathbb{E}\left[(\hat{\theta} - \theta)^2\right]
= \mathbb{E}\left[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2\right]
+ (\mathbb{E}[\hat{\theta}] - \theta)^2
$$

Expand:

$$
\mathbb{E}\left[(\hat{\theta} - \theta)^2\right]
= \mathbb{E}\left[(\hat{\theta} - \mathbb{E}[\hat{\theta}] + \mathbb{E}[\hat{\theta}] - \theta)^2\right]
$$

$$
= \mathbb{E}\left[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2
+ 2(\hat{\theta} - \mathbb{E}[\hat{\theta}])(\mathbb{E}[\hat{\theta}] - \theta)
+ (\mathbb{E}[\hat{\theta}] - \theta)^2 \right]
$$

$$
= \mathbb{E}[(\hat{\theta} - \mathbb{E}[\hat{\theta}])^2]
+ 2\mathbb{E}[(\hat{\theta} - \mathbb{E}[\hat{\theta}])](\mathbb{E}[\hat{\theta}] - \theta)
+ (\mathbb{E}[\hat{\theta}] - \theta)^2
$$

Since:

$$
\mathbb{E}[\hat{\theta} - \mathbb{E}[\hat{\theta}]] = 0
$$

$$
\mathrm{MSE}(\hat{\theta}) = \mathrm{Var}(\hat{\theta}) + \mathrm{Bias}(\hat{\theta})^2
$$

---

## 4. MSE of Linear Regression

Using bias–variance decomposition:

$$
\mathrm{MSE}(\hat{\beta} \mid X)
= \mathrm{Var}(\hat{\beta} \mid X) + \mathrm{Bias}(\hat{\beta} \mid X)^2
$$

For Ordinary Least Squares (OLS):

- Linear regression is unbiased:

$$
\mathrm{Bias}(\hat{\beta} \mid X) = 0
$$

- Variance:

$$
\mathrm{Var}(\hat{\beta} \mid X) = \sigma^2 (X^T X)^{-1}
$$

Therefore:

$$
\mathrm{MSE}(\hat{\beta} \mid X) = \sigma^2 (X^T X)^{-1}
$$

---

## Notes

- Linear regression estimator is unbiased.
- As sample size increases, variance decreases.
- With infinite data, variance converges to 0.
- OLS (MLE) is the **Best Linear Unbiased Estimator (BLUE)** under Gauss–Markov assumptions.
