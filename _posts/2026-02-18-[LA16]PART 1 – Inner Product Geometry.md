---
layout: post
title: "PART 1 --- Inner Product Geometry"
date: 2026-02-18 00:00:00 +0900
author: kang
categories: [Linear Algebra, Temporary]
tags: [Linear Algebra]
pin: false
math: true
mermaid: true
---
# 📘 PART 1 --- Inner Product Geometry

------------------------------------------------------------------------

# 1️⃣ Vector Inner Product --- Full Numerical Expansion

Let

$$
x =
\begin{bmatrix}
1 \\
2 \\
-1
\end{bmatrix},
\quad
y =
\begin{bmatrix}
3 \\
0 \\
4
\end{bmatrix}
$$

## Step 1: Definition of Inner Product

For vectors in $\mathbb{R}^n$:

$$
x^T y = \sum_{i=1}^{n} x_i y_i
$$

## Step 2: Substitute Values

$$
x^T y = (1)(3) + (2)(0) + (-1)(4)
$$

$$
= 3 + 0 - 4
$$

$$
= -1
$$

------------------------------------------------------------------------

# 2️⃣ Vector Norm and Geometric Meaning

## Norm Definition

$$
\|x\| = \sqrt{x^T x}
$$

### Compute $\|x\|$

$$
x^T x = 1^2 + 2^2 + (-1)^2
$$

$$
= 1 + 4 + 1 = 6
$$

$$
\|x\| = \sqrt{6}
$$

### Compute $\|y\|$

$$
y^T y = 3^2 + 0^2 + 4^2
$$

$$
= 9 + 0 + 16 = 25
$$

$$
\|y\| = 5
$$

------------------------------------------------------------------------

# 3️⃣ Cosine Similarity --- Full Derivation

By definition:

$$
x^T y = \|x\|\|y\|\cos\theta
$$

Thus:

$$
\cos\theta =
\frac{x^T y}{\|x\|\|y\|}
$$

Substitute:

$$
\cos\theta =
\frac{-1}{\sqrt{6} \cdot 5}
$$

$$
= -\frac{1}{5\sqrt{6}}
$$

This implies:

-   Angle is obtuse (negative cosine)
-   Vectors point in roughly opposite directions

------------------------------------------------------------------------

# 4️⃣ Matrix Multiplication --- Step-by-Step

Let

$$
A =
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix},
\quad
B =
\begin{bmatrix}
2 & 0 \\
1 & 5
\end{bmatrix}
$$

Compute $C = AB$

## Element (1,1)

$$
(1)(2) + (2)(1) = 2 + 2 = 4
$$

## Element (1,2)

$$
(1)(0) + (2)(5) = 0 + 10 = 10
$$

## Element (2,1)

$$
(3)(2) + (4)(1) = 6 + 4 = 10
$$

## Element (2,2)

$$
(3)(0) + (4)(5) = 0 + 20 = 20
$$

Thus

$$
C =
\begin{bmatrix}
4 & 10 \\
10 & 20
\end{bmatrix}
$$

------------------------------------------------------------------------

# 5️⃣ Correlation Coefficient as Normalized Inner Product

Let

$$
x =
\begin{bmatrix}
1 \\
2 \\
3
\end{bmatrix},
\quad
y =
\begin{bmatrix}
2 \\
4 \\
6
\end{bmatrix}
$$

## Step 1: Compute Means

$$
\bar{x} = \frac{1+2+3}{3} = 2
$$

$$
\bar{y} = \frac{2+4+6}{3} = 4
$$

## Step 2: Mean Center

$$
x_c =
\begin{bmatrix}
-1 \\
0 \\
1
\end{bmatrix}
$$

$$
y_c =
\begin{bmatrix}
-2 \\
0 \\
2
\end{bmatrix}
$$

## Step 3: Compute Inner Product

$$
x_c^T y_c =
(-1)(-2) + 0 + (1)(2)
$$

$$
= 2 + 2 = 4
$$

## Step 4: Compute Norms

$$
\|x_c\| =
\sqrt{(-1)^2 + 0 + 1^2}
= \sqrt{2}
$$

$$
\|y_c\| =
\sqrt{(-2)^2 + 0 + 2^2}
= \sqrt{8}
= 2\sqrt{2}
$$

## Step 5: Correlation

$$
\rho =
\frac{4}{\sqrt{2} \cdot 2\sqrt{2}}
$$

$$
= \frac{4}{4}
$$

$$
= 1
$$

This confirms perfect linear correlation.

------------------------------------------------------------------------

# 6️⃣ Orthogonality Example

Let

$$
u =
\begin{bmatrix}
1 \\
1
\end{bmatrix},
\quad
v =
\begin{bmatrix}
1 \\
-1
\end{bmatrix}
$$

Compute:

$$
u^T v = 1\cdot1 + 1(-1) = 1 - 1 = 0
$$

Therefore vectors are orthogonal.

------------------------------------------------------------------------

# 🔥 Key Takeaways

-   Inner product measures alignment
-   Norm gives vector length
-   Cosine similarity measures angle
-   Correlation is normalized inner product after centering
-   Orthogonality ⇔ inner product = 0
