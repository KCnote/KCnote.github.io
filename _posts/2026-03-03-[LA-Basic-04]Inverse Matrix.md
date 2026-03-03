---
layout: post
title: "Inverse Matrix"
date: 2026-03-01 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Inverse Matrix</b>
---
### <b>Prerequisites</b>
    Determinant

---
## <b>What is Inverse Matrix</b>

### 1. What is Inverse Matrix?

Think geometrically:

- $A$ → <b>Transformation</b>
- $X$ → vector in the “before” space
- $Y$ → vector in the “after” space

So solving:

$$
AX = Y \quad \rightarrow \quad X = A^{-1} Y
$$

A matrix is transformation from original space to A space.
But if we want to retransform from A space to original space, using inverse.

when we transform space, there're two features, scaling(included in orienetation) and direction.

> Scaling 

If it can be reformed from determinant.

> Direction

If it can be reformed from inverse direction.

### 2. Depends on determinant.

#### 2-1. $\det(A) \ne 0$

- Space is NOT collapsed
- Transformation is invertible
- Every output vector corresponds to exactly one input vector

$$
AX = Y \quad \rightarrow \quad X = A^{-1} Y
$$

The result have no squishing, no dimension loss.

Let's think A matrix is right shear and A inverse matrix is left shear.

Right shear:

$$
\begin{bmatrix}
1 & k \\
0 & 1
\end{bmatrix}
$$

Inverse is left shear:

$$
\begin{bmatrix}
1 & -k \\
0 & 1
\end{bmatrix}
$$

Original space is transformed to A space and retransform from A space to original space. The two transform result is same like the first time.

$$
A^{-1}A = I
$$

This transformation does nothing.

#### 2-2. $\det(A) = 0$

![matrix-inverse](/assets/img/develop/matrix-inverse.png)

- Space is squished to lower dimension
- Transformation is not invertible
- Some vectors collapse together

> Multiple input vectors map to the same output vector.

That's why sometimes there're no solution or infinitely many solutions.

### 3. How to calculate inverse matrix.

$$
A^{-1}=
\frac{1}{\det(A)}
\, \operatorname{adj}(A)
$$

where:
$$
\operatorname{adj}(A) =
\left[ C_{ij} \right]^T
, \quad
C_{ij}=
(-1)^{i+j}
\det(M_{ij})
$$

---
<b>Ex. Numerical Calculation</b>

$$
A =
\begin{bmatrix}
1 & 2 & 3 \\
0 & 1 & 4 \\
5 & 6 & 0
\end{bmatrix}
$$

> <b>determinant</b>

$$
\det(A) =
1
\begin{vmatrix}
1 & 4 \\
6 & 0
\end{vmatrix} -
2
\begin{vmatrix}
0 & 4 \\
5 & 0
\end{vmatrix} +
3
\begin{vmatrix}
0 & 1 \\
5 & 6
\end{vmatrix}
$$

> <b>Cofactor Matrix</b>

$$
C =
\begin{bmatrix}
-24 & 20 & -5 \\
18 & -15 & 4 \\
5 & -4 & 1
\end{bmatrix}
$$

where:

$$
C_{11} =
\begin{vmatrix}
1 & 4 \\
6 & 0
\end{vmatrix}
= 1\cdot0 - 4\cdot6
= -24
, \quad C_{12} =
\begin{vmatrix}
0 & 4 \\
5 & 0
\end{vmatrix}
= 0\cdot0 - 4\cdot5
= -20
, \quad ...
$$

> <b>Adjugate:</b>

$$
\operatorname{adj}(A) =
C^T =
\begin{bmatrix}
-24 & 18 & 5 \\
20 & -15 & -4 \\
-5 & 4 & 1
\end{bmatrix}
$$