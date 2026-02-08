---
layout: post
title: "Overfitting"
date: 2026-02-08 00:00:00 +0900
author: kang
categories: [Machince Learning, Fitting]
tags: [Machince Learning, Mathematics, ML, Supervised, Fitting, Overfitting, Underfitting, Capacity]
pin: false
math: true
mermaid: true
---

# 📘 Training, Overfitting, Capacity & High‑Dimensional Learning

---

# 📉 Training vs Validation Curves

## 🎯 Overview

Training and validation curves are **core diagnostic tools** for understanding how a machine learning model learns over time.

They help detect:

- ⚠️ Underfitting  
- ⚠️ Overfitting  
- 🐞 Training bugs  
- ⚙️ Optimization failures  

We usually monitor:

$$
\text{Loss vs Epoch}
$$

---

## 📊 Typical Learning Behavior

### 🔵 Training Curve

- Training loss should **monotonically decrease (overall)**.
- Indicates model is learning from training data.
- Small noise/oscillation is normal.

### 🔴 Validation Curve

Typical pattern:

1. Decreases initially  
2. Reaches minimum  
3. Starts increasing  

This signals **Overfitting**.

---

## ⚠️ Overfitting Pattern

$$
\text{Training loss} \downarrow,\quad \text{Validation loss} \uparrow
$$

Meaning:

> The model memorizes training data instead of learning general patterns.

### 🛠 Fix Strategies

- Early stopping  
- Regularization ($$L_2$$, Dropout)  
- More data  
- Simpler model  

---

## 🚨 When Training Loss Does NOT Decrease

If training loss:

- stays flat  
- diverges  
- oscillates randomly  

➡️ **Assume a bug first.**

### Possible Causes

1. Learning rate too high → divergence  
2. Learning rate too low → no learning  
3. Gradient blocked / detached  
4. Loss not connected to parameters  
5. Wrong labels / mismatch  
6. Data normalization issue  
7. Activation saturation  
8. Optimizer not updating  

### Rule

> A correct model must reduce training loss unless data is pure noise or capacity is insufficient.

---

## 🌟 Ideal Training Scenario

Good:

- Training ↓ steadily  
- Validation ↓ then stabilizes  
- Small gap between train/val  

Bad:

- Train ↓, Val ↑ fast → Overfitting  
- Train not decreasing → Bug  
- Both high → Underfitting  

---

# 🌐 Overfitting and High‑Dimensional Data

## 🎯 Learning Goal

We care about **generalization**, not memorization.

---

## 📉 Why Validation Loss Gets Worse

Early stage:

- Learn signal → both losses decrease  

Later:

- Learn noise → training ↓, validation ↑  

➡️ **Overfitting**

---

## 📌 Definition of Overfitting

Model learns:

- Signal ✔  
- Noise ❌  

Result:

- Training improves  
- Validation worsens  

---

## 📊 Loss Interpretation

| Phase | Training | Validation | Meaning |
|------|----------|-----------|---------|
| Early | ↓ | ↓ | Learning signal |
| Optimal | Low | Lowest | Best generalization |
| Overfit | ↓ | ↑ | Learning noise |

---

## 📐 Curse of Dimensionality

If feature dimension = $$d$$:

- Space grows exponentially  
- Data becomes sparse  
- Required samples grow exponentially  

Conceptually:

$$
N \propto O(e^d)
$$

Example:

| Dimension | Needed Data |
|----------|-------------|
| 1D | ~100 |
| 10D | ~10^{10} |
| 100D | Practically impossible |

---

## 🔎 Why High‑Dim Causes Overfitting

When $$d$$ is large and data small:

- Infinite models fit training data  
- Noise easily fitted  
- Generalization collapses  

---

## 🛠 Practical Implications

High‑dim models require:

- More data  
- Strong regularization  
- Feature selection  
- Dimensionality reduction (PCA)  

---

# 🧠 Model Capacity, Overfitting & Underfitting

## 📌 Capacity Definition

Model capacity = ability to represent complex patterns.

| Model | Capacity |
|------|----------|
| Linear | Low |
| Polynomial | Medium |
| Deep Net | High |

---

## 🔴 Overfitting (High Capacity)

- Model memorizes noise  
- Very low training loss  
- Poor generalization  

---

## 🔵 Underfitting (Low Capacity)

- Cannot learn signal  
- Both losses high  

---

## 📊 Bias–Variance Tradeoff

$$
\text{MSE} = \text{Bias}^2 + \text{Variance} + \sigma^2
$$

| Capacity | Bias | Variance |
|----------|------|----------|
| Low | High | Low |
| Optimal | Balanced | Balanced |
| High | Low | High |

---

## 📐 Mathematical Capacity Example

Polynomial model:

$$
f(x) = \beta_0 + \beta_1 x + \beta_2 x^2 + \cdots + \beta_d x^d
$$

Higher $$d$$ → Higher capacity → Higher overfitting risk.

---

# 🧩 Additional Advanced Insights

## VC Dimension (Model Complexity)

Generalization bound:

$$
R(f) \le R_{emp}(f) + O\left(\sqrt{\frac{VC}{N}}\right)
$$

Where:

- model complexity  $$VC$$
- dataset size  $$N$$

Higher complexity requires **more data**.

---

## Double Descent (Modern Insight)

Modern deep learning shows:

- Error decreases → increases → decreases again  
- Occurs when capacity exceeds interpolation threshold  

---

## Overfitting Detection Strategy

Instead of one model:

- Train multiple capacity models  
- Observe validation minima  
- Estimate overfit onset  

---

## Small‑Data Warning (Important)

When dataset is very small:

- Cross‑validation may be **misleading**
- Overfitting detection unreliable
- High variance estimates

Use:

- Multiple runs  
- Careful validation  
- Simpler models  

---

# 🌟 Final Core Takeaways

- Training loss must decrease ✔  
- Validation loss shows generalization ✔  
- High dimension needs exponential data ✔  
- Overfitting = learning noise ✔  
- Capacity must match data ✔  
- Always validate carefully ✔  

> The best model is not the one with lowest training loss,  
> but the one with **best generalization and controlled complexity**.
