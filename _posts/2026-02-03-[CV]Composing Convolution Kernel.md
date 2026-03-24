---
layout: post
title: "Composing Convolution Kernels"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Image Processing]
tags: [Computer Vision, Image Processing, Convolution, Smoothing, Composing, Kernel, Sharpening, Edge Detection]
pin: false
math: true
mermaid: true

---

> ✨ **Key insight**  
> Applying two convolutions in sequence is equivalent to  
> **a single convolution with a composed kernel**.
>
> This is not an implementation trick — it is a **fundamental property of convolution**.

This post explains **kernel composition**,  
why a *3×3 Gaussian followed by a 3×3 gradient behaves like a 5×5 kernel*,  
and how to think about this from a vision perspective.

---

## 👀 The Observation

In practice, we often do things like:

1. Smooth the image (e.g. Gaussian 3×3)
2. Compute gradients (e.g. Sobel / Prewitt 3×3)

Yet the result behaves as if:
- a **larger support** filter was applied
- structure is smoother but still directional

This is not accidental.

---

## 🧮 The Core Property of Convolution

Convolution is **associative**.

Given:
- image \(I\)
- kernel \(K_1\)
- kernel \(K_2\)

Then:

$$
(I * K_1) * K_2 = I * (K_1 * K_2)
$$

📌 This means:
> Applying kernels sequentially is equivalent to **convolving the kernels themselves**.

---

## 🧱 Why Kernel Size Grows

When two kernels are convolved:

- m x n kernel  
- with p x q kernel  

The resulting kernel size is:

$$
(m + p - 1) \times (n + q - 1)
$$

---

### Example: 3×3 + 3×3

$$
(3 + 3 - 1) \times (3 + 3 - 1) = 5 \times 5
$$

So:
- Gaussian 3×3  
- followed by Gradient 3×3  

⇢ behaves like a **single 5×5 kernel**.

---

## 🧠 What Composition Means Visually

Kernel composition combines **intent**:

- Gaussian → smooth, suppress noise  
- Gradient → detect directional change  

The composed kernel asks:
> “Is there a directional change **after** smoothing?”

This is why:
- Sobel feels more stable than raw gradient
- noise is reduced before differentiation

---

## 🔍 Concrete Example (Conceptual)

### Step 1) Define kernels

**Gaussian (3×3)** (integer form)

```text
1  2  1
2  4  2
1  2  1
```

(If you want the “true” Gaussian blur kernel, divide by 16 to normalize.)

**Simple x-gradient (3×3)** (Prewitt-style)

```text
-1  0  1
-1  0  1
-1  0  1
```

Pipeline meaning:
- Gaussian reduces noise
- Gradient measures directional change

---

### Step 2) Compose kernels (kernel convolution)

The composed kernel is:

$$
K_{comp} = G * D
$$

where the 2D discrete convolution is:

$$
K_{comp}(i,j) = \sum_{u}\sum_{v} G(u,v)\,D(i-u,\,j-v)
$$

---

### Step 3) The resulting 5×5 kernel

Convolving the two 3×3 kernels above yields:

```text
 1  2  0  -2  -1
 3  6  0  -6  -3
 4  8  0  -8  -4
 3  6  0  -6  -3
 1  2  0  -2  -1
```

✅ This 5×5 kernel is **exactly equivalent** to applying:
1) Gaussian 3×3, then  
2) Gradient 3×3.

If your Gaussian is normalized (divide by 16), then the composed kernel is simply the above values divided by 16.

### Result

Their convolution yields a **5×5 kernel** that:
- spans a larger neighborhood
- encodes smoothing *and* direction
- is equivalent to applying the two filters in sequence

You rarely write this kernel explicitly —  
but the system *feels* its effect.

---

## 🌄 A Scale Interpretation

Kernel composition naturally increases **scale of reasoning**:

- small kernels → local detail  
- composed kernels → broader context  

Without resampling the image,  
the perception scale increases.

This links directly to:
- multi-scale vision
- coarse-to-fine reasoning
- stability vs precision trade-offs

---

## 🧩 Why This Matters in Practice

Understanding kernel composition helps you:

- reason about pipeline behavior  
- predict smoothing + edge effects  
- avoid redundant computation  
- design filters intentionally  

It also explains why:
- many “special” kernels already embed smoothing
- larger kernels often feel more stable

---

## ⚠️ Practical Notes

- Composed kernels may amplify noise if not designed carefully  
- Explicit large kernels are expensive — separable filters help  
- In CNNs, this principle still applies layer by layer  

Convolution does not forget — it **accumulates intent**.

---

## 💡 Computer Vision Takeaway

> Kernel composition is how vision pipelines **build meaning over space**.

Each convolution adds:
- context  
- structure  
- intent  

Stacked together, they form a **single, larger question**  
asked of the image.

---

## ✨ Summary

- 🔹 Convolution is associative  
- 🔹 Sequential filters compose into one kernel  
- 🔹 Kernel size grows naturally  
- 🔹 Smoothing + gradient ⇢ stable edge detection  
- 🔹 Composition explains multi-stage pipelines  

---

🌈 *In vision, filters don’t disappear — they combine.*
