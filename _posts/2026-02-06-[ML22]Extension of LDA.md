---
layout: post
title: "Extension of Linear Discriminant Analysis"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Machince Learning - Foundation]
tags: [Machince Learning, Mathematics, ML, Supervised, Regression, Classification, Bayes, Discriminant, LDA]
pin: false
math: true
mermaid: true
---

# 📘 Discriminant Analysis (Multi‑Dimensional, Multi‑Class, Softmax, LDA vs Logistic) — Full Mathematical Notes

> 🎯 Complete reconstruction of the full mathematical content (no omission)  
> 📐 Covers: Multivariate Gaussian → Discriminant → Multi‑class LDA → Softmax → Interpretation → LDA vs Logistic  
> 🧠 Clean, blog‑ready structured markdown  

---

# 1. Multivariate Gaussian Distribution

For class $$k$$ in $$d$$‑dimensional feature space:

$$
p(x \mid y=k) = \mathcal{N}(x \mid \mu_k, \Sigma_k)
$$

Multivariate Gaussian density:

$$
\mathcal{N}(x \mid \mu, \Sigma)
= (2\pi)^{-d/2} |\Sigma|^{-1/2}
\exp\left(
-\frac12 (x-\mu)^T \Sigma^{-1} (x-\mu)
\right)
$$

Where:

- feature vector  $$x \in \mathbb{R}^d$$
- mean vector of class k $$\mu_k$$
- covariance matrix of class k $$\Sigma_k$$  

---

# 2. Discriminant Function (Multivariate Gaussian)

Using Bayes rule with Gaussian likelihood:

$$
\delta_k(x) = \log p(x \mid y=k) + \log \alpha_k
$$

Substitute Gaussian density:

$$
\delta_k(x)
=
-\frac12 \log |\Sigma_k|
-\frac12 (x-\mu_k)^T \Sigma_k^{-1} (x-\mu_k)
+ \log \alpha_k
$$

---

## 2.1 Expand the Quadratic Form

$$
(x-\mu_k)^T \Sigma_k^{-1} (x-\mu_k)
=
x^T \Sigma_k^{-1} x
- 2\mu_k^T \Sigma_k^{-1} x
+ \mu_k^T \Sigma_k^{-1} \mu_k
$$

Substitute into discriminant:

$$
\delta_k(x)
=
-\frac12 \log |\Sigma_k|
-\frac12 x^T \Sigma_k^{-1} x
+ \mu_k^T \Sigma_k^{-1} x
-\frac12 \mu_k^T \Sigma_k^{-1} \mu_k
+ \log \alpha_k
$$

---

# 3. LDA Assumption (Shared Covariance)

Linear Discriminant Analysis assumes:

$$
\Sigma_k = \Sigma \quad \forall k
$$

Then the term:

$$
-\frac12 x^T \Sigma^{-1} x
$$

is identical across classes → cancels when comparing classes.

Thus the discriminant simplifies to:

$$
\delta_k(x)
=
x^T \Sigma^{-1} \mu_k
-\frac12 \mu_k^T \Sigma^{-1} \mu_k
+ \log \alpha_k
$$

👉 This expression is **linear in x** → produces **linear decision boundaries**.

---

# 4. Multi‑Class Classification

For $$K$$ classes, choose the class with maximum discriminant score:

$$
\hat{y} = \arg\max_k \delta_k(x)
$$

Equivalent pairwise comparison:

For any classes $$i,j$$:

$$
\delta_i(x) - \delta_j(x) > 0 \Rightarrow \text{choose class } i
$$

Thus classification is based on comparing linear functions of $$x$$.

---

# 5. From Discriminant Scores to Posterior Probabilities (Softmax)

We can convert discriminant scores into posterior probabilities:

$$
P(Y=k \mid X=x)
=
\frac{e^{\delta_k(x)}}
{\sum_{\ell=1}^{K} e^{\delta_\ell(x)}}
$$

This is the **Softmax function**.

Prediction rule:

$$
\hat{y} = \arg\max_k P(Y=k \mid X=x)
$$

---

# 6. LDA Example Interpretation

Suppose equal class priors:

$$
\alpha_1 = \alpha_2 = \alpha_3
$$

Then only the Gaussian likelihood term affects classification.

Geometric interpretation:

- Each class corresponds to a Gaussian distribution
- Contours of equal density are ellipses (in 2D) or ellipsoids (in higher dimension)
- Shared covariance → ellipses have identical shape/orientation
- Decision boundaries between classes are **linear hyperplanes**
- True Bayes boundary may be nonlinear if covariance differs (QDA)

---

# 7. Why Discriminant Analysis?

Discriminant Analysis is a **generative approach**:

- Models full distribution $$p(x|y)$$ and priors $$p(y)$$
- Provides probabilistic interpretation
- Can work well with small data when Gaussian assumption holds
- Allows analytical understanding of decision boundary
- Naturally extends to multi‑class problems

---

# 8. LDA vs Logistic Regression — Mathematical Connection

## Logistic Regression (Discriminative)

Logistic regression directly models posterior:

$$
P(y=k \mid x)
$$

For binary case:

$$
\log \frac{P(y=1|x)}{P(y=-1|x)} = w^T x + b
$$

This is **linear in x**.

---

## LDA (Generative)

Under Gaussian with shared covariance:

$$
\delta_k(x)
=
x^T \Sigma^{-1} \mu_k
-\frac12 \mu_k^T \Sigma^{-1} \mu_k
+ \log \alpha_k
$$

Binary case difference:

$$
\delta_1(x) - \delta_2(x) = w^T x + b
$$

Thus LDA also produces a **linear decision rule**.

---

## Key Insight

- Logistic Regression = discriminative model
- LDA = generative model
- Under shared covariance assumption, both yield **linear log‑odds**
- Logistic directly models $$p(y|x)$$
- LDA models $$p(x|y)$$ then applies Bayes rule
- With large data → both often produce similar boundaries
- With small data → LDA may be more stable

---

# Final Summary

- Multivariate Gaussian defines class‑conditional distribution
- Discriminant function combines likelihood and prior
- Shared covariance → LDA → linear boundary
- Different covariance → QDA → quadratic boundary
- Softmax converts discriminant scores to probabilities
- Logistic and LDA are closely related linear classifiers under certain assumptions
