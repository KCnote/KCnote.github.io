---
layout: post
title: "Bagged Trees & Random Forests"
date: 2026-02-09 00:00:00 +0900
author: kang
categories: [Machince Learning, Machince Learning - Optimization]
tags: [Machince Learning, Optimization, Mathematics, Bagging, Random Forests]
pin: false
math: true
mermaid: true

---

# 🌲 Bagged Trees & 🌳 Random Forests

---

# 🎯 1. Bagged Trees (Bootstrap Aggregation)

Bagging = Train many decision trees on **bootstrap samples** and combine predictions.

## Procedure

1. Sample many bootstrap datasets from original data
2. Train a decision tree on each dataset
3. Combine predictions

Regression:

$$
\hat{f}_{bag}(x) = \frac{1}{B}\sum_{b=1}^{B}\hat{f}^{(b)}(x)
$$

Classification:

$$
\hat{y} = \text{majority vote}(\hat{y}^{(1)},...,\hat{y}^{(B)})
$$

---

## Why Bagging Works — Variance Reduction 📉

Averaging independent estimators reduces variance:

$$
Var(\bar{Z}) = \frac{\sigma^2}{B}
$$

Total error:

$$
MSE = Bias^2 + Variance
$$

👉 Bagging mainly **reduces variance** (trees are high‑variance models).

---

## Numerical Example 🎲

Suppose a single decision tree has:

- Bias² = 0.04
- Variance = 0.25

Then:

$$
MSE_{single} = 0.29
$$

If we bag **B = 25 independent trees**:

$$
Variance_{bag} = \frac{0.25}{25} = 0.01
$$

$$
MSE_{bag} = 0.04 + 0.01 = 0.05
$$

➡ Huge improvement from **0.29 → 0.05**

---

# 🌳 2. Random Forest

Random Forest = Bagging + **Feature Randomness**

Key idea: **Decorrelate trees** to improve ensemble.

## How it Works

- Still use bootstrap sampling
- But when splitting a node:
  - Instead of using all $p$ features
  - Randomly choose **m features**
  - Split using only those m

Typical choice:

$$
m = \sqrt{p} \quad (\text{classification})
$$

$$
m = \frac{p}{3} \quad (\text{regression})
$$

---

## Why Random Forest Beats Bagging 🧠

If trees are highly correlated → averaging does not reduce variance much.

Variance of ensemble:

$$
Var_{RF} = \rho\sigma^2 + \frac{1-\rho}{B}\sigma^2
$$

Where:

- $\rho$ = correlation between trees
- Smaller $\rho$ → stronger variance reduction

Random feature selection ↓ correlation → ↓ variance → ↓ error.

---

# 📊 3. Random Forest Behavior

As number of trees increases:

- Training error ↓
- Test error stabilizes
- Overfitting rarely happens

Because averaging stabilizes prediction.

---

# 🎲 4. Example — Effect of m (Feature Subset)

Suppose:

- p = 100 features

Compare:

| m | Behavior |
|---|----------|
| m = 100 (Bagging) | Trees very similar → high correlation |
| m = 50 | Slight decorrelation |
| m = √100 = 10 | Strong decorrelation → best performance |
| m = 1 | Too random → weak trees |

Typical best choice:

$$
m \approx \sqrt{p}
$$

---

# 📌 5. Random Forest Advantages 👍

- Strong prediction accuracy
- Handles nonlinear relationships
- Works with high‑dimensional data
- Resistant to overfitting
- Implicit feature selection
- Robust to noise

---

# ⚠️ 6. Limitations 👎

- Less interpretable than single tree
- High computation for many trees
- Large memory usage
- Can struggle with very sparse signals

---

# 📈 7. Out‑of‑Bag (OOB) Error

Each tree is trained on ~63% of data (bootstrap).

Remaining ~37% = **Out‑of‑Bag samples**

Use OOB samples as validation → unbiased error estimate.

No need for cross‑validation 👍

---

# 🔍 8. Random Forest vs Bagging

| Method | Key Idea |
|--------|----------|
| Bagging | Bootstrap + averaging |
| Random Forest | Bagging + feature randomness |
| Goal | Reduce variance |
| Difference | RF decorrelates trees |

---

# 🧠 9. When Random Forest Works Best

- High variance models (decision trees)
- Nonlinear data
- Many features
- Complex decision boundary

---

# 🚀 10. Summary

- Bagging reduces variance via averaging
- Random Forest reduces variance **even more** by decorrelating trees
- More trees → stable performance
- Feature randomness is key
- OOB error gives built‑in validation
