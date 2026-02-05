---
layout: post
title: "Supervised Learning Process"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Supervised]
tags: [Machince Learning, ML, Supervised, Model, Error, and Trade-offs]
pin: false
math: true
mermaid: true
---

# 🎯 Supervised Learning – Model, Error, and Trade-offs

---

## 🧠 Supervised Learning Workflow

**Goal:** Learn mapping from input \( x \) to output \( y \) using labeled data.

### 📌 Step-by-step

```
Step 1. Model Design
Step 2. Define the Goal (Prediction Error)
Step 3. Estimate Parameters (Optimization)
Step 4. Prediction (Inference)
```

>**Step 1. Model Design**  

Choose the functional form of the model.

Example:
$$
y = ax
$$
Inference target: parameter \( a \)

---

>**Step 2. Define the Goal (Prediction Error)**  

We want to minimize **Mean-Squared Prediction Error**:

$$
\mathbb{E}\big[(Y - \hat{f}(X))^2 \mid X = x\big]
= \mathbb{E}\big[(f(X) + \varepsilon - \hat{f}(X))^2 \mid X = x\big]
= \underbrace{(f(x) - \hat{f}(x))^2}_{\text{Reducible Error}}
+ \underbrace{\mathrm{Var}(\varepsilon)}_{\text{Irreducible Error}}
$$

### 🔍 Interpretation

- **Reducible Error** → Can be reduced by improving the model
- **Irreducible Error** → Noise inherent in the data (cannot be removed)

$$
\boxed{\text{Total Error = Reducible + Irreducible}}
$$

---

>**Step 3. Estimate Parameters (Optimization)**  

We estimate the unknown function by minimizing a loss function.

- If solvable analytically → closed-form solution (e.g., Normal Equation)
- If not solvable → use **Gradient Descent / Optimization algorithms**

$$
\boxed{\text{Learning = Optimization of parameters}}
$$

---

>**Step 4. Prediction (Inference)**  

Given unseen input \( x \), predict label:

$$
\hat{y} = \hat{f}(x)
$$

This is the **computing / inference phase**.

---

# 📉 Training vs Test Error

## Training Error (Fit to Seen Data)

$$
\mathrm{MSE}_{\mathrm{Tr}}
= \frac{1}{N}\sum_{i \in \mathrm{Tr}} \left[ y_i - f(x_i) \right]^2
$$

### ⚠️ Risk

- Overfitting → model memorizes training data
- Bias in performance estimate

---

## Test Error (Generalization)

$$
\mathrm{MSE}_{\mathrm{Te}}
= \frac{1}{M}\sum_{i \in \mathrm{Te}} \left[ y_i - \hat{f}(x_i) \right]^2
$$

### 🎯 Purpose

- Measures **true prediction ability**
- Reflects **generalization performance**

$$
\boxed{\text{Good model → Low Test Error}}
$$

---

# ⚖️ Trade-offs in Model Selection

Model design always involves balancing competing goals.

---

## 1️⃣ Good Fit vs Overfit / Underfit

| Case | Description |
|------|------------|
| Underfit | Model too simple → high bias |
| Good Fit | Balanced complexity |
| Overfit | Model too complex → high variance |

$$
\boxed{\text{Bias–Variance Trade-off}}
$$

---

## 2️⃣ Prediction Accuracy vs Interpretability

- Simple models → interpretable but less powerful  
- Complex models → accurate but harder to understand  

Examples:
- Linear Regression → interpretable
- Deep Neural Network → high accuracy, low interpretability

---

## 3️⃣ Parsimony vs Complexity

- **Parsimony (Occam's Razor):** Prefer simpler models when possible  
- Complex models may fit data better but risk overfitting  

$$
\boxed{\text{Simpler model if performance is similar}}
$$

---

# 📌 Key Insight

$$
\boxed{
\text{Learning = Model Design + Optimization + Generalization}
}
$$

- Training error ↓ does **not** guarantee good prediction
- Test error determines real-world performance
- Model selection is a balance of bias, variance, and complexity

---

# 🚀 Big Picture

$$
\boxed{
\text{Supervised Learning → Error Decomposition → Optimization → Generalization}
}
$$
