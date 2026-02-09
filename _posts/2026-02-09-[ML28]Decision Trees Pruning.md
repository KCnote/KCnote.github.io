---
layout: post
title: "Pruning of Decision Trees"
date: 2026-02-09 00:00:00 +0900
author: kang
categories: [Machince Learning, Model]
tags: [Machince Learning, Mathematics, Model, Decision Trees, Pruning]
pin: false
math: true
mermaid: true
---

# 🌳 Decision Trees — Overfitting and Pruning (Complete Reconstruction)

This document reconstructs the provided slides **with minimal summarization**, preserving equations, algorithm flow, and visual interpretation.

---

# 1️⃣ Overfitting in Decision Trees

A decision tree **can easily overfit**.

Example shown:

- A large region is predicted based on **only one training point**
- This leads to **very irregular decision regions**
- The model memorizes noise instead of learning structure

Result:

- Extremely low training error
- Very poor generalization
- High variance model

---

# 2️⃣ Model Selection Perspective

To reduce overfitting, we move toward **simpler models**.

Key ideas:

- Fewer splits → smoother boundary
- Simpler tree → higher bias, lower variance
- Complex tree → low bias, high variance

Trade-off:

| Property | Small Tree | Large Tree |
|----------|------------|-----------|
| Complexity | Low | High |
| Boundary | Smooth | Irregular |
| Bias | High | Low |
| Variance | Low | High |
| Risk | Underfitting | Overfitting |

---

# 3️⃣ Handling Overfitting

Strategy 1: Build a **smaller tree**

- Fewer regions $$R_1,\dots,R_J$$
- Reduces variance
- Slight increase in bias
- Often improves interpretability

However:

A weak early split might later lead to a strong split.  
This is due to the **greedy nature** of tree building.

Thus, stopping early may be **short-sighted**.

---

# 4️⃣ Grow Large Tree then Prune

Alternative strategy:

1. Grow a very large tree $$T_0$$
2. Prune it back to smaller trees

---

## Weakest Link (Cost-Complexity) Pruning

We define a sequence of trees indexed by $$\alpha$$:

$$
\sum_{m=1}^{|T|} \sum_{x_i \in R_m} (y_i - \hat{y}_{R_m})^2 + \alpha |T|
$$

Where:

- number of leaf nodes $$|T|$$
- controls complexity penalty $$\alpha$$
- Larger  → smaller tree $$\alpha$$
- Smaller  → larger tree $$\alpha$$
 
Goal:

Find subtree minimizing penalized loss.

Pruning works by:

- Merging leaf nodes
- Removing weakest split

---

# 5️⃣ Tree Building with Pruning

Algorithm:

1. Grow large tree using recursive binary splitting
2. Apply weakest link pruning to obtain sequence of subtrees
3. Use K-fold cross-validation to select $$\alpha$$
4. Choose subtree corresponding to best $$\alpha$$

---

# 6️⃣ Example — Unpruned Tree

A very large regression tree $$T_0$$ is first grown using the training data.

Characteristics:

- Many splits
- Low training error
- High risk of overfitting

---

# 7️⃣ Example — Selecting Tree Size

We evaluate tree size using:

- Training error
- Cross-validation error
- Test error

Observation:

Best validation performance occurs at **3 leaf nodes**.

Thus, we prune the large tree down to optimal size.

---

# 📐 Mathematical Summary

## Tree Objective

$$
\min \sum_{j=1}^{J} \sum_{i \in R_j} (y_i - \hat{y}_{R_j})^2
$$

## Cost-Complexity Objective

$$
\min \sum_{m=1}^{|T|} \sum_{x_i \in R_m} (y_i - \hat{y}_{R_m})^2 + \alpha |T|
$$

---

# 🧠 Key Insights

- Decision trees easily overfit
- Small tree → high bias, low variance
- Large tree → low bias, high variance
- Best approach = grow large tree then prune
- Cross-validation selects optimal tree size

---

# 🌟 Final Insight

> Optimal decision trees balance fit and complexity using **cost-complexity pruning and cross-validation**.

---
