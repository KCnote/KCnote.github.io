---
layout: post
title: "PART 3 --- Matrix Decompositions"
date: 2026-02-18 00:00:00 +0900
author: kang
categories: [Linear Algebra, Temporary]
tags: [Linear Algebra]
pin: false
math: true
mermaid: true
---
# 📙 PART 3 --- Matrix Decompositions

This chapter covers:

-   Gaussian Elimination
-   Gauss--Jordan Elimination (Inverse computation)
-   LU Decomposition (Full derivation)
-   Singular Value Decomposition (SVD) --- Full numerical example
-   Rank‑k approximation concept

All steps shown explicitly with numerical computation.

------------------------------------------------------------------------

# 1️⃣ Gaussian Elimination (Solving Linear System)

Solve:

$$
\begin{cases}
2x + y = 5 \\
4x + 3y = 11
\end{cases}
$$

Augmented matrix:

$$
\begin{bmatrix}
2 & 1 & | & 5 \\
4 & 3 & | & 11
\end{bmatrix}
$$

### Step 1: Eliminate first column

Multiplier:

$$
m = \frac{4}{2} = 2
$$

Row2 → Row2 − 2·Row1:

$$
\begin{bmatrix}
2 & 1 & | & 5 \\
0 & 1 & | & 1
\end{bmatrix}
$$

### Step 2: Back Substitution

$$
y = 1
$$

$$
2x + 1 = 5
$$

$$
2x = 4
$$

$$
x = 2
$$

Solution:

$$
(x,y) = (2,1)
$$

------------------------------------------------------------------------

# 2️⃣ Gauss--Jordan Elimination (Matrix Inverse)

Let

$$
A =
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
$$

Form augmented matrix with identity:

$$
\begin{bmatrix}
1 & 2 & | & 1 & 0 \\
3 & 4 & | & 0 & 1
\end{bmatrix}
$$

### Step 1: Eliminate below pivot

Row2 → Row2 − 3·Row1

$$
\begin{bmatrix}
1 & 2 & | & 1 & 0 \\
0 & -2 & | & -3 & 1
\end{bmatrix}
$$

### Step 2: Normalize Row2

Row2 ÷ (-2):

$$
\begin{bmatrix}
1 & 2 & | & 1 & 0 \\
0 & 1 & | & 3/2 & -1/2
\end{bmatrix}
$$

### Step 3: Eliminate above pivot

Row1 → Row1 − 2·Row2

$$
\begin{bmatrix}
1 & 0 & | & -2 & 1 \\
0 & 1 & | & 3/2 & -1/2
\end{bmatrix}
$$

Thus

$$
A^{-1} =
\begin{bmatrix}
-2 & 1 \\
3/2 & -1/2
\end{bmatrix}
$$

------------------------------------------------------------------------

# 3️⃣ LU Decomposition (Full Derivation)

Let

$$
A =
\begin{bmatrix}
2 & 1 \\
4 & 3
\end{bmatrix}
$$

We perform elimination.

Multiplier:

$$
m_{21} = \frac{4}{2} = 2
$$

Row2 → Row2 − 2·Row1

Upper matrix:

$$
U =
\begin{bmatrix}
2 & 1 \\
0 & 1
\end{bmatrix}
$$

Lower matrix collects multipliers:

$$
L =
\begin{bmatrix}
1 & 0 \\
2 & 1
\end{bmatrix}
$$

Check:

$$
LU =
\begin{bmatrix}
1 & 0 \\
2 & 1
\end{bmatrix}
\begin{bmatrix}
2 & 1 \\
0 & 1
\end{bmatrix}
=
\begin{bmatrix}
2 & 1 \\
4 & 3
\end{bmatrix}
= A
$$

------------------------------------------------------------------------

# 4️⃣ Singular Value Decomposition (Full 2×2 Example)

Let

$$
A =
\begin{bmatrix}
3 & 0 \\
4 & 0
\end{bmatrix}
$$

## Step 1: Compute $A^T A$

$$
A^T =
\begin{bmatrix}
3 & 4 \\
0 & 0
\end{bmatrix}
$$

$$
A^T A =
\begin{bmatrix}
3 & 4 \\
0 & 0
\end{bmatrix}
\begin{bmatrix}
3 & 0 \\
4 & 0
\end{bmatrix}
=
\begin{bmatrix}
25 & 0 \\
0 & 0
\end{bmatrix}
$$

## Step 2: Eigenvalues of $A^T A$

$$
\lambda_1 = 25, \quad \lambda_2 = 0
$$

Singular values:

$$
\sigma_1 = 5, \quad \sigma_2 = 0
$$

## Step 3: Compute V

Eigenvector for λ=25:

$$
v_1 =
\begin{bmatrix}
1 \\
0
\end{bmatrix}
$$

## Step 4: Compute U

$$
u_1 = \frac{1}{\sigma_1} A v_1
$$

$$
= \frac{1}{5}
\begin{bmatrix}
3 \\
4
\end{bmatrix}
=
\begin{bmatrix}
3/5 \\
4/5
\end{bmatrix}
$$

Thus

$$
U =
\begin{bmatrix}
3/5 & -4/5 \\
4/5 & 3/5
\end{bmatrix}
$$

$$
\Sigma =
\begin{bmatrix}
5 & 0 \\
0 & 0
\end{bmatrix}
$$

$$
V =
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
$$

So

$$
A = U \Sigma V^T
$$

------------------------------------------------------------------------

# 5️⃣ Rank‑1 Approximation Insight

If we keep only largest singular value:

$$
A \approx \sigma_1 u_1 v_1^T
$$

$$
= 5
\begin{bmatrix}
3/5 \\
4/5
\end{bmatrix}
[1\;0]
$$

$$
=
\begin{bmatrix}
3 & 0 \\
4 & 0
\end{bmatrix}
$$

Low-rank structure preserved.

------------------------------------------------------------------------

# 🔥 Key Takeaways

-   Gaussian elimination → triangular system
-   Gauss--Jordan → inverse computation
-   LU stores elimination multipliers
-   SVD uses eigenvalues of $A^TA$
-   Singular values = square roots of eigenvalues
-   Rank‑k approximation keeps largest singular values
