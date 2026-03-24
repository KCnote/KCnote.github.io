---
layout: post
title: "Eigenvectors and Eigenvalues"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Advanced]
tags: [Linear Algebra, Advanced, Eigenvector, Eigenvalue]
pin: false
math: true
mermaid: true
---
# 📘 Eigenvalues, Diagonal Matrices, and Eigenbasis

---

# 1️⃣ Matrix–Vector Multiplication

Matrix multiplication transforms a vector:

$$
A\vec{v}
$$

An **eigenvector** is special because:

$$
A\vec{v} = \lambda \vec{v}
$$

Left side → matrix transformation  
Right side → simple scalar scaling  

So an eigenvector is:

> A vector whose direction does not change under the transformation.

---

# 2️⃣ From Eigen Equation to Characteristic Equation

Start from:

$$
A\vec{v} = \lambda \vec{v}
$$

Move everything to one side:

$$
A\vec{v} - \lambda I \vec{v} = 0
$$

Factor:

$$
(A - \lambda I)\vec{v} = 0
$$

We want a **nonzero** solution for $\vec{v}$.

A homogeneous system has nonzero solutions only if:

$$
\det(A - \lambda I) = 0
$$

This is the **characteristic equation**.

Solving it gives eigenvalues $\lambda$.

---

# 3️⃣ Diagonal Matrix Case

Consider:

$$
D =
\begin{bmatrix}
-5 & 0 & 0 \\
0 & -2 & 0 \\
0 & 0 & 4
\end{bmatrix}
$$

For a diagonal matrix:

- The eigenvalues are exactly the diagonal entries.
- No computation needed.

So:

$$
\lambda_1 = -5, \quad
\lambda_2 = -2, \quad
\lambda_3 = 4
$$

---

# 4️⃣ What About Eigenvectors?

For a diagonal matrix:

$$
D\vec{v} =
\begin{bmatrix}
\lambda_1 v_1 \\
\lambda_2 v_2 \\
\lambda_3 v_3
\end{bmatrix}
$$

Each coordinate scales independently.

---

## ✅ Case 1: All eigenvalues distinct

If all diagonal entries are different:

- Each axis direction is an eigenvector.
- The eigenvectors are:

$$
\vec{e}_1 =
\begin{bmatrix}
1 \\ 0 \\ 0
\end{bmatrix},
\quad
\vec{e}_2 =
\begin{bmatrix}
0 \\ 1 \\ 0
\end{bmatrix},
\quad
\vec{e}_3 =
\begin{bmatrix}
0 \\ 0 \\ 1
\end{bmatrix}
$$

These form a basis.

---

## ⚠️ Case 2: Repeated eigenvalues

Example:

$$
D =
\begin{bmatrix}
3 & 0 \\
0 & 3
\end{bmatrix}
$$

Then:

$$
D = 3I
$$

Now:

$$
D\vec{v} = 3\vec{v}
$$

This means:

> EVERY vector is an eigenvector.

Because everything just scales by 3.

So:

- Sometimes eigenvectors are only axis directions
- Sometimes the entire space
- Sometimes only one line (non-diagonalizable case)

---

# 5️⃣ Geometric Meaning

Eigenvectors are directions that:

- Do not rotate
- Do not shear
- Only scale

If a matrix squishes space into a line:

- Only vectors along that line survive directionally
- That line is the eigenspace

---

# 6️⃣ Diagonalization

If a matrix has enough independent eigenvectors:

We can write:

$$
A = PDP^{-1}
$$

Where:

- $D$ = diagonal matrix of eigenvalues
- Columns of $P$ = eigenvectors

Then:

$$
A^n = P D^n P^{-1}
$$

And:

$$
D^n =
\begin{bmatrix}
\lambda_1^n & 0 \\
0 & \lambda_2^n
\end{bmatrix}
$$

This makes computing powers extremely easy.

---

# 7️⃣ Example Matrix

Consider:

$$
A =
\begin{bmatrix}
0 & 1 \\
1 & 1
\end{bmatrix}
$$

Its eigenvectors are:

$$
\vec{v}_1 =
\begin{bmatrix}
2 \\
1 + \sqrt{5}
\end{bmatrix}
,
\quad
\vec{v}_2 =
\begin{bmatrix}
2 \\
1 - \sqrt{5}
\end{bmatrix}
$$

If we form:

$$
P =
\begin{bmatrix}
2 & 2 \\
1+\sqrt{5} & 1-\sqrt{5}
\end{bmatrix}
$$

Then:

$$
A = P D P^{-1}
$$

So:

$$
A^n = P D^n P^{-1}
$$

---

# 8️⃣ Important Summary

For a diagonal matrix:

- Eigenvalues = diagonal entries
- Eigenvectors depend on multiplicity

Possible situations:

| Situation | Eigenvectors |
|------------|--------------|
| Distinct eigenvalues | Exactly axis directions |
| Repeated eigenvalue but full dimension | Entire space |
| Repeated but defective | Fewer eigenvectors than dimension |

---

# 🎯 Final Intuition

Eigenvectors = directions preserved by transformation  
Eigenvalues = amount of stretching  

Diagonal matrix = pure scaling in independent directions  

If a matrix becomes diagonal in some basis →  
That basis is the **eigenbasis**.

