---
layout: post
title: "MLE for Linear Regression"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Mathematics]
tags: [Machince Learning, Mathematics, Likelihood, MLE, Linear Regression]
pin: false
math: true
mermaid: true
---

# 📊 MLE for Linear Regression — Equivalence to Least Squares

---

## Model

Assume a linear model with Gaussian noise:

$$
y_i = \beta^T x_i + \epsilon_i, \quad \epsilon_i \sim \mathcal{N}(0,\sigma^2)
$$

Then the conditional distribution is:

$$
y_i \mid x_i \sim \mathcal{N}(\beta^T x_i, \sigma^2)
$$

---

## Log-Likelihood

$$
\ell(\beta) = \sum_{i=1}^{n} \log p(y_i \mid x_i)
= \sum_{i=1}^{n} \log \left( \frac{1}{\sigma\sqrt{2\pi}} 
\exp\left(-\frac{1}{2}\frac{(y_i - \beta^T x_i)^2}{\sigma^2}\right) \right)
$$

Simplify:

$$
\ell(\beta) 
= \sum_{i=1}^{n} \log \frac{1}{\sigma\sqrt{2\pi}} 
- \frac{1}{2\sigma^2} \sum_{i=1}^{n} (y_i - \beta^T x_i)^2
$$

The first term does not depend on $$ \beta $$, so it can be ignored when optimizing.

---

## MLE Optimization

$$
\arg\max_{\beta} \ell(\beta)
= \arg\min_{\beta} \sum_{i=1}^{n} (y_i - \beta^T x_i)^2
$$

Thus, **MLE = Least Squares** under Gaussian noise.

---

## Conclusion

Under assumptions:

- Linear model: $$ y = \beta^T x + \epsilon $$
- Gaussian noise: $$ \epsilon \sim \mathcal{N}(0,\sigma^2) $$

$$
\boxed{\text{MLE = Least Squares}}
$$

---

# 📈 Linear Regression with Matrix Notation

## Objective

$$
\hat{\beta} = \arg\min_{\beta} (Y - X\beta)^T (Y - X\beta)
$$

---

## Step 1. Define RSS

$$
RSS(\beta) = (Y - X\beta)^T (Y - X\beta)
$$

---

## Step 2. Derivative w.r.t. $$ \beta $$

$$
\frac{\partial RSS(\beta)}{\partial \beta}
= -X^T(Y - X\beta) - (Y - X\beta)^T X
$$

Using $$ a^T b = b^T a $$:

$$
= -2X^T(Y - X\beta)
$$

---

## Step 3. Optimality Condition

$$
-2X^T(Y - X\beta) = 0
$$

$$
X^T X \beta = X^T Y
$$

---

## Step 4. Solution (Normal Equation)

$$
\boxed{\beta = (X^T X)^{-1} X^T Y}
$$

---

## Dimension Check

| Term | Dimension |
|------|-----------|
| $$X$$ | $$n \times d$$ |
| $$Y$$ | $$n \times 1$$ |
| $$X^T X$$ | $$d \times d$$ |
| $$(X^T X)^{-1}$$ | $$d \times d$$ |
| $$X^T Y$$ | $$d \times 1$$ |
| $$\beta$$ | $$d \times 1$$ |

---

## Scalar Case (Single Feature)

$$
\beta_1 =
\frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}
{\sum_{i=1}^{n} (x_i - \bar{x})^2}
$$

---

## Summary

- Linear regression minimizes RSS
- MLE with Gaussian noise equals Least Squares
- Solve gradient = 0 → Normal Equation

$$
\beta = (X^T X)^{-1} X^T Y
$$

---

## Notes

- Requires $$X^T X$$ invertible
- Otherwise use pseudo-inverse or Ridge regression
