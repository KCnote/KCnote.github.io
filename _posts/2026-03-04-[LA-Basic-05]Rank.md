---
layout: post
title: "Rank and Null space"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Rank</b>
---
### <b>Prerequisites</b>
    Basis vector
    Independent/Dependent

---
## <b>What is Rank</b>

### 1. What is Rank?

The rank means number of independent directions in output space. In other word, it is directly related to span. 

Transformation Matrix Rank is <b>count of linearly independent information</b>.

$$
A =
\begin{bmatrix}
1 & 2 \\
2 & 4
\end{bmatrix}
$$

$
v_2(2,4) = 2 \times v_1(1,2)
$

The second column is just a multiple of the first column. So there's no information with the second column owing to already represent the information by first column.
$$
\text{rank}(A) = 1
$$

### 2. Geometric Interpret of Rank?

Rank represents the number of independent directions.

- rank = 1 → vectors lie on a line
- rank = 2 → vectors span a plane
- rank = n → full n-dimensional space

In other words, Rank = dimension of the subspace spanned by the vectors.

### 3. Data Interpret of Rank?

    Feature Redundancy
If a data matrix $X \in \mathbb{R}^{n \times d}$ has $ \text{rank}(X) < d $
Some features are redundant.
This is directly related to PCA.

    Linear Regression
$$
(X^T X)^{-1}
$$

We need $ \text{rank}(X) = d $

Otherwise:
The inverse does not exist. The solution is not unique. We use the pseudo-inverse instead.

    SVD
$$
A = U \Sigma V^T
$$

Then:

$$
\text{rank}(A) = \text{number of non-zero singular values}
$$

This is the most numerically stable definition in practice.

### 4. How to compute Rank
1. Gaussian Elimination

2. Determinant (Square Matrix Only)

3. SVD


### 5. What is null space

The **null space** (also called the **kernel**) of a matrix $A$ is the set of all vectors $x$ such that:

$$
A x = 0
$$

Formally:

$$
\text{Null}(A) = { x \mid A x = 0 }
$$

It represents all input vectors that are mapped to the zero vector.
In other word, all directions that disappear under the transformation.

$$
A =
\begin{bmatrix}
1 & 2 \\
2 & 4
\end{bmatrix}
$$

Solve:

$$
A x = 0
$$

This gives:

$$
\begin{cases}
x_1 + 2x_2 = 0 \\
2x_1 + 4x_2 = 0
\end{cases}
$$

The second equation is just a multiple of the first.

So:

$$
x_1 = -2x_2
$$

So the null space is a **line**.

Its dimension is:

$$
\text{nullity}(A) = 1
$$

### 6. Geometric Interpret of Null space?

If $A$ maps vectors from $\mathbb{R}^n$:

* Null space = directions collapsed to zero
* Column space = directions preserved

Large null space → more information loss
Small null space → closer to invertible transformation

### 7. Rank–Nullity Theorem

$$
\text{rank}(A) + \text{nullity}(A) = n
$$

Where:

rank = dimension of column space
nullity = dimension of null space
