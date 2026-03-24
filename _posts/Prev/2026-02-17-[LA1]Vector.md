---
layout: post
title: "Vector"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Basic]
tags: [Linear Algebra, Basic, Vector]
pin: false
math: true
mermaid: true
---

# 📌 Vector Basics — Complete Structured Notes

---

## 🧭 1. What is a Vector?

A **vector** is a mathematical object that has:

- ✅ **Magnitude (length)**
- ✅ **Direction**

Geometrically, a vector is drawn as an arrow:

- **Tail** → starting point  
- **Tip (Head)** → ending point  

Even if we move the vector anywhere in the coordinate system,  
as long as the **magnitude and direction stay the same**,  
it is the **same vector**.

---

## 📍 2. Coordinate System

### 2D Coordinate System

- Horizontal axis → **x-axis**
- Vertical axis → **y-axis**
- Unit size = **1**

A vector in 2D:

$$
\vec{v} =
\begin{bmatrix}
x \\
y
\end{bmatrix}
$$

This means:

- Move **x units parallel to x-axis**
- Move **y units parallel to y-axis**

---

### 📦 3D Coordinate System

- x-axis
- y-axis
- z-axis

These three axes are:

$$
\textbf{Mutually perpendicular}
$$

A vector in 3D:

$$
\vec{v} =
\begin{bmatrix}
x \\
y \\
z
\end{bmatrix}
$$

This means:

- Move **x units along x-axis**
- Move **y units along y-axis**
- Move **z units along z-axis**

So a vector describes **how far to move parallel from each axis**.

---

## 📏 3. Magnitude (Length of Vector)

For 2D:

$$
\vec{v} =
\begin{bmatrix}
x \\
y
\end{bmatrix}
$$

The magnitude is:

$$
\|\vec{v}\| = \sqrt{x^2 + y^2}
$$

For 3D:

$$
\|\vec{v}\| = \sqrt{x^2 + y^2 + z^2}
$$

This comes from the **Pythagorean theorem**.

---

## ➕ 4. Vector Addition

### 🔹 Geometric Meaning

To add two vectors:

1. Fix the first vector.
2. Move the **tail** of the second vector to the **tip** of the first.
3. The result vector goes from the first tail to the second tip.

This is called the:

### 📌 Tip-to-tail rule

---

### 🔹 Algebraic Meaning

If:

$$
\vec{a} =
\begin{bmatrix}
a_1 \\
a_2
\end{bmatrix}
,
\quad
\vec{b} =
\begin{bmatrix}
b_1 \\
b_2
\end{bmatrix}
$$

Then:

$$
\vec{a} + \vec{b}
=
\begin{bmatrix}
a_1 + b_1 \\
a_2 + b_2
\end{bmatrix}
$$

Each **same dimension is summed**.

For 3D:

$$
\begin{bmatrix}
a_1 \\
a_2 \\
a_3
\end{bmatrix}
+
\begin{bmatrix}
b_1 \\
b_2 \\
b_3
\end{bmatrix}
=
\begin{bmatrix}
a_1 + b_1 \\
a_2 + b_2 \\
a_3 + b_3
\end{bmatrix}
$$

---

## ✖ 5. Scalar Multiplication (Scaling)

A **scalar** is just a number.

If:

$$
c \in \mathbb{R}
$$

Then scaling a vector:

$$
c\vec{v}
$$

means:

$$
c
\begin{bmatrix}
x \\
y
\end{bmatrix}
=
\begin{bmatrix}
cx \\
cy
\end{bmatrix}
$$

---

### 🎯 Geometric Meaning of Scaling

- If $c > 1$ → vector **stretches**
- If $0 < c < 1$ → vector **shrinks (squishes)**
- If $c < 0$ → direction flips

Example:

$$
2
\begin{bmatrix}
1 \\
3
\end{bmatrix}
=
\begin{bmatrix}
2 \\
6
\end{bmatrix}
$$

Magnitude becomes twice as large.

---

## 🧠 6. Important Properties

### Commutative

$$
\vec{a} + \vec{b} = \vec{b} + \vec{a}
$$

---

### Associative

$$
(\vec{a} + \vec{b}) + \vec{c}
=
\vec{a} + (\vec{b} + \vec{c})
$$

---

### Distributive

$$
c(\vec{a} + \vec{b})
=
c\vec{a} + c\vec{b}
$$

---

## 🏗 7. What a Vector REALLY Represents

A vector is not just an arrow.

It represents:

- 📍 Displacement
- 🚀 Velocity
- 💡 Directional change
- 📐 Position in space
- 🔢 Component-wise movement along axes

---

## 🎯 Final Intuition

A vector is:

> "How far to move parallel from each axis."

In 3D:

$$
\vec{v} = (x, y, z)
$$

means:

- Move x along x-axis
- Move y along y-axis
- Move z along z-axis

All movements are **independent but combined** into one object.

---

# 🚀 Summary

| Concept | Meaning |
|----------|----------|
| Tail | Start point |
| Tip | End point |
| Addition | Component-wise sum |
| Scaling | Stretch / Shrink |
| Dimension | Number of perpendicular axes |
| Magnitude | Length of movement |

