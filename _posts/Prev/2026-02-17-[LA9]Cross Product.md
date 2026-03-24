---
layout: post
title: "Cross Product"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Intermediate]
tags: [Linear Algebra, Intermediate, Cross Product]
pin: false
math: true
mermaid: true
---
# 🔺 Cross Product, Determinant, and Area

---

# 1️⃣ Cross Product Formula

For two vectors in $\mathbb{R}^3$:

$$
v =
\begin{bmatrix}
v_1 \\
v_2 \\
v_3
\end{bmatrix}
,\qquad
w =
\begin{bmatrix}
w_1 \\
w_2 \\
w_3
\end{bmatrix}
$$

The cross product is:

$$
v \times w
=
\begin{vmatrix}
\hat{i} & \hat{j} & \hat{k} \\
v_1 & v_2 & v_3 \\
w_1 & w_2 & w_3
\end{vmatrix}
$$

Expanding:

$$
=
\hat{i}(v_2 w_3 - v_3 w_2)
-
\hat{j}(v_1 w_3 - v_3 w_1)
+
\hat{k}(v_1 w_2 - v_2 w_1)
$$

Each component is a $2\times2$ determinant.

---

# 2️⃣ Magnitude = Area of Parallelogram

The magnitude of the cross product is:

$$
\|v \times w\|
=
\|v\| \|w\| \sin\theta
$$

This equals the **area of the parallelogram** formed by $v$ and $w$.

So:

- More perpendicular → $\sin\theta$ large → area large
- Similar direction → $\sin\theta$ small → area small
- Same direction → area = 0

---

# 3️⃣ Orientation (Sign)

In 2D, we can think of a signed area:

$$
\det
\begin{bmatrix}
v_1 & w_1 \\
v_2 & w_2
\end{bmatrix}
=
v_1 w_2 - v_2 w_1
$$

This equals the z-component of $v \times w$.

Interpretation:

- If $v$ is counterclockwise (left) of $w$ → positive
- If $v$ is clockwise (right) of $w$ → negative

So orientation determines sign.

---

# 4️⃣ Fundamental Basis Example

$$
\hat{i} \times \hat{j} = \hat{k}
$$

Right-hand rule:

- Point fingers along $\hat{i}$
- Curl toward $\hat{j}$
- Thumb points in $+\hat{k}$ direction

So:

$$
\hat{i} \times \hat{j} = +\hat{k}
$$

But:

$$
\hat{j} \times \hat{i} = -\hat{k}
$$

Cross product is **not commutative**.

---

# 5️⃣ Connection to Determinant

In 2D:

$$
\text{Area scaling by matrix } A =
|\det(A)|
$$

Original area × determinant = new area.

If a matrix transforms two vectors:

$$
A v \times A w
$$

Its magnitude equals:

$$
|\det(A)| \cdot \|v \times w\|
$$

So determinant measures:

> How much the transformation changes area.

---

# 6️⃣ Geometric Summary

Cross product:

- Gives a vector perpendicular to both $v$ and $w$
- Magnitude = area
- Direction = orientation

Determinant:

- Measures area scaling
- Encodes orientation sign
- Zero determinant → area collapses

---

# 🔥 Final Intuition

Cross product = geometric area measurement.

Determinant = transformation’s area scaling factor.

So:

Original area × determinant  
=
New area

And:

- More perpendicular → bigger area
- More aligned → smaller area
- Same direction → zero area

That is the deep connection between cross product and determinant.
