---
layout: post
title: "Dot Product"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Dot Product</b>
---
### <b>Prerequisites</b>
    Basis vector
    Independent/Dependent

---
## <b>What is Dot Product</b>

### 1. What is Dot Product?

$$
\begin{bmatrix}
u_x & u_y
\end{bmatrix}
\begin{bmatrix}
x \\
y
\end{bmatrix} =
u_x x + u_y y
$$

This is exactly the <b>dot product</b>:

$$
u \cdot x
$$

A matrix represents a <b>linear transformation from $\mathbb{R}^2$ to $\mathbb{R}$</b>.


### 2. Geometric Meaning of the Dot Product
    Direction
    Similarity

$$
a \cdot b = |a||b|\cos\theta
$$

Where

* $|a|$ : magnitude of vector $a$
* $|b|$ : magnitude of vector $b$
* $\theta$ : angle between the two vectors

| Angle       | $\cos\theta$ | Meaning                             |
| ----------- | ------------ | ----------------------------------- |
| $0^\circ$   | $1$          | Same direction (maximum similarity) |
| $90^\circ$  | $0$          | Orthogonal (no similarity)          |
| $180^\circ$ | $-1$         | Opposite direction                  |

Thus, the dot product naturally measures **directional similarity**.

$$
\text{cosine similarity} =
\frac{a \cdot b}{|a||b|}
$$

This results in:

$$
\cos\theta
$$

which measures <b>pure directional similarity</b>.

Ex. Transformer attention

$$
score = Q \cdot K
$$

* $Q$ : query vector
* $K$ : key vector

If their directions are similar, the dot product becomes large, leading to higher attention weights.

Thus,

> The dot product measures how aligned two feature vectors are, making it a natural similarity metric.
