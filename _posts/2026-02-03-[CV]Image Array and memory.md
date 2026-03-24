---
layout: post
title: "Image Array Layout & Memory Structure"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Image]
tags: [Computer Vision, Image Layout, Stride, Padding, Bitmap, Memory Alignment]
pin: false
math: false
mermaid: true
---

## 🧠 Why Image Array Structure Matters

In computer vision, bugs often come **not from algorithms**,  
but from **wrong assumptions about image memory layout**.

Understanding array structure is essential for:
- Correct pixel access
- Fast processing
- Writing your own image loader
- Debugging strange artifacts

---

## 🧱 Image as Raw Memory

An image in memory is a **1D byte array**, interpreted as 2D or 3D.

Conceptually:

```
memory[]  →  rows  →  pixels  →  channels
```

But **rows are not always tightly packed**.

---

## 📐 Basic Image Dimensions

| Symbol | Meaning |
|------|--------|
| W | Width (pixels) |
| H | Height (pixels) |
| C | Channels |
| B | Bytes per channel |
| RowBytes | Actual bytes per row |
| Stride | Distance between row starts |

---

## 📏 Stride (Pitch)

**Stride** = number of bytes between the start of two consecutive rows.

```
row y starts at: base + y * stride
```

Stride ≥ (W × C × B)

Why larger?
- Alignment
- Padding
- SIMD / GPU constraints

---

## 🧩 Padding

**Padding** = unused bytes at the end of each row.

Purpose:
- Memory alignment
- Faster access
- Hardware constraints

Padding bytes **do not represent pixels**.

---

## 🟦 Bitmap (BMP) Row Alignment Rule

### The Rule
Each row in a BMP file must be aligned to **4-byte boundaries**.

Meaning:

```
RowBytes = ceil((W × bitsPerPixel) / 32) × 4
```

---

### Example: 24-bit BMP

- Width = 3 pixels
- 24 bits per pixel = 3 bytes

Raw row size:
```
3 × 3 = 9 bytes
```

BMP requires multiple of 4:

```
Padding = 3 bytes
RowBytes = 12 bytes
```

---

### Visual Layout

```
[B G R][B G R][B G R][P][P][P]
```

`P` = padding byte

---

## 🔄 Bottom-Up Storage (BMP)

Most BMP files store rows **bottom to top**.

Memory order:
```
Last row
...
First row
```

If ignored:
- Image appears vertically flipped

---

## 🎨 Channel Interleaving

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

BMP / PNG / JPEG files:
- Usually **interleaved**

Deep learning frameworks:
- Often **planar**

---

## 📦 Pixel Size Calculation

Bytes per pixel:

```
PixelBytes = C × B
```

Total memory (excluding padding):

```
H × W × C × B
```

Actual memory:

```
H × stride
```

---

## ⚠️ Common Bugs

### ❌ Assuming stride == width × channels
- Fails on BMP
- Fails on aligned buffers

### ❌ Ignoring bottom-up layout
- Flipped images

### ❌ Forgetting padding when copying
- Shifted or torn images

### ❌ Mixing RGB and BGR
- Color swapped output

---

## 🧪 Safe Pixel Access Formula

To read pixel (x, y):

```
address = base + y * stride + x * PixelBytes
```

Never assume contiguous rows.

---

## 🧠 Practical CV Insight

Most libraries expose:
- `width`
- `height`
- `channels`
- `stride` (or step)

Always trust **stride**, not width.

---

## 📊 Summary Table

| Concept | Why It Exists |
|------|---------------|
| Stride | Row alignment |
| Padding | Hardware efficiency |
| Bottom-up | Legacy BMP |
| Interleaving | Simplicity |
| Planar | Vectorization |

---

## 🎯 Takeaway

Images are **memory structures first**, pictures second.

If you understand:
- Stride
- Padding
- Alignment
- Layout

You can:
- Write loaders
- Avoid silent bugs
- Optimize performance

This knowledge separates **CV users** from **CV engineers** 🚀
