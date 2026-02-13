---
layout: post
title: "Transformer"
date: 2026-02-12 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligenc, Model, RNN, LSTM, GRU, Seq2Seq, Attention, Self-Attention, Transformer]
pin: false
math: true
mermaid: true
---


# 🚀 Transformer

---

# 1️⃣ Core Idea

Transformer replaces recurrence with attention.

The fundamental operation is:

$$
\text{Attention}(Q,K,V)
=
\text{softmax}\left(
\frac{QK^\top}{\sqrt{d_k}}
\right)V
$$

Everything else is structured stacking of this operation.

---

# 2️⃣ Input Embedding

Given tokens:

$$
t_1, t_2, \dots, t_N
$$

Embedding matrix:

$$
E \in \mathbb{R}^{|V|\times d_{\text{model}}}
$$

Embedding vectors:

$$
x_i = E[t_i]
$$

Stacked matrix:

$$
X \in \mathbb{R}^{N\times d_{\text{model}}}
$$

---

# 3️⃣ Positional Encoding (Inject Order Information)

Since attention is permutation invariant, we inject position:

$$
r_i = x_i + p_i
$$

### 🔷 Sinusoidal Encoding

For $i = 0,\dots,d_{\text{model}}/2 -1$:

$$
PE(pos,2i)
=
\sin\left(
\frac{pos}{10000^{2i/d_{\text{model}}}}
\right)
$$

$$
PE(pos,2i+1)
=
\cos\left(
\frac{pos}{10000^{2i/d_{\text{model}}}}
\right)
$$

### 🔎 Why it works (Relative Position Property)

Using identity:

$$
\sin(a+b) = \sin a \cos b + \cos a \sin b
$$

Relative shifts can be represented as linear combinations of sine/cosine pairs.

Thus model can learn relative offsets.

---

# 4️⃣ Linear Projections (Q, K, V)

From:

$$
R \in \mathbb{R}^{N\times d_{\text{model}}}
$$

We compute:

$$
Q = RW_Q
$$

$$
K = RW_K
$$

$$
V = RW_V
$$

Where:

$$
W_Q, W_K \in \mathbb{R}^{d_{\text{model}}\times d_k}
$$

$$
W_V \in \mathbb{R}^{d_{\text{model}}\times d_v}
$$

---

# 5️⃣ Scaled Dot-Product Attention

### Step 1: Score Matrix

$$
S = QK^\top
$$

Shape:

$$
S \in \mathbb{R}^{N\times N}
$$

Each element:

$$
S_{ij} = q_i^\top k_j
$$

### Step 2: Variance Analysis

If components have variance 1:

$$
\text{Var}(q_i^\top k_j) = d_k
$$

Thus magnitude grows with dimension.

To stabilize:

$$
S = \frac{QK^\top}{\sqrt{d_k}}
$$

### Step 3: Softmax

$$
A = \text{softmax}(S)
$$

### Step 4: Weighted Sum

$$
Z = AV
$$

Final:

$$
Z \in \mathbb{R}^{N\times d_v}
$$

---

# 6️⃣ Multi-Head Attention

For $H$ heads:

$$
Q^{(h)} = RW_Q^{(h)}
$$

$$
K^{(h)} = RW_K^{(h)}
$$

$$
V^{(h)} = RW_V^{(h)}
$$

Each head:

$$
Z^{(h)}
=
\text{softmax}
\left(
\frac{Q^{(h)}K^{(h)\top}}{\sqrt{d_k}}
\right)
V^{(h)}
$$

Concatenate:

$$
Z_{\text{cat}}
=
\text{Concat}(Z^{(1)},\dots,Z^{(H)})
$$

Final projection:

$$
Z = Z_{\text{cat}} W_O
$$

---

# 7️⃣ Feed Forward Network (Per Token)

Applied independently:

$$
\text{FFN}(z)
=
W_2 \sigma(W_1 z + b_1) + b_2
$$

Typically:

$$
\sigma = \text{ReLU}
$$

No cross-token interaction.

---

# 8️⃣ Residual + LayerNorm

After attention:

$$
R' =
\text{LayerNorm}
\left(
R + \text{MHA}(R)
\right)
$$

After FFN:

$$
R^{\text{next}}
=
\text{LayerNorm}
\left(
R' + \text{FFN}(R')
\right)
$$

Residual helps gradient flow.

LayerNorm stabilizes scale.

---

# 9️⃣ Stacking Blocks

Repeat above block $L$ times.

Final encoder output:

$$
Z^{(L)} \in \mathbb{R}^{N\times d_{\text{model}}}
$$

Same length as input.

---

# 🔟 Decoder: Masked Self-Attention

Causal mask:

$$
M_{ij}
=
\begin{cases}
0 & j \le i \\
-\infty & j > i
\end{cases}
$$

Apply before softmax:

$$
S' =
\frac{QK^\top}{\sqrt{d_k}} + M
$$

$$
A = \text{softmax}(S')
$$

Ensures autoregressive property.

---

# 1️⃣1️⃣ Encoder-Decoder Attention

Decoder queries:

$$
Q = R_{\text{decoder}} W_Q
$$

Encoder keys/values:

$$
K = Z^{(L)} W_K
$$

$$
V = Z^{(L)} W_V
$$

Allows decoder to attend full input sequence.

---

# 1️⃣2️⃣ Linear Output Layer

Map to vocabulary logits:

$$
o_t = W_o z_t + b_o
$$

$$
W_o \in \mathbb{R}^{|V|\times d_{\text{model}}}
$$

---

# 1️⃣3️⃣ Softmax

$$
p(y_t=c)
=
\frac{e^{(o_t)_c}}
{\sum_j e^{(o_t)_j}}
$$

---

# 1️⃣4️⃣ Cross-Entropy Loss

For one-hot target:

$$
\mathcal{L}_t
=
-\sum_c y_{t,c} \log p(y_t=c)
$$

Sequence loss:

$$
\mathcal{L}
=
\sum_{t=1}^{T} \mathcal{L}_t
$$

---

# 1️⃣5️⃣ Training Objective (MLE)

$$
P(y_1,\dots,y_T)
=
\prod_{t=1}^T
P(y_t \mid y_{<t}, x)
$$

Minimize negative log likelihood.

---

# 1️⃣6️⃣ Inference

Greedy decoding:

$$
\hat{y}_t
=
\arg\max_c p(y_t=c)
$$

Beam search keeps top-k sequences.

---

# 1️⃣7️⃣ Classification Variant

Mean pooling:

$$
\bar{z}
=
\frac{1}{N}
\sum_{i=1}^N z_i
$$

CLS token:

Use:

$$
z_{\text{CLS}}^{(L)}
$$

---

# 🎯 Final Core Formula

$$
\boxed{
\text{Attention}(Q,K,V)
=
\text{softmax}
\left(
\frac{QK^\top}{\sqrt{d_k}}
\right)V
}
$$

---

# ✅ End of Complete Transformer Notes
