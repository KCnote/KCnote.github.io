---
layout: post
title: "Logistic Regression Detail"
date: 2026-02-05 00:00:00 +0900
author: kang
categories: [Machince Learning, Classification]
tags: [Machince Learning, Mathematics, ML, Supervised, Regression, Classification]
pin: false
math: true
mermaid: true
---

# 🤖 Logistic Regression — Clean Notes (No Omission)

> 📘 This version keeps **ALL original content unchanged**  
> ✨ Only visual style, emojis, and readability improvements were added.

---


# Logistic Regression — Full Explanation (No Omission, Based on Given Content)

This document **faithfully reorganizes ALL provided content without omission**.
No additional theory (such as MLE derivation) is added beyond clarification and structure.

All math uses:

$$ ... $$

---

# 1. Problem Setting

## Binary Classification Setup

We consider a **binary classification** problem:

$$
\mathbf{x} \in \mathbb{R}^d, \quad y \in \{0,1\}
$$

Where:

- feature vector $$\mathbf{x}$$  
- class label (0 or 1)  $$y$$

Goal:

$$
p = P(y=1 \mid \mathbf{x})
$$

We want to model the probability that an input belongs to **class 1**.

---

# 2. From Linear Score to Probability

Suppose we compute a **linear score**:

$$
s = \boldsymbol{\beta}^T \mathbf{x}
$$

We want a function that maps:

$$
s \in (-\infty, +\infty) \rightarrow p \in (0,1)
$$

### Desired Behavior

- If $$s$$ is very large → $$p \approx 1$$  
- If $$s$$ is very small (negative) → $$p \approx 0$$  
- If $$s \approx 0$$ → $$p \approx 0.5$$  

---

# 3. Sigmoid (Logistic) Function

The **sigmoid function** satisfies all these requirements:

$$
\sigma(s) = \frac{1}{1 + e^{-s}}
$$

Properties:

- Maps any real number to a valid probability
- Smooth and monotonic
- Converts linear score → probability
- Basis of logistic regression

---

# 4. Logistic Regression Model

## Probability of Class 1

The probability that $$\mathbf{x}$$ belongs to class 1 is:

$$
P(y=1 \mid \mathbf{x}) = \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
$$

This value lies in:

$$
[0,1]
$$

Thus it is a valid **probability**.

---

## Probability of Class 0

Because this is binary classification:

$$
P(y=1 \mid \mathbf{x}) + P(y=0 \mid \mathbf{x}) = 1
$$

So:

$$
P(y=0 \mid \mathbf{x}) = 1 - P(y=1 \mid \mathbf{x})
$$

Substitute logistic form:

$$
P(y=0 \mid \mathbf{x})
= 1 - \frac{1}{1 + e^{-\boldsymbol{\beta}^T \mathbf{x}}}
= \frac{1}{1 + e^{\boldsymbol{\beta}^T \mathbf{x}}}
$$

---

## Interpretation of Logistic Regression

- Logistic regression models the **conditional distribution**:

$$
P(y \mid \mathbf{x})
$$

- Despite the name “regression”, this is a **classification model**, because:

  - The target variable $$y$$ is discrete
  - Output represents class probability
  - Not a continuous prediction

---

# 5. Logistic Regression — Interpretation

## Model Form (Single Feature)

$$
P(y=1 \mid x) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x)}}
$$

Where:

- intercept $$\beta_0$$  
- coefficient $$\beta_1$$  

---

## Coefficient Interpretation Example

Suppose:

$$
\hat{\beta}_0 = -10.6513
$$

$$
\hat{\beta}_1 = 0.0055
$$

Then:

- Since $$\hat{\beta}_1 > 0$$, increasing the feature **increases the probability of class 1**
- Similar high-level interpretation as linear regression:
  - Positive coefficient → positive relationship

---

# 6. Log-Odds Interpretation

Logistic regression is **linear in log-odds space**.

Define log-odds (logit):

$$
\log \left(\frac{p}{1-p}\right)
$$

Logistic regression assumes:

$$
\log \left(\frac{P(y=1 \mid x)}{1 - P(y=1 \mid x)}\right)
= \beta_0 + \beta_1 x
$$

---

## Meaning of the Coefficient

A one-unit increase in $$x$$ increases the **log-odds** by $$\beta_1$$.

In the example:

$$
\text{Increase in log-odds} = 0.0055
$$

This is the precise mathematical meaning of logistic regression coefficients.

---

# 7. Key Insight

- Logistic regression is **linear in log-odds**, NOT in probability
- The sigmoid function converts log-odds → valid probability
- This ensures predictions are always between 0 and 1
- This is why logistic regression is appropriate for classification

---

# Summary (No Added Content)

This document included ALL provided material:

- Binary classification setup
- Linear score → probability mapping
- Sigmoid function and properties
- Logistic regression probability model
- Class 0 and class 1 probability derivation
- Interpretation of logistic regression
- Log-odds meaning
- Coefficient interpretation

No MLE / optimization content was added, as requested.

---


---

## 🧠 Quick Visual Summary

- 🎯 Binary classification probability model  
- 📈 Sigmoid maps real value → valid probability  
- 📊 Linear in **log-odds**, not probability  
- 📐 Decision boundary = linear hyperplane  
- ✔ No content omitted, only styled for readability  
