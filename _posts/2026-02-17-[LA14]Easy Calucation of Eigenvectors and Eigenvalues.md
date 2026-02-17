---
layout: post
title: "Easy Calcuation of Eigenvectors and Eigenvalues"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Intermediate]
tags: [Linear Algebra, Intermediate, Eigenvector, Eigenvalue]
pin: false
math: true
mermaid: true
---
# Eigenvalues of a 2×2 Matrix --- Trace, Determinant & Pauli Form

------------------------------------------------------------------------

## 1️⃣ Trace = Sum of Eigenvalues

For a matrix

$$
A = \begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$

The trace is:

$$
\mathrm{tr}(A) = a + d
$$

The average of eigenvalues:

$$
m = \frac{a + d}{2}
= \frac{\lambda_1 + \lambda_2}{2}
$$

So:

$$
\lambda_1 + \lambda_2 = a + d
$$

------------------------------------------------------------------------

## 2️⃣ Determinant = Product of Eigenvalues

The determinant is:

$$
\det(A) = ad - bc
$$

And this equals the product of eigenvalues:

$$
\lambda_1 \lambda_2 = ad - bc
$$

Let:

$$
p = ad - bc
$$

------------------------------------------------------------------------

## 3️⃣ Eigenvalue Formula

Characteristic equation:

$$
\det(A - \lambda I) = 0
$$

Compute:

$$
\det
\begin{bmatrix}
a - \lambda & b \\
c & d - \lambda
\end{bmatrix}
=
(a - \lambda)(d - \lambda) - bc
$$

Expanding:

$$
\lambda^2 - (a + d)\lambda + (ad - bc) = 0
$$

Using

-   $m = \frac{a+d}{2}$
-   $p = ad - bc$

Eigenvalues are:

$$
\lambda_{1,2}
=
m \pm \sqrt{m^2 - p}
$$

------------------------------------------------------------------------

## 📌 Geometric Meaning

-   Trace → controls average scaling
-   Determinant → controls area scaling
-   Discriminant $m^2 - p$ determines real vs complex eigenvalues

------------------------------------------------------------------------

## 📌 Pauli Matrix Representation

Any 2×2 Hermitian matrix can be written as:

$$
a\sigma_x + b\sigma_y + c\sigma_z
$$

Where:

$$
\sigma_x =
\begin{bmatrix}
0 & 1 \\
1 & 0
\end{bmatrix}
$$

$$
\sigma_y =
\begin{bmatrix}
0 & -i \\
i & 0
\end{bmatrix}
$$

$$
\sigma_z =
\begin{bmatrix}
1 & 0 \\
0 & -1
\end{bmatrix}
$$

Then the matrix becomes:

$$
\begin{bmatrix}
c & a - bi \\
a + bi & -c
\end{bmatrix}
$$

For this matrix:

$$
m = 0
$$

$$
p = - (a^2 + b^2 + c^2)
$$

Eigenvalues:

$$
\lambda = \pm \sqrt{a^2 + b^2 + c^2}
$$

If:

$$
a^2 + b^2 + c^2 = 1
$$

Then:

$$
\lambda = \pm 1
$$

------------------------------------------------------------------------

## Complex Number Explanation

The symbol $i$ is the imaginary unit satisfying:

$$
i^2 = -1
$$

It appears in $\sigma_y$ to allow complex Hermitian structure. This
representation connects 3D vectors $(a,b,c)$ to 2×2 matrices in quantum
mechanics.

The eigenvalues depend only on the vector length
$\sqrt{a^2 + b^2 + c^2}$.
