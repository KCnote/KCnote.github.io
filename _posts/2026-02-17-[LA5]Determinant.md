---
layout: post
title: "Determinant"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Intermediate]
tags: [Linear Algebra, Intermediate, Determinant]
pin: false
math: true
mermaid: true
---
# 🧮 Determinant = How Much Space Is Stretched or Squished

---

# 1️⃣ Determinant of a Transformation

A matrix represents a **linear transformation**.

The determinant tells you:

- How much **area (2D)** or **volume (3D)** is scaled
- Whether the transformation **flips orientation**

So:

$$
|\det(A)| = \text{area or volume scale factor}
$$

and

$$
\text{sign of } \det(A) = \text{orientation}
$$

---

# 2️⃣ 2D Case — Area Scaling

Take a $2 \times 2$ matrix:

$$
A =
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$

It transforms the basis vectors:

$$
A\hat{i} =
\begin{bmatrix}
a \\
c
\end{bmatrix},
\qquad
A\hat{j} =
\begin{bmatrix}
b \\
d
\end{bmatrix}
$$

A unit square becomes a **parallelogram** formed by these two vectors.

The determinant equals the signed area of that parallelogram:

$$
\det(A) = ad - bc
$$

So:

- If $|\det(A)| = 3$, area triples
- If $|\det(A)| = 0.5$, area halves
- If $\det(A) = 0$, space collapses to a line

---

# 3️⃣ Orientation (Why Determinant Can Be Negative)

Determinant is **signed area**.

- If $A\hat{j}$ is to the **left of** $A\hat{i}$ → orientation preserved → positive
- If $A\hat{j}$ is to the **right of** $A\hat{i}$ → orientation flipped → negative

So:

$$
\det(A) > 0 \Rightarrow \text{no flip}
$$

$$
\det(A) < 0 \Rightarrow \text{reflection occurred}
$$

---

# 4️⃣ Geometric Meaning of $a, b, c, d$

Given:

$$
A =
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$

- $a$ affects stretching along x-direction
- $d$ affects stretching along y-direction
- $b$ and $c$ introduce **shearing / diagonal distortion**

---

## Case 1: $b = 0$ and $c = 0$

$$
A =
\begin{bmatrix}
a & 0 \\
0 & d
\end{bmatrix}
$$

Then:

- x-direction stretched by $a$
- y-direction stretched by $d$

Area scaling:

$$
\det(A) = ad
$$

Pure rectangle → scaled rectangle.

---

## Case 2: $b$ and $c$ are not zero

Then the square becomes a **tilted parallelogram**.

- $b$ moves the x-basis in y-direction
- $c$ moves the y-basis in x-direction

They introduce **diagonal stretching / shearing**.

Area becomes:

$$
\det(A) = ad - bc
$$

So the cross-interaction term $bc$ reduces (or increases) area depending on orientation.

---

# 5️⃣ 3D Case — Volume Scaling

In 3D, determinant represents the **volume of a parallelepiped**.

$$
|\det(A)| = \text{volume scale factor}
$$

Sign represents orientation:

- Right-hand rule → positive
- Left-hand rule → negative

If determinant is zero:

Space collapses from 3D into a plane.

---

# 6️⃣ 3×3 Determinant (Cofactor Expansion)

For:

$$
\det
\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}
$$

Expand along first row:

$$
=
a
\det
\begin{bmatrix}
e & f \\
h & i
\end{bmatrix}
-
b
\det
\begin{bmatrix}
d & f \\
g & i
\end{bmatrix}
+
c
\det
\begin{bmatrix}
d & e \\
g & h
\end{bmatrix}
$$

Each $2\times2$ determinant equals:

$$
\begin{bmatrix}
x & y \\
z & w
\end{bmatrix}
=
xw - yz
$$

---

# 🔥 Big Picture Summary

Determinant tells you:

1. How much space is scaled
2. Whether orientation is preserved or flipped
3. Whether dimension collapses

In 2D:

$$
\det(A) = ad - bc
$$

In 3D:

$$
\det(A) = \text{signed volume of parallelepiped}
$$

---

# 🎯 Final Intuition

Think of determinant as:

> The amount of “space distortion” caused by the transformation.

- Stretching → magnitude increases
- Squishing → magnitude decreases
- Flipping → sign changes
- Collapsing → determinant zero

That is the true geometric meaning of determinant.
