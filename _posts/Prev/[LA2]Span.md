---
layout: post
title: "Span and Linear Combination"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Basic]
tags: [Linear Algebra, Basic, Vector, Span, Linear Combination]
pin: false
math: true
mermaid: true
---
# 📐 Basis, Span, Linear Combination & Linear Independence

---

# 🧭 1️⃣ Basis Vectors of the XY Coordinate System

In 2D space, we define two **standard basis vectors**:

$$
\hat{i} =
\begin{bmatrix}
1 \\
0
\end{bmatrix}
,
\quad
\hat{j} =
\begin{bmatrix}
0 \\
1
\end{bmatrix}
$$

They are also written as:

- $\hat{i}$ → x-direction (horizontal)
- $\hat{j}$ → y-direction (vertical)

---

## 🎯 Why Are They Special?

- Each has **unit length**
- Each points along one axis
- They are **perpendicular**
- Together they describe every vector in $\mathbb{R}^2$

---

# 📦 2️⃣ Any Vector as Scaled Basis Vectors

Take any vector:

$$
\vec{v} =
\begin{bmatrix}
x \\
y
\end{bmatrix}
$$

We can rewrite it as:

$$
\vec{v} = x\hat{i} + y\hat{j}
$$

Proof:

$$
x
\begin{bmatrix}
1 \\
0
\end{bmatrix}
+
y
\begin{bmatrix}
0 \\
1
\end{bmatrix}
=
\begin{bmatrix}
x \\
0
\end{bmatrix}
+
\begin{bmatrix}
0 \\
y
\end{bmatrix}
=
\begin{bmatrix}
x \\
y
\end{bmatrix}
$$

So:

> A vector is just scaled versions of $\hat{i}$ and $\hat{j}$ added together.

---

# ➕ 3️⃣ Linear Combination

Let:

$$
\vec{v}, \vec{w} \in \mathbb{R}^2
$$

A **linear combination** of $\vec{v}$ and $\vec{w}$ is:

$$
a\vec{v} + b\vec{w}
$$

where:

$$
a,b \in \mathbb{R}
$$

are called **scalars**.

---

## 🎯 Meaning

- We stretch/squish $\vec{v}$ by $a$
- We stretch/squish $\vec{w}$ by $b$
- Then we add them

That’s it.

No powers.
No nonlinear operations.
Only scaling and adding.

---

# 🌌 4️⃣ Span of Vectors

The **span** of $\vec{v}$ and $\vec{w}$ is:

$$
\text{span}\{\vec{v},\vec{w}\}
=
\{a\vec{v}+b\vec{w} \mid a,b \in \mathbb{R}\}
$$

It is the set of **all possible linear combinations**.

---

## 🔍 What Can Span Look Like?

### Case 1: Independent Vectors

If $\vec{v}$ and $\vec{w}$ point in different directions:

👉 The span is the **entire 2D plane**.

You can reach any point.

---

### Case 2: Dependent Vectors

If:

$$
\vec{w} = c\vec{v}
$$

Then they lie on the same line.

Their span is:

👉 Just a **line**

Because:

$$
a\vec{v}+b(c\vec{v})
=
(a+bc)\vec{v}
$$

So everything collapses into one direction.

---

# 🧠 5️⃣ Linear Dependence

Vectors are **linearly dependent** if:

There exist scalars (not all zero) such that:

$$
a\vec{v}+b\vec{w} = 0
$$

This means:

One vector can be written as a combination of the other.

---

### Another View

If:

$$
\vec{u} = a\vec{v} + b\vec{w}
$$

for some scalars,

and $\vec{u}$ doesn’t add new direction,

then vectors are dependent.

---

# 🚀 6️⃣ Linear Independence

Vectors are **linearly independent** if:

$$
a\vec{v}+b\vec{w}=0
$$

implies:

$$
a = 0,\quad b = 0
$$

only trivial solution.

Meaning:

No vector can be formed from others.

They each add new direction.

---

# 🏗 7️⃣ Basis of a Vector Space

A **basis** is a set of vectors that:

### ✅ Are linearly independent  
### ✅ Span the entire space  

Formally:

A set $\{\vec{v}_1, \dots, \vec{v}_n\}$ is a basis if:

1. They are linearly independent
2. Their span equals the whole vector space

---

## 🎯 Example in $\mathbb{R}^2$

Standard basis:

$$
\left\{
\hat{i},
\hat{j}
\right\}
$$

Why?

- Independent ✅
- Span whole $\mathbb{R}^2$ ✅

---

## 🎯 Example in $\mathbb{R}^3$

Standard basis:

$$
\hat{i} =
\begin{bmatrix}
1\\0\\0
\end{bmatrix}
,
\quad
\hat{j} =
\begin{bmatrix}
0\\1\\0
\end{bmatrix}
,
\quad
\hat{k} =
\begin{bmatrix}
0\\0\\1
\end{bmatrix}
$$

---

# 📏 8️⃣ Dimension

The number of vectors in a basis equals:

$$
\text{Dimension of the space}
$$

- $\mathbb{R}^2$ → dimension 2
- $\mathbb{R}^3$ → dimension 3

---

# 🔥 Geometric Intuition

| Situation | Span |
|------------|--------|
| 1 vector | Line |
| 2 independent vectors | Plane |
| 3 independent vectors (3D) | Whole space |

---

# 🎯 Ultimate Summary

### Linear Combination

$$
a\vec{v}+b\vec{w}
$$

### Span

All possible linear combinations.

### Dependent

One vector is redundant.

### Independent

Each vector adds new direction.

### Basis

Independent + Spans whole space.

---

# 🧠 Deep Intuition

Think of basis as:

> The minimum number of directions needed to describe the entire space.

In $\mathbb{R}^2$:

You need exactly two independent directions.

In $\mathbb{R}^3$:

You need exactly three.
