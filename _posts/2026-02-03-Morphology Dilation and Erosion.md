---
layout: post
title: "Morphology Dilation and Erosion"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Image Processing, Morphology]
tags: [Computer Vision, Image Processing, Morphology, Region, Dilation, Erosion]
pin: false
math: true
mermaid: true

---

> 🧠 **Core idea**  
> Dilation and erosion are not filters in the convolution sense.  
> They are **set-based geometric operations** defined by how a shape  
> (the structuring element) interacts with another shape (the image).

This post explains **dilation and erosion mathematically**,  
and then reconnects the math back to intuition.

---

## 🔢 Two Viewpoints in Morphology

Morphological operations can be defined in two equivalent ways:

1. **Set-theoretic (binary images)**
2. **Lattice / min–max (grayscale images)**

Both describe the same idea:  
👉 *how shapes occupy space*.

---

## 🟦 Binary Morphology (Set-Theoretic Definition)

Let:

- Set of foreground pixels (the image)
$$
A \subset \mathbb{Z}^2
$$


- Structuring element
$$
B \subset \mathbb{Z}^2
$$

- Translation
$$
B_x = \{ b + x \mid b \in B \}
$$

---

## ➕ Dilation (Binary)

### Definition

$$
A \oplus B = \{ x \mid (\hat{B})_x \cap A \neq \varnothing \}
$$

where:
- Reflected structuring element:

$$
\hat{B}
$$

Reflection of \(B\) about the origin.

---

### Interpretation

A point \(x\) belongs to the dilated set if:

> **any part** of the structuring element,  
> when placed at \(x\), overlaps the object.

So dilation answers:
> “Does the object touch the structuring element here?”

---

## ➖ Erosion (Binary)

### Definition

$$
A \ominus B = \{ x \mid B_x \subseteq A \}
$$

---

### Interpretation

A point \(x\) remains after erosion if:

> **all of** the structuring element,  
> when placed at \(x\), fits inside the object.

So erosion asks:
> “Does the object fully contain the structuring element here?”

---

## 🟨 Grayscale Morphology (Min–Max Form)

For grayscale images:

- \( f(x) \) = image intensity
- \( b(y) \) = structuring element (flat or weighted)

---

## ➕ Dilation (Grayscale)

$$
(f \oplus b)(x) = \max_{y \in B} \big[ f(x - y) + b(y) \big]
$$

If the structuring element is **flat**:
- \( b(y) = 0 \)

then:

$$
(f \oplus B)(x) = \max_{y \in B} f(x - y)
$$

---

### Meaning

Dilation selects the **maximum value** in the neighborhood defined by \(B\).

This:
- expands bright regions
- fills small dark gaps
- thickens structures

---

## ➖ Erosion (Grayscale)

$$
(f \ominus b)(x) = \min_{y \in B} \big[ f(x + y) - b(y) \big]
$$

For a flat structuring element:

$$
(f \ominus B)(x) = \min_{y \in B} f(x + y)
$$

---

### Meaning

Erosion selects the **minimum value** in the neighborhood.

This:
- shrinks bright regions
- removes small bright noise
- thins structures

---

## 🧠 Duality Between Dilation and Erosion

Dilation and erosion are **duals**:

$$
(A \oplus B)^c = A^c \ominus \hat{B}
$$

This means:
- one can be expressed in terms of the other
- they form the fundamental building blocks of morphology

---

## 🔍 Why This Is Not Convolution

| Convolution | Morphology |
|------------|------------|
| Weighted sum | Max / Min |
| Linear | Nonlinear |
| Signal-based | Shape-based |
| Frequency interpretation | Spatial occupancy interpretation |

Morphology replaces **averaging** with **extreme selection**.

---

## 💡 Computer Vision Takeaway

> **Dilation asks “does anything fit?”**  
> **Erosion asks “does everything fit?”**

That single distinction explains:
- shape growth vs shrinkage
- noise behavior
- why opening and closing work

---

## ✨ Summary

- 🔹 Dilation and erosion are defined by set inclusion
- 🔹 Binary morphology uses overlap and containment
- 🔹 Grayscale morphology uses max–min operations
- 🔹 They are nonlinear but local
- 🔹 All morphological operators are built from these two

---

🧱 *Once you understand dilation and erosion, morphology becomes intuitive.*
