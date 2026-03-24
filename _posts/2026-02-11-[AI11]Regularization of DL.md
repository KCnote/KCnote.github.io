---
layout: post
title: "Regularization"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Optimization]
tags: [Artificial Intelligenc, Optimization, Learning Rate]
pin: false
math: true
mermaid: true
---
# 🧠 Regularization & Dropout (GitHub Safe Version)

---

# 1️⃣ Training Pipeline (Big Picture)

Machine Learning is data-driven:

1. Design model (e.g., neural network)
2. Initialize parameters $W$
3. Feed training data $x$
4. Predict $\hat{y}$
5. Compute loss $\mathcal{L}(\hat{y}, y)$
6. Update $W$
7. Repeat

---

# 2️⃣ Overfitting

Overfitting occurs when:

- Training loss decreases
- Validation loss increases

Formally:

Let

$$
\mathcal{L}_{train}(t)
$$

and

$$
\mathcal{L}_{val}(t)
$$

If

$$
\mathcal{L}_{train} \downarrow \quad \text{but} \quad \mathcal{L}_{val} \uparrow
$$

we have overfitting.

---

# 3️⃣ Regularization

Original objective:

$$
\min_{\theta} \mathcal{L}(\theta)
$$

Regularized objective:

$$
\min_{\theta} \mathcal{L}(\theta) + \lambda \Omega(\theta)
$$

Where:

- $\lambda$ = regularization strength
- $\Omega(\theta)$ = penalty term

---

# 4️⃣ Linear Regression

Loss:

$$
\min_{\theta} (Y - X\theta)^T (Y - X\theta)
$$

Closed form:

$$
\hat{\theta} = (X^T X)^{-1} X^T Y
$$

---

# 5️⃣ Ridge Regression (L2)

Loss:

$$
\min_{\theta} (Y - X\theta)^T (Y - X\theta) + \lambda ||\theta||_2^2
$$

Where:

$$
||\theta||_2^2 = \theta^T \theta = \sum_i \theta_i^2
$$

Closed form:

$$
\hat{\theta} = (X^T X + \lambda I)^{-1} X^T Y
$$

If $\lambda = 0$, it becomes standard linear regression.

---

# 6️⃣ Weight Decay in Neural Networks

L2 penalty:

$$
\Omega(W) = \sum_i \sum_j W_{ij}^2
$$

Full objective:

$$
\mathcal{L}(W) + \lambda \sum_i \sum_j W_{ij}^2
$$

L1 penalty:

$$
\Omega(W) = \sum_i \sum_j |W_{ij}|
$$

---

# 7️⃣ Early Stopping

Choose stopping time:

$$
t^* = \arg\min_t \mathcal{L}_{val}(t)
$$

Do NOT use test set for stopping.

---

# 8️⃣ Dropout

For hidden activation vector $h$:

Sample mask:

$$
m_i \sim \text{Bernoulli}(p)
$$

Apply:

$$
\tilde{h} = m \odot h
$$

---

# 9️⃣ Expected Value Problem

Because:

$$
E[m_i] = p
$$

We get:

$$
E[\tilde{h}_i] = p E[h_i]
$$

This changes scale.

---

# 🔟 Inverted Dropout (Correct Version)

During training:

$$
\tilde{h} = \frac{m \odot h}{p}
$$

Then:

$$
E[\tilde{h}_i] = E[h_i]
$$

So inference requires no scaling.

---

# 1️⃣1️⃣ Cutout

Randomly choose rectangle region $R$ and set:

$$
I(u,v) = 0 \quad \forall (u,v) \in R
$$

Improves robustness to occlusion.

---

# 1️⃣2️⃣ Final Summary

Regularized objective:

$$
\mathcal{L} + \lambda \Omega
$$

Ridge solution:

$$
(X^T X + \lambda I)^{-1} X^T Y
$$

Dropout training:

$$
\tilde{h} = \frac{m \odot h}{p}
$$

Early stopping:

$$
t^* = \arg\min_t \mathcal{L}_{val}(t)
$$
