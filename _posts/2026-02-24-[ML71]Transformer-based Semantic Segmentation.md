---
layout: post
title: "Semantic Segmentation"
date: 2026-02-24 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligenc, Semantic Segmentation]
pin: false
math: true
mermaid: true

---
---------------------------------------------------------------------
# 🧠 Transformer-based Semantic Segmentation

## SETR · Segmenter · DPT -- Complete Mathematical & Architectural Notes

------------------------------------------------------------------------

# 1️⃣ SETR (SEgmentation TRansformer)

📄 Paper: https://arxiv.org/abs/2012.15840

## 🔷 Core Idea

First work applying **pure ViT encoder** to semantic segmentation.

Instead of CNN encoder + decoder, SETR uses:

$$
\textbf{Image} \rightarrow \textbf{Patch Embedding} \rightarrow \textbf{Transformer Encoder} \rightarrow \textbf{Light Decoder}
$$

------------------------------------------------------------------------

## 🔷 Patch Embedding

Input:

$$
X \in \mathbb{R}^{H \times W \times C}
$$

Split into patches of size:

$$
P \times P
$$

Number of patches:

$$
N = \frac{HW}{P^2}
$$

Flatten each patch:

$$
x_i \in \mathbb{R}^{P^2C}
$$

Linear projection:

$$
z_i = W_E x_i
$$

Where:

$$
W_E \in \mathbb{R}^{D \times (P^2C)}
$$

Add positional encoding:

$$
z_i^{(0)} = z_i + p_i
$$

------------------------------------------------------------------------

## 🔷 Transformer Encoder

For layer ℓ:

Self-attention:

$$
\text{Attention}(Q,K,V) =
\text{softmax}\left(\frac{QK^T}{\sqrt{D}}\right)V
$$

With:

$$
Q = ZW_Q,\quad K = ZW_K,\quad V = ZW_V
$$

Stack L layers:

$$
Z_L \in \mathbb{R}^{N \times D}
$$

Each token contains **global contextualized information**.

------------------------------------------------------------------------

# 2️⃣ SETR Decoders

### 🟢 Naive Decoder

-   1×1 Conv
-   Bilinear upsampling
-   Pixel-wise cross-entropy

### 🔵 Progressive Upsampling (PUP)

Alternating:

Conv → Upsample → Conv → Upsample

Gradually restore resolution.

### 🔴 Multi-Level Aggregation (MLA)

Use features from multiple layers:

$$
Z^{(6)}, Z^{(12)}, Z^{(18)}, Z^{(24)}
$$

Since same size:

$$
Z_{agg} = \sum_i Z^{(i)}
$$

Then upsample.

Best performance: MLA \> PUP \> Naive

------------------------------------------------------------------------

# 3️⃣ Segmenter

📄 Paper: https://arxiv.org/abs/2105.05633

## 🔷 Encoder

Standard ViT:

$$
Z_L \in \mathbb{R}^{N \times D}
$$

------------------------------------------------------------------------

## 🔷 Mask Transformer Decoder

Introduce:

$$
K \text{ learnable class embeddings}
$$

Append to patch tokens:

$$
Z' = [Z_L ; C]
$$

Run another Transformer.

Compute mask logits via dot-product:

$$
M = Z_L C^T
$$

Where:

$$
M \in \mathbb{R}^{N \times K}
$$

Reshape to 2D and upsample.

Apply softmax over classes.

------------------------------------------------------------------------

# 🔎 Impact of Patch Size

Smaller patch size:

-   Better spatial precision
-   Higher memory
-   Higher compute

Trade-off:

  Patch Size   Precision   Compute
  ------------ ----------- ----------
  32×32        Low         Fast
  16×16        Medium      Balanced
  8×8          High        Heavy

------------------------------------------------------------------------

# 4️⃣ DPT (Dense Prediction Transformer)

📄 Paper: https://arxiv.org/abs/2103.13413

## 🔷 Core Difference

Instead of single-scale tokens:

Reassemble features at multiple resolutions.

------------------------------------------------------------------------

## 🔷 Reassemble Block

Transform token sequence:

$$
Z \in \mathbb{R}^{N \times D}
$$

Back to spatial grid:

$$
Z \rightarrow F \in \mathbb{R}^{H' \times W' \times D}
$$

Project via convolution to different scales.

Multi-scale fusion similar to FPN.

------------------------------------------------------------------------

## 🔷 CLS Token Handling

Options:

-   Ignore
-   Add to features
-   Project via MLP

------------------------------------------------------------------------

# 5️⃣ Applications

✔ Semantic segmentation\
✔ Depth estimation\
✔ Dense prediction tasks

------------------------------------------------------------------------

# 6️⃣ Comparison

  Model       Encoder   Decoder Type         Multi-scale   Global Context
  ----------- --------- -------------------- ------------- ----------------
  SETR        ViT       Simple / PUP / MLA   ❌            ✅
  Segmenter   ViT       Mask Transformer     ❌            ✅
  DPT         ViT       Multi-scale fusion   ✅            ✅

------------------------------------------------------------------------

# 7️⃣ Mathematical Insight

CNN segmentation:

$$
\text{Local receptive field}
$$

Transformer segmentation:

$$
\text{Global receptive field}
$$

Self-attention complexity:

$$
\mathcal{O}(N^2)
$$

Where:

$$
N = \frac{HW}{P^2}
$$

Thus smaller patches → quadratic explosion.

------------------------------------------------------------------------

# 🔥 Key Takeaways

1.  SETR proved ViT works for segmentation.
2.  Segmenter introduced class embeddings for masks.
3.  DPT solved multi-scale issue.
4.  Patch size controls resolution vs compute.
5.  Transformer gives global context without convolution.
