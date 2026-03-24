---
layout: post
title: "Regularization"
date: 2026-02-08 00:00:00 +0900
author: kang
categories: [Machince Learning, Machince Learning - Fitting]
tags: [Machince Learning, Mathematics, ML, Supervised, Fitting, Overfitting, Underfitting, Capacity, Regularization, Ridge, Lasso]
pin: false
math: true
mermaid: true
---

# 📘 Regularization, Overfitting, Ridge & Lasso

---

## 🎯 Goal of Learning — Generalization

The objective of machine learning is **not to minimize training error**, but to **generalize well to unseen data**.

Typical behavior:

$$
\text{Training loss} \downarrow,\quad \text{Validation loss} \uparrow
$$

⚠️ This indicates **Overfitting** — the model starts memorizing noise instead of learning true patterns.

---

## 📉 Small Data Problem (e.g., Video Summarization)

Some problems have extremely limited data:

- 🎬 Video summarization  
- 🧬 Medical / rare events  
- 💸 Expensive labeling  

Even collecting **30 samples** can be costly.

### Risks
- Cross‑validation may become **misleading**
- Overfitting is hard to detect
- High variance in evaluation

### Practical Strategy
- ✔ Strict validation  
- ✔ Train **multiple capacity models**  
- ✔ Estimate overfitting onset  
- ✔ Avoid trusting a single run  

---

## ⏹️ Early Stopping

Under i.i.d. assumption, overfitting tends to begin at similar times across splits.

$$
\text{Stop at } \arg\min_{\text{epoch}} \text{Validation Metric}
$$

⚠️ Notes:

- Surrogate loss ≠ true metric  
- Metrics may overfit at different points  
- Use **most important real metric**  

---

## 🧠 Model Capacity

| Capacity | Behavior |
|----------|----------|
| 🔹 Low | Underfitting |
| ⭐ Optimal | Best Generalization |
| 🔺 High | Overfitting |

Bias–Variance:

| Capacity | Bias | Variance |
|----------|------|----------|
| Low | High | Low |
| Optimal | Balanced | Balanced |
| High | Low | High |

---

## 📏 Regularization / Shrinkage

General form:

$$
\min_{\beta} RSS + \lambda \cdot \text{Penalty}(\beta)
$$

Goal:

$$
\text{Reduce variance by shrinking coefficients } \beta
$$

---

## 🔵 Ridge Regression (L2)

Objective:

$$
\hat{\beta} = \arg\min_\beta \sum_{i=1}^{n}(y_i - x_i^T\beta)^2 + \lambda \|\beta\|_2^2
$$

Matrix solution:

$$
\hat{\beta} = (X^T X + \lambda I)^{-1} X^T Y
$$

### Properties
- Shrinks coefficients toward 0  
- Rarely exactly 0  
- Keeps all features  
- Variance ↓, Bias ↑  

Limits:

$$
\lambda = 0 \Rightarrow \text{OLS}
$$

$$
\lambda \to \infty \Rightarrow \beta \to 0
$$

---

## ⚖️ Scaling of Predictors (Critical)

Ridge is **scale sensitive**, therefore predictors must be standardized:

$$
\tilde{x}_{ij} = \frac{x_{ij} - \bar{x}_j}{\sqrt{\frac{1}{n}\sum_{i=1}^{n}(x_{ij}-\bar{x}_j)^2}}
$$

Why?

Penalty term:

$$
\lambda \sum_{j=1}^{p} \beta_j^2
$$

- Large‑scale feature → Larger penalty ❌  
- Small‑scale feature → Smaller penalty ❌  

✔ Scaling ensures **fair regularization**.

---

## 📊 Bias–Variance (Ridge Insight)

$$
\text{MSE} = \text{Bias}^2 + \text{Variance} + \sigma^2
$$

As $$\lambda$$ increases:

- Variance ↓  
- Bias ↑  
- Optimal $$\lambda$$ exists ⭐  

---

## 🔶 Lasso (L1)

Objective:

