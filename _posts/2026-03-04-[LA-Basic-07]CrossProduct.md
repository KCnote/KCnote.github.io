---
layout: post
title: "Cross Product"
date: 2026-03-04 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Cross Product</b>
---
### <b>Prerequisites</b>
    Determinant
    Dot Product

---
## <b>What is Cross Product</b>

### 1. What is Cross Product?

For two vectors in $\mathbb{R}^3$:

$$
v = \begin{bmatrix}
v_1 \\
v_2 \\
v_3 \end{bmatrix},\qquad
w = \begin{bmatrix}
w_1 \\
w_2 \\
w_3 \end{bmatrix}
$$

The cross product is:

$$
v \times w
=\begin{vmatrix}
\hat{i} & \hat{j} & \hat{k} \\
v_1 & v_2 & v_3 \\
w_1 & w_2 & w_3
\end{vmatrix}
$$

Expanding:

$$
\hat{i}(v_2 w_3 - v_3 w_2) - \hat{j}(v_1 w_3 - v_3 w_1) + \hat{k}(v_1 w_2 - v_2 w_1)
$$

Each component is a $2\times2$ determinant.

### 2. Magnitude and Orientation

> <b>Magnitude</b>

The magnitude of the cross product is:

$$
\|v \times w\| =
\|v\| \|w\| \sin\theta
$$

This equals the **area of the parallelogram** formed by $v$ and $w$.

So:

- More perpendicular → $\sin\theta$ large → area large
- Similar direction → $\sin\theta$ small → area small
- Same direction → area = 0

$
\text{Area of parallelogram} =
\text{base} \times \text{height} =
\|\vec{v}\|(\|\vec{w}\|\sin\theta)
,\quad \|\vec{v} \times \vec{w}\| =
\text{Area}
$

> <b>Orientation</b>

In 2D, we can think of a signed area:

$$
\det
\begin{bmatrix}
v_1 & w_1 \\
v_2 & w_2
\end{bmatrix}
= v_1 w_2 - v_2 w_1
$$

This equals the z-component of $v \times w$.

Interpretation:

- If $v$ is counterclockwise (left) of $w$ → positive
- If $v$ is clockwise (right) of $w$ → negative

So orientation determines sign.

### 3. Basis vector Example

$$
\hat{i} \times \hat{j} = \hat{k}
$$

<b>Right-hand rule:</b>

- Index fingers along $\hat{i}$
- Middle toward $\hat{j}$
- Thumb points in $+\hat{k}$ direction

So:

$$
\hat{i} \times \hat{j} = +\hat{k}
$$

But:

$$
\hat{j} \times \hat{i} = -\hat{k}
$$

Cross product is <b>not commutative</b>.

### 4. Required Properties of a Cross Product

> A binary vector operation satisfying all these properties exists naturally only in **3-dimensional space** (and a special case in 7 dimensions).

A valid cross product must satisfy the following properties:

| Property          | Meaning                                 |
| ----------------- | --------------------------------------- |
| Bilinearity       | Linear in both vector inputs            |
| Anticommutativity | Reversing order flips the sign          |
| Orthogonality     | Result is perpendicular to both vectors |
| Magnitude Rule    | Magnitude equals parallelogram area     |
| Orientation       | Direction determined by right-hand rule |


#### 4.1 Bilinearity

The operation must be **linear in each argument**.

$$
(a+b) \times c = a \times c + b \times c
$$

$$
a \times (b+c) = a \times b + a \times c
$$

$$
(\lambda a) \times b = \lambda (a \times b)
$$

$$
a \times (\lambda b) = \lambda (a \times b)
$$

#### 4.2 Anticommutativity

$$
a \times b = -(b \times a)
$$

#### 4.3 Orthogonality

The result must be perpendicular to both input vectors.

$$
(a \times b) \cdot a = 0
$$

$$
(a \times b) \cdot b = 0
$$

#### 4.4 Magnitude Condition

The magnitude of the cross product must equal the <b>area of the parallelogram</b> formed by the vectors.

$$
|a \times b| = |a||b|\sin\theta
$$

#### 4.5 Orientation (Right-Hand Rule)

The direction of the cross product follows the **right-hand rule**.

If the fingers of the right hand rotate from (a) toward (b), the thumb points in the direction of

$$
a \times b
$$

This defines the **orientation** of the resulting vector.

### 5. Determinant

$$
\text{Area scaling by matrix } A =
|\det(A)|
$$

Original area × determinant = new area.

If a matrix transforms two vectors:

$$
A v \times A w
$$

Its magnitude equals:

$$
|\det(A)| \cdot \|v \times w\|
$$

### 6. Scalar Triple Product

For three vectors (a, b, c \in \mathbb{R}^3), the **scalar triple product** is defined as

$$
(a \times b) \cdot c =
\det
\begin{pmatrix}
a_1 & a_2 & a_3 \\
b_1 & b_2 & b_3 \\
c_1 & c_2 & c_3
\end{pmatrix} =
signed\; volume(V)
$$

The computation occurs in two steps:

1. Compute the <b>cross product</b> $a \times b$
2. Take the <b>dot product</b> of the result with $c$

The result is a <b>scalar value</b>.

The scalar triple product connects three important geometric operations:

| Operation             | Meaning    |
| --------------------- | ---------- |
| Dot Product           | Projection |
| Cross Product         | Area       |
| Scalar Triple Product | Volume     |

### 7. What if the Dimension is over 3?
    NOT Cross product -> Wedge product