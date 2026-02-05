---
layout: post
title: "Likelihood and MLE"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Mathematics]
tags: [Machince Learning, Mathematics, Likelihood, MLE, Hessian]
pin: false
math: true
mermaid: true
---

# 📊 Likelihood, Log-Likelihood, and Maximum Likelihood Estimation (MLE)

---

# Likelihood

**Definition**  
Likelihood is the joint probability (or probability density) of the observed data, viewed as a function of the parameters of a statistical model.

Intuitively, it represents how probable the observed data is under a given parameter.

$$
L(\theta; x_1, \ldots, x_n) = p_\theta(x_1, \ldots, x_n)
$$

---

## i.i.d. Assumption

If samples are independent and identically distributed:

$$
L(\theta; x_1, \ldots, x_n)
= p_\theta(x_1)\cdot p_\theta(x_2)\cdots p_\theta(x_n)
$$

$$
L(\theta; x_1, \ldots, x_n)
= \prod_{i=1}^{n} p_\theta(x_i)
$$

---

# Log-Likelihood

Log-likelihood is the logarithm of the likelihood:

$$
\ell(\theta; x_1, \ldots, x_n)
= \log \prod_{i=1}^{n} p_\theta(x_i)
= \sum_{i=1}^{n} \log p_\theta(x_i)
$$

---

## Equivalent Optimization

The parameter that maximizes likelihood also maximizes log-likelihood:

$$
\arg\max_x f(x) = \arg\max_x \log f(x)
$$

---

# Why Use Log-Likelihood?

### 1. Numerical Stability
Multiplying many probabilities produces extremely small numbers (underflow).  
Taking log converts products into sums.

---

### 2. Same Optimum
Log is strictly monotonic, so maximizing likelihood or log-likelihood gives the same parameter.

---

### 3. Easier Optimization
Sums are easier to differentiate than products.

---

# Maximum Likelihood Estimator (MLE)

**Definition**  
MLE estimates parameters by maximizing the likelihood so that the observed data is most probable under the assumed model.

$$
\hat{\theta}(x_1, \ldots, x_n) = \arg\max_{\theta} L(\theta; x_1, \ldots, x_n)
$$

$$
= \arg\max_{\theta} \ell(\theta; x_1, \ldots, x_n)
$$

$$
= \arg\max_{\theta} \sum_{i=1}^{n} \log p_{\theta}(x_i)
$$

MLE is the parameter \( \theta \) that gives the highest probability to the observed data.

---

## Closed-Form Solution

If parameters can be solved analytically → **Closed-form solution**.  
Otherwise → numerical optimization.

---

# MLE — Optimization View

## First-Order Condition

A function reaches a local optimum when its gradient is zero:

$$
\nabla \ell(\theta) = 0
$$

Component-wise:

$$
\frac{\partial \ell(\theta)}{\partial \theta_j} = 0
\quad \text{for } j = 1, \dots, d
$$

---

## Second-Order Condition

To ensure the solution is a **maximum**, check the Hessian:

$$
H(\theta) = \left[ \frac{\partial^2 \ell(\theta)}{\partial \theta_i \, \partial \theta_j} \right]
$$

A local maximum occurs when the Hessian is **negative definite**:

$$
v^T H(\theta) v < 0 \quad \text{for any nonzero vector } v
$$

---

# Key Takeaways

$$
\boxed{
\text{Likelihood → Log-Likelihood → Optimization → MLE}
}
$$

- Likelihood measures how probable observed data is under parameters  
- Log-likelihood improves numerical stability and simplifies optimization  
- MLE maximizes likelihood (or log-likelihood)  
- Solve gradient = 0 to find candidates  
- Check Hessian < 0 to confirm maximum  
