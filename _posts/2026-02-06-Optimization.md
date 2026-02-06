---
layout: post
title: "Optimization and Gradient Descent"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Optimization]
tags: [Machince Learning, Overview, ML, Supervised, Classification, MLE, Optimization, Gradient Descent]
pin: false
math: true
mermaid: true
---

# 🚀 Optimization in Machine Learning — Styled Notes

> 📘 Clean, blog‑ready version with emojis & visual structure  
> 🎯 All original content preserved (no omission)  

---

# 🧠 What is Optimization?

According to Wikipedia:

> **Mathematical optimization** is the process of selecting the **best element**,
> according to some **criterion**, from a set of possible alternatives.

---

## 🔑 Key Components of Optimization

Every optimization problem has three essential parts:

### 1️⃣ Candidates (Alternatives)
Possible solutions to the problem.

Examples:
- Model parameters
- Actions
- Decisions

---

### 2️⃣ Criterion (Objective Function)

A function measuring how good a candidate is.

$$
\mathcal{L}(\hat{y}, y)
$$

or

$$
J(\theta)
$$

Examples:
- Loss function 📉  
- Cost function 📊  
- Likelihood function 📈  

---

### 3️⃣ Best Element

Goal: find the optimal candidate.

$$
\theta^* = \arg\min_{\theta} J(\theta)
$$

or

$$
\theta^* = \arg\max_{\theta} J(\theta)
$$

---

## 🤖 Optimization in Machine Learning

In ML:

- Candidates → model parameters ⚙️  
- Criterion → loss function 📉  
- Optimization → learning parameters from data 🧠  

Examples:

- Linear Regression → minimize squared error  
- Logistic Regression → maximize likelihood / minimize cross‑entropy  

---

# 🔍 Primitive Ideas in Optimization

Before advanced methods, simple strategies help build intuition.

---

## Basic Approaches

### 🔎 1. Exhaustive Search
Try all parameter combinations.

- Guarantees global optimum ✔  
- Computationally expensive ❌  

---

### 🎲 2. Random Search
Randomly sample candidate solutions.

- Often effective in high dimension  
- No guarantee of optimum  

---

### 📊 3. Visualization
Visualize objective function when possible.

Helps understand:
- Loss surface shape  
- Local minima / maxima  
- Convexity vs non‑convexity  

---

### 🧩 4. Greedy Search
Optimize one variable at a time.

- Simple ✔  
- May get stuck in local optimum ❌  

---

## 💡 Key Insight

These ideas motivate modern optimization methods:

- Gradient Descent 📉  
- Stochastic Gradient Descent (SGD) ⚡  
- Newton’s Method 📐  
- Convex Optimization  

---

# ⛰️ Following the Slope — Gradient Descent

## Objective

Minimize loss function:

$$
\min_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta})
$$

---

## Core Idea

At current parameters:

1. Compute gradient

$$
\nabla_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta})
$$

2. Move in **negative gradient direction**
3. Repeat until convergence

---

## 📐 Update Rule

$$
\boldsymbol{\beta}^{(t+1)}
=
\boldsymbol{\beta}^{(t)}
-
\eta \, \nabla_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta}^{(t)})
$$

Where:

- $\eta$ = learning rate  
- Gradient = steepest ascent direction  
- Negative gradient = steepest descent  

---

## 🤔 Why Negative Gradient?

- Gradient → steepest increase ⬆️  
- Negative gradient → steepest decrease ⬇️  
- Leads toward minimum 🎯  

---

## 📘 Interpretation

- Gradient = best local linear approximation  
- Each step slightly improves parameters  
- Repeated updates → reach minimum  

---

## 🌍 Where Gradient Descent is Used

- Linear Regression  
- Logistic Regression  
- Neural Networks  
- Deep Learning  

---

# 📐 Gradient Descent (Vector Form)

$$
\boldsymbol{\beta}^{new} = \boldsymbol{\beta}^{old} - \alpha \nabla_{\boldsymbol{\beta}} \mathcal{L}(\boldsymbol{\beta})
$$

Where $\alpha$ = learning rate.

---

## Single Parameter Form

$$
\beta_i^{new} = \beta_i^{old} - \alpha \frac{\partial}{\partial \beta_i}\mathcal{L}(\boldsymbol{\beta})
$$

- If slope < 0 → parameter increases  
- If slope > 0 → parameter decreases  

---

## 💻 Pseudocode

```python
beta = rand(vector)

while True:
    beta_grad = evaluate_gradient(L, data, beta)
    beta = beta - alpha * beta_grad

    if norm(beta_grad) <= threshold:
        break
```

---

## 🧠 Intuition

- Gradient → steepest ascent  
- Negative gradient → reduce loss  
- Iterative updates → reach minimum  

---

## ⚠️ Notes

Learning rate $\alpha$ is critical:

- Too small → slow convergence 🐢  
- Too large → divergence / oscillation ❌  

Convergence often when gradient norm is very small.

---

# ⚠️ Gradient Descent — Potential Problems

---

## 1️⃣ Non‑Convex Surface

Loss may not be convex:

- Multiple local minima  
- Saddle points  
- Gradient may be zero but not global optimum  

---

## 2️⃣ Requires Differentiable Loss

Gradient descent requires differentiable cost.

0/1 loss is not differentiable → use smooth surrogate:

- Logistic loss  
- Cross‑entropy  
- Squared error  

---

## 3️⃣ Slow Convergence

- Full gradient uses all data → expensive  
- Large datasets → slow training  
- Convergence may take long time  

---

# 🚀 Practical Improvements

Modern optimizers improve performance:

- Stochastic Gradient Descent (SGD) ⚡  
- Mini‑Batch Gradient Descent 📦  
- Momentum 🚀  
- Adam / RMSProp ⚙️  
- Second‑order methods (Newton, Quasi‑Newton) 📐  
