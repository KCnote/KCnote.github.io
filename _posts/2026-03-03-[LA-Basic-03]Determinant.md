---
layout: post
title: "Determinant"
date: 2026-03-01 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Determinant</b>
---
### <b>Prerequisites</b>
    Basis Vector
    Span
    Linear independent/dependent

---
## <b>What is Determinant</b>

### 1. What is Determinant?

A matrix represents a <b>linear transformation</b>.

The determinant tells you How much <b>area (2D)</b> is scaled or Whether the transformation <b>flips orientation</b>

$$
|\det(A)| = \text{area or volume scale factor}
$$

- Stretching → magnitude increases
- Squishing → magnitude decreases
- Collapsing → determinant zero

### 2. Orientation of determinant.

$$
\text{sign of } \det(A) = \text{orientation}
$$

Determinant is NOT absolute are, but <b>signed</b> area.

- If $A\hat{j}$ is to the **left of** $A\hat{i}$ → orientation preserved → positive
- If $A\hat{j}$ is to the **right of** $A\hat{i}$ → orientation flipped → negative

So:

$$
\det(A) > 0 \Rightarrow \text{no flip}
$$

$$
\det(A) < 0 \Rightarrow \text{reflection occurred}
$$

- Flipping → sign changes


### 3. 2D Matrix Interpret

>### 3-1. Scale of Determinant
Take a $2 \times 2$ matrix:

$$
A =
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$

where basis vector: 

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

- Left column → where $\hat{i}$ lands
- Right column → where $\hat{j}$ lands

A unit square becomes a <b>parallelogram</b> formed by these two vectors.

The determinant equals the signed area of that parallelogram:

$$
\det(A) = ad - bc
$$

If $\det(A) = 0$, space collapses to a line

>### 3-2. Area shape of Determinant

- $a$ affects stretching along x-direction
- $d$ affects stretching along y-direction
- $b$ and $c$ introduce **shearing / diagonal distortion**

##### Case 1: $b = 0$ and $c = 0$

$$
A =
\begin{bmatrix}
a & 0 \\
0 & d
\end{bmatrix}
$$

Just x-direction stretched by $a$ and y-direction stretched by $d$.
Pure rectangle → scaled rectangle.

##### Case 2: $b$ and $c$ are not zero

Then the square becomes a <b>tilted parallelogram</b>.

- $b$ moves the x-basis in y-direction
- $c$ moves the y-basis in x-direction

They introduce <b>diagonal stretching / shearing</b>.
So the cross-interaction term $bc$ reduces (or increases) area depending on orientation.

### 4. 3x3 Determinant

$$
det(A)=
\det
\begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}
$$

$$
\det(A) = a(ei-fh)+b(fg-di)+c(dh-eg)
$$

### 5. NxN Determinant

$$
det = 
\begin{bmatrix}
C_{11} & C_{12} & C_{13} \\
C_{21} & C_{22} & C_{23} \\
C_{31} & C_{23} & C_{33}
\end{bmatrix}
$$

$$
\det(A) = a_{11}C_{11}+a_{12}C_{12}+a_{13}C_{13}
$$

by Row:
$$
\det(A)
=
\sum_{j=1}^{N}
a_{ij}
(-1)^{i+j}
\det(M_{ij})
$$

by Column:
$$
\det(A)
=
\sum_{i=1}^{N}
a_{ij}
(-1)^{i+j}
\det(M_{ij})
$$
