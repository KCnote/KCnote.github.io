---
layout: post
title: "Symmetric Matrix"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Symmetric Matrix</b>
---
### <b>Prerequisites</b>
    NOTHING

---
## <b>What is Symmetric Matrices</b>

### 1. What is Symmetric Matrices?

A matrix $A \in \mathbb{R}^{n \times n}$ is **symmetric** if

$$
A = A^T
$$

Example:

$$
A =
\begin{bmatrix}
2 & 1 & 3 \\
1 & 4 & 5 \\
3 & 5 & 6
\end{bmatrix}
$$

Since

$$
A_{ij} = A_{ji}
$$

the matrix is symmetric.

### 2. Eigenvectors of a Symmetric Matrix Are Orthogonal

One of the most important results in linear algebra is:

> <b>Eigenvectors corresponding to different eigenvalues of a symmetric matrix are orthogonal.</b>

Let

$$
Av_1 = \lambda_1 v_1
$$

$$
Av_2 = \lambda_2 v_2
$$

with

$$
\lambda_1 \ne \lambda_2
$$

Then

$$
v_1^T v_2 = 0
$$

which means

$$
v_1 \perp v_2
$$

Thus, eigenvectors of symmetric matrices form an **orthogonal basis**.

> A symmetric matrix can always be diagonalized using an **orthogonal matrix**.

$$
A = Q \Lambda Q^T
$$

Where

* $Q$ is an orthogonal matrix
* $\Lambda$ is a diagonal matrix of eigenvalues

$$
Q^T Q = I
$$

This is called **orthogonal diagonalization**.


> Proof

$
Av_1 = \lambda_1 v_1
\quad \rightarrow \quad
v_2^T A v_1 = \lambda_1 v_2^T v_1
$

$
v_2^T A v_1 \; is\; scalar
\quad \rightarrow \quad
v_2^T A v_1 = (v_2^T A v_1)^T = v_1^T A^T v_2 = v_1^T A v_2
$

$
v_1^T A v_2 = \lambda_2 v_1^T v_2 = \lambda_1 v_2^T v_1 
$

Thus:

$$
(\lambda_1 - \lambda_2) v_1^T v_2 = 0
$$

Since $\lambda_1 \ne \lambda_2$

$$
v_1^T v_2 = 0
$$

### 3. Important Properties of Symmetric Matrices

> 3-1. All Eigenvalues Are Real

There are **no complex eigenvalues**.

> 3-2. Eigenvectors Are Orthogonal

Eigenvectors corresponding to different eigenvalues satisfy

$$
v_i^T v_j = 0 \quad (i \ne j)
$$

After normalization they form an **orthonormal basis**.

> 3-3. Orthogonal Diagonalization

A symmetric matrix always has the decomposition

$$
A = Q \Lambda Q^T
$$

> 3-4. Quadratic Form

For any vector $x$

$$
x^T A x
$$

represents a **quadratic form**, whose geometry depends on the eigenvalues of $A$.

$
x^T A x,\;\; A = Q \Lambda Q^T
$

$
x^T A x = x^T Q \Lambda Q^T x = y^T \Lambda y
\quad \leftarrow \quad
y = Q^T x
$

Since $\Lambda$ is diagonal,

$$
x^T A x =
\lambda_1 y_1^2 +
\lambda_2 y_2^2 +
\cdots +
\lambda_n y_n^2
$$

> 3-5. Positive Definite Case

$$
x^T A x > 0
$$

for all $x \ne 0$, then $A$ is **positive definite**.

$$
\lambda_i > 0\; +\; Symmetric \rightarrow Positive\; Definite
$$

### 4. Geometric Interpretation

A symmetric matrix performs a transformation that:

* **stretches space along orthogonal directions**
* those directions are the **eigenvectors**
* the stretching factors are the **eigenvalues**

> orthogonal basis directions → scaled independently
