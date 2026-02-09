---
layout: post
title: "Decision Trees"
date: 2026-02-09 00:00:00 +0900
author: kang
categories: [Machince Learning, Model]
tags: [Machince Learning, Mathematics, Model, Decision Trees]
pin: false
math: true
mermaid: true
---

# 🌳 Decision Trees

Decision trees are intuitive, interpretable models that **split the feature space into simple regions**.

---

# 1️⃣ Tree Terminology Review

## 🌲 General Tree Structure

A general tree **T** is partitioned into:

- **Root node** r  
- A set of **subtrees** attached to the root  

Example structure:

        A (root)
       / \
      B   C
     /|\
    D E F

---

## 📘 Basic Terminology

| Term | Meaning |
|------|---------|
| 🔵 Node (Vertex) | A point in the tree |
| ➖ Edge | Connection between nodes |
| 👨 Parent | Node with children |
| 👶 Child | Node below parent |
| 👥 Siblings | Nodes with same parent |
| 🌳 Root | Top node |
| 🍃 Leaf | Node with no children |
| ⬆ Ancestor | Any node above |
| ⬇ Descendant | Any node below |
| 🌿 Subtree | Tree inside tree |

---

# 2️⃣ Tree-Based Learning

## 🎯 Core Idea

Tree-based learning **segments the predictor space into simple regions**.

✔ Recursive partitioning  
✔ Forms decision rules  
✔ Works for both:

- 📈 Regression  
- 🔎 Classification  

---

## ⚙️ How It Works

1. Choose best feature & split value  
2. Divide feature space  
3. Repeat recursively  
4. Leaf → prediction  

---

## 📊 Interpretation

- Space split into **rectangular regions**
- Prediction is **constant inside each region**
- Model behaves like a **step function**

---

# 3️⃣ Introduction to Decision Trees

## ✅ Pros

- 🧠 Easy to interpret
- 🔍 Transparent decision rules
- ⚖️ No feature scaling required
- 🔄 Handles nonlinear relationships
- 📊 Works with mixed data types

---

## ❌ Cons

- 📉 Often lower prediction accuracy
- ⚠️ High variance (unstable)
- 🌪 Prone to overfitting

---

# 4️⃣ Ensemble Methods 🌲🌲🌲

Multiple trees → Stronger model

| Method | Idea |
|--------|------|
| 🎲 Bagging | Reduce variance |
| 🌲 Random Forest | Randomized bagging |
| 🚀 Boosting | Reduce bias sequentially |

➡ Dramatically improves accuracy

---

# 5️⃣ Model Flexibility vs Interpretability

| Model | Flexibility | Interpretability |
|-------|------------|------------------|
| Linear Models | Low | High |
| Decision Trees | Medium | Medium |
| Random Forest / Boosting | High | Low |
| Deep Learning | Very High | Very Low |

---

# 🧠 Key Insights

- Decision trees **partition feature space**
- Produce **interpretable rules**
- Single tree → simple but weak
- Many trees → powerful model

---

# 📌 One-Line Summary

Decision Trees split the feature space into simple regions to form interpretable decision rules 🌳
