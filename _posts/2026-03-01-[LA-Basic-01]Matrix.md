---
layout: post
title: "Matrix"
date: 2026-03-01 00:00:00 +0900
author: kang
categories: [Linear Algebra, Linear Algebra - Foundations]
tags: [Linear Algebra, Linear Algebra - Foundations]
pin: false
math: true
mermaid: true
---

# <b>Matrix</b>
---
### <b>Prerequisites</b>
    NOTHING

---
## <b>What is Matrix</b>

### 1. What is Matrix?

$$
A =
\begin{bmatrix}
| & | &  & | \\
a_1 & a_2 & \dots & a_n \\
| & | &  & |
\end{bmatrix}
$$

Matrix is based on vector set. Each column $a_i$ is a vector in $\mathbb{R}^m$.
So we can interpret a matrix as: A collection of $n$ vectors in $\mathbb{R}^m$.

### 2. What is Vector

![Vector-brief](/assets/img/develop/Vector-brief.png)

Vector is kind of direction information. It is composed of magnitude and direction. And a starting point is called Tail, ending point is called Tip. It can move anywhere itself, not fixed some place except some case.

If you add two vectors, fix the first vector and move the tail of the second vector to the tip of the first. The result vector goes from the first tail to the second tip.

If you multiply(or scale) a vector, greater than 1 means <b>vector streches</b>, greater than 0 and less than 1 means <b>vector shrinks(or squishes)</b> and less than 0 means <b>vector flips</b>.

### 3. Relationship between coordinate systems and matrix.

I already say about that matrix is set of vectors. It means each column vector of matrix corresponds to basis vector.

<b>2x2 A Matrix have two basis</b>.

$$
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
$$

If A matrix is identiy matrix, the first column vector is same with x-axis unit vector and th second column vector is same with y-axis unit vector.

In 2D space, we define two <b>standard basis vectors</b>:

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

So: A vector is just scaled versions of $\hat{i}$ and $\hat{j}$ added together.



$$
\begin{bmatrix}
1 & 1 \\
0 & 1
\end{bmatrix}
$$

If A matrix is shear matrix, the first column vector is same with x-axis unit vector, but the second column vector is different with y-axis unit vector, same with $y=x$.

This meaning is when the matrix multiply with a original space, the space is to become other space. In other word, the matrix systems affect to coordinate systems.

### 4. What is span.

Let:

$$
\vec{v}, \vec{w} \in \mathbb{R}^2
$$

A <b>linear combination</b> of $\vec{v}$ and $\vec{w}$ is:

$$
a\vec{v} + b\vec{w}
$$

where:

$$
a,b \in \mathbb{R}
$$

The <b>span</b> of $\vec{v}$ and $\vec{w}$ is:

$$
\text{span}\{\vec{v},\vec{w}\}
=
\{a\vec{v}+b\vec{w} \mid a,b \in \mathbb{R}\}
$$

It is the set of **all possible linear combinations**.

### 5. What is Linear Dependcence/Independence.

If one vector can be written as a combination of the other, That is called linear dependence.

Aspect to space(or span), some basis vector that have dependency can make more small dimension than liner independence set.

Aspect to Information, some basis vector that have dependency can make more less information than liner independence set. This is because issue causes information dependency from others

Vectors are **linearly dependent** if there exist scalars (not all zero) such that:

$$
a\vec{v}+b\vec{w} = 0
$$

or 

$$
\vec{u} = a\vec{v} + b\vec{w}
$$

This means: One vector can be written as a combination of the other. then vectors are dependent.

Vectors are **linearly independent** if:

$$
a\vec{v}+b\vec{w}=0
$$

implies:

$$
a = 0,\quad b = 0
$$

Meaning: No vector can be formed from others. They <b>each add new direction</b>.
