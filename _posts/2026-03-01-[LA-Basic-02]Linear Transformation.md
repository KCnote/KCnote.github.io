---
layout: post
title: "Linear Transformation"
date: 2026-03-01 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Linear Transformation</b>
---
### <b>Prerequisites</b>
    Basis Vector
    Span
    Linear independent/dependent

---
## <b>What is Linear Transformation</b>

### 1. What is Transformatin?

Any linear transformation in 2D can be written as:

$$
A =
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$

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

This A matrix transform Identiy space(usual 2D coordination systems) to A space.
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

### 2. What if the matrix is linear dependent

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

Everything lies along one direction. Instead of mapping to a 2D plane, all outputs lie on a single line The transformation is called <b>Compresses dimension</b>(2D → 1D) or <b>Rank deficiency</b>

<b>Additional: Determinant Connection</b>

If columns are dependent:

$$
\det(A) = 0
$$

Because area collapses to zero. we already see the Compresses dimesion 2D → 1D, a square becomes a line. It means area is to become 0. So determinant measures: Area scaling factor.

### 3. Matrix Muliplication as Composition of Transformations

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

> Matrix multiplication represents **composition of transformations**.

### Insight of Composition

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

The columns of $ M_1 $ are:

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

The product $ M_2M_1 $ is formed by transforming each column of $ M_1 $ by $ M_2 $:

$$
M_2M_1 =
\begin{bmatrix}
M_2\text{col}_1 & M_2\text{col}_2
\end{bmatrix}
$$

In other words:

- First column of result = $ M_2 $ applied to first column of $ M_1 $
- Second column of result = $ M_2 $ applied to second column of $ M_1 $

It is also related to change of basis from previous post.

#### With Example: Rotation + Shear as Composition

Rotation by 90° (counterclockwise)

$$
R =
\begin{bmatrix}
0 & -1 \\
1 & 0
\end{bmatrix}
$$

Shear in x-direction (parameter $ k $)

$$
S =
\begin{bmatrix}
1 & k \\
0 & 1
\end{bmatrix}
$$

We want: First Rotate, Then Shear

So transformation is:

$$
T = SR
$$

Because:

$$
Tx = S(Rx)
$$

#### Compute $ SR $

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

It means a matrix is not just one transformation, sometimes composed of over 2 matrix transformations. So later we can see decomposition method. It can connect with the perspective easily.

### 4. Properties of composition

1. <b>Non-Commutativity</b>

$$
M_1M_2 \ne M_2M_1
$$

Because Rotate → Shear is <b>NOT</b> the same as Shear → Rotate

<b>The order changes the geometric result</b>. Matrix multiplication is <b>not commutative</b>.

2. Associativity

$$
(AB)C = A(BC)
$$

This is always true. 
Applying three transformations in sequence:

$$
x \xrightarrow{C} Cx \xrightarrow{B} B(Cx) \xrightarrow{A} A(B(Cx))
$$

No matter how we group them, the final transformation is:

$$
A(B(Cx))
$$

- Commutative ❌  
- Associative ✅  

