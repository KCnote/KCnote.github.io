---
layout: post
title: "02. Embedding Interpret with Eigen"
date: 2026-02-25 00:00:00 +0900
author: kang
categories: [Deep Learning, Insights]
tags: [Artificial Intelligence, Embedding, Eigenvalue]
pin: false
math: true
mermaid: true
---

---
# <b>Embedding Interpret with Eigen</b>
---
### <b>Prerequisites</b>
    1. Eigenvalue and Eigenvector
    2. Embedding

<b>Eigen</b> can interpret the transformation space and dimension reduction.
<b>Embedding</b> is vector of representation word or somethings.

---
## <b>How can interpret embedding</b>

### 1. Interpret embedding as usual?
- Usually, between embedding relationship A and B, using the dot-product. because if the dot-product value is closer to 1, it means the embedding meaning is so close.

$$
Embedding A \cdot Embedding B \approx 1 
$$

### 2. visualizaiton of embedding relationship.

The relationship of embedding with visiualization is not easy because of the embedding vector size that is over 4D.
So i wanna check the relationship with visualization. I will use the PCA or SVD with eigenvalue for dimension reduction.

.... Not yet i will ..Plan
