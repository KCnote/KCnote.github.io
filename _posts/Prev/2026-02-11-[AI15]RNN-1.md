---
layout: post
title: "RNN - 1"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Model]
tags: [Artificial Intelligenc, Model, RNN]
pin: false
math: true
mermaid: true
---
# 📘 Sequential Modeling & RNN

---

# 🔹 1. Encoder Concept

We encode a sequence:

$$
x_1, x_2, \dots, x_T
$$

into a single vector:

$$
h_T
$$

Then apply classifier:

$$
\hat{y} = g(h_T)
$$

---

# 🔹 2. RNN Recurrence

Recurrent formula:

$$
h_t = f_W(h_{t-1}, x_t)
$$

Same parameters shared across time.

---

# 🔹 3. Vanilla RNN Cell

We perform linear transformation + nonlinearity:

$$
h_t = \tanh(W_{hh} h_{t-1} + W_{xh} x_t + b_h)
$$

Where:

- $W_{hh}$ : hidden-to-hidden weights  
- $W_{xh}$ : input-to-hidden weights  
- $b_h$ : bias  

All weights are shared across time.

---

# 🔹 4. Output Layer

## Binary classification

$$
\hat{y} = \sigma(W_{hy} h_T + b_y)
$$

## Regression

$$
\hat{y} = W_{hy} h_T + b_y
$$

---

# 🔹 5. Loss Function

Binary cross-entropy:

$$
\mathcal{L}
=
- \left[
y \log \hat{y}
+
(1-y) \log (1-\hat{y})
\right]
$$

---

# 🔹 6. Forward Pass

For $t = 1, \dots, T$:

$$
h_t = \tanh(W_{hh} h_{t-1} + W_{xh} x_t)
$$

Final prediction:

$$
\hat{y} = \sigma(W_{hy} h_T)
$$

---

# 🔹 7. Backpropagation Through Time (BPTT)

Because weights are shared:

$$
\frac{\partial \mathcal{L}}{\partial W_{hh}}
=
\sum_{t=1}^{T}
\frac{\partial \mathcal{L}}{\partial h_t}
\frac{\partial h_t}{\partial W_{hh}}
$$

Gradient w.r.t hidden state:

$$
\frac{\partial \mathcal{L}}{\partial h_{t-1}}
=
\frac{\partial \mathcal{L}}{\partial h_t}
\cdot
\frac{\partial h_t}{\partial h_{t-1}}
$$

---

# 🔹 8. Vanishing Gradient

Repeated multiplication:

$$
\frac{\partial \mathcal{L}}{\partial h_0}
=
\left( W_{hh}^T \right)^T
\cdots
\left( W_{hh}^T \right)^1
\frac{\partial \mathcal{L}}{\partial h_T}
$$

If eigenvalues < 1 → vanishing  
If eigenvalues > 1 → exploding  

---

# 🔹 9. Word Embedding

Each word is represented as:

$$
w \in \mathbb{R}^d
$$

---

# 🔹 10. Word2Vec (Skip-gram Objective)

$$
\mathcal{L}(\theta)
=
\prod_{t=1}^{N}
\prod_{\substack{-m \le j \le m \\ j \ne 0}}
p(w_{t+j} \mid w_t; \theta)
$$

---

# 🔹 11. GloVe Objective

$$
J
=
\sum_{i,j=1}^{V}
f(X_{ij})
\left(
w_i^T w_j + b_i + b_j - \log X_{ij}
\right)^2
$$

Where:

$$
X_{ij}
$$

is the co-occurrence count.

---

# 🔥 Core Equations Summary

Hidden update:

$$
h_t = \tanh(W_{hh} h_{t-1} + W_{xh} x_t)
$$

Prediction:

$$
\hat{y} = \sigma(W_{hy} h_T)
$$

Loss:

$$
\mathcal{L}
=
- \left[
y \log \hat{y}
+
(1-y) \log (1-\hat{y})
\right]
$$

---
