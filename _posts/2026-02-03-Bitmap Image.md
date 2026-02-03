---
layout: post
title: "Bitmap Images in Computer Vision"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Image]
tags: [Computer Vision, Image, Bitmap Image, Grayscale, Multi-Channel, RGB, Image Representation]
pin: false
math: true
mermaid: true
---

## 🖼️ What Is a Bitmap Image?

A **bitmap image** represents an image as a **grid of pixels**,  
where each pixel stores **explicit intensity or color values**.

Key properties:
- Discrete spatial sampling
- Discrete intensity values
- Direct memory mapping

In computer vision, bitmap images are treated as **numerical matrices**.

---

## 📐 Bitmap as a Matrix

A bitmap image of size $$H \times W$$ can be written as:

- **Single-channel**:  
$$
I \in \mathbb{R}^{H \times W}
$$

- **Multi-channel**:  
$$
I \in \mathbb{R}^{H \times W \times C}
$$

Where:
- height $$H$$  
- width $$W$$  
- number of channels $$C$$ 

---

## ⚫ Grayscale Image (1 Channel)

### Representation
Each pixel stores **one intensity value**.

$$
I(x,y) \in [0, 255]
$$

(8-bit grayscale)

### Example
```
0   → black
255 → white
```

### Characteristics
- Simple
- Memory efficient
- Most classical CV algorithms operate on grayscale

### Typical Usage
- Edge detection
- Thresholding
- Geometry (Hough, LSM)

---

## 🎨 Multi-Channel Images

### 3-Channel (RGB / BGR)

Each pixel has **three values**:

$$
I(x,y) = [R, G, B]
$$

Each channel:
$$
R, G, B \in [0, 255]
$$

### Characteristics
- Encodes color information
- Higher memory cost
- Often converted to grayscale for processing

### Typical Usage
- Visualization
- Color-based segmentation
- Feature extraction

---

## 🧠 N-Channel Images (General Case)

In computer vision, channels are **not limited to color**.

$$
I(x,y) = [c_1, c_2, \dots, c_N]
$$

Examples:
- RGB → 3 channels
- RGBA → 4 channels
- Hyperspectral → dozens or hundreds of channels
- Feature maps in CNNs

---

## 🌈 Common Channel Types

| Channels | Meaning | Usage |
|--------|--------|------|
| Gray | Intensity | Geometry, filtering |
| RGB | Color | Visualization |
| HSV | Hue/Saturation | Color segmentation |
| Depth | Distance | 3D vision |
| Normal | Surface orientation | 3D reconstruction |
| Feature | Learned | Deep learning |

---

## 📦 Memory Layout (Important)

### Interleaved (HWC)
```
[R G B][R G B][R G B]
```

### Planar (CHW)
```
RRR...
GGG...
BBB...
```

Layout matters for:
- Performance
- SIMD / GPU access
- Library compatibility

---

## 🔢 Bit Depth

Pixel values can have different precision:

| Bit Depth | Range |
|---------|------|
| 1-bit | 0–1 |
| 8-bit | 0–255 |
| 16-bit | 0–65535 |
| Float | Real values |

Higher bit depth:
- More dynamic range
- Higher memory cost

---

## ⚖️ Grayscale vs Multi-Channel

| Aspect | Grayscale | Multi-Channel |
|-----|-----------|--------------|
| Memory | Low | High |
| Speed | Fast | Slower |
| Information | Intensity only | Rich |
| CV usage | Core algorithms | Specialized tasks |

---

## 🧠 Practical Insight

In classical computer vision:

```
Input (RGB)
   ↓
Convert to Grayscale
   ↓
Statistics / Geometry
   ↓
Decision
```

Color is often **auxiliary**, not primary.

---

## 🎯 Takeaway

Bitmap images are **structured numeric data**, not pictures.

Understanding:
- Channels
- Bit depth
- Memory layout

is essential for:
- Performance
- Correct processing
- Algorithm design 🚀
