---
layout: post
title: "Semantic Segmentation"
date: 2026-02-24 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Model]
tags: [Artificial Intelligenc, Semantic Segmentation]
pin: false
math: true
mermaid: true

---
# 🎯 Semantic Segmentation, FCN, Deconvolution & U-Net
---


# 1️⃣ Computer Vision Tasks Overview

## Classification

-   Input: Image
-   Output: Single label
-   No spatial information

## Object Detection

-   Output: Bounding boxes + labels
-   Multiple objects possible

## Semantic Segmentation

-   Output: Pixel-wise class labels
-   Every pixel classified

## Instance Segmentation

-   Pixel-wise + instance separation

------------------------------------------------------------------------

# 2️⃣ Semantic Segmentation Problem Definition

Given image:

$$
X \in \mathbb{R}^{H \times W \times C}
$$

Goal:

$$
Y \in \{1,\dots,K\}^{H \times W}
$$

Each pixel is assigned class probability:

$$
p_k(i,j) = \frac{\exp(a_k(i,j))}{\sum_{c=1}^{K}\exp(a_c(i,j))}
$$

Loss (pixel-wise cross entropy):

$$
\mathcal{L} = -\sum_{i,j}\sum_{k} y_{k}(i,j)\log p_k(i,j)
$$

------------------------------------------------------------------------

# 3️⃣ Naive Patch-Based Approach

For each pixel:

-   Extract patch centered at (i,j)
-   Classify independently

Problem:

-   Heavy overlap
-   Redundant convolution
-   Inference cost = O(HW × patch cost)

------------------------------------------------------------------------

# 4️⃣ Fully Convolutional Networks (FCN)

Instead of patch-wise:

Apply CNN over full image.

Let feature map:

$$
F \in \mathbb{R}^{H' \times W' \times D}
$$

Final 1×1 convolution produces:

$$
S \in \mathbb{R}^{H' \times W' \times K}
$$

Need:

$$
H' = H, \quad W' = W
$$

But CNN reduces resolution via stride and pooling.

------------------------------------------------------------------------

# 5️⃣ Downsampling Mathematics

1D convolution:

$$
y[n] = \sum_{k=0}^{K-1} x[n\cdot s + k] w[k]
$$

Output size:

$$
\left\lfloor \frac{N - K}{s} \right\rfloor + 1
$$

Stride \> 1 ⇒ resolution reduction.

------------------------------------------------------------------------

# 6️⃣ Transposed Convolution (Deconvolution)

Transpose convolution defined as:

$$
y = X^T w
$$

Where convolution is:

$$
y = X w
$$

Thus transpose convolution uses matrix transpose.

------------------------------------------------------------------------

## 1D Example (Stride 2)

Input:

$$
[a, b]
$$

Kernel:

$$
[x, y, z]
$$

Output:

$$
[ax, ay, az + bx, by, bz]
$$

Overlapping contributions are summed.

------------------------------------------------------------------------

# 7️⃣ Output Size of Transposed Convolution

Given:

-   Kernel size k
-   Stride s
-   Padding p

Output:

$$
O = (I - 1)s - 2p + k
$$

This restores spatial resolution.

------------------------------------------------------------------------

# 8️⃣ Encoder--Decoder Architecture

Encoder:

-   Conv
-   Pooling
-   Stride

Decoder:

-   Transposed convolution
-   Upsampling
-   Recover spatial resolution

------------------------------------------------------------------------

# 9️⃣ U-Net Architecture

Originally for biomedical segmentation.

Structure:

Encoder (contracting path):

$$
H \to H/2 \to H/4 \to H/8
$$

Decoder (expanding path):

$$
H/8 \to H/4 \to H/2 \to H
$$

Skip connections:

$$
F_{decoder} = \text{Concat}(F_{encoder}, F_{upsampled})
$$

This preserves high-resolution boundary information.

------------------------------------------------------------------------

# 🔟 U-Net Loss Function

Pixel-wise softmax:

$$
p_k(x) = \frac{\exp(a_k(x))}{\sum_{c=1}^K \exp(a_c(x))}
$$

Weighted cross entropy:

$$
\mathcal{L} = -\sum_{x \in \Omega} w(x)\log p_{l(x)}(x)
$$

Weight map:

$$
w(x) = w_c(x) + w_0 \exp\left(-\frac{(d_1(x)+d_2(x))^2}{2\sigma^2}\right)
$$

Where:

-   d₁, d₂: distances to nearest object boundaries
-   Encourages learning borders

------------------------------------------------------------------------

# 🔥 Why Weight Map?

Without it:

-   Borders underrepresented
-   Multiple instances merge

With weighting:

-   Strong gradient on boundaries
-   Better instance separation

------------------------------------------------------------------------

# 🧠 Final Summary

  Model         Key Idea               Strength
  ------------- ---------------------- ---------------------
  Patch-based   Local classification   Simple
  FCN           Full image conv        Efficient
  Deconv        Learnable upsampling   Resolution recovery
  U-Net         Skip connections       Sharp boundaries

------------------------------------------------------------------------

# 🚀 Key Insights

1.  Convolution reduces resolution via stride.
2.  Transposed convolution restores via matrix transpose.
3.  Skip connections recover fine details.
4.  Weighted loss improves boundary precision.

------------------------------------------------------------------------

End of Complete Notes.
