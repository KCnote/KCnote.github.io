---
layout: post
title: "Sequential Data"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Miscellaneous]
tags: [Artificial Intelligenc, Miscellaneous, Data]
pin: false
math: true
mermaid: true
---
# 📦 Sequential Data & Word Embedding — Clean GitHub Version

---

# 🔁 1. Sequential Data

## 📌 Definition

Instead of a single input x, we now have a sequence:

$$
x = \{x_1, x_2, \dots, x_T\}
$$

Examples:

- 🎥 Sequence of images (video)
- 📈 Sequence of numbers (time-series)
- 📝 Sequence of words (sentence)
- 🌍 Sequence of geographic states

We must model temporal / sequential dependencies across multiple x_t.

---

# 🎯 2. Output Types in Sequential Problems

## 🟢 Many-to-Many

$$
\{x_1, x_2, ..., x_T\}
\rightarrow
\{y_1, y_2, ..., y_T\}
$$

Examples:
- POS tagging
- Frame classification
- Sequence labeling

---

## 🔵 Many-to-One

$$
\{x_1, x_2, ..., x_T\}
\rightarrow
y
$$

Examples:
- Sentiment classification
- Video action recognition

---

## 🟡 One-to-Many

$$
x
\rightarrow
\{y_1, y_2, ..., y_T\}
$$

Examples:
- Image captioning
- Visual Q&A answer generation

---

# 🧠 3. Problem Mapping Summary

| Type | Mapping | Example |
|------|--------|----------|
| One-to-One | x → y | Image classification |
| Many-to-One | {x_t} → y | Sentiment analysis |
| Many-to-Many | {x_t} → {y_t} | Translation |
| One-to-Many | x → {y_t} | Captioning |

---

# 📌 4. Word Embedding

Words are represented as vectors:

$$
w \in \mathbb{R}^d
$$

Goal:
- Similar words → close in vector space
- Semantic relationships preserved

Example:

$$
\text{king} - \text{man} + \text{woman}
\approx
\text{queen}
$$

---

# 🚀 5. Word2Vec

## 1️⃣ CBOW

Predict center word from context:

$$
P(w_t | w_{t-m}, ..., w_{t+m})
$$

---

## 2️⃣ Skip-Gram

Predict surrounding words from center word:

$$
P(w_{t+j} | w_t)
$$

for:

$$
-m \le j \le m, \quad j \ne 0
$$

---

## 🎯 Objective Function

Maximize likelihood:

$$
\mathcal{L}(\theta) =
\prod_{t=1}^{N}
\prod_{-m \le j \le m, j \ne 0}
P(w_{t+j} | w_t; \theta)
$$

Log-likelihood form:

$$
\sum_{t=1}^{N}
\sum_{-m \le j \le m, j \ne 0}
\log P(w_{t+j} | w_t)
$$

---

## 🔎 Softmax Model

$$
P(w_O | w_I) =
\frac{\exp(v_{w_O}^T v_{w_I})}
{\sum_{w=1}^{V} \exp(v_w^T v_{w_I})}
$$

Where:
- V = vocabulary size
- v = embedding vectors

To reduce cost → Negative Sampling

---

# 🌍 6. GloVe

Uses global co-occurrence statistics.

Let:

$$
X_{ij}
$$

= number of times word j appears in context of word i.

---

## Core Equation

$$
w_i^T w_j + b_i + b_j = \log X_{ij}
$$

---

## Objective

$$
J =
\sum_{i,j=1}^{V}
f(X_{ij})
\left(
w_i^T w_j + b_i + b_j - \log X_{ij}
\right)^2
$$

---

## Weighting Function

$$
f(x) =
\begin{cases}
\left(\frac{x}{x_{max}}\right)^\alpha & x < x_{max} \\
1 & x \ge x_{max}
\end{cases}
$$

Typical:
- alpha = 0.75
- x_max = 100

---

# 🧮 7. Word Analogy

Embedding arithmetic works:

$$
v_{king} - v_{man} + v_{woman}
\approx
v_{queen}
$$

---

# 🧩 8. Summary

| Method | Key Idea |
|--------|----------|
| Word2Vec | Local prediction objective |
| GloVe | Global co-occurrence matrix factorization |

---

# 🎯 Big Picture

Sequential modeling requires:

- Temporal modeling
- Vector representation
- Structured prediction
- Context modeling

