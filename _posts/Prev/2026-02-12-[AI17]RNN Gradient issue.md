---
layout: post
title: "RNN Exploding & Vanishing Gradients"
date: 2026-02-12 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Model]
tags: [Artificial Intelligenc, Model, RNN, Gradient]
pin: false
math: true
mermaid: true
---
# 🧠 RNN Gradient Issues (Vanilla RNN) — Exploding & Vanishing Gradients

---

The **vanilla RNN** we covered is powerful, but it has major training issues:

1. **Exploding / Vanishing gradients** → hard to learn **long-range dependencies** → motivates **LSTM / GRU**  
2. **Many-to-many RNN is not flexible** for different input/output lengths → motivates **Seq2Seq**  
3. Even LSTM/GRU can struggle on very long sequences → motivates **Attention / Transformers**

---

## 1) Vanilla RNN Setup 🧩

### 1.1 Recurrence (hidden state update)

For a sequence input $\{x_1, x_2, \dots, x_T\}$, vanilla RNN computes hidden states:

$$
h_t = \tanh(W_{hh}h_{t-1} + W_{xh}x_t + b_h)
$$

and an output (classification/regression style):

$$
\hat{y}_t = g(W_{hy}h_t + b_y)
$$

- $x_t \in \mathbb{R}^{d_x}$
- $h_t \in \mathbb{R}^{d_h}$
- $W_{xh} \in \mathbb{R}^{d_h \times d_x}$
- $W_{hh} \in \mathbb{R}^{d_h \times d_h}$  (**reused at every time step**)
- $W_{hy} \in \mathbb{R}^{d_y \times d_h}$

> 🔁 **Key point:** the same matrix $W_{hh}$ is multiplied across time, which is the root cause.

---

## 2) Where Gradients Come From 🔥❄️

Assume we compute a loss at each time step:

$$
\mathcal{L} = \sum_{t=1}^{T}\mathcal{L}_t
$$

Backpropagation Through Time (BPTT) means gradients flow from later time steps back to earlier ones.

---

## 3) Gradient Flow Inside the RNN Cell 🧠➡️🧠

### 3.1 Jacobian of $h_t$ w.r.t. $h_{t-1}$

Let

$$
a_t = W_{hh}h_{t-1} + W_{xh}x_t + b_h
\quad\Rightarrow\quad
h_t = \tanh(a_t)
$$

Then:

