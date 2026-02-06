---
layout: post
title: "Convolution for Sharping and Edge Detection"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Image Processing, Convolution]
tags: [Computer Vision, Image Processing, Convolution, Sharpening, Edge Detection, HighPass, Laplacian, Sobel]
pin: false
math: true
mermaid: true

---

> 🧠 **Common confusion**  
> High-pass, Laplacian, Sobel, Prewitt, Roberts…  
> Are these **sharpening filters** or **edge detectors**?
>
> The short answer: **they are the same operations used for different purposes**.

This post clarifies the **conceptual boundary** between sharpening and edge extraction,
using kernels, equations, and interpretation — not just names.

---

## 🔍 Start with the Key Distinction

The difference is **not the kernel**.  
The difference is **how the response is used**.

- **Edge detection**  
  → output *is* the edges

- **Sharpening**  
  → edge-like response is **added back to the original image**

Same math. Different intent.

---

## 🧱 High-Pass Filtering (The Root Concept)

### Idea

- Low frequencies → smooth structure  
- High frequencies → edges, transitions, fine detail  

A **high-pass filter** suppresses low-frequency content and keeps rapid changes.

---

### Simple High-Pass Kernel (3×3)

```text
-1  -1  -1
-1   8  -1
-1  -1  -1
```

or

```text
 0  -1   0
-1   4  -1
 0  -1   0
```

---

### Interpretation

- If you **use this output directly** → edge / detail map  
- If you **add it to the original image** → sharpening

---

## ✨ Image Sharpening (Unsharp View)

Sharpening is often expressed as:

$$
I_{sharp} = I + \alpha \cdot (I * H)
$$

Where:
- $I$ = original image  
- $H$ = high-pass filter  
- $\alpha$ = sharpening strength  

This is why sharpening feels like “boosting edges”.

---

## 🧮 Laplacian Filter

### Kernel

```text
 0  -1   0
-1   4  -1
 0  -1   0
```

or

```text
-1  -1  -1
-1   8  -1
-1  -1  -1
```

### Meaning

- Second-order derivative  
- Responds to **rapid intensity changes in all directions**  
- Very sensitive to noise

📌 Laplacian by itself → edge map  
📌 Laplacian added to image → sharpening

---

## ➡️ First-Order Gradient Filters

These approximate **first derivatives**.

They respond to *directional change*.

---

## 🧭 Roberts Operator

### Kernels

```text
 1   0
 0  -1
```

```text
 0   1
-1   0
```

### Characteristics
- Very small support  
- Extremely sensitive to noise  
- Historically important

Mostly used for **edge detection**, rarely for sharpening.

---

## 🧭 Prewitt Operator

### Kernels (Horizontal / Vertical)

```text
-1  0  1
-1  0  1
-1  0  1
```

```text
 1   1   1
 0   0   0
-1  -1  -1
```

### Characteristics
- Simple gradient estimate  
- Uniform weighting  
- Moderate noise sensitivity  

---

## 🧭 Sobel Operator

### Kernels (Horizontal / Vertical)

```text
-1  0  1
-2  0  2
-1  0  1
```

```text
 1   2   1
 0   0   0
-1  -2  -1
```

### Why Sobel Is Popular

- Includes smoothing in one direction  
- More robust to noise than Prewitt  
- Good balance between edge clarity and stability

---

## 🧠 Gradient Magnitude

From Sobel / Prewitt:

$$
|\nabla I| = \sqrt{G_x^2 + G_y^2}
$$

This is a **pure edge representation**.

Used directly → edge extraction  
Added back → sharpening

---

## 🎯 So… Sharpening or Edge Detection?

| Filter | Edge Detection | Sharpening |
|------|---------------|------------|
| High-pass | ✅ | ✅ |
| Laplacian | ✅ | ✅ |
| Roberts | ✅ | ❌ |
| Prewitt | ✅ | ⚠️ |
| Sobel | ✅ | ⚠️ |

⚠️ = possible, but uncommon

---

## 💡 A Vision-Centric Takeaway

> **Edges are information. Sharpening is emphasis.**

- Edge detection isolates structure  
- Sharpening reinjects structure into appearance  

The math does not change.  
The **interpretation does**.

---

## ✨ Summary

- Sharpening and edge detection share the same operators  
- Difference lies in *usage*, not *definition*  
- High-pass & Laplacian are most common for sharpening  
- Gradient operators are primarily for edge extraction  
- Understanding intent prevents conceptual confusion  

---

🌈 *In image processing, the same math can answer very different questions.*
