---
layout: post
title: "Pseudo-Inverse Matrix"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Pseudo-Inverse Matrix</b>
---
### <b>Pseudo-Inverse Matrix</b>
    Transpose
    Inverse

---
## <b>What is Pseudo-Inverse Matrix</b>

### 1. What is Pseudo-Inverse Matrix?

When we want to get optimal solution and the transformation have determinant is not zero, we can always solve the problem without difficulty. But we can usually see there're not having inverse in real-world.

For example, for linear regression, we collect some data and some data have linear dependence or same. It will happen to make <b>lack of rank, the matrix is not square but rectangular matrix</b>. That means in our real-world is not easy to solve the problem.

So Pseudo-Inverse is kind of solutions for get approximate optimal solution like Least Square.

Suppose we want to solve the linear system

$$
Ax = b
$$

where

- $A \in \mathbb{R}^{m \times n}$
- $x \in \mathbb{R}^{n}$
- $b \in \mathbb{R}^{m}$

If $A$ is **square and invertible**, the solution is

$$
x = A^{-1} b
$$

However, in many real-world problems:

- $m > n$ (more equations than unknowns)
- The system has <b>no exact solution</b>

So instead we solve the <b>least squares problem</b>:

$$
\min_x ||Ax - b||^2
$$

This finds the vector $x$ that <b>minimizes the squared error</b>.

### 2. Expanding the Objective Function

The squared error can be written as

$
||Ax-b||^2
\quad \rightarrow \quad
(Ax-b)^T(Ax-b)
\quad \rightarrow \quad
= x^T A^T A x - 2 b^T A x + b^T b
$

$
\frac{\partial}{\partial x}(x^T A^T A x) = 2A^TAx
,\quad
\frac{\partial}{\partial x}(b^TAx) = A^Tb
$

Therefore,

$$
\nabla_x ||Ax-b||^2
\quad \rightarrow \quad
2A^TAx - 2A^Tb
\quad \rightarrow \quad
2A^TAx - 2A^Tb = 0
$$

This equation is known as the **Normal Equation**.
$$
A^TAx = A^Tb
$$

If $A^TA$ is invertible, we obtain

$$
x = (A^TA)^{-1}A^Tb
$$

### 3. How to solve above $A^TA$ to be NOT invertible

The formula above requires $A^TA$ to be invertible.  
However, matrices can be **rank deficient**.

Using Singular Value Decomposition (SVD):

$$
A = U \Sigma V^T
$$

The pseudo-inverse is defined as

$$
A^+ = V \Sigma^+ U^T
$$

where $\Sigma^+$ contains the **reciprocal of the non-zero singular values**.

This definition works for **any matrix**.

#### Ex.

$$
A = \begin{bmatrix}
1 & 2 \\
2 & 4
\end{bmatrix}
\quad \rightarrow \quad
\text{rank}(A) = 1
$$

So the matrix is **rank deficient**.

$$
A^TA = \begin{bmatrix}
1 & 2 \\
2 & 4
\end{bmatrix}^T
\begin{bmatrix}
1 & 2 \\
2 & 4
\end{bmatrix}
= \begin{bmatrix}
5 & 10 \\
10 & 20
\end{bmatrix}
$$

$$
\det(A^TA) = 5 \times 20 - 10 \times 10 = 100 - 100 = 0
\quad \rightarrow \quad
A^+ = (A^TA)^{-1}A^T
$$

cannot be used.

Instead, we compute the pseudo-inverse using **SVD**.

$$
A = U \Sigma V^T
$$

Since the matrix is symmetric and rank 1, only **one singular value is non-zero**.

Find eigenvalues of $A$:

$$
A = \begin{bmatrix}
1 & 2 \\
2 & 4
\end{bmatrix}
$$

$$
\lambda_1 = 5, \quad \lambda_2 = 0
,\quad
v_1 =
\frac{1}{\sqrt{5}}
\begin{bmatrix}
1 \\
2
\end{bmatrix}
,\quad
v_2 =
\frac{1}{\sqrt{5}}
\begin{bmatrix}
-2 \\
1
\end{bmatrix}
$$


Thus

$$
V =
\begin{bmatrix}
\frac{1}{\sqrt{5}} & \frac{-2}{\sqrt{5}} \\
\frac{2}{\sqrt{5}} & \frac{1}{\sqrt{5}}
\end{bmatrix}
$$

$$
\Sigma =
\begin{bmatrix}
5 & 0 \\
0 & 0
\end{bmatrix}
\quad \rightarrow \quad
\Sigma^+ =
\begin{bmatrix}
\frac{1}{5} & 0 \\
0 & 0
\end{bmatrix}
$$

$$
U =
\begin{bmatrix}
\frac{1}{\sqrt{5}} & \frac{-2}{\sqrt{5}} \\
\frac{2}{\sqrt{5}} & \frac{1}{\sqrt{5}}
\end{bmatrix}
$$

The general formula is

$$
A^+ = V \Sigma^+ U^T
\quad \rightarrow \quad
A^+ = \frac{1}{25}
\begin{bmatrix}
1 & 2 \\
2 & 4
\end{bmatrix}
$$
