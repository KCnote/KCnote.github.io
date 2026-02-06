---
layout: post
title: "Discriminant Analysis"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Classification]
tags: [Machince Learning, Mathematics, ML, Supervised, Regression, Classification, Bayes, Discriminant]
pin: false
math: true
mermaid: true
---

# 📘 Classification Strategies → Discriminant Analysis (Gaussian) → 1D Full Derivation + MLE

> 🎯 Complete, no‑omission derivation  
> 📐 All math preserved, step‑by‑step expansion  
> 🧠 Generative classification → Bayes → Gaussian discriminant → full MLE derivation  

---

## 0. Setup and Notation

- Input: $$x$$ (scalar for now)
- Label: $$y \in \{+1,-1\}$$
- Prior:
  $$
  \alpha = p(y=+1), \quad 1-\alpha = p(y=-1)
  $$
- Likelihood (Gaussian):
  $$
  p(x|y=+1) = \mathcal{N}(x|\mu_1,\sigma_1^2), \quad
  p(x|y=-1) = \mathcal{N}(x|\mu_{-1},\sigma_{-1}^2)
  $$

Gaussian PDF:

$$
\mathcal{N}(x|\mu,\sigma^2)
=
\frac{1}{\sqrt{2\pi\sigma^2}}
\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$

---

## 1. Classification Strategies

We want a classifier minimizing expected loss:

$$
f^* = \arg\min_f \mathbb{E}_{p(x,y)}[\mathcal{L}(y,f(x))]
$$

Since $$p(x,y)$$ is unknown, we estimate from data.

### Two approaches

### (A) Generative

Estimate:

$$
p(x|y), \quad p(y)
$$

Then use Bayes:

$$
p(y|x) = \frac{p(x|y)p(y)}{p(x)}
$$

Examples: Bayes classifier, LDA, QDA.

---

### (B) Discriminative

Directly model:

$$
p(y|x)
$$

Example: Logistic regression.

---

## 2. Bayes Decision Rule

For 0‑1 loss:

$$
f^*(x) = \arg\max_y p(y|x)
$$

Using Bayes:

$$
f^*(x) = \arg\max_y p(x|y)p(y)
$$

Log‑ratio form:

$$
f^*(x) =
\text{sign}\left(
\log \frac{p(x|y=+1)p(y=+1)}
{p(x|y=-1)p(y=-1)}
\right)
$$

Substitute priors:

$$
f^*(x) =
\text{sign}\left(
\log \frac{p(x|y=+1)\alpha}
{p(x|y=-1)(1-\alpha)}
\right)
$$

---

## 3. Gaussian Likelihood Expansion

Assume Gaussian class conditionals:

$$
p(x|y=+1)=\mathcal{N}(x|\mu_1,\sigma_1^2), \quad
p(x|y=-1)=\mathcal{N}(x|\mu_{-1},\sigma_{-1}^2)
$$

Decision:

$$
f^*(x)=\text{sign}(g(x))
$$

where

$$
g(x)=\log\frac{\mathcal{N}(x|\mu_1,\sigma_1^2)\alpha}
{\mathcal{N}(x|\mu_{-1},\sigma_{-1}^2)(1-\alpha)}
$$

---

### Expand Gaussian Ratio

$$
\frac{\mathcal{N}(x|\mu_1,\sigma_1^2)}
{\mathcal{N}(x|\mu_{-1},\sigma_{-1}^2)}
=
\frac{\sqrt{\sigma_{-1}^2}}{\sqrt{\sigma_1^2}}
\exp\left(
-\frac{(x-\mu_1)^2}{2\sigma_1^2}
+
\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}
\right)
$$

Take log and include priors:

$$
g(x)=
-\frac12\log\left(\frac{\sigma_1^2}{\sigma_{-1}^2}\right)
-\frac{(x-\mu_1)^2}{2\sigma_1^2}
+\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}
+
\log\left(\frac{\alpha}{1-\alpha}\right)
$$

---

## 4. Maximum Likelihood Estimation (MLE)

Given training set $$\{(x_i,y_i)\}_{i=1}^n$$.

Let:

$$
\mathcal{I}_1 = \{i:y_i=+1\}, \quad
\mathcal{I}_{-1} = \{i:y_i=-1\}
$$

Sizes:

$$
n_1 = |\mathcal{I}_1|, \quad n_{-1}=|\mathcal{I}_{-1}|, \quad n=n_1+n_{-1}
$$

---

### 4.1 Prior MLE

$$
\ell(\alpha)=n_1\log\alpha + n_{-1}\log(1-\alpha)
$$

Derivative:

$$
\frac{d\ell}{d\alpha}=\frac{n_1}{\alpha}-\frac{n_{-1}}{1-\alpha}=0
$$

Solution:

$$
\hat{\alpha}=\frac{n_1}{n}
$$

---

### 4.2 Mean MLE

$$
\hat{\mu}_1 = \frac{1}{n_1}\sum_{i\in\mathcal{I}_1}x_i, \quad
\hat{\mu}_{-1} = \frac{1}{n_{-1}}\sum_{i\in\mathcal{I}_{-1}}x_i
$$

---

### 4.3 Variance MLE

$$
\hat{\sigma}_1^2 = \frac{1}{n_1}\sum_{i\in\mathcal{I}_1}(x_i-\hat{\mu}_1)^2
$$

$$
\hat{\sigma}_{-1}^2 = \frac{1}{n_{-1}}\sum_{i\in\mathcal{I}_{-1}}(x_i-\hat{\mu}_{-1})^2
$$

Note: MLE uses denominator $$n_k$$ (not $$n_k-1$$).

---

## 5. Final Classifier

Plug parameters into discriminant:

$$
g(x)=
-\frac12\log\left(\frac{\sigma_1^2}{\sigma_{-1}^2}\right)
-\frac{(x-\mu_1)^2}{2\sigma_1^2}
+\frac{(x-\mu_{-1})^2}{2\sigma_{-1}^2}
+
\log\left(\frac{\alpha}{1-\alpha}\right)
$$

Prediction:

$$
f^*(x)=\text{sign}(g(x))
$$

---

## Appendix — Log Expansion Form

$$
\log\mathcal{N}(x|\mu,\sigma^2)
=
-\frac12\log(2\pi\sigma^2)
-\frac{(x-\mu)^2}{2\sigma^2}
$$

Substituting into log‑ratio yields the same discriminant expression.

---

✍️ *Complete Gaussian discriminant + full MLE derivation (no omission)*
