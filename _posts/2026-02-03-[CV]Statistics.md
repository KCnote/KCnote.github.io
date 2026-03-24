---
layout: post
title: "Statistical Measures Commonly Used in Computer Vision"
date: 2026-02-03 00:00:00 +0900
author: kang
categories: [Computer Vision, Computer Vision - Mathematics]
tags: [Computer Vision, Mathematics, Statistics, Mean, Variance, Histogram, Entropy, Correlation, Robust Statistics]
pin: false
math: true
mermaid: true
---

## 📊 Why Statistics Matter in Computer Vision

Computer vision systems rely heavily on **statistics** to:
- Handle noise and uncertainty
- Make decisions from pixel distributions
- Design robust algorithms

Most classical CV algorithms are **statistical at their core**.

---

## 📐 Mean (Average)

### Definition
$$
\mu = \frac{1}{N} \sum_{i=1}^{N} x_i
$$

### Usage
- Global / local brightness
- Adaptive thresholding
- Integral image based methods

### Characteristics
- Simple and fast
- Sensitive to outliers

---

## 📉 Variance

### Definition
$$
\sigma^2 = \frac{1}{N} \sum_{i=1}^{N} (x_i - \mu)^2
$$

### Usage
- Texture strength
- Focus measure
- Otsu thresholding

### Characteristics
- Measures spread
- Noise sensitive

---

## 📏 Standard Deviation

### Definition
$$
\sigma = \sqrt{\sigma^2}
$$

### Usage
- Adaptive thresholding (Niblack, Sauvola)
- Noise estimation

### Characteristics
- Same unit as data
- Intuitive interpretation

---

## 📊 Histogram

### Definition
Frequency distribution of pixel intensities.

### Usage
- Global thresholding
- Histogram equalization
- Otsu / Entropy methods

### Characteristics
- Captures global distribution
- Loses spatial information

---

## 🧠 Entropy

### Definition
$$
H = - \sum_i p(i) \log p(i)
$$

### Usage
- Maximum entropy thresholding
- Texture complexity
- Information-based segmentation

### Characteristics
- Measures uncertainty
- Robust to illumination changes

---

## 🔗 Covariance

### Definition
$$
\text{Cov}(X,Y) = E[(X-\mu_X)(Y-\mu_Y)]
$$

### Usage
- PCA
- Feature correlation
- Motion analysis

### Characteristics
- Directional relationship
- Scale dependent

---

## 🔄 Correlation Coefficient

### Definition
$$
r = \frac{\text{Cov}(X,Y)}{\sigma_X \sigma_Y}
$$

### Usage
- Template matching
- Stereo correspondence

### Characteristics
- Normalized
- Range [-1, 1]

---

## 📌 Median

### Definition
Middle value of sorted data.

### Usage
- Median filtering
- Background modeling

### Characteristics
- Robust to outliers
- Slower than mean

---

## 🧮 Percentile / Quantile

### Definition
Value below which a percentage of data falls.

### Usage
- Contrast stretching
- Robust thresholding

### Characteristics
- Resistant to noise
- Distribution-aware

---

## ⚖️ Skewness

### Definition
$$
\text{Skewness} = E\left[ \left( \frac{x-\mu}{\sigma} \right)^3 \right]
$$

### Usage
- Histogram shape analysis
- Illumination bias detection

### Characteristics
- Detects asymmetry
- Sensitive to noise

---

## 📈 Kurtosis

### Definition
$$
\text{Kurtosis} = E\left[ \left( \frac{x-\mu}{\sigma} \right)^4 \right]
$$

### Usage
- Texture classification
- Outlier detection

### Characteristics
- Measures tail heaviness
- Sensitive to extreme values

---

## 🛡️ Robust Statistics (CV Practice)

| Statistic | Robust to Outliers | Typical Use |
|--------|-------------------|------------|
| Mean | ❌ | Fast estimation |
| Median | ✅ | Noise removal |
| MAD | ✅ | Robust spread |
| Percentile | ✅ | Contrast control |

---

## 🧠 Practical Insight

Typical CV pipeline:

```
Image
 ↓
Local / Global Statistics
 ↓
Decision (Threshold / Model)
 ↓
Geometry / Logic
```

Understanding **which statistic to use** is often more important than the algorithm itself.

---

## 🎯 Takeaway

Statistics are the **decision engine** of computer vision.

Choosing the right measure:
- Improves robustness
- Reduces parameter tuning
- Explains algorithm behavior

Classical CV = **Statistics + Geometry + Logic** 🚀
