---
layout: post
title: "Dot Product"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Intermediate]
tags: [Linear Algebra, Intermediate, Dot Product]
pin: false
math: true
mermaid: true
---
# 🔵 Dot Product, Projection, and Linear Transformations  
## Why a 2D → 1D Matrix Is a Projection

---

# 1️⃣ Matrix–Vector Product = Dot Product (Row View)

From the image:

$$
\begin{bmatrix}
u_x & u_y
\end{bmatrix}
\begin{bmatrix}
x \\
y
\end{bmatrix}
=
u_x x + u_y y
$$

This is exactly the **dot product**:

$$
u \cdot x
$$

So:

> A $1 \times 2$ matrix is the same thing as taking a dot product with a fixed vector.

That already tells us something deep:

A $1 \times 2$ matrix represents a **linear transformation from $\mathbb{R}^2$ to $\mathbb{R}$**.

---

# 2️⃣ Geometric Meaning of the Dot Product

Recall:

$$
u \cdot v = \|u\| \|v\| \cos\theta
$$

Where $\theta$ is the angle between the vectors.

So:

- Same direction → positive  
- Perpendicular → 0  
- Opposite direction → negative  

The dot product measures:

> How much one vector lies in the direction of another.

---

# 3️⃣ Why Is This a Projection?

Let $u$ be fixed.

Define transformation:

$$
T(x) = u \cdot x
$$

This maps every 2D vector onto a **number line**.

But what number?

$$
T(x) = \|u\| \|x\| \cos\theta
$$

That is exactly the **length of the shadow of $x$ onto $u$**, scaled by $\|u\|$.

If we normalize $u$:

$$
\hat{u} = \frac{u}{\|u\|}
$$

Then:

$$
\hat{u} \cdot x = \|x\| \cos\theta
$$

This equals the **signed length of the projection of $x$ onto $u$**.

So:

> A dot product is measuring projection.

---

# 4️⃣ Why Is This a Linear Transformation?

Because:

$$
T(ax + by)
=
u \cdot (ax + by)
=
a(u \cdot x) + b(u \cdot y)
$$

It satisfies:

- Additivity
- Scaling

So it is a **linear transformation**.

And any linear transformation from 2D → 1D must look like:

$$
T(x) = u \cdot x
$$

for some vector $u$.

This is a fundamental fact of linear algebra.

---

# 5️⃣ Where Do $\hat{i}$ and $\hat{j}$ Land?

If:

$$
T(x,y) = u_x x + u_y y
$$

Then:

$$
T(1,0) = u_x
$$

$$
T(0,1) = u_y
$$

So:

- $\hat{i}$ maps to $u_x$
- $\hat{j}$ maps to $u_y$

The row vector contains exactly where the basis vectors land.

That is why matrix–vector multiplication equals dot product.

---

# 6️⃣ Why Projection Creates Evenly Spaced Lines

In the image, you see parallel lines mapped to evenly spaced numbers.

That is because:

$$
T(x) = u \cdot x
$$

is linear.

If you move along direction perpendicular to $u$, dot product doesn't change.

If you move along direction parallel to $u$, dot product increases linearly.

So lines perpendicular to $u$ collapse to single values.

Lines parallel to $u$ remain evenly spaced.

That is projection behavior.

---

# 7️⃣ Duality — Symmetry of Dot Product

Important property:

$$
u \cdot v = v \cdot u
$$

Order does not matter.

This symmetry implies:

Projection of $v$ onto $u$  
and projection of $u$ onto $v$

are deeply related.

They are mirror operations in terms of angle.

Even if one vector is scaled, symmetry of angle is preserved.

This is why projection formulas feel symmetric.

---

# 8️⃣ Projection Formula (Full Vector Projection)

The projection of $w$ onto $v$ is:

$$
\text{proj}_v(w)
=
\frac{w \cdot v}{v \cdot v} v
$$

Interpretation:

1. Take dot product → get scalar amount in direction of $v$
2. Normalize by $v \cdot v$
3. Multiply back by $v$ to get vector in that direction

This is exactly “measure shadow, then rebuild it.”

---

# 9️⃣ Why Projection Is Connected to Transformations

Because:

- A $1 \times n$ matrix → dot product → projection to number line
- A rank-1 matrix → full projection onto a line in space

Example projection matrix onto unit vector $\hat{u}$:

$$
P = \hat{u}\hat{u}^T
$$

Then:

$$
Px = (\hat{u} \cdot x)\hat{u}
$$

This is pure projection onto direction $\hat{u}$.

So:

> Projection is just a specific linear transformation built from dot products.

---

# 🔥 Final Intuition

Dot product is not just a number operation.

It is:

- A measurement of alignment
- A 2D → 1D linear transformation
- The foundation of projection
- The mechanism that builds projection matrices

Projection is just:

1. Measure how much aligns (dot product)
2. Keep only that component
3. Discard the perpendicular part

That is why dot product naturally leads to projection.
