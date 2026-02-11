---
layout: post
title: "Data Preprocessing and Augmentation"
date: 2026-02-11 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Optimization]
tags: [Artificial Intelligenc, Optimization, Preprocessing, Augmentation]
pin: false
math: true
mermaid: true
---
# 📊 Data Preprocessing and Augmentation

------------------------------------------------------------------------

# 1️⃣ Zero-Centering & Normalization

## Zero-Centering

Given dataset:

$$
X \in \mathbb{R}^{N \times D}
$$

Compute mean:

$$
\mu = \frac{1}{N} \sum_{i=1}^{N} x_i
$$

Zero-centered data:

$$
X_{centered} = X - \mu
$$

------------------------------------------------------------------------

## Normalization (Standardization)

Standard deviation:

$$
\sigma = \sqrt{\frac{1}{N} \sum_{i=1}^{N} (x_i - \mu)^2}
$$

Normalized data:

$$
X_{norm} = \frac{X - \mu}{\sigma}
$$

------------------------------------------------------------------------

# 2️⃣ PCA (Principal Component Analysis)

Covariance matrix:

$$
\Sigma = \frac{1}{N} X_{centered}^T X_{centered}
$$

Eigen decomposition:

$$
\Sigma = U \Lambda U^T
$$

Projection:

$$
X_{PCA} = X_{centered} U
$$

------------------------------------------------------------------------

# 3️⃣ Whitening

Whitened data:

$$
X_{white} = X_{centered} U \Lambda^{-1/2}
$$

After whitening:

$$
Cov(X_{white}) = I
$$

------------------------------------------------------------------------

# 4️⃣ Data Augmentation

General transformation:

$$
x' = T(x)
$$

Where $T$ may represent:

-   Translation
-   Rotation
-   Scaling
-   Shearing
-   Cropping
-   Noise injection

------------------------------------------------------------------------

# 5️⃣ Color Jitter (RGB → HSL)

Normalize RGB:

$$
R' = \frac{R}{255}, \quad G' = \frac{G}{255}, \quad B' = \frac{B}{255}
$$

Define:

$$
C_{max} = \max(R', G', B')
$$

$$
C_{min} = \min(R', G', B')
$$

$$
\Delta = C_{max} - C_{min}
$$

------------------------------------------------------------------------

## Hue

$$
H =
\begin{cases}
0 & \Delta = 0 \\
60^\circ \times \frac{G' - B'}{\Delta} \mod 6 & C_{max} = R' \\
60^\circ \times \left(\frac{B' - R'}{\Delta} + 2\right) & C_{max} = G' \\
60^\circ \times \left(\frac{R' - G'}{\Delta} + 4\right) & C_{max} = B'
\end{cases}
$$

------------------------------------------------------------------------

## Saturation

$$
S =
\begin{cases}
0 & \Delta = 0 \\
\frac{\Delta}{1 - |2L - 1|} & \Delta \ne 0
\end{cases}
$$

------------------------------------------------------------------------

## Lightness

$$
L = \frac{C_{max} + C_{min}}{2}
$$

------------------------------------------------------------------------

# 6️⃣ Random Cropping & Scaling

Training:

1.  Pick random $L \in [256,480]$
2.  Resize shorter side to $L$
3.  Sample $224 \times 224$ crop

Testing:

-   Resize to multiple scales
-   Use multiple crops
-   Average predictions

------------------------------------------------------------------------

# 🎯 Summary

-   Zero-centering improves optimization stability.
-   Normalization equalizes feature scales.
-   PCA decorrelates features.
-   Whitening enforces identity covariance.
-   Data augmentation improves generalization.
