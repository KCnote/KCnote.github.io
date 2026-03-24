---
layout: post
title: "Inverse, Rank, Null space"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Intermediate]
tags: [Linear Algebra, Intermediate, Inverse, Rank, Null Space, Columns Space, Kernel]
pin: false
math: true
mermaid: true
---
# 📐 Linear Systems of Equations — Geometry of $Ax = v$

---

# 1️⃣ Writing a Linear System in Matrix Form

A system of linear equations can always be written as:

$$
Ax = v
$$

Where:

- $A$ = matrix of coefficients  
- $x$ = vector of variables  
- $v$ = vector of constants  

---

## 🔹 How to Set It Up Properly

1. Put **all variables on the left**
2. Put **all constants on the right**
3. Vertically align variables
4. Add zeros where needed

Example:

\[
\begin{aligned}
2x + 3y &= 5 \\
4x - y &= 7
\end{aligned}
\]

Becomes:

$$
\begin{bmatrix}
2 & 3 \\
4 & -1
\end{bmatrix}
\begin{bmatrix}
x \\
y
\end{bmatrix}
=
\begin{bmatrix}
5 \\
7
\end{bmatrix}
$$

---

# 2️⃣ Geometric Meaning of $Ax = v$

Think geometrically:

- $A$ → **Transformation**
- $x$ → vector in the “before” space
- $v$ → vector in the “after” space

So solving:

$$
Ax = v
$$

means:

> Which vector $x$ gets transformed by $A$ into $v$?

---

# 3️⃣ Case 1: $\det(A) \ne 0$

If:

$$
\det(A) \ne 0
$$

Then:

- Space is NOT collapsed
- Transformation is invertible
- Every output vector corresponds to exactly one input vector

So:

$$
A^{-1} \text{ exists}
$$

Multiply both sides:

$$
A^{-1}Ax = A^{-1}v
$$

Since:

$$
A^{-1}A = I
$$

We get:

$$
x = A^{-1}v
$$

### ✅ Unique solution exists.

---

## 🔹 Geometric Meaning

- No squishing
- No dimension loss
- One-to-one transformation
- Every vector in output space has a unique preimage

---

## 🔹 Example of Inverse Intuition

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

Apply both:

$$
A^{-1}A = I
$$

Identity matrix:

$$
I =
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
$$

This transformation does nothing.

---

# 4️⃣ Case 2: $\det(A) = 0$

If:

$$
\det(A) = 0
$$

Then:

- Space is squished to lower dimension
- Transformation is not invertible
- Some vectors collapse together

Meaning:

> Multiple input vectors map to the same output vector.

So:

- Some $v$ → no solution
- Some $v$ → infinitely many solutions

---

# 5️⃣ Rank — How Many Dimensions Survive?

**Rank** = number of independent directions in output space.

It equals:

- Number of pivot columns
- Dimension of column space

---

## Examples

If all transformed vectors land on a:

- Line → rank = 1
- Plane → rank = 2
- Full 3D space → rank = 3

---

# 6️⃣ Column Space

The **column space** of $A$ is:

$$
\{Av \mid v \in \mathbb{R}^n\}
$$

It is the set of all possible outputs.

So solving:

$$
Ax = v
$$

has a solution only if:

$$
v \in \text{Column Space of } A
$$

---

# 7️⃣ Full Rank

If:

$$
\text{rank}(A) = \text{number of columns}
$$

Then:

- Columns are independent
- No dimension lost
- If square → invertible

---

# 8️⃣ Null Space (Kernel)

The **null space** is:

$$
\{x \mid Ax = 0\}
$$

This means:

Which vectors get sent to the origin?

---

## If $\det(A) \ne 0$

Only solution:

$$
x = 0
$$

---

## If $\det(A) = 0$

There are infinitely many nonzero solutions:

- Entire line collapses
- Entire plane collapses

Null space dimension > 0

---

# 🔥 Big Picture Summary

For $Ax = v$:

### If $\det(A) \ne 0$

- Invertible
- Unique solution
- Full rank
- Null space = {0}

---

### If $\det(A) = 0$

- Space squished
- Not invertible
- Rank < number of columns
- Null space contains nonzero vectors
- Either no solution or infinitely many

---

# 🎯 Final Intuition

Think of $A$ as reshaping space.

Solving $Ax = v$ asks:

> Is $v$ reachable?  
> And if so, from how many inputs?

Everything depends on:

- Determinant
- Rank
- Column space
- Null space

This is the full geometric meaning of linear systems.
