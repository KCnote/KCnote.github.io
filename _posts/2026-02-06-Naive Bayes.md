---
layout: post
title: "Naive Bayes"
date: 2026-02-06 00:00:00 +0900
author: kang
categories: [Machince Learning, Classification]
tags: [Machince Learning, Mathematics, ML, Supervised, Regression, Classification, Discriminant, Naive Bayes]
pin: false
math: true
mermaid: true
---
# 🤖 Naive Bayes 

> Complete mathematical reconstruction with intuition and practical fixes  
> Covers Bayes rule, independence assumption, smoothing, and numerical stability.

---

# 1. 🧠 Bayes Optimal Classifier

Bayes risk:

$$
f^* = \arg\min_f \mathbb{E}_{p(x,y)}[\mathcal{L}(y, f(x))]
$$

Equivalent decision rule:

$$
f^*(x) = \arg\max_y p(y \mid x)
$$

Using Bayes rule:

$$
p(y \mid x) = \frac{p(x \mid y)p(y)}{p(x)}
$$

Since $$p(x)$$ is constant across classes:

$$
f^*(x) = \arg\max_y p(x \mid y)p(y)
$$

---

# 2. 📦 Naive Bayes Assumption

For feature vector:

$$
x = (x_1,\dots,x_d)
$$

Naive Bayes assumes **conditional independence**:

$$
p(x \mid y) = \prod_{j=1}^{d} p(x_j \mid y)
$$

Meaning:

- Features are independent **given the class**
- Simplifies high-dimensional probability estimation

---

# 3. 🤔 Why Naive Bayes Works (Even When False)

Independence assumption is usually **incorrect**, but classification still works well because:

- True joint distribution $$p(x|y)$$ may be wrong
- But **class ranking often preserved**

$$
p(x|y_1)p(y_1) > p(x|y_2)p(y_2)
$$

✔ Correct ordering → correct classification

This explains Naive Bayes' surprisingly strong performance.

---

# 4. 🌤 Weather Dataset Example

Goal:

$$
P(\text{PlayGolf}=yes \mid x)
\quad vs \quad
P(\text{PlayGolf}=no \mid x)
$$

Given:

$$
x = (\text{Outlook=sunny, Temp=Hot, Humidity=High, Windy=False})
$$

---

## 4.1 Prior Probabilities

From dataset:

$$
P(yes) = \frac{9}{14}, \quad
P(no) = \frac{5}{14}
$$

---

## 4.2 Likelihood via Naive Factorization

### For class = **yes**

$$
P(x|yes)
= P(\text{sunny}|yes)
P(\text{Hot}|yes)
P(\text{High}|yes)
P(\text{False}|yes)
$$

From counts:

$$
= \frac{3}{9} \cdot \frac{2}{9} \cdot \frac{3}{9} \cdot \frac{6}{9}
\approx 0.016
$$

Posterior (unnormalized):

$$
P(yes|x) \propto 0.016 \cdot \frac{9}{14}
$$

---

### For class = **no**

$$
P(x|no)
= P(\text{sunny}|no)
P(\text{Hot}|no)
P(\text{High}|no)
P(\text{False}|no)
$$

From counts:

$$
= \frac{2}{5} \cdot \frac{2}{5} \cdot \frac{4}{5} \cdot \frac{2}{5}
\approx 0.051
$$

Posterior:

$$
P(no|x) \propto 0.051 \cdot \frac{5}{14}
$$

---

## 4.3 Prediction

Since:

$$
P(no|x) > P(yes|x)
$$

Prediction:

$$
\hat y = no
$$

---

# 5. ⚠️ Zero-Frequency Problem

If any conditional probability is zero:

$$
p(x_j|y) = 0
$$

Then:

$$
p(x|y) = 0
$$

→ catastrophic failure (class impossible).

---

## 5.1 Laplace Smoothing (Additive Smoothing)

Add small constant $$\epsilon$$ (usually 1):

$$
p(x_j|y) = \frac{count(x_j,y) + \epsilon}{count(y) + \epsilon K}
$$

Where:

- $$K$$ = number of possible values of feature $$x_j$$

✔ Prevents zero probabilities  
✔ Stabilizes estimation with small data  

---

# 6. 🔢 Numerical Stability — Log Trick

Probability product becomes extremely small:

$$
p(x|y) = \prod_j p(x_j|y)
$$

Instead compute in log space:

$$
\log p(x|y) = \sum_j \log p(x_j|y)
$$

Decision rule unchanged:

$$
\arg\max_y \log p(x|y) + \log p(y)
$$

✔ Avoids floating-point underflow  
✔ Faster computation  

---

# 7. 🚀 Key Insights

- Naive Bayes assumes conditional independence (rarely true)
- Still works because **class ranking preserved**
- Laplace smoothing fixes zero-probability problem
- Log-probability prevents numerical underflow
- Extremely fast & scalable
- Performs surprisingly well in:
  - Text classification
  - Spam detection
  - Document categorization
  - High-dimensional sparse data

---
