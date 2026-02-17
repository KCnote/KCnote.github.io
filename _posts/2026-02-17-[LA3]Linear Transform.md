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
# 🔄 Linear Transformation

---

# 🧭 1️⃣ What is a Transformation?

The word **transformation** suggests:

> 🎥 Movement  
> 📦 Deformation  
> 📐 Changing space  

A transformation moves vectors to new positions.

---

# 📏 2️⃣ What Makes It "Linear"?

A **linear transformation** must satisfy two rules:

### (1) Additivity

$$
T(\vec{u} + \vec{v}) = T(\vec{u}) + T(\vec{v})
$$

### (2) Scaling

$$
T(c\vec{v}) = cT(\vec{v})
$$

These two rules imply:

$$
T(a\vec{v} + b\vec{w})
=
aT(\vec{v}) + bT(\vec{w})
$$

So:

> Linear transformations preserve linear combinations.

---

# 📐 3️⃣ Geometric Properties of Linear Transformations

When a transformation is linear:

### ✅ Grid lines remain parallel  
### ✅ Evenly spaced lines remain evenly spaced  
### ✅ Lines remain lines  
### ✅ The origin stays fixed  

Why must origin stay fixed?

Let $\vec{0}$ be zero vector.

$$
T(\vec{0})
=
T(0\vec{v})
=
0T(\vec{v})
=
\vec{0}
$$

So origin cannot move.

---

# 🧱 4️⃣ 2×2 Matrix as Linear Transformation

Any linear transformation in 2D can be written as:

$$
A =
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$

---

## 🎯 Key Insight

The columns of the matrix tell us:

- Left column → where $\hat{i}$ lands
- Right column → where $\hat{j}$ lands

Because:

$$
A\hat{i}
=
A
\begin{bmatrix}
1 \\
0
\end{bmatrix}
=
\begin{bmatrix}
a \\
c
\end{bmatrix}
$$

$$
A\hat{j}
=
A
\begin{bmatrix}
0 \\
1
\end{bmatrix}
=
\begin{bmatrix}
b \\
d
\end{bmatrix}
$$

So:

$$
A =
\begin{bmatrix}
\; A\hat{i} \;|\; A\hat{j} \;
\end{bmatrix}
$$

Matrix = transformed basis vectors.

---

# 🚀 5️⃣ Matrix-Vector Multiplication Meaning

Take any vector:

$$
\vec{x} =
\begin{bmatrix}
x \\
y
\end{bmatrix}
$$

We can rewrite it as:

$$
\vec{x} = x\hat{i} + y\hat{j}
$$

Now apply transformation:

$$
A\vec{x}
=
A(x\hat{i} + y\hat{j})
$$

Using linearity:

$$
=
xA\hat{i} + yA\hat{j}
$$

Substitute:

$$
=
x
\begin{bmatrix}
a \\
c
\end{bmatrix}
+
y
\begin{bmatrix}
b \\
d
\end{bmatrix}
$$

Which equals:

$$
=
\begin{bmatrix}
ax + by \\
cx + dy
\end{bmatrix}
$$

---

## 🎯 Interpretation

Matrix multiplication means:

> Scale the transformed $\hat{i}$ by x  
> Scale the transformed $\hat{j}$ by y  
> Add them  

That’s all matrix multiplication is.

---

# 📉 6️⃣ Linear Dependent Columns → Dimension Collapse

Suppose:

$$
\text{column}_2 = c \cdot \text{column}_1
$$

Then:

$$
A =
\begin{bmatrix}
\vec{v} & c\vec{v}
\end{bmatrix}
$$

Now compute:

$$
A
\begin{bmatrix}
x \\
y
\end{bmatrix}
=
x\vec{v} + y(c\vec{v})
=
(x + cy)\vec{v}
$$

Everything lies along one direction.

---

## 🎯 Geometric Meaning

Instead of mapping to a 2D plane,

All outputs lie on:

> 📏 A single line

The transformation:

### 💥 Compresses dimension  
2D → 1D

This is called:

### Rank deficiency

---

# 📊 7️⃣ Determinant Connection

If columns are dependent:

$$
\det(A) = 0
$$

Why?

Because area collapses to zero.

A square becomes a line.

Area → 0

So determinant measures:

> Area scaling factor.

---

# 🧠 8️⃣ Big Intuition Summary

