---
layout: post
title: "Entry of Classification"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Machince Learning - Classification]
tags: [Machince Learning, Overview, ML, Supervised, Regression, Classification]
pin: false
math: true
mermaid: true
---

# 🎯 Classification — Linear Regression vs Logistic Regression

---

## 📌 Overview

In **classification problems**, our goal is to predict a **discrete class label** rather than a continuous value.

This post explains:

- Why **Linear Regression** is not suitable for classification ❌  
- Why **Logistic Regression** is the correct probabilistic model ✅  
- The mathematical intuition behind both approaches 📐  

---

# 🧠 Classification Basics

## 🔎 What is Classification?

Given:

- Feature vector: $$\mathbf{x}$$  
- Class label: $$y \in C$$  

We learn a function:

$$
f(\mathbf{x}) \in C
$$

Often, we prefer **probabilities** instead of hard labels:

$$
P(y = c \mid \mathbf{x})
$$

---

## 💡 Why Probabilities Matter

Probabilistic outputs enable:

- 🎯 Risk‑based decision making  
- ⚙️ Threshold tuning  
- 💰 Cost‑sensitive classification  
- 📊 Confidence estimation  

Example: Fraud detection → probability is more valuable than a binary decision.

---

# ⚠️ Can Linear Regression Be Used for Classification?

## Binary Encoding

$$
y =
\begin{cases}
0 & \text{No} \\
1 & \text{Yes}
\end{cases}
$$

One might try:

$$
\hat{y} > 0.5 \Rightarrow \text{Class 1}
$$

---

## 👍 Why It *Sometimes* Works

Because:

$$
\mathbb{E}[y \mid \mathbf{x}] = P(y=1 \mid \mathbf{x})
$$

Linear regression can approximate probabilities **in limited cases**.

---

## ❌ Major Problems

### 1. Predictions outside [0,1]

Linear regression may produce:

- Negative probabilities ❌  
- Probabilities > 1 ❌  

Which is **invalid for probability modeling**.

---

### 2. Multiclass Problem

Numeric coding introduces fake ordering:

$$
1=\text{stroke},\quad 2=\text{overdose},\quad 3=\text{seizure}
$$

Implies meaningless distance relationships → ❌ incorrect structure.

---

## 🚫 Conclusion

Linear regression is **not suitable for classification**.

Better alternatives:

- Logistic Regression ✅  
- Softmax / Multinomial Logistic Regression  
- LDA / QDA  
- Probabilistic classifiers  

---

# 🔷 Logistic Regression

## 🎯 Goal

Model probability:

$$
P(y=1 \mid \mathbf{x})
$$

We need a function mapping:

$$
(-\infty,+\infty) \rightarrow (0,1)
$$

---

## 📈 Sigmoid Function

$$
\sigma(s) = \frac{1}{1+e^{-s}}
$$

Properties:

- Smooth & monotonic 📈  
- Valid probability output 🎯  
- Basis of Logistic Regression  

---

## 📊 Logistic Model

$$
P(y=1 \mid \mathbf{x}) = \frac{1}{1+e^{-\boldsymbol{\beta}^T\mathbf{x}}}
$$

$$
P(y=0 \mid \mathbf{x}) = 1 - P(y=1 \mid \mathbf{x})
$$

---

# 🔍 Interpretation

## Log‑Odds (Logit)

$$
\log \frac{p}{1-p} = \boldsymbol{\beta}^T \mathbf{x}
$$

Meaning:

- Logistic regression is **linear in log‑odds**, not probability.

---

## Coefficient Meaning

If:

$$
\hat{\beta}_1 > 0
$$

→ Increasing feature **increases probability of class 1**.

Each +1 unit change in feature increases **log‑odds by $$\beta_1$$**.

---

# 📐 Maximum Likelihood Estimation (MLE)

## Likelihood

For binary outcome:

$$
P(y_i=1|\mathbf{x}_i)=\sigma(\boldsymbol{\beta}^T\mathbf{x}_i)
$$

Dataset likelihood:

$$
\mathcal{L}(\boldsymbol{\beta})=\prod_{i=1}^n p_i^{y_i}(1-p_i)^{1-y_i}
$$

---

## Log‑Likelihood

$$
\ell(\boldsymbol{\beta})=\sum_{i=1}^n \left[ y_i \log p_i + (1-y_i)\log(1-p_i) \right]
$$

Equivalent to minimizing **cross‑entropy loss**.

---

# 🧮 Linear vs Logistic — Key Differences

| Feature | Linear Regression | Logistic Regression |
|--------|------------------|---------------------|
| Output | Real value | Probability (0–1) |
| Task | Regression | Classification |
| Valid Probabilities | ❌ | ✅ |
| Decision Boundary | Linear | Linear (in log‑odds) |
| Optimization | Least Squares | MLE / Cross‑Entropy |
| Multiclass Extension | ❌ | Softmax |

---

# 🚀 Key Takeaways

- Linear regression can approximate classification but is **not probabilistically valid** ❌  
- Logistic regression models **true probabilities** using sigmoid ✅  
- Model is **linear in log‑odds** 📐  
- Estimated using **Maximum Likelihood** 📊  
- Foundation of modern classification methods 🧠  

---

# 📚 (Optional Extensions)

- 🔹 Regularization (L1 / L2)  
- 🔹 Multiclass Softmax Regression  
- 🔹 Decision boundary geometry  
- 🔹 Gradient / Hessian derivation  
- 🔹 Newton / IRLS optimization  
