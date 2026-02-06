---
layout: post
title: "Image File Formats"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Image]
tags: [Computer Vision, Image, Format, BMP, PNG, JPEG, Image IO]
pin: false
math: false
mermaid: true
---

## 🖼️ Why Image Formats Matter in Computer Vision

Image file formats define:
- How pixels are **stored**
- Whether data is **compressed**
- Whether information is **lost**
- How images are **read and written**

To process images correctly, you must understand **what happens before pixels become a matrix**.

---

## 📦 Common Image Formats Overview

| Format | Compression | Lossy | Alpha | Typical Use |
|------|------------|-------|-------|------------|
| BMP | None | ❌ | ❌ | Raw storage, debugging |
| PNG | Lossless | ❌ | ✅ | Masks, UI, screenshots |
| JPEG | Lossy | ✅ | ❌ | Photos, datasets |

---

## 🟦 BMP (Bitmap Image)

### Concept
BMP stores **raw pixel values** with almost no processing.

- Very simple structure
- Large file size
- Easy to parse manually

---

### BMP File Structure

```
[ Bitmap File Header ]
[ DIB Header ]
[ Color Table ] (optional)
[ Pixel Array ]
```

#### Bitmap File Header (14 bytes)
- Signature: 'BM'
- File size
- Pixel data offset

#### DIB Header (40 bytes typical)
- Image width / height
- Bits per pixel (8 / 24 / 32)
- Compression (usually none)

---

### Pixel Storage
- Stored **bottom-up** by default
- Each row padded to **4-byte alignment**
- Common format: BGR (not RGB)

---

### When to Use BMP
- Debugging
- Teaching
- Raw inspection

**Not suitable** for large-scale CV pipelines.

---

## 🟩 PNG (Portable Network Graphics)

### Concept
PNG uses **lossless compression**.

- Pixel-perfect reconstruction
- Supports alpha channel
- More complex structure

---

### PNG File Structure

```
[ Signature ]
[ IHDR ]
[ PLTE ] (optional)
[ IDAT ] (compressed data)
[ IEND ]
```

Each block is called a **chunk**.

---

### Key Chunks

#### IHDR
- Width, Height
- Bit depth (8 / 16)
- Color type (Gray, RGB, RGBA)

#### IDAT
- zlib-compressed pixel data

---

### PNG Filtering
Before compression, each row is **filtered**:
- Sub
- Up
- Average
- Paeth

Purpose:
- Improve compression efficiency

---

### When to Use PNG
- Binary masks
- Labels
- Screenshots
- Lossless datasets

---

## 🟥 JPEG (JPG)

### Concept
JPEG uses **lossy compression** based on human perception.

- Much smaller files
- Information is lost
- No alpha channel

---

### JPEG Compression Pipeline

```
RGB
 ↓
YCbCr conversion
 ↓
Block splitting (8×8)
 ↓
DCT
 ↓
Quantization (lossy)
 ↓
Entropy coding
```

---

### Key Idea: Quantization

High-frequency components are **discarded**.

Result:
- Compression
- Blocking artifacts

---

### When to Use JPEG
- Natural images
- Large datasets
- When small size matters

**Avoid** JPEG for:
- Masks
- Depth images
- Scientific measurements

---

## 🧠 Reading Images (Conceptual)

Reading means:
1. Parse header
2. Decode compression (if any)
3. Reconstruct pixel array
4. Convert to matrix

Result:
$$
I \in \mathbb{R}^{H \times W \times C}
$$

---

## ✍️ Writing Images (Conceptual)

Writing means:
1. Prepare pixel array
2. Apply format-specific encoding
3. Write headers + data

Key decision:
- Lossy vs lossless
- Bit depth
- Channel order

---

## ⚠️ Practical CV Pitfalls

- JPEG introduces **non-linear noise**
- PNG preserves exact values
- BMP row padding causes bugs
- Channel order differs (RGB vs BGR)

---

## 🧩 Summary: How to Choose

| Need | Best Choice |
|----|------------|
| Exact pixel values | PNG / BMP |
| Small file size | JPEG |
| Binary mask | PNG |
| Debugging | BMP |
| Training images | JPEG / PNG |

---

## 🎯 Takeaway

Image formats are **not just containers**.

They define:
- Pixel integrity
- Numerical accuracy
- Pipeline correctness

Understanding BMP / PNG / JPEG is essential to build **reliable computer vision systems** 🚀
