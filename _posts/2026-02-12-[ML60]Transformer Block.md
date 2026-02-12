---
layout: post
title: "Transformer Block"
date: 2026-02-12 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligenc, Model, RNN, LSTM, GRU, Seq2Seq, Attention, Self-Attention]
pin: false
math: true
mermaid: true
---

# 🧠 Review → Attention → Transformer (Main Idea)
> **Theme:** we stop compressing a sequence into *one* vector and instead let **each token** build a better representation by looking at **other tokens**.

---

## ✅ 0) What MLPs / CNNs learn (big picture)

### 🎯 Supervised learning view
We learn parameters $W$ that best map inputs $x$ to labels $y$ on training data:

$$
\hat{y} = f(x; W)
$$

and optimize:

$$
W^\star = \arg\min_W \; \mathcal{L}\big(f(x;W), y\big).
$$

### 💡 “Weighted sums + fixed unary ops”
A very typical structure is:
- **linear mixing** (weighted sum)  
- followed by **nonlinearities** (fixed unary ops like ReLU, tanh)

Example (2-layer MLP):

$$
\hat{y} = W_2 \,\sigma(W_1 x + b_1) + b_2.
$$

CNNs do the same idea but with structured linear maps (convolutions).

---

## 🔁 1) What about RNNs?

### RNN recurrence
RNN processes a sequence $x_1,\dots,x_T$:

$$
h_t = f_W(h_{t-1}, x_t) 
= \tanh\!\big(W_{hh} h_{t-1} + W_{xh} x_t + b\big).
$$

Unrolling (showing the “nested tanh” effect):

$$
\begin{aligned}
h_1 &= \tanh(W_{hh}h_0 + W_{xh}x_1 + b), \\
h_2 &= \tanh(W_{hh}h_1 + W_{xh}x_2 + b) \\
    &= \tanh\!\Big(W_{hh}\tanh(W_{hh}h_0 + W_{xh}x_1 + b) + W_{xh}x_2 + b\Big), \\
h_3 &= \tanh(W_{hh}h_2 + W_{xh}x_3 + b) \\
    &= \tanh\!\Big(W_{hh}\tanh\!\big(W_{hh}\tanh(W_{hh}h_0 + W_{xh}x_1 + b) + W_{xh}x_2 + b\big) + W_{xh}x_3 + b\Big).
\end{aligned}
$$

✅ Still: **learned weights** + **fixed nonlinearities**.

### Key pain points (review)
- hard to model **very long** dependencies
- sequential computation (slow / not parallel)
- “information bottleneck” in classic seq2seq: compressing into one vector

---

## 👀 2) Attention idea (review)

### Definition (query + key-value memory)
Attention takes:
- **Query** $Q$ (what I’m looking for)
- **Keys** $K$ (what’s in memory, used for matching)
- **Values** $V$ (what I actually retrieve / average)

and returns a weighted average of values:

$$
\text{Attention}(Q,K,V) = \sum_i \alpha_i V_i,
\quad
\sum_i \alpha_i = 1.
$$

Weights come from “relevance” between query and keys.

---

## 🧩 3) Transformer main idea (conceptual)

### Basic assumption
The input $x$ can be split into multiple elements that are **organically related**:
- words in a sentence
- frames in a video
- people in a social network

### Self-attention = “contextualize each element”
Each element builds a refined representation by attending to the other elements.

> 🔥 핵심: the “mixing weights” are **not fixed parameters** like a normal layer —  
> they are computed **from the input itself** (data-dependent).

---

## 🧱 4) What are Query / Key / Value in Transformer?

For each input token embedding $x_i \in \mathbb{R}^{d_{\text{model}}}$, we make:

$$
q_i = W_Q x_i,\qquad
k_i = W_K x_i,\qquad
v_i = W_V x_i.
$$

Where:

$$
W_Q \in \mathbb{R}^{d_k \times d_{\text{model}}},
\;
W_K \in \mathbb{R}^{d_k \times d_{\text{model}}},
\;
W_V \in \mathbb{R}^{d_v \times d_{\text{model}}}.
$$

✅ Same matrices $W_Q,W_K,W_V$ are shared for all tokens.

---

## 🧮 5) How does self-attention compute a new embedding?

### Step 1) Similarity scores
For token $i$ attending to token $j$:

