---
layout: post
title: "Arithmetic Operations in Image Processing - Part 1"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Image Processing, Arithmetic]
tags: [Computer Vision, Image Processing, Arithmetic]
pin: false
math: true
mermaid: true

---

> 🧠 **Core idea**  
> Arithmetic operations treat an image as a matrix of values.  
> By combining pixel values in different ways, we can control **brightness, contrast, noise, and details**.

This post summarizes the **most commonly used arithmetic operations in image processing**,  
with formulas written in **kramdown / MathJax-safe Markdown**.

---

## 🖼️ Image as Numbers

An image can be viewed as:

- Grayscale: 
$$
I(x,y) = [0,255] \; or  \; [0,1]
$$
- Color: operations are applied **per channel** (R, G, B)

All arithmetic operations are applied **pixel-wise**.

---

## 1️⃣ Addition (Add)

### Formula

$$
I_{out}(x) = I_1(x) + I_2(x)
$$

### Effect
- Increases brightness
- Combines information from two images
- Noise is also amplified

### Typical Uses
- Brightness adjustment
- Frame accumulation / averaging
- Original image + edge map (sharpening)

### Notes
- Values may overflow → clipping required

---

## 2️⃣ Subtraction (Subtract)

### Formula

$$
I_{out}(x) = I_1(x) - I_2(x)
$$

### Effect
- Highlights differences
- Removes common background
- Emphasizes changes

### Typical Uses
- Background subtraction
- Frame differencing
- Defect detection

### Notes
- Negative values may appear
- Often followed by absolute value

---

## 3️⃣ Absolute Difference

### Formula

$$
I_{out}(x) = | I_1(x) - I_2(x) |
$$

### Effect
- Direction-independent difference
- Clear visualization of change

### Typical Uses
- Motion detection
- Change detection maps

---

## 4️⃣ Multiplication (Multiply)

### Formula

$$
I_{out}(x) = I(x) \cdot c
$$

### Effect
- Adjusts contrast
- Dark regions get darker faster

### Typical Uses
- Contrast scaling
- Masking (ROI weighting)

### Notes
- Requires normalization

---

## 5️⃣ Division (Divide)

### Formula

$$
I_{out}(x) = \frac{I_1(x)}{I_2(x) + \epsilon}
$$

### Effect
- Illumination normalization
- Enhances relative contrast

### Typical Uses
- Flat-field correction
- Illumination compensation

### Notes
- \( \epsilon \) avoids division by zero
- Noise can be amplified in dark areas

---

## 6️⃣ Weighted Addition (Blending)

### Formula

$$
I_{out}(x) = \alpha I_1(x) + (1-\alpha) I_2(x)
$$

### Effect
- Smooth blending
- Controlled combination

### Typical Uses
- Image blending
- Overlay visualization

---

## 7️⃣ Power / Square Operation

### Formula

$$
I_{out}(x) = I(x)^2
$$

### Effect
- Enhances strong responses
- Suppresses weak noise

### Typical Uses
- Energy maps
- Feature emphasis

---

## 🧠 Practical Summary

| Operation | Core Effect | Typical Use |
|---------|------------|-------------|
| Add | Brightness ↑ | Accumulation |
| Subtract | Difference | Background removal |
| Abs Diff | Change only | Motion detection |
| Multiply | Contrast | Masking |
| Divide | Normalize | Illumination fix |
| Blend | Mix images | Visualization |
| Square | Emphasize peaks | Feature energy |

---

## ✨ Final Takeaway

> **Arithmetic operations are simple but powerful.**  
> When combined with filtering and morphology,  
> they form the foundation of most classical image-processing pipelines.

---