$$
\hat{\beta} = \arg\min_\beta \sum_{i=1}^{n}(y_i - x_i^T\beta)^2 + \lambda \|\beta\|_1
$$

$$
\|\beta\|_1 = \sum_{j=1}^{p} |\beta_j|
$$

### Properties
- Some coefficients become **exactly 0**  
- Produces **Sparse Model**  
- Performs feature selection  

---

## 💎 Why Lasso Produces Zeros

Constraint:

$$
\|\beta\|_1 \le t
$$

Diamond geometry → solutions at corners:

$$
\beta_j = 0
$$

✔ Natural feature selection

---

## 🌐 High‑Dimensional Effect

Higher dimension → more corners → more zeros → **sparser model**

---

## ⚔️ Ridge vs Lasso

| Property | Ridge 🔵 | Lasso 🔶 |
|----------|----------|----------|
| Penalty | $$L_2$$ | $$L_1$$ |
| Coefficients | Close to 0 | Can be exactly 0 |
| Feature selection | ❌ | ✔ |
| Stability | High | Medium |
| Sparse model | ❌ | ✔ |

---

## 🤔 Which is Better?

No universal winner.

- Sparse truth → Lasso better  
- Dense truth → Ridge better  

Best practice:

$$
\text{Choose using Cross Validation}
$$

---

## 🎛️ Hyperparameter Selection

1. Choose grid of $$\lambda$$  
2. Compute CV error  
3. Select minimum  
4. Refit with all data  

---

## 🧩 Practical Insights

- Small data → CV may deceive ⚠️  
- Always scale predictors ✔  
- Ridge → Smooth shrinkage  
- Lasso → Sparse solution  
- Capacity tuning critical  
- Estimate overfitting onset  

---

## 🌟 Final Insight

A good model is **not the one with lowest training loss**,  
but the one achieving **best generalization with controlled complexity**.


----
----

# Ridge Regression — Matrix Derivation

---

## Objective Function

Ridge regression minimizes:

$$
\hat{\beta} = \arg\min_{\beta} (Y - X\beta)^T (Y - X\beta) + \lambda \|\beta\|_2^2
$$

---

## Define the Objective

$$
J(\beta) = (Y - X\beta)^T (Y - X\beta) + \lambda \beta^T \beta
$$

---

## Gradient w.r.t. $$\beta$$

Taking derivative:

$$
\frac{\partial RSS(\beta)}{\partial \beta}
= -X^T (Y - X\beta) - (Y - X\beta)^T X + 2\lambda \beta
$$

Using transpose property:

$$
(Y - X\beta)^T X = X^T (Y - X\beta)
$$

So:

$$
\frac{\partial RSS(\beta)}{\partial \beta}
= -X^T (Y - X\beta) - X^T (Y - X\beta) + 2\lambda \beta
$$

---

## Combine Terms

$$
\frac{\partial RSS(\beta)}{\partial \beta}
= -2X^T (Y - X\beta) + 2\lambda \beta
$$

---

## Set Gradient to Zero

$$
-2X^T (Y - X\beta) + 2\lambda \beta = 0
$$

Divide by 2:

$$
X^T (Y - X\beta) = \lambda \beta
$$

---

## Expand

$$
X^T Y - X^T X \beta = \lambda \beta
$$

---

## Rearrangement

$$
X^T Y - X^T X \beta - \lambda \beta = 0
$$

$$
X^T Y = (X^T X + \lambda I)\beta
$$

---

## Closed-form Solution

$$
\hat{\beta} = (X^T X + \lambda I)^{-1} X^T Y
$$

---

## Dimension Check

$$
\beta \in \mathbb{R}^{d \times 1}
$$

$$
X^T X \in \mathbb{R}^{d \times d}
$$

$$
X^T Y \in \mathbb{R}^{d \times 1}
$$

Therefore:

$$
(X^T X + \lambda I)^{-1} X^T Y \in \mathbb{R}^{d \times 1}
$$

✔ Dimensions match — valid solution.
