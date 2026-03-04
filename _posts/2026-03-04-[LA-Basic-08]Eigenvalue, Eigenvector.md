---
layout: post
title: "Eigenvectors and Eigenvalues"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Eigenvectors and Eigenvalues</b>
---
### <b>Prerequisites</b>
    transformation

---
## <b>What is Eigenvectors and Eigenvalues</b>

### 1. What is Eigenvectors and Eigenvalues?

The eigenvectors and eigenvalues are <b>related with transformation status</b>.
Usually almost vectors on original space are change their direction. But eigenvector is not changed by transformation. Just stretch or squish the eigenvector by the size of the eigenvalues.

$$
A\vec{v}
$$

An **eigenvector** is special because:

$$
A\vec{v} = \lambda \vec{v}
$$


### 2. From Eigen Equation to Characteristic Equation

$$
A\vec{v} = \lambda \vec{v}
\quad \rightarrow \quad 
A\vec{v} - \lambda I \vec{v} = 0
\quad \rightarrow \quad 
(A - \lambda I)\vec{v} = 0
$$

We want a **nonzero** solution for $\vec{v}$.

A homogeneous system has nonzero solutions only if:

$$
\det(A - \lambda I) = 0
$$

This is the <b>characteristic equation</b>. Solving it gives eigenvalues $\lambda$.

If the determinant is not zero, it means that the vetor is just a zero vetor. But the determinant is zero, there're various case for solutions.

### 3. Diagonal Matrix 

$$
D =
\begin{bmatrix}
-5 & 0 & 0 \\
0 & -2 & 0 \\
0 & 0 & 4
\end{bmatrix}
$$

- The eigenvalues are exactly the diagonal entries.
- No computation needed.

The eigenvalues are

$$
\lambda_1 = -5, \quad
\lambda_2 = -2, \quad
\lambda_3 = 4
$$

### 4. Geometric Interpret

![Eigen](/assets/img/develop/Eigen.png)

After transformation, the eigenvector is not rotate, shear and so on. just changed thing is only scale about squishing or stretching. That line is the eigenspace

### 5. Eigen Decomposition of Matrix


$
Av_1 = \lambda_1 v_1,\; Av_2 = \lambda_2 v_2,\; ... \;Av_n = \lambda_n v_n
$


$
\rightarrow
A
\begin{bmatrix}
v_1 & v_2 & \cdots & v_n
\end{bmatrix} =
\begin{bmatrix}
v_1 & v_2 & \cdots & v_n
\end{bmatrix}
\begin{bmatrix}
\lambda_1 & 0 & \cdots & 0 \\
0 & \lambda_2 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & \lambda_n
\end{bmatrix}
$
<br></br>
$
\rightarrow
AP = PD
$


If a matrix has enough independent eigenvectors:

$$
A = PDP^{-1}
$$

Where:

- $D$ = diagonal matrix of eigenvalues
- Columns of $P$ = eigenvectors

Then:

$$
A^n = P D^n P^{-1}
\quad \rightarrow \quad
D^n =
\begin{bmatrix}
\lambda_1^n & 0 \\
0 & \lambda_2^n
\end{bmatrix}
$$

This makes computing powers extremely easy.

<b>Ex.</b>

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
\quad \rightarrow \quad
P =
\begin{bmatrix}
2 & 2 \\
1+\sqrt{5} & 1-\sqrt{5}
\end{bmatrix}
$$

Then:

$$
A = P D P^{-1}
\quad \rightarrow \quad
A^n = P D^n P^{-1}
$$







