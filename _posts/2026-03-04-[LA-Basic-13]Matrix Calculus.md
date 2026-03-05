---
layout: post
title: "Matrix Calculus"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Matrix Calculus</b>
---
### <b>Matrix Calculus</b>
    Calculus

---
## <b>What is Matrix Calculus</b>

### 1. What is Matrix Calculus?

Matrix calculus is widely used in **machine learning, optimization, computer vision, and statistics**.  
It extends ordinary derivatives to **vectors and matrices**.

> 1. Scalar Function of a Vector

$$
f(x) \in \mathbb{R}
,\quad
x \in \mathbb{R}^n
$$

The derivative of \(f\) with respect to \(x\) is called the **gradient**:

$$
\nabla_x f =
\begin{bmatrix}
\frac{\partial f}{\partial x_1} \\
\frac{\partial f}{\partial x_2} \\
\vdots \\
\frac{\partial f}{\partial x_n}
\end{bmatrix}
$$

This gives the **direction of the steepest increase**.

---

> 2. Derivative of a Linear Function


$$
f(x) = a^T x
,\quad
a \in \mathbb{R}^n
$$

Then

$$
\frac{\partial}{\partial x}(a^T x) = a
,\quad
\frac{\partial}{\partial x}(x^T a) = a
$$

> 3. Derivative of a Quadratic Form

$$
f(x) = x^T A x
,\quad 
A \in \mathbb{R}^{n \times n}
$$

The derivative is

$$
\frac{\partial}{\partial x}(x^T A x) = (A + A^T)x
$$

> 4. Norm Derivative

$$
f(x) = ||x||^2
\quad \rightarrow \quad
||x||^2 = x^T x
$$

The derivative becomes

$$
\frac{\partial}{\partial x}(x^T x) = 2x
$$

> 5. Least Squares Derivative

$$
f(x) = ||Ax - b||^2
$$

where

- $A \in \mathbb{R}^{m\times n},\;  x \in \mathbb{R}^n,\; b \in \mathbb{R}^m$

$$
f(x) = (Ax-b)^T(Ax-b)
= x^T A^T A x - 2b^T A x + b^T b
$$

Now differentiate with respect to \(x\):

$$
\nabla_x f = 2A^TAx - 2A^Tb
$$

Setting the gradient to zero gives the **normal equation**:

$$
A^TAx = A^Tb
$$

> 6. Trace trick

Often matrix derivatives are easier using **trace identities**.

Example:

$$
x^T A x = \text{tr}(x^T A x)
$$

Using trace rules simplifies many derivations in machine learning.
