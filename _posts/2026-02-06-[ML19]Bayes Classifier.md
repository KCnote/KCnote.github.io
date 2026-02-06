---
layout: post
title: "Bayes Classifier"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Classification]
tags: [Machince Learning, Mathematics, ML, Supervised, Regression, Classification, Bayes]
pin: false
math: true
mermaid: true
---

# 📘 Classification, i.i.d. Assumption & Bayes Classifier

> 🎯 Clean structured markdown for blog use  
> 📐 Includes full math, intuition, and decision theory interpretation  
> 🧠 No omission — complete explanation  

---

# 🤖 Classification Problem

## Problem Definition

Given a feature vector **x** and a qualitative response **y** taking values in a set **C**,  
the classification task is to learn a function

$$
f(\mathbf{x})
$$

such that

$$
f(\mathbf{x}) \in C
$$

This function predicts the label **y** from input **x**.

---

## 📊 Training Data Assumption

We assume **n training samples** drawn from a joint distribution:

$$
p(\mathbf{x}, y)
$$

$$
(\mathbf{x}^{(i)}, y^{(i)}), \quad i = 1,\dots,n
$$

All samples are assumed **i.i.d.**

---

## 🔍 What Does i.i.d. Mean?

**i.i.d. = independent and identically distributed**

### 1️⃣ Independence

$$
p((\mathbf{x}^{(1)}, y^{(1)}), \dots, (\mathbf{x}^{(n)}, y^{(n)}))
=
\prod_{i=1}^{n} p(\mathbf{x}^{(i)}, y^{(i)})
$$

Interpretation:

- One sample gives **no information** about another
- Enables likelihood factorization
- Simplifies optimization and statistical analysis

---

### 2️⃣ Identically Distributed

$$
(\mathbf{x}^{(1)}, y^{(1)}), \dots, (\mathbf{x}^{(n)}, y^{(n)}) \sim p(\mathbf{x}, y)
$$

Meaning:

- All samples come from the **same underlying distribution**
- Training and test data share the same distribution
- Critical for **generalization**

---

## 🔁 Classification as Function Approximation

Classification learns a mapping:

$$
\mathbf{x} \rightarrow y
$$

We approximate this unknown mapping using:

$$
f(\mathbf{x})
$$

---

## Binary Classification Setup

$$
y \in \{+1, -1\}
$$

---

## 💡 Key Insight

- Classification learns mapping **x → y**
- Training samples assumed **i.i.d.**
- i.i.d. enables:
  - Likelihood factorization
  - Statistical learning theory
  - Generalization guarantees

---

# 📉 Risk & Bayes Classifier

## Risk = Expected Loss

Risk is defined as:

$$
R(f) = \mathbb{E}_{p(x,y)}[\mathcal{L}(y, f(x))]
$$

Discrete form:

$$
R(f) = \sum_x \sum_y p(x,y)\mathcal{L}(y,f(x))
$$

---

## Bayes Optimal Classifier

The optimal classifier minimizes risk:

$$
f^* = \arg\min_f \mathbb{E}_{p(x,y)}[\mathcal{L}(y,f(x))]
$$

Pointwise decision rule:

$$
f^*(x) =
\arg\min_{\hat{y}} \mathbb{E}_{p(y|x)}[\mathcal{L}(y,\hat{y})]
$$

$$
=
\arg\min_{\hat{y}} \sum_y p(y|x)\mathcal{L}(y,\hat{y})
$$

---

## Binary Case

$$
y \in \{+1,-1\}
$$

$$
f^*(x) =
\arg\min_{\hat{y}}
\Big[
p(y=1|x)\mathcal{L}(1,\hat{y})
+
p(y=-1|x)\mathcal{L}(-1,\hat{y})
\Big]
$$

Decision rule:

- If $$p(y=1|x) > p(y=-1|x)$$ → predict **+1**
- Else → predict **-1**

---

## 📐 Log‑Odds Decision Rule

$$
f^*(x) =
\text{sign}\left(
\log \frac{p(y=1|x)}{p(y=-1|x)}
\right)
$$

Interpretation:

- log‑odds > 0 → class +1
- log‑odds < 0 → class −1

---

## Using Bayes Rule

$$
p(y|x) = \frac{p(x|y)p(y)}{p(x)}
$$

Substitute into decision rule:

$$
f^*(x) =
\text{sign}\left(
\log \frac{p(x|y=1)p(y=1)}
{p(x|y=-1)p(y=-1)}
\right)
$$

Since $p(x)$ cancels, decision depends only on:

- Likelihood $p(x|y)$  
- Prior $p(y)$  

---

## 🧠 Key Concepts

- Risk = Expected loss  
- Bayes classifier = **risk minimizer**  
- Optimal decision = **largest posterior probability**  
- Log‑odds transforms probability comparison into decision rule  
- Bayes rule expresses posterior as likelihood × prior  

---

## Connection to Logistic Regression

Logistic regression assumes:

$$
\log \frac{p(y=1|x)}{p(y=-1|x)} = w^T x + b
$$

Thus logistic regression is a **parametric approximation** of the Bayes classifier under a **linear log‑odds assumption**.

---

## 🌟 Final Insight

The Bayes classifier assumes the **true probability distributions are known**:

$$
p(x|y), \quad p(y)
$$

Under this assumption, it produces the **optimal decision boundary** (minimum possible classification error).

All practical classifiers (logistic regression, LDA, neural networks, etc.) try to **approximate this Bayes optimal rule**.
