---
layout: post
title: "R-CNN"
date: 2026-02-24 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Model]
tags: [Artificial Intelligenc, R-CNN]
pin: false
math: true
mermaid: true

---
# 🚀 Object Detection Evolution: From R-CNN to Faster R-CNN

---

# 🧭 1. Object Detection: Formal Problem Definition

Given an image:

$$
I \in \mathbb{R}^{H \times W \times 3}
$$

Goal: predict a variable-length set:

$$
\mathcal{D} = \{(c_i, b_i, s_i)\}_{i=1}^{N}
$$

Where:

- 🎯 $c_i$ : class label
- 📦 $b_i = (x_i, y_i, w_i, h_i)$ : bounding box (center format)
- 🔥 $s_i$ : confidence score

We must solve simultaneously:

1️⃣ **Classification**  
2️⃣ **Localization (Regression)**  

---

# 🧠 2. Detection = Classification + Localization

CNN backbone produces feature:

$$
f = \phi(I)
$$

## 2.1 Classification Head

$$
p = \mathrm{softmax}(W_c f)
$$

Cross entropy:

$$
L_{cls} = - \sum_{k=1}^{C} y_k \log p_k
$$

---

## 2.2 Bounding Box Regression

Predict:

$$
\hat{b} = W_r f
$$

Naive L2 loss:

$$
L_{reg} = \sum_{j \in \{x,y,w,h\}} (b_j - \hat{b}_j)^2
$$

---

## 2.3 Multi‑Task Learning

$$
L = L_{cls} + \lambda L_{reg}
$$

📌 This multi-task formulation is foundational for all R-CNN family models.

---

# 🏗 3. R‑CNN (2014)

## Architecture

1️⃣ Region proposal (Selective Search)  
2️⃣ ~2000 cropped regions  
3️⃣ CNN forward pass per region  
4️⃣ SVM classifier + box regressor

---

## ❌ Computational Bottleneck

If CNN forward cost = F

Total cost:

$$
2000F
$$

This makes inference extremely slow (~0.5 FPS).

---

# 📦 4. Bounding Box Regression (Core Derivation)

Given proposal:

$$
p = (p_x, p_y, p_w, p_h)
$$

Ground truth:

$$
g = (g_x, g_y, g_w, g_h)
$$

We define transformation targets:

$$
t_x = \frac{g_x - p_x}{p_w}
$$

$$
t_y = \frac{g_y - p_y}{p_h}
$$

$$
t_w = \log \frac{g_w}{p_w}
$$

$$
t_h = \log \frac{g_h}{p_h}
$$

🔎 Why log?  
Because width & height are positive and scaling invariant.

Reconstruction:

$$
\hat{g}_x = p_x + p_w \hat{t}_x
$$

$$
\hat{g}_w = p_w \exp(\hat{t}_w)
$$

---

# 📊 5. IoU (Intersection over Union)

$$
IoU = \frac{|B_p \cap B_g|}{|B_p \cup B_g|}
$$

Expanded:

$$
A_{union} = A_p + A_g - A_{intersection}
$$

IoU determines positive/negative samples.

---

# ⚡ 6. Fast R‑CNN (2015)

## 🔥 Key Improvement

Single CNN forward pass per image.

Instead of cropping 2000 regions, we:

1️⃣ Compute conv feature map once  
2️⃣ Project proposals onto feature map  
3️⃣ Apply RoI Pooling  

---

## 🧩 RoI Pooling Mathematics

Given region projected to feature map:

Divide into $k \times k$ bins.

Each bin applies:

$$
y_{ij} = \max_{(x,y) \in \text{bin}_{ij}} f(x,y)
$$

Output fixed-size feature.

---

## 🧮 Fast R-CNN Loss

$$
L = L_{cls} + \lambda [u \ge 1] L_{reg}
$$

Where u = ground truth class.

---

## 📉 Smooth L1 Loss

$$
SmoothL1(x) =
\begin{cases}
0.5x^2 & |x| < 1 \\
|x| - 0.5 & \text{otherwise}
\end{cases}
$$

Why?

- Quadratic near zero (stable gradients)
- Linear for large errors (robust)

---

# 🧠 Difference: R‑CNN vs Fast R‑CNN

| Feature | R‑CNN | Fast R‑CNN |
|----------|----------|--------------|
| CNN Passes | ~2000 per image | 1 per image |
| Speed | Very slow | 80‑200× faster |
| Memory | High | Much lower |
| Training | Multi‑stage | End‑to‑end |

---

# 🚀 7. Faster R‑CNN (2015)

## 💡 Main Idea

Replace Selective Search with learnable **Region Proposal Network (RPN)**.

---

# 📌 8. Anchors

At each feature map location:

Define k anchors.

Example:

- 3 aspect ratios
- 3 scales

Total:

$$
k = 9
$$

---

# 🎯 Anchor Classification

Positive:

$$
IoU > 0.7
$$

Negative:

$$
IoU < 0.3
$$

---

# 🧮 9. RPN Output Size

Per location:

- 2k classification scores
- 4k regression outputs

Total:

$$
2k + 4k
$$

---

# 🧮 10. RPN Loss (Full Expression)

Let:

- $p_i$ = predicted objectness
- $p_i^*$ = ground truth label
- $t_i$ = predicted box
- $t_i^*$ = ground truth transform

Full loss:

$$
L = \frac{1}{N_{cls}} \sum_i L_{cls}(p_i, p_i^*)
+
\lambda \frac{1}{N_{reg}} \sum_i p_i^* L_{reg}(t_i, t_i^*)
$$

Where:

$$
L_{cls} = - p_i^* \log p_i - (1 - p_i^*) \log(1 - p_i)
$$

Regression applied only when $p_i^* = 1$.

---

# 🧠 11. Faster R‑CNN Training Strategy

4‑Step Alternating Training:

1️⃣ Train RPN  
2️⃣ Train Fast R‑CNN detector  
3️⃣ Share conv layers & fine‑tune RPN  
4️⃣ Fine‑tune detector  

Later simplified into joint training.

---

# 🎯 12. RoI Align (Mask R‑CNN)

Problem with RoI Pooling:

Quantization error.

Solution:

Use bilinear interpolation.

Given sample point (x,y):

$$
f(x,y) =
\sum_{i,j} w_{ij} f(x_i, y_j)
$$

Weights proportional to distance.

Removes rounding → improves mask accuracy.

---

# 📊 13. Speed Comparison

| Model | FPS |
|--------|------|
| R‑CNN | ~0.5 |
| Fast R‑CNN | ~0.5–2 |
| Faster R‑CNN | 5–17 |

Speed gain comes from removing external proposal.

---

# 🧬 14. Evolution Summary

## 🟥 R‑CNN
- Accurate
- Extremely slow
- Not end‑to‑end

## 🟧 Fast R‑CNN
- Shared conv backbone
- RoI pooling
- Still external proposal

## 🟩 Faster R‑CNN
- RPN integrated
- End‑to‑end
- Anchor-based detection
- Practical speed

---

# 🏁 Final Takeaway

The R‑CNN family progressively:

- Reduced redundant CNN computation
- Integrated proposal generation
- Unified classification + regression
- Improved robustness with Smooth L1
- Introduced anchor parameterization

This evolution laid the foundation for modern detectors like:

- YOLO
- SSD
- RetinaNet
- DETR
