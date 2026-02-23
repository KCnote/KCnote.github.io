---
layout: post
title: "2D CNN Case Study"
date: 2026-02-23 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligenc, CNN]
pin: false
math: true
mermaid: true
---
# 📚 2D CNN Case Study: Evolution of Architectures

------------------------------------------------------------------------

# 1️⃣ AlexNet (2012)

## Core Structure

Input (224×224×3)\
→ Conv(11×11, stride 4, 96)\
→ ReLU\
→ LRN\
→ MaxPool\
→ Conv(5×5, 256)\
→ MaxPool\
→ Conv(3×3, 384)\
→ Conv(3×3, 384)\
→ Conv(3×3, 256)\
→ MaxPool\
→ FC(4096) → FC(4096) → FC(1000)

## Key Innovation

-   ReLU activation
-   Dropout
-   GPU training

## Difference

-   First large‑scale deep CNN success

------------------------------------------------------------------------

# 2️⃣ ZFNet (2013)

## Improvement Over AlexNet

-   Smaller first kernel (7×7 instead of 11×11)
-   Reduced stride
-   Better feature visualization

## Difference

-   Better receptive field control

------------------------------------------------------------------------

# 3️⃣ VGGNet (2014)

## Core Idea

Stack small 3×3 convolutions

Example (VGG16):

\[Conv64 ×2\] → Pool\
\[Conv128 ×2\] → Pool\
\[Conv256 ×3\] → Pool\
\[Conv512 ×3\] → Pool\
\[Conv512 ×3\] → Pool\
→ FC4096 → FC4096 → FC1000

## Receptive Field Derivation

For stacked convolutions:

$$
RF_{l} = RF_{l-1} + (k - 1)
$$

Two 3×3 layers:

$$
RF = 1 + (3-1) + (3-1) = 5
$$

Thus:

$$
3\times3 + 3\times3 \approx 5\times5
$$

## Difference

-   Very deep but uniform design

------------------------------------------------------------------------

# 4️⃣ GoogLeNet (Inception v1) (2014)

## Inception Module Structure

Parallel branches:

-   1×1
-   3×3
-   5×5
-   3×3 MaxPool

Concatenate outputs.

## Bottleneck Trick

Use 1×1 conv to reduce channel dimension:

If input channel = C, reduce to C′:

$$
C' < C
$$

Reduces parameter count.

## Difference

-   Multi‑scale processing in same layer
-   Fewer parameters than VGG

------------------------------------------------------------------------

# 5️⃣ ResNet (2015)

## Residual Learning

Instead of learning H(x), learn residual:

$$
H(x) = F(x) + x
$$

Residual block:

$$
y = F(x) + x
$$

Where:

$$
F(x) = W_2 \sigma(W_1 x)
$$

## Gradient Flow Insight

Backward gradient:

$$
\frac{\partial L}{\partial x}
= \frac{\partial L}{\partial y}
\left( 1 + \frac{\partial F}{\partial x} \right)
$$

The identity term prevents vanishing gradient.

## Difference

-   Enables very deep networks (100+ layers)

------------------------------------------------------------------------

# 6️⃣ Inception v2 / v3 / v4

## Factorized Convolution

Replace 5×5 with two 3×3:

$$
5\times5 \rightarrow 3\times3 + 3\times3
$$

Replace 3×3 with:

$$
3\times3 \rightarrow 1\times3 + 3\times1
$$

## Improvements

-   Batch Normalization
-   Better computational efficiency

------------------------------------------------------------------------

# 7️⃣ ResNeXt (2017)

## Aggregated Transformations

$$
y = x + \sum_{i=1}^{C} T_i(x)
$$

Where C = cardinality.

## Difference

-   Uses grouped convolution
-   Increases width via parallel paths

------------------------------------------------------------------------

# 8️⃣ DenseNet (2017)

## Dense Connectivity

Each layer receives all previous outputs:

$$
x_l = H_l([x_0, x_1, \dots, x_{l-1}])
$$

Concatenation instead of addition.

## Difference

-   Strong feature reuse
-   Efficient parameter usage

------------------------------------------------------------------------

# 9️⃣ MobileNet (2017)

## Depthwise Separable Convolution

### Standard Convolution Cost

$$
D_k^2 \cdot M \cdot N \cdot D_f^2
$$

### Depthwise Separable Cost

$$
D_k^2 \cdot M \cdot D_f^2
+
M \cdot N \cdot D_f^2
$$

Reduction ratio:

$$
\frac{D_k^2 M D_f^2 + M N D_f^2}{D_k^2 M N D_f^2}
=
\frac{1}{N} + \frac{1}{D_k^2}
$$

## Difference

-   Designed for mobile efficiency

------------------------------------------------------------------------

# 🔎 Evolution Trend Summary

| Model       |Main Focus
|  -----------|----------------------------
|  AlexNet    | Deep learning breakthrough
|  VGG        | Depth via small filters
|  GoogLeNet  | Multi‑branch width
|  ResNet     | Skip connections
|  DenseNet   | Dense connectivity
|  MobileNet  | Computational efficiency

------------------------------------------------------------------------

# 🚀 Final Insight

Evolution direction:

Depth ↑\
Width ↑\
Connectivity ↑\
Efficiency ↑

Modern CNNs combine residual learning, multi‑branch modules, and
parameter efficiency.
