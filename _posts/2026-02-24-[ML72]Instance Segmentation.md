---
layout: post
title: "Instance Segmentation"
date: 2026-02-24 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligenc, Instance Segmentation]
pin: false
math: true
mermaid: true

---
---------------------------------------------------------------------
# 🎯 Instance Segmentation & Mask R-CNN -- Complete Mathematical Notes

------------------------------------------------------------------------

# 1️⃣ Instance Segmentation Problem

Instance Segmentation =

$$
\text{Object Detection} + \text{Semantic Segmentation}
$$

Goal:

For each object instance:

-   Class label
-   Bounding box
-   Pixel-wise mask

Output:

$$
\{ (c_i, b_i, M_i) \}_{i=1}^{N}
$$

Where:

-   ( c_i ): class
-   ( b_i `\in `{=tex}`\mathbb{R}`{=tex}\^4 ): bounding box
-   ( M_i `\in `{=tex}{0,1}\^{m `\times `{=tex}m} ): binary mask

------------------------------------------------------------------------

# 2️⃣ Mask R-CNN Overview

📄 Paper: https://arxiv.org/abs/1703.06870

Mask R-CNN extends Faster R-CNN by adding a **mask prediction branch**.

Architecture:

Image → Backbone → RPN → RoIAlign → Parallel Heads:

-   Classification
-   Bounding box regression
-   Mask prediction

------------------------------------------------------------------------

# 3️⃣ RoIAlign (Critical Component)

Problem with RoIPool:

-   Quantization of coordinates
-   Misalignment

RoIAlign solution:

For sampling location (x, y):

Use bilinear interpolation:

$$
f(x,y) =
\sum_{i,j} w_{ij} f(x_i, y_j)
$$

Where weights depend on distance.

Preserves spatial correspondence.

------------------------------------------------------------------------

# 4️⃣ Mask Head

For each RoI:

Output mask:

$$
\hat{M} \in \mathbb{R}^{K \times m \times m}
$$

Important:

-   Fully convolutional
-   No FC layers
-   Maintains spatial info

During inference:

Select mask corresponding to predicted class.

------------------------------------------------------------------------

# 5️⃣ Multi-Task Loss

Total loss:

$$
\mathcal{L} =
\mathcal{L}_{cls} +
\lambda \mathcal{L}_{box} +
\lambda' \mathcal{L}_{mask}
$$

------------------------------------------------------------------------

## 🔷 Classification Loss

Cross-entropy:

$$
\mathcal{L}_{cls} =
-\sum_i y_i \log \hat{y}_i
$$

------------------------------------------------------------------------

## 🔷 Bounding Box Loss

Smooth L1:

$$
\mathcal{L}_{box} =
\sum_i \text{SmoothL1}(b_i - \hat{b}_i)
$$

Where:

$$
\text{SmoothL1}(x) =
\begin{cases}
0.5x^2 & |x|<1 \\
|x|-0.5 & \text{otherwise}
\end{cases}
$$

------------------------------------------------------------------------

## 🔷 Mask Loss

Binary cross-entropy per pixel:

$$
\mathcal{L}_{mask} =
-\sum_{i}\sum_{p} \left[
M_i(p)\log \hat{M}_i(p)
+ (1-M_i(p))\log (1-\hat{M}_i(p))
\right]
$$

Only computed for positive RoIs.

------------------------------------------------------------------------

# 6️⃣ Why Separate Mask Head?

Mask branch:

-   Independent from classification
-   Per-pixel supervision
-   Improves detection performance

Key insight:

Decoupling mask and class prediction improves accuracy.

------------------------------------------------------------------------

# 7️⃣ Mathematical Insight

Mask prediction is:

$$
\text{Dense classification inside each RoI}
$$

Different from semantic segmentation:

-   Works on region proposals
-   Instance-specific

------------------------------------------------------------------------

# 8️⃣ Advantages

✔ High accuracy\
✔ Clean multi-task learning\
✔ Minimal modification to Faster R-CNN

------------------------------------------------------------------------

# 9️⃣ Limitations

❌ Two-stage → slower\
❌ RoI-based → memory heavy\
❌ Hard to scale to very dense scenes

------------------------------------------------------------------------

# 🔥 Final Summary

  Component    Purpose
  ------------ ---------------------------
  RPN          Generate proposals
  RoIAlign     Precise alignment
  Class head   Category prediction
  Box head     Localization
  Mask head    Pixel-level instance mask

------------------------------------------------------------------------

# 🧠 Key Takeaways

1.  RoIAlign fixes quantization error.
2.  Mask head is fully convolutional.
3.  Multi-task loss jointly optimizes detection + mask.
4.  Mask R-CNN is still a strong baseline.

