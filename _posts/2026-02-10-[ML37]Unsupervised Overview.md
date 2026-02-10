---
layout: post
title: "Unsupervised Learning"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Machince Learning, Unsupervised]
tags: [Machince Learning, Unsuperviseding, Overview]
pin: false
math: true
mermaid: true
---

# 🧠 Unsupervised Learning & Clustering

> Lecture-style structured notes with intuition, examples, and math

---

## 📌 Why Unsupervised Learning?

### 🎯 Goal
Unsupervised learning aims to **discover interesting structure** in data **without labels**.

- 🔍 Discover **subgroups / patterns** among observations or variables
- 📊 Find informative ways to **visualize high-dimensional data**

### 💡 Why is it important?
- Unlabeled data is **easier and cheaper** to obtain
- Labeling requires **human labor & expertise**

### ⚠️ Key Characteristic
- No single objective like prediction accuracy
- Results are often **subjective**

---

## 🧩 Clustering Problem

### 📖 Definition
Finding **natural groupings** among objects.

### ✅ Objective
- High **intra-cluster similarity**
- Low **inter-cluster similarity**

---

## 🧪 Clustering Examples

### 🧬 Gene Clustering
- Microarrays measure gene activity across conditions
- Similar expression patterns → clustered genes
- Helps infer **functions of unknown genes**

---

### 👤 User Clustering (Recommendation Systems)
- Core idea of **collaborative filtering**
- Users with similar tastes are grouped

> “Users like you also liked …”

---

### 🖼️ Image Compression

Each pixel is a vector:
$$
\mathbf{x}_i = [R_i, G_i, B_i]^T
$$

Cluster centers:
$$
\{\mu_1, \mu_2, \dots, \mu_K\}
$$

Assignment:
$$
\arg\min_k \| \mathbf{x}_i - \mu_k \|_2
$$

Fewer colors → smaller storage size

---

## 🆚 Classification vs Clustering

| Classification | Clustering |
|---------------|------------|
| Uses labels | No labels |
| Predict class | Discover structure |
| Supervised | Unsupervised |

---

## 🎭 Clustering is Subjective

There is **no single correct clustering**.

Possible groupings:
- Family-based
- Gender-based
- Occupation-based

Depends on **similarity definition**.

---

## 📏 Similarity / Distance Metrics

### L1 Distance
$$
L_1(A,B) = \sum_{i,j} |A_{ij} - B_{ij}|
$$

### L2 Distance
$$
L_2(A,B) = \sqrt{ \sum_{i,j} (A_{ij} - B_{ij})^2 }
$$

### Distance Matrix
$$
D =
\begin{bmatrix}
0 & d_{12} & d_{13} \\
d_{21} & 0 & d_{23} \\
d_{31} & d_{32} & 0
\end{bmatrix}
$$

---

## 🧱 Two Types of Clustering

### 🌲 Hierarchical Clustering
- Bottom-up (agglomerative)
- Produces a dendrogram

### 📦 Partitional Clustering
- Top-down
- Requires number of clusters K
- Example: K-means

---

## 🧠 Summary

- Unsupervised learning finds structure without labels
- Clustering is the most common technique
- Results depend on:
  - Distance metric
  - Number of clusters
  - Interpretation goal
