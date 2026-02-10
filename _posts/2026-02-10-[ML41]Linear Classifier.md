---
layout: post
title: "Linear Classifier"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligence, Model, Linear Classifier]
pin: false
math: true
mermaid: true
---

# 🟨 Parametric Approach: Linear Classifier (Multi-class)

> **Goal:** build an image classifier that is **fast at test time** and does **not** require storing all training images (unlike kNN).  
> We do this by learning a **parametric model** with parameters **W** (and optionally **b**). ✨

---

## 1) 🧠 From “Image” to “Vector”

An image (e.g., CIFAR-10) is typically:

- Height × Width × Channels = $$32 \times 32 \times 3$$
- Total numbers (features):
$$
D = 32 \cdot 32 \cdot 3 = 3072
$$

We **flatten** the image into a vector:
$$
x \in \mathbb{R}^{D} = \mathbb{R}^{3072}
$$

---

## 2) 🧩 Linear Function for Classification

### Question
**What form should the classifier function be?**

### Answer (simple start)
Use a **linear function**: a weighted sum of input pixels (features).

For one class score:
$$
s = \sum_{j=1}^{D} w_j x_j
$$

For **C classes**, we compute a score for each class.

---

## 3) 🧮 Multi-class Linear Classifier (Matrix Form)

Let:

- $x \in \mathbb{R}^{D}$ : input image vector  
- $W \in \mathbb{R}^{C \times D}$ : weight matrix (one row per class)  
- $s \in \mathbb{R}^{C}$ : score vector  

### Score function
$$
s = f(x; W) = Wx
$$

### Dimension check (CIFAR-10 example)
- Classes: $$C = 10$$  
- Features: $$D = 3072$$  

So:
$$
W \in \mathbb{R}^{10 \times 3072},\quad x \in \mathbb{R}^{3072 \times 1}
$$
Therefore:
$$
s = Wx \in \mathbb{R}^{10 \times 1}
$$

Each entry $$s_k$$ is the score for class $$k$$ (airplane, automobile, …).

---

## 4) ➕ Adding Bias Term

Often we want an **input-independent offset** per class.

Let:
$$
b \in \mathbb{R}^{C}
$$

Then:
$$
s = f(x; W, b) = Wx + b
$$

### Why bias?
- Without bias, decision boundaries must pass through the origin (in feature space)
- Bias allows shifting decision boundaries **up/down** (or left/right), improving flexibility

---

## 5) 🧠 Bias Trick: Absorb Bias into the Matrix

A separate $$b$$ can be inconvenient, so we **augment** the vector.

Define augmented input:
$$
\tilde{x} =
\begin{bmatrix}
x \\
1
\end{bmatrix}
\in \mathbb{R}^{D+1}
$$

Define augmented weights:
$$
\tilde{W} =
\begin{bmatrix}
W & b
\end{bmatrix}
\in \mathbb{R}^{C \times (D+1)}
$$

Then:
$$
\tilde{W}\tilde{x}
=
\begin{bmatrix}
W & b
\end{bmatrix}
\begin{bmatrix}
x \\
1
\end{bmatrix}
= Wx + b
$$

✅ Now the model can be written simply as:
$$
s = \tilde{W}\tilde{x}
$$

---

## 6) ✅ Final Model Summary

A multi-class linear classifier outputs **scores**:

$$
s = Wx + b
$$

Prediction is typically:
$$
\hat{y} = \arg\max_{k \in \{1,\dots,C\}} s_k
$$

---

## 7) 🧪 Toy Example (4 Pixels, 3 Classes)

Suppose an image has **4 pixels** (already flattened):

$$
x =
\begin{bmatrix}
56 \\
231 \\
24 \\
2
\end{bmatrix}
$$

Three classes: **cat / dog / ship**.  
Let:

$$
W =
\begin{bmatrix}
0.2 & -0.5 & 0.1 & 2.0\\
1.5 & 1.3 & 2.1 & 0.0\\
0 & 0.25 & 0.2 & -0.3
\end{bmatrix},
\quad
b =
\begin{bmatrix}
1.1 \\
3.2 \\
0
\end{bmatrix}
$$

### Step 1) Compute raw scores $$Wx$$

#### Cat score (row 1)
$$
\begin{aligned}
(Wx)_\text{cat}
&= 0.2\cdot 56 + (-0.5)\cdot 231 + 0.1\cdot 24 + 2.0\cdot 2\\
&= 11.2 - 115.5 + 2.4 + 4\\
&= -97.9
\end{aligned}
$$

#### Dog score (row 2)
$$
\begin{aligned}
(Wx)_\text{dog}
&= 1.5\cdot 56 + 1.3\cdot 231 + 2.1\cdot 24 + 0\cdot 2\\
&= 84 + 300.3 + 50.4 + 0\\
&= 434.7
\end{aligned}
$$

#### Ship score (row 3)
$$
\begin{aligned}
(Wx)_\text{ship}
&= 0\cdot 56 + 0.25\cdot 231 + 0.2\cdot 24 + (-0.3)\cdot 2\\
&= 0 + 57.75 + 4.8 - 0.6\\
&= 61.95
\end{aligned}
$$

### Step 2) Add bias $$b$$

$$
s = Wx + b
$$

So:
$$
s_\text{cat} = -97.9 + 1.1 = -96.8
$$
$$
s_\text{dog} = 434.7 + 3.2 = 437.9
$$
$$
s_\text{ship} = 61.95 + 0 = 61.95
$$

### Step 3) Prediction
$$
\hat{y} = \arg\max\{-96.8,\ 437.9,\ 61.95\} = \text{dog}
$$

---

## 8) 🧭 Geometric Viewpoint

Each class $$k$$ has a weight vector (row of $$W$$):
$$
w_k^T \in \mathbb{R}^{1 \times D}
$$

Class score:
$$
s_k = w_k^T x + b_k
$$

### Decision boundary between class a and b
Predict class a if:
$$
w_a^T x + b_a > w_b^T x + b_b
$$

Rearrange:
$$
(w_a - w_b)^T x + (b_a - b_b) > 0
$$

Boundary (where equal):
$$
(w_a - w_b)^T x + (b_a - b_b) = 0
$$

✅ This is a **hyperplane** in $$\mathbb{R}^D$$.

### What changes do?
- Change in **W** → hyperplane **rotates**
- Change in **b** → hyperplane **shifts**

---

## 9) 🖼️ Visual Viewpoint (Template Matching 느낌)

A linear classifier can be seen as learning **templates**:

- During training: learn the template weights in $$W$$
- At test: compute dot-product similarity with each template

It is similar to kNN in that it uses “comparison”, but:

- kNN compares against **N training examples**
- Linear classifier compares against **C class templates** (usually $$C \ll N$$)

---

## 10) ✅ Why Parametric Models? (vs kNN)

### 👍 Advantages
- **Space efficient**: store only $$W$$ (and $$b$$), not all training data
- **Fast inference**: one matrix-vector multiply
  $$
  s = Wx + b
  $$

### 🚀 Test-time Complexity
If $$W \in \mathbb{R}^{C \times D}$$:
$$
O(CD)
$$

Typically much faster than kNN’s:
$$
O(ND)
$$

---

## 🔥 One-line Summary

✅ **Linear classifier**: learn parameters $$W,b$$ so that  
$$
s = Wx + b,\quad \hat{y} = \arg\max_k s_k
$$

✨ Fast, compact, and a foundation for modern deep learning classifiers.
