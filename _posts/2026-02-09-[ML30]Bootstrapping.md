---
layout: post
title: "Bootstrapping"
date: 2026-02-09 00:00:00 +0900
author: kang
categories: [Machince Learning, Model]
tags: [Machince Learning, Mathematics, Model, Classification Trees]
pin: false
math: true
mermaid: true
---

# 🎲 Bootstrapping

---

## 📌 1. What is Bootstrapping?

Bootstrapping is a **resampling technique** used when we **cannot sample additional data from the true distribution**.

- The true distribution is usually **unknown**
- Our goal is often to **estimate properties of that distribution**

Instead of collecting new independent data, we:

👉 Repeatedly sample **from the original dataset with replacement**

Each bootstrap dataset:

- Same size as original dataset
- Some samples appear **multiple times**
- Some samples **may not appear at all**

---

## 🔁 2. Bootstrap Procedure

Let original dataset be:

$$
Z = \{(x_1,y_1), (x_2,y_2), ..., (x_n,y_n)\}
$$

We generate **B bootstrap datasets**:

$$
Z^{*1}, Z^{*2}, ..., Z^{*B}
$$

Each created by **sampling with replacement** from $Z$.  

For each dataset, compute estimator:

$$
\hat{\alpha}^{*b}, \quad b = 1,2,...,B
$$

### 📏 Bootstrap Standard Error

$$
SE_B(\hat{\alpha}) =
\sqrt{
\frac{1}{B-1}
\sum_{b=1}^{B}
\left(\hat{\alpha}^{*b} - \bar{\alpha}^*\right)^2
}
$$

where

$$
\bar{\alpha}^* = \frac{1}{B} \sum_{b=1}^{B} \hat{\alpha}^{*b}
$$

This estimates the **standard error of the estimator**.

---

## ⚠️ 3. Limitations of Bootstrapping

Bootstrapping assumes:

### i.i.d assumption

Samples must be:

$$
\text{Independent and Identically Distributed (i.i.d)}
$$

If NOT true (e.g. **time series data**):

- Sampling individual observations breaks temporal structure
- Instead use **block bootstrap**

### Block Bootstrap

- Create blocks of consecutive observations
- Sample blocks with replacement
- Reconstruct dataset from sampled blocks

Used in:

- Time series
- Session‑based recommendation systems

---

## 🔍 4. Bootstrapping vs Cross‑Validation

### Can bootstrap estimate prediction error?

**Short answer: ❌ No**

### Reason

#### Cross‑Validation

- No overlap between training and validation sets
- Independent validation → unbiased estimate

#### Bootstrapping

- Samples drawn **with replacement**
- Bootstrap datasets **overlap heavily**
- Not independent → biased estimate

---

## 📊 5. Why does each bootstrap contain ~2/3 of data?

Probability a sample is **NOT selected** in one draw:

$$
1 - \frac{1}{n}
$$

Probability it is never selected in $n$ draws:

$$
\left(1 - \frac{1}{n}\right)^n
$$

Taking limit:

$$
\lim_{n \to \infty}
\left(1 - \frac{1}{n}\right)^n
= e^{-1} \approx 0.368
$$

So:

- About **36.8% NOT included**
- About **63.2% included**

👉 Each bootstrap sample contains **≈ 2/3 of original data**

---

## 🚨 6. Bias of Bootstrap Error

Because bootstrap datasets overlap:

- Bootstrap tends to **underestimate true prediction error**
- Validation sets are not fully independent

---

## 🧠 7. Summary

- Bootstrapping = sampling **with replacement**
- Used to estimate **variance, SE, confidence intervals**
- Requires **i.i.d assumption**
- Each bootstrap contains ~63% unique samples
- Cannot reliably estimate prediction error
- Use **cross‑validation** instead for model evaluation
