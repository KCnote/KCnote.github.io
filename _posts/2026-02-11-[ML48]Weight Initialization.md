---
layout: post
title: "Weight Initialization"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Optimization]
tags: [Artificial Intelligenc, Optimization, Weight, Initialization]
pin: false
math: true
mermaid: true
---

# 🔥 Weight Initialization in Deep Neural Networks

---

# 1️⃣ Small Gaussian Random Initialization

Initialize weights as:

$$
W_{ij} \sim \mathcal{N}(0, \sigma^2)
$$

with

$$
\sigma = 0.01
$$

In practice:

```python
W = 0.01 * np.random.randn(d_in, d_out)
```

---

## ❗ Problem in Deep Networks

Let:

$$
y = Wx
$$

where

$$
x \in \mathbb{R}^{d_{in}}, \quad
W \in \mathbb{R}^{d_{out} \times d_{in}}
$$

Each output unit:

$$
y_i = \sum_{j=1}^{d_{in}} W_{ij} x_j
$$

If weights are very small:

$$
W_{ij} \approx 0
$$

Then:

$$
y_i \approx 0
$$

For tanh activation:

$$
\tanh(0) = 0
$$

Gradients:

$$
\frac{\partial \tanh(z)}{\partial z} = 1 - \tanh^2(z)
$$

If activations shrink layer by layer:

$$
x^{(l)} \to 0
$$

Then:

$$
\frac{\partial L}{\partial W} \propto x^{(l)}
$$

So:

$$
\frac{\partial L}{\partial W} \to 0
$$

🚨 No learning (vanishing gradients)

---

# 2️⃣ Large Gaussian Initialization

Now increase scale:

```python
W = 5 * np.random.randn(d_in, d_out)
```

Then:

$$
y_i = \sum W_{ij} x_j
$$

becomes very large.

For tanh:

$$
\tanh(z) \to \pm 1
$$

Derivative:

$$
\frac{d}{dz}\tanh(z) = 1 - \tanh^2(z)
$$

If:

$$
\tanh^2(z) \approx 1
$$

Then:

$$
1 - \tanh^2(z) \approx 0
$$

Again:

$$
\frac{\partial L}{\partial W} \to 0
$$

🚨 Saturation → No learning

---

# 3️⃣ Xavier Initialization (Glorot)

Goal:

Maintain

$$
\text{Var}(y) = \text{Var}(x)
$$

---

## 📌 Derivation

Start from:

$$
y_i = \sum_{j=1}^{d_{in}} W_{ij} x_j
$$

Variance:

$$
\text{Var}(y_i)
= \text{Var}\left(\sum_{j=1}^{d_{in}} W_{ij} x_j \right)
$$

Assuming independence:

$$
= \sum_{j=1}^{d_{in}} \text{Var}(W_{ij} x_j)
$$

Since:

$$
\text{Var}(XY) = \text{Var}(X)\text{Var}(Y)
$$

and all terms i.i.d:

$$
= d_{in} \cdot \text{Var}(W) \cdot \text{Var}(x)
$$

To preserve variance:

$$
\text{Var}(y) = \text{Var}(x)
$$

So:

$$
d_{in} \cdot \text{Var}(W) = 1
$$

Thus:

$$
\text{Var}(W) = \frac{1}{d_{in}}
$$

Standard deviation:

$$
\sigma = \frac{1}{\sqrt{d_{in}}}
$$

Implementation:

```python
W = np.random.randn(d_in, d_out) / np.sqrt(d_in)
```

✅ Keeps activations stable across layers

---

# 4️⃣ Why Xavier Fails for ReLU

ReLU:

$$
\text{ReLU}(z) = \max(0, z)
$$

It zeroes half of the distribution.

Thus:

$$
\text{Var}(x_{after}) = \frac{1}{2}\text{Var}(x_{before})
$$

So variance shrinks by factor 1/2 each layer.

---

# 5️⃣ Kaiming / MSRA Initialization (He Initialization)

Correct for ReLU by compensating factor 2.

We want:

$$
d_{in} \cdot \text{Var}(W) \cdot \frac{1}{2} = 1
$$

Thus:

$$
\text{Var}(W) = \frac{2}{d_{in}}
$$

Standard deviation:

$$
\sigma = \sqrt{\frac{2}{d_{in}}}
$$

Implementation:

```python
W = np.random.randn(d_in, d_out) * np.sqrt(2.0 / d_in)
```

✅ Works well for deep ReLU networks

---

# 📊 Summary

| Initialization | Variance |
|---------------|----------|
| Small Gaussian | Too small → vanish |
| Large Gaussian | Too large → saturate |
| Xavier | 1/d_in |
| Kaiming | 2/d_in |

---

# 🚀 Final Recommendation

- tanh → Xavier
- ReLU → Kaiming (He)
- Deep networks → Always scale by fan-in