| Concept | Meaning |
|----------|----------|
| Linear transformation | Moves space but preserves structure |
| Columns | Where basis vectors land |
| Matrix-vector multiply | Combine transformed basis |
| Dependent columns | Space compressed |
| Determinant zero | Dimension collapse |

---

# 🔥 Final Mental Model

A matrix does NOT "multiply numbers."

It:

1. Moves basis vectors.
2. Rebuilds every vector from moved basis.
3. Possibly stretches, rotates, shears, or collapses space.

If columns are independent:

→ Space preserved (possibly distorted)

If columns are dependent:

→ Space compressed into lower dimension





# 🔄 Linear Transformation — Full Geometric Interpretation (With Example)

---

# 🧭 1️⃣ Given Matrix Transformation

From the image, the matrix is:

$$
A =
\begin{bmatrix}
3 & 1 \\
1 & 2
\end{bmatrix}
$$

And the vector being transformed is:

$$
\vec{x} =
\begin{bmatrix}
-1 \\
2
\end{bmatrix}
$$

---

# 🧱 2️⃣ Columns = Where Basis Vectors Land

Remember:

- First column = where $\hat{i}$ lands
- Second column = where $\hat{j}$ lands

Compute:

$$
A\hat{i}
=
A
\begin{bmatrix}
1 \\
0
\end{bmatrix}
=
\begin{bmatrix}
3 \\
1
\end{bmatrix}
$$

$$
A\hat{j}
=
A
\begin{bmatrix}
0 \\
1
\end{bmatrix}
=
\begin{bmatrix}
1 \\
2
\end{bmatrix}
$$

So:

$$
A =
\begin{bmatrix}
\; A\hat{i} \;|\; A\hat{j} \;
\end{bmatrix}
=
\begin{bmatrix}
3 & 1 \\
1 & 2
\end{bmatrix}
$$

---

# 🎯 3️⃣ Rewriting the Input Vector

Any vector can be written as:

$$
\vec{x} = x\hat{i} + y\hat{j}
$$

Here:

$$
\vec{x}
=
\begin{bmatrix}
-1 \\
2
\end{bmatrix}
=
-1\hat{i} + 2\hat{j}
$$

---

# 🚀 4️⃣ Apply the Transformation

Using linearity:

$$
A\vec{x}
=
A(-1\hat{i} + 2\hat{j})
$$

$$
=
-1A\hat{i} + 2A\hat{j}
$$

Substitute the transformed basis:

$$
=
-1
\begin{bmatrix}
3 \\
1
\end{bmatrix}
+
2
\begin{bmatrix}
1 \\
2
\end{bmatrix}
$$

Now compute each term.

---

### Step 1: Scale First Column

$$
-1
\begin{bmatrix}
3 \\
1
\end{bmatrix}
=
\begin{bmatrix}
-3 \\
-1
\end{bmatrix}
$$

---

### Step 2: Scale Second Column

$$
2
\begin{bmatrix}
1 \\
2
\end{bmatrix}
=
\begin{bmatrix}
2 \\
4
\end{bmatrix}
$$

---

### Step 3: Add

$$
\begin{bmatrix}
-3 \\
-1
\end{bmatrix}
+
\begin{bmatrix}
2 \\
4
\end{bmatrix}
=
\begin{bmatrix}
-1 \\
3
\end{bmatrix}
$$

---

# ✅ Final Result

$$
A
\begin{bmatrix}
-1 \\
2
\end{bmatrix}
=
\begin{bmatrix}
-1 \\
3
\end{bmatrix}
$$

---

# 🎨 Geometric Interpretation (Like the Image)

From the grid:

1. The entire coordinate grid is transformed.
2. Grid lines remain parallel.
3. Squares become parallelograms.
4. Origin stays fixed.

The red and green arrows show:

- The transformed $\hat{i}$ → $(3,1)$
- The transformed $\hat{j}$ → $(1,2)$

The yellow vector shows:

- The combination:
  - −1 times first column
  - +2 times second column

---

# 📐 Why This Always Works

Because:

$$
\vec{x} = x\hat{i} + y\hat{j}
$$

And linearity guarantees:

$$
A(x\hat{i} + y\hat{j})
=
xA\hat{i} + yA\hat{j}
$$

So matrix multiplication is just:

> Scaling transformed basis vectors and adding them.

---

# 📊 5️⃣ Determinant & Area Interpretation

Compute determinant:

$$
\det(A