$$
\frac{\partial h_t}{\partial h_{t-1}}
=
\frac{\partial \tanh(a_t)}{\partial a_t}\cdot \frac{\partial a_t}{\partial h_{t-1}}
=
\operatorname{diag}\!\big(\tanh'(a_t)\big)\, W_{hh}
$$

and since

$$
\tanh'(z) = 1 - \tanh^2(z)
$$

we have

$$
\operatorname{diag}\!\big(\tanh'(a_t)\big) = \operatorname{diag}\!\big(1 - h_t^2\big)
$$

So:

$$
\boxed{
\frac{\partial h_t}{\partial h_{t-1}}
=
\operatorname{diag}(1-h_t^2)\, W_{hh}
}
$$

---

## 4) Why Vanishing / Exploding Happens 📉📈

When gradients pass through many time steps, they multiply many Jacobians:

$$
\frac{\partial h_t}{\partial h_k}
=
\prod_{i=k+1}^{t}
\frac{\partial h_i}{\partial h_{i-1}}
=
\prod_{i=k+1}^{t}
\left(\operatorname{diag}(1-h_i^2)\, W_{hh}\right)
$$

Then by chain rule, gradients of a loss $\mathcal{L}_t$ to an earlier state $h_k$ look like:

$$
\frac{\partial \mathcal{L}_t}{\partial h_k}
=
\frac{\partial \mathcal{L}_t}{\partial h_t}\,
\frac{\partial h_t}{\partial h_k}
=
\frac{\partial \mathcal{L}_t}{\partial h_t}
\prod_{i=k+1}^{t}\left(\operatorname{diag}(1-h_i^2)\, W_{hh}\right)
$$

### 4.1 Intuition with norms

Take norms (roughly):

$$
\left\|\frac{\partial \mathcal{L}_t}{\partial h_k}\right\|
\approx
\left\|\frac{\partial \mathcal{L}_t}{\partial h_t}\right\|
\prod_{i=k+1}^{t}
\left\|\operatorname{diag}(1-h_i^2)\, W_{hh}\right\|
$$

- For $\tanh$, we usually have $|1-h_i^2| \le 1$ and often **much smaller than 1** when saturated.
- If the effective multiplier is **< 1** repeatedly → gradient shrinks exponentially → **vanishing**
- If the effective multiplier is **> 1** repeatedly (e.g., large spectral norm of $W_{hh}$) → gradient grows exponentially → **exploding**

### 4.2 Spectral perspective (common explanation)

Let $\|W_{hh}\|$ denote a matrix norm (or think largest singular value).

- If $\|W_{hh}\| < 1$ → repeated multiplication tends to shrink signals/gradients  
- If $\|W_{hh}\| > 1$ → repeated multiplication tends to grow signals/gradients  

So gradient magnitude behaves like:

$$
\text{(roughly)}\quad \|W_{hh}\|^{(t-k)}
$$

> 🧠 This is why long sequence length behaves like “deep network depth”.

---

## 5) “Sequence Length Acts Like Depth” 🏗️

A feed-forward network of depth $K$ multiplies $K$ layer Jacobians.

A vanilla RNN over $T$ time steps multiplies **$T$ Jacobians** through time.

So increasing sequence length makes optimization harder in the same way as training a much deeper network.

---

## 6) Exploding Gradients ✅ Solution: Gradient Clipping ✂️

### 6.1 Idea

If the gradient norm is too large, rescale it:

Let $g$ be the gradient vector (or all gradients flattened). Choose threshold $c>0$.

$$
g \leftarrow
\begin{cases}
g & \text{if } \|g\| \le c \\
\frac{c}{\|g\|}g & \text{if } \|g\| > c
\end{cases}
$$

✅ This keeps the direction but limits the magnitude.

### 6.2 Pseudocode (norm clipping)

```text
g = compute_gradients()
if ||g|| > c:
    g = (c / ||g||) * g
apply_update(g)
```

---

## 7) Vanishing Gradients ✅ Common Mitigations 🧯

### 7.1 Better initialization (Xavier / He) 🎛️

For $\tanh$ networks, **Xavier/Glorot** is commonly used.

For a weight matrix with fan-in $n_{in}$ and fan-out $n_{out}$:

$$
W_{ij} \sim \mathcal{U}\left(-\sqrt{\frac{6}{n_{in}+n_{out}}},\; \sqrt{\frac{6}{n_{in}+n_{out}}}\right)
$$

or

$$
W_{ij} \sim \mathcal{N}\left(0,\; \frac{2}{n_{in}+n_{out}}\right)
$$

> ⚠️ Helps, but **does not fully solve** long-range dependency for vanilla RNN.

### 7.2 Gated RNNs: LSTM / GRU 🚪

They introduce gates to control information flow and improve gradient propagation.

### 7.3 Attention 🔍

Instead of forcing all information through $h_T$, attention allows “direct connections” to earlier positions.

---

## 8) Summary 🧾

- Vanilla RNN uses the same $W_{hh}$ repeatedly:
  - Gradients are products of many Jacobians  
  - This causes **vanishing** (multipliers < 1) or **exploding** (multipliers > 1)
- **Exploding** → gradient clipping is a simple and effective fix
- **Vanishing** → better init helps, but real solutions are **LSTM/GRU** and **Attention**

---

## 9) Quick Checklist ✅

- [ ] If training is unstable / loss becomes NaN → try **gradient clipping**
- [ ] If model can’t learn long-term patterns → use **LSTM/GRU** or **Attention**
- [ ] If training is slow / stuck → check init + learning rate + saturation of $\tanh$

---

*Made for GitHub Markdown ✨ (copy/paste friendly).*
