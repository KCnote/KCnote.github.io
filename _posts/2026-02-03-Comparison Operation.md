---
layout: post
title: "Comparison Operations in Image Processing (>, <, ==, !=)"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Image Processing]
tags: [Image Processing, Comparison, Thresholding, Binary Image]
pin: false
math: true
mermaid: true
---

## 🔍 What Are Comparison Operations?

Comparison operations compare **pixel intensities** and produce a **binary result**.

They are the foundation of:
- Thresholding
- Mask generation
- Segmentation
- Rule-based inspection systems

All operations are applied **pixel-wise**.

---

## 🔢 Basic Comparison Operators

| Operator | Meaning |
|--------|--------|
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater or equal |
| `<=` | Less or equal |
| `==` | Equal |
| `!=` | Not equal |

---

## 🔺 Greater Than ( > )

$$
I_{out}(x,y) =
\begin{cases}
255 & I(x,y) > T \\
0 & \text{otherwise}
\end{cases}
$$

**Example**
```
Pixel = 180
Threshold = 150
Result = 255
```

**Usage**
- Bright object extraction
- Simple global thresholding

**Pros**
- Extremely fast
- Deterministic

**Cons**
- Sensitive to illumination changes

---

## 🔻 Less Than ( < )

$$
I_{out}(x,y) =
\begin{cases}
255 & I(x,y) < T \\
0 & \text{otherwise}
\end{cases}
$$

**Example**
```
Pixel = 90
Threshold = 120
Result = 255
```

**Usage**
- Dark region detection
- Shadow extraction

---

## ⚖️ Greater or Equal ( >= )

$$
I_{out}(x,y) =
\begin{cases}
255 & I(x,y) \ge T \\
0 & \text{otherwise}
\end{cases}
$$

**Usage**
- Inclusive threshold
- Boundary-safe segmentation

---

## ⚖️ Less or Equal ( <= )

$$
I_{out}(x,y) =
\begin{cases}
255 & I(x,y) \le T \\
0 & \text{otherwise}
\end{cases}
$$

---

## 🟰 Equal ( == )

Exact value comparison (rare in noisy images).

$$
I_{out}(x,y) =
\begin{cases}
255 & I(x,y) = V \\
0 & \text{otherwise}
\end{cases}
$$

**Usage**
- Label images
- Encoded mask extraction

---

## 🚫 Not Equal ( != )

$$
I_{out}(x,y) =
\begin{cases}
255 & I(x,y) \ne V \\
0 & \text{otherwise}
\end{cases}
$$

**Usage**
- Background removal
- Mask refinement

---

## 🧠 Range Comparison (Between)

$$
T_{low} \le I(x,y) \le T_{high}
$$

**Example**
```
100 ≤ Pixel ≤ 160 → 255
```

**Usage**
- Band-pass segmentation
- Color / depth filtering

---

## ⚖️ Comparison vs Other Operations

| Type | Input | Output | Typical Use |
|----|------|-------|------------|
| Arithmetic | Intensity | Intensity | Brightness, blending |
| Bitwise | Binary | Binary | Mask logic |
| Comparison | Intensity | Binary | Segmentation |

---

## 🎯 Takeaway

Comparison operations are the **decision boundary** in image processing.

They are:
- Fast
- Deterministic
- Hardware-friendly

Every thresholding method is built on **comparison logic**.
