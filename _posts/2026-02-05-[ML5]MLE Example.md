---
layout: post
title: "MLE Example"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Mathematics]
tags: [Machince Learning, Mathematics, Likelihood, MLE]
pin: false
math: true
mermaid: true
---

# 📊 Maximum Likelihood Estimation (MLE) — Bernoulli & Gaussian

---

# MLE Example: Bernoulli Distribution

## Bernoulli Model

For a Bernoulli random variable $$x \in \{0,1\}$$:

$$
p_\theta(x) = \theta^x (1-\theta)^{1-x}
$$

- $$x \in \{0,1\}$$
- $$\theta = \mathbb{P}(x=1)$$

---

## Log-Likelihood

$$
\ell(\theta)
= \sum_{i=1}^{n} x_i \log \theta
+ \sum_{i=1}^{n} (1 - x_i) \log(1 - \theta)
$$

---

## First Derivative

$$
\frac{\partial \ell(\theta)}{\partial \theta}
= \frac{1}{\theta} \sum_{i=1}^{n} x_i
- \frac{1}{1-\theta} \sum_{i=1}^{n} (1 - x_i)
= 0
$$

$$
(1-\theta) \sum_{i=1}^{n} x_i
= \theta \sum_{i=1}^{n} (1 - x_i)
$$

$$
\sum_{i=1}^{n} x_i = n\theta
$$

$$
\hat{\theta} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

---

## Second Derivative

$$
\frac{\partial^2 \ell(\theta)}{\partial \theta^2}
= -\frac{1}{\theta^2} \sum_{i=1}^{n} x_i
- \frac{1}{(1-\theta)^2} \sum_{i=1}^{n} (1 - x_i)
$$

This is negative ⇒ maximum.

---

## Result

$$
\hat{\theta} = \bar{x}
$$

---

# MLE Example: Gaussian Distribution

## Gaussian Model

$$
p_{\theta}(x) = \frac{1}{\sigma \sqrt{2\pi}} 
\exp\left(-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2\right)
$$

Parameters: $$\theta = (\mu, \sigma)$$

---

## Log-Likelihood

$$
\ell(\mu, \sigma)
= -n \log \sigma
- \frac{n}{2} \log (2\pi)
- \frac{1}{2\sigma^2} \sum_{i=1}^{n} (x_i - \mu)^2
$$

---

## First Derivative w.r.t. $$\mu$$

$$
\frac{\partial \ell}{\partial \mu}
= \sum_{i=1}^{n} \frac{x_i - \mu}{\sigma^2} = 0
$$

$$
\sum_{i=1}^{n} x_i = n\mu
$$

$$
\hat{\mu} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

---

## First Derivative w.r.t. $$\sigma$$

$$
\frac{\partial \ell}{\partial \sigma}
= -\frac{n}{\sigma}
+ \frac{1}{\sigma^3} \sum_{i=1}^{n} (x_i - \mu)^2 = 0
$$

$$
\sigma^2 = \frac{1}{n} \sum_{i=1}^{n} (x_i - \mu)^2
$$

---

## Final Result

$$
\hat{\mu} = \frac{1}{n} \sum_{i=1}^{n} x_i
$$

$$
\hat{\sigma}^2 = \frac{1}{n} \sum_{i=1}^{n} (x_i - \hat{\mu})^2
$$

---

## Key Takeaways

- Bernoulli MLE = sample mean
- Gaussian mean MLE = sample mean
- Gaussian variance MLE uses $$1/n$$, not $$1/(n-1)$$
