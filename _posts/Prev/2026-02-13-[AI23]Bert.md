---
layout: post
title: "Bert"
date: 2026-02-12 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Model]
tags: [Artificial Intelligenc, Model, RNN, LSTM, GRU, Seq2Seq, Attention, Self-Attention, Transformer, Bert]
pin: false
math: true
mermaid: true
---

# 🧠 BERT & ViT Complete Lecture Notes
> GitHub LaTeX Compatible ($, $$ only)

---

# 🟡 Part 1: BERT (Bidirectional Encoder Representations from Transformers)

---

## 1️⃣ What is BERT?

BERT uses:

- Transformer **Encoder only**
- Fully **bidirectional self-attention**
- Large-scale **self-supervised pretraining**

Paper:
https://arxiv.org/pdf/1810.04805.pdf

---

## 2️⃣ BERT Input Representation

Each input token embedding is the sum of:

$$
\text{InputEmbedding} = E_{\text{token}} + E_{\text{segment}} + E_{\text{position}}
$$

### 🔹 1. Token Embedding
- WordPiece embedding
- Includes special tokens:
  - `[CLS]`
  - `[SEP]`

### 🔹 2. Segment Embedding

Indicates sentence A or B:

$$
E_{\text{segment}} \in \{E_A, E_B\}
$$

### 🔹 3. Position Embedding

Learned positional embedding:

$$
E_{\text{position}} = E_0, E_1, ..., E_n
$$

---

## 3️⃣ Special Tokens

### 📌 [CLS]
Used as sequence-level representation.

Final hidden state:

$$
h_{\text{CLS}}^{(L)}
$$

Used for classification.

### 📌 [SEP]
Marks sentence boundary.

---

# 🟢 Part 2: BERT Pretraining Tasks

---

## 1️⃣ Masked Language Modeling (MLM)

Randomly mask 15% of tokens.

Example:

Input:
```
The dog is [MASK]
```

Model predicts masked word.

---

### 🔢 MLM Objective

For masked positions:

$$
\mathcal{L}_{MLM} = - \sum_{i \in \mathcal{M}} \log P(x_i | x_{\setminus \mathcal{M}})
$$

Where:
- $\mathcal{M}$ = masked positions
- $x_{\setminus \mathcal{M}}$ = visible tokens

---

## 2️⃣ Next Sentence Prediction (NSP)

Binary classification:

$$
P(\text{IsNext} | [CLS])
$$

Loss:

$$
\mathcal{L}_{NSP} = - y \log p - (1-y)\log(1-p)
$$

Total loss:

$$
\mathcal{L} = \mathcal{L}_{MLM} + \mathcal{L}_{NSP}
$$

---

# 🔵 Part 3: Vision Transformer (ViT)

Paper:
https://arxiv.org/pdf/2010.11929.pdf

---

## 1️⃣ Image to Patch Tokens

Image of size:

$$
H \times W \times C
$$

Split into patches:

$$
P \times P
$$

Number of patches:

$$
N = \frac{HW}{P^2}
$$

---

## 2️⃣ Patch Embedding

Flatten each patch:

$$
x_p \in \mathbb{R}^{P^2 C}
$$

Linear projection:

$$
z_p = x_p E
$$

Where:

$$
E \in \mathbb{R}^{(P^2 C) \times D}
$$

---

## 3️⃣ Add CLS Token

Sequence becomes:

$$
z_0 = [x_{cls}; z_1; z_2; ...; z_N]
$$

Add position embeddings:

$$
z_0 = z_0 + E_{pos}
$$

---

# 🟣 Transformer Encoder in ViT

For each layer:

---

## 🔹 Multi-head Self Attention

$$
Q = XW^Q
$$

$$
K = XW^K
$$

$$
V = XW^V
$$

Scaled dot-product attention:

$$
\text{Attention}(Q,K,V)
=
\text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

Multi-head:

$$
\text{MHA}(X)
=
\text{Concat}(head_1,...,head_h)W^O
$$

---

## 🔹 Feed Forward Network

$$
\text{FFN}(x)
=
\text{GELU}(xW_1 + b_1)W_2 + b_2
$$

---

## 🔹 Residual + LayerNorm

$$
X' = \text{LayerNorm}(X + \text{MHA}(X))
$$

$$
Z = \text{LayerNorm}(X' + \text{FFN}(X'))
$$

---

# 🟠 ViT Classification

Use final CLS token:

$$
y = \text{Softmax}(W h_{cls}^{(L)})
$$

Cross entropy loss:

$$
\mathcal{L} = - \sum y_i \log \hat{y}_i
$$

---

# 🟡 Why ViT Needs Large Data?

Unlike CNN:

- No locality bias
- No translation invariance

CNN inductive bias:

$$
f(x_{i,j}) \approx f(x_{i+\Delta,j+\Delta})
$$

ViT must learn this from data.

Hence requires:

$$
\text{Large-scale dataset (e.g., JFT-300M)}
$$

---

# 🟢 Positional Embedding in ViT

Cosine similarity between position embeddings:

$$
\text{sim}(i,j)
=
\frac{E_i \cdot E_j}
{\|E_i\|\|E_j\|}
$$

Observation:

- Nearby patches → higher similarity
- Row-column structure emerges

---

# 🔥 Final Summary

| Model | Core Idea |
|-------|-----------|
| BERT | Bidirectional masked LM |
| ViT | Transformer on image patches |
| CNN | Local inductive bias |
| Transformer | Global attention |
