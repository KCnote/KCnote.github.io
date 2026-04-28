---
layout: post
title: "Decision Trees"
date: 2026-02-09 00:00:00 +0900
author: kang
categories: [Machince Learning, Machince Learning - Foundation]
tags: [Machince Learning, Mathematics, Model, Classification Trees]
pin: false
math: true
mermaid: true
---

# 🌳 Classification Trees

---

## 📘 1. What is a Classification Tree?

A **classification tree** is very similar to a regression tree, except:

- Regression → predicts **continuous value**
- Classification → predicts **discrete (categorical) class** 🎯

For a classification tree, prediction is:

$$
\hat{y} = \text{most frequent class in region } R_j
$$

---

## ⚙️ 2. Decision Tree Algorithm (Classification)

### Step 1 — Partition the Feature Space 🔪

We divide the predictor space into **J disjoint regions**:

$$
R_1, R_2, ..., R_J
$$

Using **recursive binary splitting (top‑down greedy)**.

For feature $x_j$ and split point $s$:

$$
R_1(j,s) = \{x \mid x_j < s\}
$$
$$
R_2(j,s) = \{x \mid x_j \ge s\}
$$

We choose $(j,s)$ minimizing classification error via impurity:

$$
\frac{n_1}{n} \, \text{Impurity}(R_1) + \frac{n_2}{n} \, \text{Impurity}(R_2)
$$

Goal 👉 make regions **as pure as possible** 🧼

---

## 📊 3. Impurity Measures

### Entropy (Cross‑Entropy) 🔥

$$
\text{Impurity}(R_j) = -\sum_{k=1}^{K} p_{j,k} \log p_{j,k}
$$

Where:

- $p_{j,k}$ = proportion of class $k$ in region $R_j$

#### Properties

- Pure region → Entropy = **0**
- Uniform distribution → Entropy = **max = log(K)**

Examples:

$$
[1,0,0,0] \Rightarrow 0
$$

$$
[0.5,0.5] \Rightarrow \log 2
$$

$$
[0.2,0.2,0.2,0.2,0.2] \Rightarrow \log 5
$$

---

## 🎯 4. Prediction in Each Region

### Majority Vote

$$
\hat{y}_j = \arg\max_k \; p_{j,k}
$$

Equivalent form:

$$
\hat{y}_j = \arg\max_k \frac{1}{n_j} \sum_{x_i \in R_j} I(y_i = k)
$$

Where:

- $n_j$ = number of samples in region
- $I(\cdot)$ = indicator function

---

## 🧠 5. Decision Boundary Behavior

| Model | Boundary Shape |
|------|---------------|
| Linear Model | Straight line 📏 |
| Decision Tree | Axis‑aligned blocks 🧱 |

Trees create **step‑wise rectangular decision boundaries**.

---

## 👍 6. Pros of Decision Trees

- Very **interpretable** 👀  
- Graphical structure easy to understand  
- Mimics **human decision making** 🧠  
- Handles **categorical features naturally**  

---

## 👎 7. Cons of Decision Trees

- Often **lower predictive accuracy** than advanced models  
- High **variance / unstable** ⚠️  
- Small data change → very different tree  

---

## 🧩 8. Summary

- Classification tree partitions space into regions
- Uses impurity (entropy / Gini) to choose splits
- Predicts by **majority vote**
- Produces **rectangular decision boundaries**
- Easy to interpret but can overfit
