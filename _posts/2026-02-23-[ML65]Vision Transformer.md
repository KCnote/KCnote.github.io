---
layout: post
title: "Vision Transformer"
date: 2026-02-23 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligenc, Transformer]
pin: false
math: true
mermaid: true

---

# Vision Transformer (ViT)

## Patch Embedding

Given image:

$$
X \in \mathbb{R}^{H \times W \times 3}
$$

Patch size:

$$
P \times P
$$

Number of patches:

$$
N = \frac{HW}{P^2}
$$

Flatten each patch:

$$
x_i \in \mathbb{R}^{P^2 \cdot 3}
$$

Linear projection:

$$
z_i = E x_i
$$

where

$$
E \in \mathbb{R}^{D \times (P^2 \cdot 3)}
$$

Add class token and position encoding:

$$
Z_0 = [z_{cls}, z_1, ..., z_N] + P_{pos}
$$

---

## Self-Attention

Given

$$
Z \in \mathbb{R}^{(N+1) \times D}
$$

Compute

$$
Q = ZW_Q
$$

$$
K = ZW_K
$$

$$
V = ZW_V
$$

Attention:

$$
\mathrm{Attention}(Q,K,V) =
\mathrm{softmax}
\left(
\frac{QK^T}{\sqrt{D}}
\right)
V
$$

Transformer block:

$$
Z' = Z + \mathrm{MHA}(\mathrm{LN}(Z))
$$

$$
Z'' = Z' + \mathrm{MLP}(\mathrm{LN}(Z'))
$$

---

# DeiT

Add distillation token:

$$
[z_{cls}, z_{dist}, z_1, ..., z_N]
$$

Two heads:

- Classification head
- Distillation head

---

# Soft-Label Distillation

Teacher output:

$$
p_T = \mathrm{softmax}
\left(
\frac{z_T}{T}
\right)
$$

Student output:

$$
p_S = \mathrm{softmax}
\left(
\frac{z_S}{T}
\right)
$$

Distillation loss:

$$
\mathcal{L}_{distill}
=
\mathrm{KL}(p_T || p_S)
$$

Full loss:

$$
\mathcal{L}
=
(1-\alpha)\mathcal{L}_{CE}
+
\alpha T^2 \mathcal{L}_{distill}
$$

---

# Hard-Label Distillation

Teacher label:

$$
y_T = \arg\max p_T
$$

Distillation loss:

$$
\mathcal{L}_{distill}
=
\mathrm{CE}(y_T, p_S)
$$

Full loss:

$$
\mathcal{L}
=
(1-\alpha)\mathcal{L}_{CE}
+
\alpha \mathcal{L}_{distill}
$$

---

# Swin Transformer

## Hierarchical Structure

Before merging:

$$
H \times W \times C
$$

After patch merging:

$$
\frac{H}{2} \times \frac{W}{2} \times 2C
$$

---

## Window Attention

Window size:

$$
M \times M
$$

Complexity:

$$
O\left(\frac{HW}{M^2} \cdot M^2\right)
=
O(HW)
$$

---

## Relative Position Bias

Attention score:

$$
A_{ij}
=
\frac{q_i k_j^T}{\sqrt{d}}
+
B_{(i-j)}
$$

---

# Convolutional Vision Transformer (CvT)

Convolutional embedding:

$$
z = \mathrm{Conv}(X)
$$

Convolutional Q, K, V:

$$
Q = \mathrm{Conv}(Z)
$$

$$
K = \mathrm{Conv}(Z)
$$

$$
V = \mathrm{Conv}(Z)
$$
