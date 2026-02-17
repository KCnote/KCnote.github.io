---
layout: post
title: "Linear Transformation"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Basic]
tags: [Linear Algebra, Basic, Vector, Span, Linear Transformation]
pin: false
math: true
mermaid: true
---
# 🔁 Matrix Multiplication as Composition of Transformations  
*(Rotation + Shear, Read Right to Left)*

---

# 1️⃣ What Does “Composition” Mean?

A matrix represents a **linear transformation**.

If we have two matrices:

- $ M_1 $
- $ M_2 $

Applying them sequentially to a vector \( x \):

$$
x \xrightarrow{M_1} M_1x \xrightarrow{M_2} M_2(M_1x)
$$

This entire process can be written as:

$$
M_2(M_1x) = (M_2M_1)x
$$

---

## ✅ Key Rule

> Matrix multiplication represents **composition of transformations**.

And we always:

> **Read from right to left.**

---

# 2️⃣ Why Right to Left?

If we write:

$$
(M_2M_1)x
$$

This means:

1. Apply \( M_1 \) first  
2. Then apply \( M_2 \)

Because:

$$
(M_2M_1)x = M_2(M_1x)
$$

The matrix closest to the vector acts first.

---

# 3️⃣ Column Interpretation of Matrix Multiplication

Let

$$
M_2 =
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
,
\quad
M_1 =
\begin{bmatrix}
e & f \\
g & h
\end{bmatrix}
$$

The columns of \( M_1 \) are:

$$
\text{col}_1 =
\begin{bmatrix}
e \\
g
\end{bmatrix}
,
\quad
\text{col}_2 =
\begin{bmatrix}
f \\
h
\end{bmatrix}
$$

### 🔹 Important Insight

The product \( M_2M_1 \) is formed by transforming each column of \( M_1 \) by \( M_2 \):

$$
M_2M_1 =
\begin{bmatrix}
M_2\text{col}_1 & M_2\text{col}_2
\end{bmatrix}
$$

In other words:

- First column of result = \( M_2 \) applied to first column of \( M_1 \)
- Second column of result = \( M_2 \) applied to second column of \( M_1 \)

---

# 4️⃣ Explicit Formula

Carrying out multiplication:

$$
M_2M_1 =
\begin{bmatrix}
ae + bg & af + bh \\
ce + dg & cf + dh
\end{bmatrix}
$$

This comes directly from transforming each column vector.

---

# 5️⃣ Example

Let

$$
M_2 =
\begin{bmatrix}
0 & 2 \\
1 & 0
\end{bmatrix}
,
\quad
M_1 =
\begin{bmatrix}
1 & -2 \\
1 & 0
\end{bmatrix}
$$

### First column of \( M_1 \):

$$
\begin{bmatrix}
1 \\
1
\end{bmatrix}
$$

Transform it by \( M_2 \):

$$
M_2
\begin{bmatrix}
1 \\
1
\end{bmatrix}
=
\begin{bmatrix}
0\cdot1 + 2\cdot1 \\
1\cdot1 + 0\cdot1
\end{bmatrix}
=
\begin{bmatrix}
2 \\
1
\end{bmatrix}
$$

---

### Second column of \( M_1 \):

$$
\begin{bmatrix}
-2 \\
0
\end{bmatrix}
$$

Transform it:

$$
M_2
\begin{bmatrix}
-2 \\
0
\end{bmatrix}
=
\begin{bmatrix}
0 \\
-2
\end{bmatrix}
$$

---

### Final Result

$$
M_2M_1 =
\begin{bmatrix}
2 & 0 \\
1 & -2
\end{bmatrix}
$$

---

# 6️⃣ Rotation + Shear as Composition

Let’s build an example.

---

## (a) Rotation by 90° (counterclockwise)

$$
R =
\begin{bmatrix}
0 & -1 \\
1 & 0
\end{bmatrix}
$$

---

## (b) Shear in x-direction (parameter \( k \))

$$
S =
\begin{bmatrix}
1 & k \\
0 & 1
\end{bmatrix}
$$

---

## (c) First Rotate, Then Shear

We want:

1. Rotate
2. Then shear

So transformation is:

$$
T = SR
$$

Because:

$$
Tx = S(Rx)
$$

---

### Compute \( SR \)

$$
SR =
\begin{bmatrix}
1 & k \\
0 & 1
\end{bmatrix}
\begin{bmatrix}
0 & -1 \\
1 & 0
\end{bmatrix}
=
\begin{bmatrix}
k & -1 \\
1 & 0
\end{bmatrix}
$$

So the combined transformation is:

$$
T =
\begin{bmatrix}
k & -1 \\
1 & 0
\end{bmatrix}
$$

---

# 7️⃣ Non-Commutativity

Generally:

$$
M_1M_2 \ne M_2M_1
$$

Why?

Because:

- Rotate → Shear  
is not the same as  
- Shear → Rotate

The order changes the geometric result.

Matrix multiplication is **not commutative**.

---

# 8️⃣ Associativity (Always True)

However:

$$
(AB)C = A(BC)
$$

This is always true.

Reason:

Applying three transformations in sequence:

$$
x \xrightarrow{C} Cx \xrightarrow{B} B(Cx) \xrightarrow{A} A(B(Cx))
$$

No matter how we group them, the final transformation is:

$$
A(B(Cx))
$$

So matrix multiplication is:

- Not commutative ❌  
- But associative ✅  

---

# 🔥 Final Intuition

Matrix multiplication is not about numbers.

It is about:

> Applying one transformation,  
> then applying another.

And the rule is always:

### Read right to left.

$$
M_2M_1x = M_2(M_1x)
$$

That is the true meaning of matrix multiplication as composition.
