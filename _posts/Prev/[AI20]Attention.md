---
layout: post
title: "Attention"
date: 2026-02-12 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Model]
tags: [Artificial Intelligenc, Model, RNN, LSTM, GRU, Seq2Seq, Attention]
pin: false
math: true
mermaid: true
---

# 🚀 From RNN to Attention Mechanism

### Deep Learning Lecture Notes (Complete Derivations Included)

------------------------------------------------------------------------

# 📌 1. Motivation: Why Attention?

## 🔴 Issue 1: Vanishing / Exploding Gradient

For a vanilla RNN:

$$
h_t = \phi(W_h h_{t-1} + W_x x_t + b)
$$

By recursive expansion:

$$
h_t = \phi(W_h \phi(W_h h_{t-2} + ... ))
$$

Gradient:

$$
\frac{\partial h_t}{\partial h_{t-k}} = \prod_{i=t-k+1}^{t} W_h^T \cdot \phi'(z_i)
$$

If eigenvalues of $W_h$:

-   $|\lambda| < 1$ → Vanishing\
-   $|\lambda| > 1$ → Exploding

Thus long-range dependency fails.

→ LSTM / GRU partially solve this.

------------------------------------------------------------------------

## 🔴 Issue 2: Single Vector Bottleneck

In Seq2Seq:

Encoder compresses entire sequence into one vector:

$$
c = h_T
$$

Regardless of input length $T$.

Information capacity is limited → Information loss inevitable.

------------------------------------------------------------------------

## 🔴 Issue 3: Extremely Long Sequences

Even LSTM struggles when:

-   Sequence length very large
-   Important early tokens forgotten

💡 Solution → **Attention Mechanism**

------------------------------------------------------------------------

# 🧠 2. Attention Idea

Instead of compressing everything into one vector:

Decoder attends to **all encoder hidden states**.

------------------------------------------------------------------------

# 📐 3. Formal Definition of Attention

## Attention Function

$$
\text{Attention}(Q, K, V) = \text{weighted sum of } V
$$

Where weights depend on similarity between Q and K.

------------------------------------------------------------------------

# 🎯 4. Dot-Product Attention (Full Derivation)

## Step 1: Define Hidden States

Encoder hidden states:

$$
h_1, h_2, ..., h_T \in \mathbb{R}^d
$$

Decoder state at time t:

$$
s_t \in \mathbb{R}^d
$$

------------------------------------------------------------------------

## Step 2: Compute Attention Scores

Score between decoder state and each encoder state:

$$
e_{t,i} = s_t^T h_i
$$

Vector form:

$$
e_t = 
\begin{bmatrix}
s_t^T h_1 \\
s_t^T h_2 \\
\vdots \\
s_t^T h_T
\end{bmatrix}
\in \mathbb{R}^T
$$

------------------------------------------------------------------------

## Step 3: Softmax → Attention Coefficients

$$
\alpha_{t,i} = \frac{\exp(e_{t,i})}{\sum_{j=1}^{T} \exp(e_{t,j})}
$$

Properties:

$$
\sum_{i=1}^{T} \alpha_{t,i} = 1
$$

------------------------------------------------------------------------

## Step 4: Compute Attention Vector

Weighted sum:

$$
a_t = \sum_{i=1}^{T} \alpha_{t,i} h_i
$$

This is convex combination of encoder states.

------------------------------------------------------------------------

## Step 5: Combine with Decoder State

Concatenate:

$$
\tilde{s}_t = \tanh(W_c [a_t ; s_t])
$$

Output probability:

$$
P(y_t | y_{<t}, x) = \text{softmax}(W_o \tilde{s}_t)
$$

------------------------------------------------------------------------

# 📊 5. Matrix Form (Efficient Implementation)

Let:

$$
H = [h_1, ..., h_T]^T \in \mathbb{R}^{T \times d}
$$

Then:

$$
e_t = H s_t
$$

$$
\alpha_t = \text{softmax}(e_t)
$$

$$
a_t = H^T \alpha_t
$$

------------------------------------------------------------------------

# 🌍 6. Application: Machine Translation

Example:

Input:

    are you still at home

Output:

    todavía están en casa

Each output token focuses on different input words.

Attention map visualizes alignment.

------------------------------------------------------------------------

# 🔥 7. Why Attention Solves Information Loss

Without attention:

$$
c = h_T
$$

With attention:

$$
a_t = \sum_i \alpha_{t,i} h_i
$$

Each decoding step dynamically retrieves relevant information.

No fixed bottleneck.

------------------------------------------------------------------------

# 📈 8. Information-Theoretic Insight

Encoder compression:

$$
x_{1:T} \rightarrow h_T
$$

Entropy loss inevitable.

Attention:

$$
x_{1:T} \rightarrow \{h_1,...,h_T\}
$$

Decoder queries full memory.

More expressive capacity.

------------------------------------------------------------------------

# 📝 9. Another Application: Text Summarization

Attention enables:

-   Long coherent summary
-   Context tracking
-   Word alignment learning

------------------------------------------------------------------------

# 🎉 Final Summary

  Component          Meaning
  ------------------ --------------------
  Query              Decoder state
  Key                Encoder states
  Value              Encoder states
  Attention weight   Similarity measure
  Output             Weighted average

------------------------------------------------------------------------

# 🚀 Key Takeaway

Attention replaces fixed bottleneck with dynamic memory lookup.

It paved the way to:

-   Self-Attention
-   Transformers
-   Large Language Models