**dot-product score**
$$
e_{ij} = q_i^\top k_j.
$$

(Some slides use cosine similarity. If you want cosine explicitly:)

$$
\cos(q_i,k_j) = \frac{q_i^\top k_j}{\|q_i\|\;\|k_j\|}.
$$

### Step 2) Softmax weights
Convert scores to probabilities over all $j$:

$$
\alpha_{ij} = \frac{\exp(e_{ij})}{\sum_{m=1}^{N}\exp(e_{im})}.
$$

So:

$$
\sum_{j=1}^{N}\alpha_{ij} = 1.
$$

### Step 3) Weighted sum of values (context vector for token i)
The new contextual representation for token $i$:

$$
z_i = \sum_{j=1}^{N}\alpha_{ij}\, v_j.
$$

This means:
- if token $j$ is relevant to $i$, then $\alpha_{ij}$ is large  
- thus $v_j$ contributes more to $z_i$

### Step 4) Output projection back to the original space
Slides often add an additional learned projection:

$$
\tilde{z}_i = W_O z_i,
\qquad
W_O \in \mathbb{R}^{d_{\text{model}}\times d_v}.
$$

So the block returns something in the same embedding dimension.

---

## 📌 6) Matrix form (clean + fast)

Stack token embeddings into a matrix:

$$
X \in \mathbb{R}^{N \times d_{\text{model}}}.
$$

Then:

$$
Q = XW_Q^\top,\quad
K = XW_K^\top,\quad
V = XW_V^\top.
$$

Scores:

$$
E = QK^\top \in \mathbb{R}^{N\times N}.
$$

Row-wise softmax:

$$
A = \text{softmax}(E),
$$

so each row of $A$ sums to 1.

Weighted sum:

$$
Z = AV \in \mathbb{R}^{N\times d_v}.
$$

Output projection:

$$
\tilde{Z} = ZW_O^\top \in \mathbb{R}^{N\times d_{\text{model}}}.
$$

---

## 🔁 7) “Why does $z_i$ stay similar to $x_i$?”

Many examples show that the self-score tends to be large:

- $e_{ii} = q_i^\top k_i$ often bigger than $q_i^\top k_j$ for unrelated $j$  
- so $\alpha_{ii}$ can be dominant

Then:

$$
z_i = \alpha_{ii}v_i + \sum_{j\ne i}\alpha_{ij}v_j
$$

and if $\alpha_{ii}$ is large, $z_i$ is close to “self”, but still nudged by context.

✅ Repeating this multiple times (stacking blocks) makes embeddings **more contextual**.

---

## 🧱 8) Transformer Block (high-level)

A simplified view of one Transformer block:

1) **Self-Attention**: $X \rightarrow \tilde{Z}$  
2) (usually) **Feed-Forward Network**: token-wise MLP

In many diagrams, multiple Transformer blocks are stacked:

- block 1 produces $Z^{(1)}$
- block 2 produces $Z^{(2)}$
- …

So each layer refines representations further.

---

## 🖼️ 9) “Block stacking” intuition

- Lower layers: local / surface cues
- Higher layers: richer semantics (more context)

This matches the slides showing multiple Transformer blocks stacked vertically.

---

## 🧭 10) GitHub Mermaid: Self-Attention flow

```mermaid
flowchart LR
  X[X: token embeddings] --> Q[Q = XWQ^T]
  X --> K[K = XWK^T]
  X --> V[V = XWV^T]
  Q --> S[S = QK^T]
  K --> S
  S --> A[A = softmax(S)]
  A --> Z[Z = A V]
  V --> Z
  Z --> O[Z~ = ZWO^T]
```

---

# ✅ Final Summary (1-minute)

- MLP/CNN/RNN: learn fixed weights $W$ to map $x \to y$
- Attention: retrieve info by **data-dependent weights** computed from relevance
- Transformer: uses **self-attention** so each token builds a better embedding using other tokens
- Q/K/V are created by learned linear maps:
  $$q_i=W_Qx_i,\;k_i=W_Kx_i,\;v_i=W_Vx_i$$
- Self-attention outputs:
  $$z_i=\sum_j \alpha_{ij}v_j,\quad \alpha_{ij}=\text{softmax}(q_i^\top k_j)$$
- Stacking Transformer blocks repeats contextualization → richer representations

---
