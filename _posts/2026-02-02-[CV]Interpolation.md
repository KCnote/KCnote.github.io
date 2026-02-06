---
layout: post
title: "Interpolation"
date: 2026-02-02 00:00:00 +0900
author: kang
categories: [Computer Vision, Mathematics]
tags: [Computer Vision, Mathematics, Interpolation, Pixel, Nearest Neighbor, Bilinear, Bicubic, Quadratic]
pin: false
math: true
mermaid: true

---

> 🎯 **Core idea**  
> Interpolation allows computer vision systems to **infer values and positions between pixels**,  
> enabling **sub-pixel accuracy** beyond the discrete image grid.

This post explains *what interpolation is*, *why sub-pixel estimation is possible*, and *how it is used in practice*.

---

## 1) Why Interpolation Exists

Digital images are **discrete samples** of an underlying **continuous signal**.

- Real-world light intensity is continuous
- Sensors sample it on a pixel grid
- Information between pixels is lost during sampling

Interpolation attempts to **reconstruct local continuity** from these discrete samples.

---

## 2) What Sub-Pixel Estimation Means

Sub-pixel estimation refers to:

> Inferring positions or values at **fractions of a pixel** (e.g. 0.2 px, 0.35 px)

This is possible because:
- Image structures (edges, patterns, score maps) vary smoothly
- Neighboring pixel values encode local signal shape

Sub-pixel accuracy does **not** create new information —  
it extracts more precision from existing information.

---

## 3) Common Interpolation Methods

### Nearest Neighbor

- Chooses the closest pixel value
- Fast but discontinuous
- No meaningful sub-pixel inference

---

### Linear / Bilinear Interpolation

#### 1D Linear Interpolation

For two samples:

$$
f(x) = (1 - t)f(x_0) + tf(x_1)
$$

- Assumes local linearity
- Simple and stable
- Limited accuracy near curvature or edges

#### 2D Bilinear Interpolation

- Applies linear interpolation in both x and y
- Widely used for image resampling
- Supports sub-pixel value estimation

---

### Bicubic Interpolation

- Uses a **4×4 neighborhood (16 pixels)**
- Fits a smooth cubic surface
- Better edge behavior and smoothness
- Higher computational cost

---

## 4) Sub-Pixel Peak Estimation (Key CV Use Case)

In many CV problems (e.g. pattern matching, correlation, feature detection),
we search for **peaks** in a score map.

The true maximum rarely lies exactly at an integer pixel.

---

## 5) Quadratic Interpolation for Sub-Pixel Peaks

### 1D Case

Given three samples:

$$
(s_{-1}, s_0, s_{+1})
$$

A quadratic fit yields the sub-pixel offset:

$$
\Delta x
= \frac{s_{-1} - s_{+1}}
       {2(s_{-1} - 2s_0 + s_{+1})}
$$

- $\Delta x \in (-0.5, 0.5)$
- Computationally cheap
- Extremely common in practice

---

### 2D Extension

In 2D, a quadratic surface is fitted:

$$
f(x,y) = ax^2 + by^2 + cxy + dx + ey + f
$$

The sub-pixel peak is found by solving:

$$
\nabla f(x,y) = 0
$$

This approach is widely used in:
- pattern matching
- optical flow
- corner and blob detection

---

## 6) Why Sub-Pixel Accuracy Matters

### 🎯 Precision

- 1-pixel error can mean millimeters or microns in real space
- Industrial and metrology systems require sub-pixel precision

---

### ⚡ Performance

- Avoids extremely dense grid searches
- Enables **coarse-to-fine** strategies

---

### 🧠 Stability

- Interpolation smooths noise
- Peak location becomes more stable

---

## 7) Limitations and Caveats

Interpolation assumes:
- local smoothness
- sufficient signal-to-noise ratio

It degrades when:
- noise dominates
- aliasing is severe
- the signal is non-smooth or discontinuous

Sub-pixel estimation improves precision, **not robustness by itself**.

---

## 8) Computer Vision Takeaway

> Interpolation does not invent data.  
> It **recovers continuity** from discrete samples.

Sub-pixel estimation works because:
- real-world signals are smooth
- pixel grids are approximations

This principle underlies:
- precise pattern matching
- geometric measurement
- modern vision pipelines (classical and deep)

---

✨ *In computer vision, precision emerges from understanding the signal between pixels.*
