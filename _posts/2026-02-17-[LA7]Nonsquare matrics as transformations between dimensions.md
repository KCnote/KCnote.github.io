---
layout: post
title: "Nonsquare Matrics as Transformations between Dimensions"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Intermediate]
tags: [Linear Algebra, Intermediate, Dimension]
pin: false
math: true
mermaid: true
---
# 📐 Rectangular Matrices — Geometric Interpretation

A matrix does not just "multiply numbers."

It defines a **linear transformation** between spaces.

If a matrix has size:

$$
m \times n
$$

Then it represents a transformation:

$$
A : \mathbb{R}^n \rightarrow \mathbb{R}^m
$$

- Number of columns $n$ → dimension of input space
- Number of rows $m$ → dimension of output space

---

# 1️⃣ 3×2 Matrix  
### Mapping 2D → 3D

Let:

$$
A =
\begin{bmatrix}
a & b \\
c & d \\
e & f
\end{bmatrix}
$$

This is a $3 \times 2$ matrix.

So:

$$
A : \mathbb{R}^2 \rightarrow \mathbb{R}^3
$$

---

## 🔹 What Does This Mean?

Input vector:

$$
x =
\begin{bmatrix}
x_1 \\
x_2
\end{bmatrix}
$$

Output:

$$
Ax =
x_1
\begin{bmatrix}
a \\
c \\
e
\end{bmatrix}
+
x_2
\begin{bmatrix}
b \\
d \\
f
\end{bmatrix}
$$

The two columns of $A$ live in **3D space**.

So the output is always inside:

> The plane spanned by the two column vectors.

---

## 🎯 Geometric Meaning

A 2D space is being embedded into 3D.

Possible outcomes:

- If columns are independent → output is a **plane in 3D**
- If columns are dependent → output is a **line in 3D**

But it can never fill full 3D space because:

$$
\text{rank} \le 2
$$

---

# 2️⃣ 2×3 Matrix  
### Mapping 3D → 2D

Let:

$$
A =
\begin{bmatrix}
a & b & c \\
d & e & f
\end{bmatrix}
$$

This is $2 \times 3$.

So:

$$
A : \mathbb{R}^3 \rightarrow \mathbb{R}^2
$$

---

## 🔹 What Happens?

Input vector:

$$
x =
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
$$

Output:

$$
Ax =
x_1
\begin{bmatrix}
a \\
d
\end{bmatrix}
+
x_2
\begin{bmatrix}
b \\
e
\end{bmatrix}
+
x_3
\begin{bmatrix}
c \\
f
\end{bmatrix}
$$

We start with 3 basis directions.

But output only has 2 dimensions.

So something must collapse.

---

## 🎯 Geometric Meaning

This transformation:

- Projects 3D space onto a 2D plane.
- Always reduces dimension.

Rank can be:

- 2 → full 2D output
- 1 → collapse to line
- 0 → everything goes to origin

---

# 3️⃣ 1×2 Matrix  
### Mapping 2D → Number Line

Let:

$$
A =
\begin{bmatrix}
a & b
\end{bmatrix}
$$

This is $1 \times 2$.

So:

$$
A : \mathbb{R}^2 \rightarrow \mathbb{R}
$$

---

## 🔹 What Does It Do?

Input:

$$
x =
\begin{bmatrix}
x_1 \\
x_2
\end{bmatrix}
$$

Output:

$$
Ax = ax_1 + bx_2
$$

This is a **dot product** with vector:

$$
\begin{bmatrix}
a \\
b
\end{bmatrix}
$$

---

## 🎯 Geometric Meaning

It:

- Projects 2D vectors onto a line
- Collapses plane into 1 dimension

All vectors land somewhere on the number line.

If $(a,b)$ is nonzero → rank = 1  
If $(a,b) = (0,0)$ → rank = 0

---

# 4️⃣ General Rule for Rectangular Matrices

For $m \times n$ matrix:

$$
A : \mathbb{R}^n \rightarrow \mathbb{R}^m
$$

Key ideas:

- Columns determine output directions.
- Rank = number of independent columns.
- Output space dimension = rank.
- Rank ≤ min$(m,n)$.

---

# 🔥 Dimension Behavior

If:

- $m > n$ → embedding into higher dimension
- $m < n$ → projection / compression
- $m = n$ → possible invertibility

---

# 🎯 Big Picture

| Matrix Size | Interpretation |
|-------------|---------------|
| $3 \times 2$ | 2D plane embedded in 3D |
| $2 \times 3$ | 3D projected to 2D |
| $1 \times 2$ | 2D projected to number line |
| $m \times n$ | $\mathbb{R}^n \rightarrow \mathbb{R}^m$ |

---

# 🧠 Final Intuition

Rectangular matrices always:

- Either embed into higher dimension  
- Or collapse into lower dimension  

They reshape space by:

- Keeping some directions
- Destroying others

Everything depends on **rank**.

That is the geometric meaning of rectangular matrices.
