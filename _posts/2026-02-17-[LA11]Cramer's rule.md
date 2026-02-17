---
layout: post
title: "Cramer's rule"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Intermediate]
tags: [Linear Algebra, Intermediate]
pin: false
math: true
mermaid: true
---
# 🧩 Cramer’s Rule (What Those Images Were Actually About)

You’re right — the last set of images was basically explaining **Cramer’s Rule** using a **geometric determinant interpretation** (area/volume scaling).  
Below is the missing core content, with the “replace one column” idea and why it works.

---

# 1️⃣ Problem Setup: Solve $Ax=b$

Let $A$ be an $n\times n$ matrix (square), $x$ unknown, $b$ known:

$$
Ax = b
$$

Write $A$ by its columns:

$$
A = [a_1\; a_2\; \cdots\; a_n]
$$

and the unknown vector:

$$
x =
\begin{bmatrix}
x_1\\
x_2\\
\vdots\\
x_n
\end{bmatrix}
$$

Then matrix-vector multiplication is a **linear combination of columns**:

$$
Ax = x_1 a_1 + x_2 a_2 + \cdots + x_n a_n
$$

So the equation $Ax=b$ means:

$$
b = x_1 a_1 + x_2 a_2 + \cdots + x_n a_n
$$

✅ This is exactly the idea you said:  
**the columns $a_i$ live in the output space**, and $x_i$ are the “weights” that combine them to hit $b$.

---

# 2️⃣ Condition for Unique Solution

Cramer’s rule requires:

$$
\det(A) \ne 0
$$

That means:

- $A$ is invertible
- columns are linearly independent
- the transformation doesn’t collapse dimension
- there is a **unique** $x$

---

# 3️⃣ Key Determinant Property Used in the Proof

Determinant is **multilinear in columns** and **alternating**:

### (1) Multilinear in columns  
If you change only the $k$-th column, determinant is linear in that column:

$$
\det(a_1,\dots,\alpha u + \beta v,\dots,a_n)
=
\alpha \det(a_1,\dots,u,\dots,a_n)
+
\beta \det(a_1,\dots,v,\dots,a_n)
$$

### (2) Alternating  
If two columns are equal, determinant is zero:

$$
\det(\dots,a_i,\dots,a_i,\dots)=0
$$

These two facts are the entire engine of Cramer’s rule.

---

# 4️⃣ Cramer’s Rule Statement

Define $A_k$ as the matrix where you replace the $k$-th column of $A$ with $b$:

$$
A_k = [a_1\;\cdots\;a_{k-1}\; b\; a_{k+1}\;\cdots\; a_n]
$$

Then the solution is:

$$
\boxed{x_k = \frac{\det(A_k)}{\det(A)}} \qquad (k=1,\dots,n)
$$

---

# 5️⃣ Full Derivation (No Skips)

We know:

$$
b = x_1 a_1 + x_2 a_2 + \cdots + x_n a_n
$$

Now take determinant of the matrix formed by columns  
$a_1,\dots,a_{k-1}, b, a_{k+1},\dots,a_n$:

$$
\det(A_k) = \det(a_1,\dots,a_{k-1}, b, a_{k+1},\dots,a_n)
$$

Substitute $b$ using the linear combination:

$$
\det(A_k)
=
\det(a_1,\dots,a_{k-1}, x_1a_1 + \cdots + x_n a_n, a_{k+1},\dots,a_n)
$$

Use multilinearity in the $k$-th column:

$$
\det(A_k)
=
x_1\det(a_1,\dots,a_{k-1}, a_1, a_{k+1},\dots,a_n)
+
\cdots
+
x_n\det(a_1,\dots,a_{k-1}, a_n, a_{k+1},\dots,a_n)
$$

Now note: for every term where the inserted column is $a_i$ with $i\ne k$, the matrix contains **two identical columns** ($a_i$ already exists somewhere else), so that determinant is zero.

So all terms vanish except the one where the inserted column is $a_k$:

$$
\det(A_k)
=
x_k\det(a_1,\dots,a_{k-1}, a_k, a_{k+1},\dots,a_n)
$$

But that last determinant is just $\det(A)$:

$$
\det(A_k) = x_k \det(A)
$$

Therefore:

$$
\boxed{x_k = \frac{\det(A_k)}{\det(A)}}
$$

✅ Done. That’s Cramer’s rule, fully derived.

---

# 6️⃣ 2D Geometric Meaning (Matches Your Image “Area = det(A) y”)

In 2D, the determinant measures **signed area scaling**.

Let:

$$
A = [a_1\; a_2],\qquad b = x_1 a_1 + x_2 a_2
$$

Take determinant with $a_1$ fixed and $b$ in the second column:

$$
\det(a_1, b)
=
\det(a_1, x_1 a_1 + x_2 a_2)
$$

By linearity:

$$
\det(a_1, b)
=
x_1\det(a_1,a_1) + x_2\det(a_1,a_2)
$$

But $\det(a_1,a_1)=0$, so:

$$
\det(a_1,b) = x_2\det(a_1,a_2)
$$

Thus:

$$
\boxed{x_2 = \frac{\det(a_1,b)}{\det(a_1,a_2)}} = \frac{\det(A_2)}{\det(A)}
$$

This is exactly the “area ratio” idea:

- $\det(A)$ = area of parallelogram spanned by $(a_1,a_2)$  
- $\det(A_2)$ = area of parallelogram spanned by $(a_1,b)$  
- ratio gives the coordinate weight $x_2$

Similarly:

$$
x_1 = \frac{\det(b,a_2)}{\det(a_1,a_2)}
$$

So in 2D, Cramer’s rule literally says:

> The coefficients are ratios of signed areas.

---

# 7️⃣ 3D Meaning (Volume Ratio)

In 3D:

- $\det(A)$ = signed volume of parallelepiped spanned by $(a_1,a_2,a_3)$
- $\det(A_k)$ = same but with $a_k$ replaced by $b$

Then:

$$
x_k = \frac{\text{volume with } b \text{ in slot } k}{\text{original volume}}
$$

So:

> Cramer’s rule = “coordinate weights are volume ratios”.

---

# 8️⃣ Important Notes

✅ Works for square $A$ with $\det(A)\ne 0$ only.  
❌ Not efficient for large systems (determinants are expensive).  
✅ But it’s perfect for geometry + proofs + small $2\times2$, $3\times3$ systems.

