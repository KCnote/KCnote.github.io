---
layout: post
title: "Nearest Neighbor"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Model]
tags: [Artificial Intelligence, Model, Nearest Neighbor]
pin: false
math: true
mermaid: true
---

# 🖼️ Image Classification & Nearest Neighbor

> Supervised Learning Basics – Image Classification  
> Lecture-style notes with full mathematical details ✨

---

## 🧠 An Image Classifier

Goal:
$$
f(\text{image}) \rightarrow \text{class label}
$$

There is no rule-based way to hard-code image recognition.

---

## 🤖 Machine Learning: Data-driven Approach

1. Collect labeled images  
2. Train a classifier  
3. Predict unseen images  

$$
\text{model} = \text{train}(\text{images}, \text{labels})
$$

$$
\hat{y} = \text{predict}(\text{model}, \text{image})
$$

---

## 🔍 Nearest Neighbor Classification

- Store all training images
- Assign the label of the most similar image

---

## 📏 Distance Metrics

Images are vectorized:
$$
A, B \in \mathbb{R}^d
$$

### L1 Distance
$$
L_1(A,B) = \sum_i |A_i - B_i|
$$

### L2 Distance
$$
L_2(A,B) = \sqrt{\sum_i (A_i - B_i)^2}
$$

---

## ⚙️ Algorithm Complexity

- Training: $$O(1)$$
- Prediction: $$O(N)$$

---

## 👥 k-Nearest Neighbor

- Use majority vote from k closest points
- Larger k → smoother boundary

---

## ⚠️ Issues

- Curse of dimensionality
- Pixel distance not semantic
- Slow at test time

---

## 🛠️ Practical Tips

- Normalize features
- Reduce dimensionality
- Tune k using validation
- Use approximate NN

---

## 🧠 Summary

Nearest Neighbor is simple but rarely used directly on pixels.
