---
layout: post
title: "Change of basis"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Intermediate]
tags: [Linear Algebra, Intermediate]
pin: false
math: true
mermaid: true
---
# 🔄 Change of Basis (Copy Ready)

---

## 1️⃣ Two Coordinate Systems

### Our standard basis

$
\mathbf{e}_1 =
\begin{bmatrix}
1 \\
0
\end{bmatrix}
\quad
\mathbf{e}_2 =
\begin{bmatrix}
0 \\
1
\end{bmatrix}
$

---

### Jennifer’s basis (written in our coordinates)

$
A =
\begin{bmatrix}
2 & -1 \\
1 & 1
\end{bmatrix}
$

Columns are her basis vectors:

$
\mathbf{b}_1 =
\begin{bmatrix}
2 \\
1
\end{bmatrix}
\quad
\mathbf{b}_2 =
\begin{bmatrix}
-1 \\
1
\end{bmatrix}
$

So matrix $A$ is literally **her grid written in our coordinates**.

---

## 2️⃣ What Does $A$ Do?

If a vector is written in Jennifer’s language:

$
\begin{bmatrix}
x_j \\
y_j
\end{bmatrix}
$

Multiplying by $A$ gives the same geometric vector in our coordinates:

$$
A
\begin{bmatrix}
x_j \\
y_j
\end{bmatrix}
=
\begin{bmatrix}
x_o \\
y_o
\end{bmatrix}
$$

$So:

- $A$ = Jennifer → Our language

---

## 3️⃣ What Does $A^{-1}$ Do?

If a vector is written in our coordinates:

$$
\begin{bmatrix}
x_o \\
y_o
\end{bmatrix}
$$

Then:

$$
A^{-1}
\begin{bmatrix}
x_o \\
y_o
\end{bmatrix}
=
\begin{bmatrix}
x_j \\
y_j
\end{bmatrix}
$$

So:

- $A^{-1}$ = Our → Jennifer language

---

## 4️⃣ What Does $A^{-1} M A$ Mean?

Suppose:

- $M$ is a transformation written in our coordinates
- We want to know how it looks in Jennifer’s coordinates

We compute:

$
A^{-1} M A
$

Step-by-step meaning:

1. Multiply by $A$ → Convert Jennifer-coordinates to ours  
2. Apply $M$ → Do transformation  
3. Multiply by $A^{-1}$ → Convert back to Jennifer-coordinates  

So:

$
A^{-1} M A
$

means:

"How does transformation $M$ look in her coordinate system?"

---

## 5️⃣ Geometric Meaning of Columns

If

$
M =
\begin{bmatrix}
| & | \\
\mathbf{v}_1 & \mathbf{v}_2 \\
| & |
\end{bmatrix}
$

Then:

- First column = where $(1,0)$ goes  
- Second column = where $(0,1)$ goes  

Matrix = transformed basis vectors.

---


## 7️⃣ Core Intuition

Change of basis does NOT move the vector.

It changes only:

- The coordinate numbers  
- The language describing the same arrow  

And:

$
A^{-1} M A
$

is a coordinate-language sandwich:

New view = $A^{-1}$ (Old view) $A$
