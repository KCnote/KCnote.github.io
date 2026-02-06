---
layout: post
title: "Why Noise Cancels Out in Regions: A Statistical View"
date: 2026-02-02 00:00:00 +0900
author: kang
categories: [Computer Vision, Insights]
tags: [Computer Vision, Insights, Noise, Shot Noise, Quantum Noise, Statistics, Match]
pin: false
math: true
mermaid: true

---

> ✅ **Key clarification**  
> Noise does not become *exactly zero* inside a region.  
> Instead, if noise is **zero-mean and (approximately) independent**,  
> its **expected value is zero**, and its **variance shrinks as the region grows**.

This section formalises that idea with equations commonly used in computer vision.

---

## 1) Additive Zero-Mean Noise Model

Let an observed pixel be

$$
x_i = s_i + n_i
$$

where:
- $s_i$ : true signal
- $n_i$ : noise, with $\mathbb{E}[n_i] = 0$

---

### Region Sum

For a region $R$ containing $N$ pixels:

$$
X_R = \sum_{i \in R} x_i
= \sum_{i \in R} s_i + \sum_{i \in R} n_i
= S_R + N_R
$$

The noise term is

$$
N_R = \sum_{i \in R} n_i
$$

#### Expected Value

$$
\mathbb{E}[N_R]
= \sum_{i \in R} \mathbb{E}[n_i]
= 0
$$

📌 **Meaning**:  
The *expected* noise contribution of a region is zero, even though a single realisation may not be.

---

## 2) Why Region Averaging Is Powerful

Define the region mean:

$$
\bar{x}_R = \frac{1}{N}\sum_{i \in R} x_i
= \bar{s}_R + \bar{n}_R
$$

where

$$
\bar{n}_R = \frac{1}{N}\sum_{i \in R} n_i
$$

Assuming:
- $n_i$ are independent
- $\mathrm{Var}(n_i) = \sigma^2$

then

$$
\mathrm{Var}(\bar{n}_R)
= \frac{1}{N^2}\sum_{i=1}^{N} \mathrm{Var}(n_i)
= \frac{\sigma^2}{N}
$$

and

$$
\mathrm{Std}(\bar{n}_R) = \frac{\sigma}{\sqrt{N}}
$$

✅ **As the region size increases, noise fluctuations decay as $1/\sqrt{N}$.**

This is why noise appears to “disappear” at the region level.

---

## 3) Shot / Quantum Noise (Poisson Model)

Photon-counting noise is commonly modelled as Poisson.

Let

$$
X_i \sim \mathrm{Poisson}(\lambda_i)
$$

Then

$$
\mathbb{E}[X_i] = \lambda_i,
\quad
\mathrm{Var}(X_i) = \lambda_i
$$

Define noise as

$$
n_i = X_i - \lambda_i
$$

so that

$$
\mathbb{E}[n_i] = 0,
\quad
\mathrm{Var}(n_i) = \lambda_i
$$

---

### Region Sum (Poisson Additivity)

For independent pixels:

$$
X_R = \sum_{i \in R} X_i
\sim \mathrm{Poisson}(\Lambda),
\quad
\Lambda = \sum_{i \in R} \lambda_i
$$

Thus,

$$
\mathbb{E}[X_R] = \Lambda,
\quad
\mathrm{Var}(X_R) = \Lambda
$$

---

### Region Mean Variance

$$
\bar{X}_R = \frac{1}{N} X_R
$$

$$
\mathrm{Var}(\bar{X}_R)
= \frac{1}{N^2} \mathrm{Var}(X_R)
= \frac{\Lambda}{N^2}
$$

If $\lambda_i \approx \lambda$ within the region:

$$
\Lambda \approx N\lambda
$$

then

$$
\mathrm{Var}(\bar{X}_R)
\approx \frac{\lambda}{N}
$$

✅ **Even for shot (quantum) noise, region averaging reduces variance by $1/N$.**

---

## 4) Signal-to-Noise Ratio (SNR) Intuition

Two common perspectives:

- **Region sum**:
  - Signal grows as $\propto N$
  - Noise standard deviation grows as $\propto \sqrt{N}$  
  → SNR improves as $\sqrt{N}$

- **Region mean**:
  - Noise standard deviation shrinks as $1/\sqrt{N}$

Both explain why region-based methods are inherently more stable.

---

## ⚠️ Important Practical Notes

Noise averaging is not magic.

It works well only if:
- noise is approximately independent
- noise is zero-mean (no strong bias)

It fails or weakens when:
- fixed-pattern noise exists
- structured noise is present
- illumination bias dominates

This is why real systems also apply:
- normalisation
- background compensation
- calibration and correction

---

## 💡 Computer Vision Takeaway

> Pixel-level operations treat noise as signal.  
> Region-level operations treat noise as *something to be averaged away*.

This statistical reality underpins:
- region-based matching
- integral images
- pooling operations
- robust classical vision pipelines

---

✨ *Robust vision emerges from aggregation, not precision at the pixel level.*
