---
layout: post
title: "Canny Edge Detector"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Image Processing]
tags: [Computer Vision, Image Processing, Canny, Edge Detection, Gradient, Non-Maximum Suppression, Hysteresis]
pin: false
math: true
mermaid: true
---

## 🎯 What Canny Tries to Optimize

The Canny edge detector was designed to be an **optimal edge detector** under three criteria:

1. **Good detection**  
   Detect real edges while minimizing false detections caused by noise.

2. **Good localization**  
   The detected edge position should be as close as possible to the true edge.

3. **Single response**  
   Each real edge should be detected only once (thin edges).

These goals directly define the algorithmic steps.

---

## ✅ Inputs / Outputs

**Input**
- Grayscale image

**Output**
- Binary edge image

---

## 🧠 High-Level Pipeline

```
Input Image
   ↓
Gaussian Smoothing
   ↓
Gradient (Magnitude & Direction)
   ↓
Non-Maximum Suppression
   ↓
Double Thresholding
   ↓
Edge Tracking by Hysteresis
   ↓
Final Edge Map
```

---

## 1️⃣ Gaussian Smoothing (Noise Suppression)

### Why?
Differentiation amplifies noise.  
Edges must be detected **after smoothing**.

### Gaussian Kernel

$$
G(x,y) = \frac{1}{2\pi\sigma^2}
\exp\left(-\frac{x^2 + y^2}{2\sigma^2}\right)
$$

Smoothed image:

$$
I_s = I * G
$$

### Practical Notes
- Kernel size: typically \(5 \times 5\) or \(7 \times 7\)
- \(\sigma\) controls noise vs detail trade-off

---

## 2️⃣ Gradient Computation

### Intensity Gradient

$$
G_x = \frac{\partial I_s}{\partial x}, \quad
G_y = \frac{\partial I_s}{\partial y}
$$

Usually approximated using Sobel filters.

### Gradient Magnitude

$$
M(x,y) = \sqrt{G_x^2 + G_y^2}
$$

### Gradient Direction

$$
\theta(x,y) = \operatorname{atan2}(G_y, G_x)
$$

---

## 3️⃣ Non-Maximum Suppression (Edge Thinning)

### Purpose
Remove thick edges by keeping only **local maxima** of gradient magnitude.

### Principle
For each pixel:
- Look along the gradient direction
- Compare with two neighboring pixels
- Keep the pixel only if it is the maximum

### Direction Quantization
Gradient direction is quantized to:
- 0°
- 45°
- 90°
- 135°

---

## 4️⃣ Double Thresholding

### Why Two Thresholds?
A single threshold either:
- Breaks edges, or
- Keeps too much noise

### Classification

- **Strong edge**:  
$$
M(x,y) \ge T_H
$$

- **Weak edge**:  
$$
T_L \le M(x,y) < T_H
$$

- **Non-edge**:  
$$
M(x,y) < T_L
$$

Typical choice:
- \( T_L = 0.4 \sim 0.5 \cdot T_H \)

---

## 5️⃣ Edge Tracking by Hysteresis

### Core Idea
Weak edges are valid **only if connected** to strong edges.

### Connectivity
- Use **8-connected neighborhood**
- Ensures diagonal continuity

### Algorithm
1. Start from all strong edges
2. Recursively include connected weak edges
3. Discard isolated weak edges

This step guarantees **continuous edges**.

---

## 📐 Why Canny Works So Well

| Problem | Solution |
|------|---------|
| Noise | Gaussian smoothing |
| Thick edges | Non-maximum suppression |
| Broken edges | Hysteresis |
| False positives | Double thresholding |

---

## ⚠️ Common Implementation Pitfalls

- Too small \(\sigma\) → noisy edges
- Too large \(\sigma\) → lost fine details
- High \(T_H\) → broken edges
- Low \(T_L\) → noise leakage

---

## 🧪 Minimal Pseudo-Code

```text
smooth = gaussian_filter(I)
Gx, Gy = gradient(smooth)
M, theta = magnitude_direction(Gx, Gy)
N = non_maximum_suppression(M, theta)
E = hysteresis_threshold(N, TL, TH)
```

---

## 🎯 Takeaway

Canny is not just an edge filter — it is a **carefully engineered pipeline**.

Each step directly corresponds to an optimality criterion:
- Smoothing → noise robustness
- Gradient → edge strength
- NMS → single response
- Hysteresis → edge continuity

Understanding this pipeline allows you to **implement, tune, and debug** Canny from scratch 🚀
