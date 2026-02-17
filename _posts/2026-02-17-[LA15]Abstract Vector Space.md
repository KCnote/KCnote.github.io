---
layout: post
title: "Abstract Vector Spaces"
date: 2026-02-17 00:00:00 +0900
author: kang
categories: [Linear Algebra, Basic]
tags: [Linear Algebra, Basic]
pin: false
math: true
mermaid: true
---
# Abstract Vector Spaces

## 1. The Big Question

This lecture is about:

> What is a vector space really?  
> Why do we define it abstractly?

It moves away from thinking of vectors as arrows in space and instead focuses on **structure**.

---

# 2. Vector Space Axioms

A vector space is defined by a set of rules (axioms).

For vectors **u, v, w** and scalars **a, b**, the following must hold:

1. **Associativity of addition**  
   u + (v + w) = (u + v) + w

2. **Commutativity of addition**  
   v + w = w + v

3. **Additive identity**  
   There exists a vector 0 such that  
   0 + v = v

4. **Additive inverse**  
   For every vector v, there exists −v such that  
   v + (−v) = 0

5. **Scalar multiplication associativity**  
   a(bv) = (ab)v

6. **Multiplicative identity**  
   1v = v

7. **Distributivity over vector addition**  
   a(v + w) = av + aw

8. **Distributivity over scalar addition**  
   (a + b)v = av + bv

These are not computational tricks.

They are the **definition** of a vector space.

---

# 3. Linearity

A transformation L is **linear** if:

Additivity:
L(v + w) = L(v) + L(w)

Scaling:
L(cv) = cL(v)

A linear transformation preserves:

- Vector addition
- Scalar multiplication

Geometrically, this means:

- Grid lines remain parallel
- Spacing scales uniformly

---

# 4. What Makes It “Abstract”?

We usually think vectors are arrows in 2D or 3D.

But the lecture’s key idea is:

> A vector is not about shape.  
> A vector is about structure.

Anything that satisfies the 8 axioms is a vector space.

Examples:

- Coordinate vectors in R² or R³
- Polynomials
- Functions
- Matrices
- Signals
- Sequences

If the operations obey the axioms, it is a vector space.

---

# 5. Why Abstraction?

“Abstractness is the price of generality.”

If we define vector spaces abstractly:

- All theorems apply everywhere
- Linear algebra works in any dimension
- The same theory applies to functions, signals, and data

Instead of proving everything separately for:

- 2D arrows
- 3D vectors
- Polynomial spaces
- Function spaces

We prove it once — for all vector spaces.

---

# 6. Core Insight

Vector spaces are not about geometry.

They are about **algebraic structure**.

Linearity is not about straight lines.

It is about preserving structure.

---

# 7. One-Sentence Summary

An abstract vector space is:

> The idea of a vector stripped of its shape,  
> leaving only the rules that govern it.
