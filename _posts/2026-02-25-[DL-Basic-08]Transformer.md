---
layout: post
title: "08. Transformer"
date: 2026-02-25 00:00:00 +0900
author: kang
categories: [Deep Learning, Foundations]
tags: [Artificial Intelligence, RNN, LSTM, GRU, Seq2Seq, Attention, Transformer]
pin: false
math: true
mermaid: true
---

---
# <b>Transformer</b>
---
### <b>Transformer</b>
    1. Multi-layer Perceptron
    2. Convolutional Neural Network
    3. Recurrent Neural Network
    4. Seq2Seq
    5. Attention

<b>MLP/CNN/RNN</b> learn fixed weights $W$ to map $x \to y$  
<b>Attention</b> have a good performance that retrieve info by <b>data-dependent weights</b> computed from relevance. So In this transformer model is more maximize the performance with combination and self attetion.

---
## <b>What is Transformer</b>

### 1. What is Transformer?
- A Model that <b>use attention model</b></span>. The main idea is the input can be split into muliple elements that are organically related to each other.
- With Self-Attention, each element learns to refine its own representation by attending its context. $$Self-attention = “contextualize\: each\: element$$
    
- <b>Structure</b>

    $$
    Input \rightarrow  ENCODER \rightarrow  DECODER \rightarrow  Softmax \rightarrow  Output
    $$

- Uses self-attention so each token builds a better embedding using other tokens
- Stacking Transformer blocks repeats contextualization → richer representations


### 2. Why use Transformer?


### 3. How use Transformer?
    1. Transformer Block
    2. Self Attention
    3. Multi-Head Attention
    4. FFN
    5. LayerNorm + Residual
    6. Positional Encoding
    7. Masked Multi-Head Attention
    8. Encoder-Decoder Attention

>### $Transformer\: Block$
![Transformer-Block](/assets/img/develop/Transformer-Block.png)

>### $Self-Attention$

### How to work on self-attention
![Self-Attention](/assets/img/develop/Self-Attention.png)

#### All of the i have same procedure. Transforming from x to z is changing <span style="color:cyan">from just token embedding to contextualized token</span> within sequences.

For each input token embedding $x_i \in \mathbb{R}^{d_{\text{model}}}$, we make:

$$
q_i = W_Q x_i,\qquad
k_i = W_K x_i,\qquad
v_i = W_V x_i.
$$

Where: $
W_Q \in \mathbb{R}^{d_k \times d_{\text{model}}},
\;
W_K \in \mathbb{R}^{d_k \times d_{\text{model}}},
\;
W_V \in \mathbb{R}^{d_v \times d_{\text{model}}}.
$

✅ Same matrices $W_Q,W_K,W_V$ are shared for all tokens.

### How to work on self-attention with dimension
![Self-Attention-Breakdown1](/assets/img/develop/Self-Attention-Breakdown1.png)
![Self-Attention-Breakdown2](/assets/img/develop/Self-Attention-Breakdown2.png)

$$
\text{Attention}(Q,K,V)
=
\text{softmax}\left(
\frac{QK^\top}{\sqrt{d_k}}
\right)V
$$

- Self-attention outputs:
  $$z_i=\sum_j \alpha_{ij}v_j,\quad \alpha_{ij}=\text{softmax}(q_i^\top k_j)$$

#### <b>Self-Attention main problem is that the mehod <span style="color:red">ignore the order of tokens</span></b>.

>### $Multi-Head\: Attention$
![Multi-Head-Attention](/assets/img/develop/Multi-Head-Attention.png)
$$
head_i = Attention(Q W_Q^i, K W_K^i, V W_V^i) 
$$

$$
MultiHead(Q, K, V) =
Concat(head_1, ..., head_h) W_O
$$

![Multi-Head-Attention-All](/assets/img/develop/Multi-Head-Attention-All.png)

>### $FFN$

Applied independently:

$$
\text{FFN}(z)
=
W_2 \sigma(W_1 z + b_1) + b_2
$$

where: $ \sigma = \text{ReLU} $

>### $Residual + LayerNorm$

![LayerNorm](/assets/img/develop/LayerNorm.png)

After attention:

$$
R' =
\text{LayerNorm}
\left(
R + \text{MultiHead}(R)
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

- Residual helps gradient flow.
- LayerNorm stabilizes scale.

>### $Positional\: Encoding$


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

>### $Stacking\: Blocks$
Repeat above block $L$ times.

Final encoder output:

$$
Z^{(L)} \in \mathbb{R}^{N\times d_{\text{model}}}
$$

Same length as input.


>### $Masked\: Multi-Head\: Attention$
Just Decoding Process, There are two type of tokens that already are known and didn't be known owing to future.
The token of future use $[mask]$ token. 

>### $Encoder-Decoder\: Attention$
