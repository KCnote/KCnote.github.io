---
layout: post
title: "Convolution"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, CNN]
tags: [Artificial Intelligence, CNN]
pin: false
math: true
mermaid: true
---

# 🧠 From Fully-Connected Layers to Convolutional Neural Networks (CNNs)


---

## 1️⃣ Fully-Connected (FC) Layer

### Definition

A fully-connected layer assumes **every input influences every output**.

Let:
- Input vector: $x \in \mathbb{R}^d$
- Weight matrix: $W \in \mathbb{R}^{c \times d}$
- Bias: $b \in \mathbb{R}^c$
- Output (score): $s \in \mathbb{R}^c$

Then,

$s = Wx + b$

📌 Example:
- CIFAR-10 image: $32 \times 32 \times 3 = 3072$
- Classes: $c = 10$
- Parameters: $3072 \times 10 + 10 = 30,730$

⚠️ Problems:
- Too many parameters
- No spatial structure
- Overfitting risk

---

## 2️⃣ Spatial Locality

### Key Idea

Nearby pixels are more related than distant ones.

Instead of connecting everything:
- Look at **local patches**
- Reuse the same filter across space

This leads to **convolution**.

🧠 Human intuition:
- Eyes, nose, edges → local patterns

---

## 3️⃣ Convolutional Layer

### Single-Channel (Grayscale)

- Input: $32 \times 32 \times 1$
- Filter: $3 \times 3$
- Operation: slide filter and compute dot product

At each location:

$y_{ij} = \sum_{u=1}^{3}\sum_{v=1}^{3} x_{i+u, j+v} \cdot w_{uv} + b$

---

### Multi-Channel (RGB)

- Input: $32 \times 32 \times 3$
- Filter: $3 \times 3 \times 3$

Each output pixel:

$y_{ij} = \sum_{c=1}^{3} \sum_{u,v} x_{i+u, j+v, c} \cdot w_{u,v,c} + b$

➡️ Parameters per filter:
$3 \times 3 \times 3 + 1 = 28$

---

## 4️⃣ Multiple Filters = Multiple Feature Maps

If we use $K$ filters:

- Output depth = $K$
- Output volume: $W' \times H' \times K$

📌 Example:
- Input: $32 \times 32 \times 3$
- Filters: $4$ filters of $5 \times 5 \times 3$
- Stride: $1$, Padding: $0$

Output size:

$W' = 32 - 5 + 1 = 28$

➡️ Output: $28 \times 28 \times 4$

---

## 5️⃣ Output Size Formula (Very Important ⭐)

For convolution:

$W' = \frac{W - F + 2P}{S} + 1$  
$H' = \frac{H - F + 2P}{S} + 1$

Where:
- $F$: filter size
- $S$: stride
- $P$: padding

---

## 6️⃣ Padding

### Why Padding?

- Preserve spatial size
- Allow deeper networks

Common choice:

$P = \frac{F - 1}{2}$

📌 Examples:
- $F=3 \Rightarrow P=1$
- $F=5 \Rightarrow P=2$

With padding, output size remains unchanged.

---

## 7️⃣ Padding Example

Given:
- Input: $32 \times 32 \times 3$
- Filters: $10$ of size $5 \times 5 \times 3$
- Stride: $1$
- Padding: $2$

Output:

$32 \times 32 \times 10$

Parameters:

$10 \times (5 \times 5 \times 3 + 1) = 760$

💥 Compared to FC:
$(32 \times 32 \times 10)(32 \times 32 \times 3 + 1) = 31,467,520$

🔥 Massive reduction!

---

## 8️⃣ 1×1 Convolution

### What is it?

Filter size: $1 \times 1 \times C$

Each filter computes:

$y = \sum_{c=1}^{C} x_c w_c + b$

➡️ Mixes **channels**, not space

📌 Example:
- Input: $32 \times 32 \times 3$
- Filters: $6$ of $1 \times 1 \times 3$

Output:
$32 \times 32 \times 6$

Parameters:
$6 \times (3 + 1) = 24$

✨ Used in:
- Bottlenecks
- Channel reduction
- ResNet, Inception

---

## 9️⃣ Nested Convolutional Layers

CNNs learn hierarchical features:

| Level | Learns |
|------|-------|
| Low | Edges, colors |
| Mid | Corners, textures |
| High | Objects, faces |

🎯 Each level builds on the previous one.

---

## 🔟 Stride

Stride controls how far the filter moves.

📌 Example:
- Input: $7 \times 7$
- Filter: $3 \times 3$
- Stride: $2$

Output size:

$\frac{7 - 3}{2} + 1 = 3$

➡️ Output: $3 \times 3$

🚀 Larger stride → smaller output → faster

---

## 1️⃣1️⃣ Pooling Layer

### Purpose

- Downsampling
- Reduce computation
- Improve robustness

No learning parameters ❌

---

### Max Pooling

Filter: $2 \times 2$, Stride: $2$

Selects:

$\max \{x_{ij}\}$

Preserves strongest activation 💪

---

### Average Pooling

Computes:

$\frac{1}{4}\sum x_{ij}$

Smoother but less sharp

---

## 1️⃣2️⃣ Pooling Output Size

Same formula as convolution (without padding):

$W' = \frac{W - F}{S} + 1$  
$H' = \frac{H - F}{S} + 1$

Depth remains unchanged.

---

## 1️⃣3️⃣ Convolution vs Fully-Connected

| Aspect | FC | Conv |
|------|----|------|
| Connectivity | Global | Local |
| Parameters | Huge | Small |
| Spatial info | ❌ | ✅ |
| Weight sharing | ❌ | ✅ |

🧠 Conv is a **special case of FC** with many zero weights.

---

## 🎯 Final Summary

### Convolutional Layer Hyperparameters

- Number of filters: $K$
- Filter size: $F$
- Stride: $S$
- Padding: $P$

Output:
$W' \times H' \times K$

Parameters:
$K(F^2C + 1)$

---

### Pooling Layer Hyperparameters

- Filter size: $F$
- Stride: $S$

Parameters:
$0$ ❌

---

## ✅ Takeaways

- CNNs exploit **spatial locality**
- Weight sharing drastically reduces parameters
- Deep stacking → hierarchical features
- Pooling + stride control resolution

🚀 CNNs scale to large images efficiently

