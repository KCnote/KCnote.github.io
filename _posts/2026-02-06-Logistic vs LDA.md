---
layout: post
title: "Logistic vs LDA"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Classification]
tags: [Machince Learning, Mathematics, ML, Supervised, Regression, Classification, Discriminant, LDA]
pin: false
math: true
mermaid: true
---

# 📘 Why Discriminant Analysis? & LDA vs Logistic Regression — Mathematical Notes

> 🎯 Clean blog-ready markdown  
> 📐 Full math preserved (no omission)  
> 🧠 Focus: stability, small sample behavior, multi-class support, and mathematical link to logistic regression  

---

# 7. Why Discriminant Analysis?

## 7.1 Stability with Well‑Separated Classes

When classes are far apart:

- Logistic regression parameters can become **unstable**
- LDA often remains **stable**

**Reason:**  
Few data points lie near the decision boundary → logistic likelihood becomes **flat**, making parameter estimation sensitive and unstable.

---

## 7.2 Small Sample Size

When the number of samples $$n$$ is small and the Gaussian assumption is reasonable:

- LDA often provides **more stable estimates**
- Logistic regression may suffer from **high variance**

This is because LDA uses a **generative model**, imposing structure through Gaussian assumptions.

---

## 7.3 Natural Multi‑Class Support

LDA naturally handles multiple classes $$K > 2$$ using discriminant functions:

$$
\hat{y} = \arg\max_k \delta_k(x)
$$

No need for one-vs-rest or other decomposition strategies.

---

# 8. LDA vs Logistic Regression — Mathematical Connection

For binary classification, LDA produces:

$$
\log \frac{p(y=1 \mid x)}{p(y=-1 \mid x)} = w^T x + b
$$

This has the **same functional form** as logistic regression.

---

## Key Difference: Estimation Objective

### Logistic Regression (Discriminative)

Maximizes **conditional likelihood**:

$$
\prod_i p(y_i \mid x_i)
$$

Directly models the posterior probability.

---

### LDA (Generative)

Maximizes **joint likelihood**:

$$
\prod_i p(x_i, y_i)
$$

Models class-conditional distribution and priors, then applies Bayes rule.

---

## Practical Consequence

Even though estimation methods differ, **decision boundaries are often similar**, especially when:

- Gaussian assumption is approximately valid
- Sample size is sufficiently large

---

## 8.1 Logistic Regression Approximating QDA

If we extend logistic regression with **quadratic features**:

$$
x_1^2, \quad x_2^2, \quad x_1 x_2
$$

then the model becomes:

$$
w^T \phi(x) + b
$$

where $$\phi(x)$$ includes quadratic terms.

This allows logistic regression to produce a **quadratic decision boundary**, similar to **QDA**.

---

# Final Insight

- LDA = generative, structured, stable with small data  
- Logistic = discriminative, flexible, fewer distribution assumptions  
- With linear features → both produce linear log‑odds  
- With quadratic features → logistic can mimic QDA  
- Choice depends on:
  - Data size
  - Distribution assumptions
  - Model flexibility vs stability trade‑off  
