---
layout: post
title: "PART 4 — PCA & Statistical Geometry"
date: 2026-02-18 00:00:00 +0900
author: kang
categories: [Linear Algebra, Temporary]
tags: [Linear Algebra]
pin: false
math: true
mermaid: true
---

# 📕 PART 4 — PCA & Statistical Geometry

---

# 1️⃣ Dataset

$$
X =
\begin{bmatrix}
1 & 2 \\
3 & 4 \\
5 & 6
\end{bmatrix}
$$

---

# 2️⃣ Mean Computation

$$
\mu_1 = \frac{1+3+5}{3} = 3
$$

$$
\mu_2 = \frac{2+4+6}{3} = 4
$$

$$
\mu =
\begin{bmatrix}
3 \\
4
\end{bmatrix}
$$

---

# 3️⃣ Center Data

$$
X_c =
\begin{bmatrix}
-2 & -2 \\
0 & 0 \\
2 & 2
\end{bmatrix}
$$

---

# 4️⃣ Covariance Matrix

Definition:

$$
\Sigma = \frac{1}{n} X_c^T X_c
$$

Transpose:

$$
X_c^T =
\begin{bmatrix}
-2 & 0 & 2 \\
-2 & 0 & 2
\end{bmatrix}
$$

Multiply:

$$
X_c^T X_c =
\begin{bmatrix}
8 & 8 \\
8 & 8
\end{bmatrix}
$$

Thus:

$$
\Sigma =
\frac{1}{3}
\begin{bmatrix}
8 & 8 \\
8 & 8
\end{bmatrix}
=
\begin{bmatrix}
8/3 & 8/3 \\
8/3 & 8/3
\end{bmatrix}
$$

---

# 5️⃣ Eigen-Decomposition

Solve:

$$
\det(\Sigma - \lambda I) = 0
$$

$$
\det
\begin{bmatrix}
8/3 - \lambda & 8/3 \\
8/3 & 8/3 - \lambda
\end{bmatrix}
= 0
$$

Compute determinant:

$$
(8/3 - \lambda)^2 - (8/3)^2 = 0
$$

Using difference of squares:

$$
\lambda_1 = 16/3, \quad \lambda_2 = 0
$$

---

# 6️⃣ Eigenvectors

For largest eigenvalue:

$$
v_1 =
\begin{bmatrix}
1 \\
1
\end{bmatrix}
$$

Normalize:

$$
u_1 =
\frac{1}{\sqrt{2}}
\begin{bmatrix}
1 \\
1
\end{bmatrix}
$$

---

# 7️⃣ Rayleigh Quotient Proof

Projected variance:

$$
Var(w) = w^T \Sigma w
$$

Constraint:

$$
w^T w = 1
$$

Lagrangian:

$$
L(w,\lambda) = w^T \Sigma w - \lambda (w^T w - 1)
$$

Derivative:

$$
2 \Sigma w - 2 \lambda w = 0
$$

Thus:

$$
\Sigma w = \lambda w
$$

Maximum variance direction = eigenvector with largest eigenvalue.

---

# 8️⃣ PCA Projection

Project:

$$
z = X_c u_1
$$

First row:

$$
(-2,-2) \cdot \frac{1}{\sqrt{2}}
\begin{bmatrix}
1 \\
1
\end{bmatrix}
= -\frac{4}{\sqrt{2}}
$$

Reduced 1D representation obtained.

---

# 🔥 Key Takeaways

- PCA = eigen-decomposition of covariance
- Largest eigenvalue = maximum variance direction
- Projection = linear transformation
- Dimensionality reduction via rank truncation
