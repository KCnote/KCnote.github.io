---
layout: post
title: "PART 2 --- Eigen Theory"
date: 2026-02-18 00:00:00 +0900
author: kang
categories: [Linear Algebra, Temporary]
tags: [Linear Algebra]
pin: false
math: true
mermaid: true
---
# 📗 PART 2 --- Eigen Theory

------------------------------------------------------------------------

# 1️⃣ Eigenvalue & Eigenvector --- Complete 2×2 Derivation

Let

$$
A =
\begin{bmatrix}
4 & 1 \\
2 & 3
\end{bmatrix}
$$

We solve

$$
Av = \lambda v
$$

which implies

$$
(A - \lambda I)v = 0
$$

Non-trivial solution exists only when

$$
\det(A - \lambda I) = 0
$$

------------------------------------------------------------------------

## Step 1: Compute Characteristic Polynomial

$$
A - \lambda I =
\begin{bmatrix}
4-\lambda & 1 \\
2 & 3-\lambda
\end{bmatrix}
$$

Determinant:

$$
\det(A-\lambda I)
= (4-\lambda)(3-\lambda) - (2)(1)
$$

Expand:

$$
= 12 -4\lambda -3\lambda + \lambda^2 -2
$$

$$
= \lambda^2 -7\lambda +10
$$

------------------------------------------------------------------------

## Step 2: Solve Polynomial

$$
\lambda^2 -7\lambda +10 = 0
$$

Factor:

$$
(\lambda-5)(\lambda-2)=0
$$

Thus

$$
\lambda_1 = 5, \quad \lambda_2 = 2
$$

------------------------------------------------------------------------

# 2️⃣ Compute Eigenvectors

## For λ = 5

$$
A - 5I =
\begin{bmatrix}
-1 & 1 \\
2 & -2
\end{bmatrix}
$$

Solve

$$
- v_1 + v_2 = 0
$$

Thus

$$
v_1 = v_2
$$

Eigenvector:

$$
v^{(1)} =
\begin{bmatrix}
1 \\
1
\end{bmatrix}
$$

------------------------------------------------------------------------

## For λ = 2

$$
A - 2I =
\begin{bmatrix}
2 & 1 \\
2 & 1
\end{bmatrix}
$$

Solve

$$
2v_1 + v_2 = 0
$$

$$
v_2 = -2v_1
$$

Eigenvector:

$$
v^{(2)} =
\begin{bmatrix}
1 \\
-2
\end{bmatrix}
$$

------------------------------------------------------------------------

# 3️⃣ Eigenvalue Decomposition (EVD)

Construct matrix of eigenvectors:

$$
P =
\begin{bmatrix}
1 & 1 \\
1 & -2
\end{bmatrix}
$$

Diagonal matrix:

$$
D =
\begin{bmatrix}
5 & 0 \\
0 & 2
\end{bmatrix}
$$

Compute inverse of P.

Determinant:

$$
\det(P) = (1)(-2) - (1)(1) = -2 -1 = -3
$$

Inverse:

$$
P^{-1} =
\frac{1}{-3}
\begin{bmatrix}
-2 & -1 \\
-1 & 1
\end{bmatrix}
$$

Thus

$$
A = P D P^{-1}
$$

------------------------------------------------------------------------

# 4️⃣ Symmetric Matrix Orthogonality Proof (Numerical)

Let

$$
S =
\begin{bmatrix}
2 & 1 \\
1 & 2
\end{bmatrix}
$$

Characteristic polynomial:

$$
\det(S-\lambda I)
= (2-\lambda)^2 -1
$$

$$
= \lambda^2 -4\lambda +3
$$

Solve:

$$
(\lambda-3)(\lambda-1)=0
$$

Eigenvalues:

$$
3, 1
$$

Eigenvectors:

For λ=3:

$$
\begin{bmatrix}
-1 & 1 \\
1 & -1
\end{bmatrix}
v=0
$$

Solution:

$$
v^{(1)} =
\begin{bmatrix}
1 \\
1
\end{bmatrix}
$$

For λ=1:

$$
\begin{bmatrix}
1 & 1 \\
1 & 1
\end{bmatrix}
v=0
$$

Solution:

$$
v^{(2)} =
\begin{bmatrix}
1 \\
-1
\end{bmatrix}
$$

Check orthogonality:

$$
(1,1)\cdot(1,-1)=1-1=0
$$

Thus symmetric matrix eigenvectors are orthogonal.

------------------------------------------------------------------------

# 5️⃣ Rotation Matrix Eigenvalues

Let

$$
R =
\begin{bmatrix}
\cos\theta & -\sin\theta \\
\sin\theta & \cos\theta
\end{bmatrix}
$$

Characteristic equation:

$$
\det(R-\lambda I)=0
$$

$$
(\cos\theta-\lambda)^2 + \sin^2\theta =0
$$

Using identity

$$
\cos^2\theta+\sin^2\theta=1
$$

Solve:

$$
\lambda^2 -2\lambda\cos\theta +1 =0
$$

Solutions:

$$
\lambda = \cos\theta \pm i\sin\theta
$$

$$
= e^{\pm i\theta}
$$

Thus rotation has complex conjugate eigenvalues.

------------------------------------------------------------------------

# 🔥 Key Takeaways

-   Eigenvalues come from determinant condition
-   Eigenvectors solve homogeneous linear system
-   Symmetric matrices → orthogonal eigenvectors
-   Rotation matrices → complex eigenvalues
-   EVD expresses matrix as basis change
