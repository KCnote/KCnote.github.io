---
layout: post
title: "Softmax and Cross Entropy Loss"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Artificial Intelligence, Artificial Intelligence - Optimization]
tags: [Artificial Intelligence, Optimization, Softmax, Cross Entropy]
pin: false
math: true
mermaid: true
---

# 🟨 Limitations of Linear Classifier & Probabilistic Interpretation

> From raw **scores** to **probabilities**, and why loss functions like **cross-entropy** naturally arise.  
> Full derivations included, no steps skipped ✍️

---

## 1️⃣ What is the Meaning of the Score?

A linear classifier outputs **scores**:
$$
s_k = w_k^T x + b_k
$$

- $s_k$ is **not** a probability
- Only **relative differences** between scores matter

### Binary case (2 classes)

Let:
$$
s = s_1 - s_2
$$

- If $$s_1 > s_2$$ → class 1 more likely
- Larger gap $$|s_1 - s_2|$$ → higher confidence

But:
❌ Scores are **unbounded**  
❌ Not normalized  
❌ Not probabilities  

👉 We need a **mapping to [0,1]**

---

## 2️⃣ Sigmoid Function (Binary Classification)

We want a function such that:

- Large positive $$s$$ → close to 1  
- Large negative $$s$$ → close to 0  
- $s=0$ → 0.5  

### Sigmoid definition
$$
\sigma(s) = \frac{1}{1 + e^{-s}}
$$

### Apply to score difference
$$
p(y=c_1 \mid x) = \frac{1}{1 + e^{-(s_1 - s_2)}}
$$

$$
p(y=c_2 \mid x) = \frac{1}{1 + e^{-(s_2 - s_1)}}
$$

✅ Now we have **probabilities**  
❌ But only works for **2 classes**

---

## 3️⃣ Softmax Classifier (Multi-class)

For $$C > 2$$ classes, we generalize sigmoid.

### Softmax definition
$$
p(y = c_k \mid x)
= \frac{e^{s_k}}{\sum_{j=1}^{C} e^{s_j}}
$$

### Check probability properties

1. $0 \le p(y=c_k\mid x) \le 1$  
2. $\sum_{k=1}^C p(y=c_k\mid x) = 1$  

✅ Valid probability distribution  
✅ Differentiable (important for training)

---

## 4️⃣ Probabilistic Setting for Classification

### Ground truth labels

#### Binary
$$
y \in \{0,1\}
$$

#### Multi-class
One-hot vector:
$$
y =
\begin{bmatrix}
0 \\ \vdots \\ 1 \\ \vdots \\ 0
\end{bmatrix}
\in \{0,1\}^C
$$

with:
$$
\sum_{k=1}^C y_k = 1
$$

### Model output
Predicted probabilities:
$$
\hat{y}_k = p(y=c_k \mid x)
$$

Now we compare:

- True distribution $$y$$  
- Predicted distribution $$\hat{y}$$

👉 This naturally leads to **distribution distance measures**

---

## 5️⃣ Cross Entropy Loss (General Form)

From information theory:

> Cross entropy measures the **expected number of bits** needed to encode events from $$P$$ using a code optimized for $$Q$$.

### General multi-class loss
$$
\mathcal{L}
= -\frac{1}{N}
\sum_{i=1}^{N}
\sum_{k=1}^{C}
y_{ik} \log(\hat{y}_{ik})
$$

---

## 6️⃣ Cross Entropy Simplification (Key Insight)

Because **one-hot labels** satisfy:
$$
y_{ik} = 1 \text{ for only one } k
$$

All other terms vanish:

$$
\mathcal{L}
= -\frac{1}{N}
\sum_{i=1}^{N}
\log(\hat{y}_{i,T_i})
$$

Where:
- $$T_i$$ = true class index of sample $$i$$

👉 Interpretation:

> Loss = sum of **−log(predicted probability of the true class)**

---

## 7️⃣ Binary Cross Entropy (Special Case)

For binary classification:

$$
\mathcal{L}
= -\frac{1}{N}
\sum_{i=1}^{N}
\Big[
y_i \log(\hat{y}_i)
+ (1-y_i)\log(1-\hat{y}_i)
\Big]
$$

### Behavior
- If $$\hat{y} \to 1$$ and $$y=1$$ → loss → 0  
- If $$\hat{y} \to 0$$ and $$y=1$$ → loss → ∞  

🚨 Confident wrong predictions are **heavily penalized**

---

## 8️⃣ Why Logarithm?

Consider $$-\log(x)$$:

- $$x \in (0,1]$$
- $$x \to 1$$ → $$-\log(x) \to 0$$
- $$x \to 0$$ → $$-\log(x) \to \infty$$

This gives:
- Smooth gradients
- Strong penalty for confident mistakes

---

## 9️⃣ KL Divergence (Connection)

### Definition
For two distributions $$P$$ (true) and $$Q$$ (predicted):

$$
D_{KL}(P \| Q)
= \sum_i P(i)\log\frac{P(i)}{Q(i)}
$$

(or continuous form)

$$
D_{KL}(P \| Q)
= \int P(x)\log\frac{P(x)}{Q(x)}dx
$$

### Properties
- $$D_{KL}(P\|Q) \ge 0$$
- Asymmetric:
$$
D_{KL}(P\|Q) \ne D_{KL}(Q\|P)
$$
- Not a true metric (no triangle inequality)

---

## 🔗 Cross Entropy vs KL Divergence

Cross entropy:
$$
H(P,Q) = H(P) + D_{KL}(P\|Q)
$$

Since $$H(P)$$ is fixed (ground truth):

$$
\arg\min_Q H(P,Q)
= \arg\min_Q D_{KL}(P\|Q)
$$

✅ **Minimizing cross entropy = minimizing KL divergence**

---

## 🔚 Final Takeaways

- Linear classifier outputs **scores**, not probabilities
- Sigmoid → binary probabilities
- Softmax → multi-class probabilities
- Cross entropy naturally measures mismatch between:
  - true distribution
  - predicted distribution
- Training = minimizing KL divergence in disguise ✨

🔥 This forms the backbone of modern deep learning classifiers.
