---
layout: post
title: "01. Multi-layer Perceptrons"
date: 2026-02-10 00:00:00 +0900
author: kang
categories: [Deep Learning, Foundations]
tags: [Artificial Intelligence, MLP]
pin: false
math: true
mermaid: true
---

---
# <b>Multi-layer Perceptrons</b>
---
### <b>Prerequisites</b>
    1. Linear Classifiers
<b>Linear Classifiers</b> $ f(x) = Wx $ have a problem that <span style="color:#FFD5D5">FAILS on NON-LINEAR SEPERABLE DATA</span>. 

---
## <b>What is Multi-Layer Perceptrons</b>

### 1. What is Multi-layer Perceptrons(MLP)?
- A <span style="color:blue"><b>Feedforward Neural Network</b></span> composed of multiple fully connected layers with nonlinear activation functions.
    
- <b>Structure</b>

    $$
    Input -> FC -> Activation -> FC ... -> Softmax -> Output
    $$

- Can express non-linear seperable data like XOR

### 2. Why use Multi-layer Perceptrons(MLP)?
     Linear/Non-Linear Seperable Data


$$
y = f(w_1x_1+w_2x_2),
\:\:\:\:\:\:\:\
\text{f}(x) = 
\begin{cases} 
1, & x > 0 \\
0, & \le 0 
\end{cases}
$$

<div style="width:100%;">

| x1 | x2 | y |
|----|----|---|
| 0  | 0  | 0 |
| 0  | 1  | 0 |
| 1  | 0  | 0 |
| 1  | 1  | 1 |

<\div>

![AND Decision Boundary](/assets/img/develop/AND-Decision-Boundary.png)

Seperating is POSSIBLE on linear data.

| x1 | x2 | y |
|----|----|---|
| 0  | 0  | 0 |
| 0  | 1  | 1 |
| 1  | 0  | 1 |
| 1  | 1  | 0 |




Linear classifiers:
- Learn one template per class
- Can only form linear decision boundaries
- Fail on non-linearly separable data

---

## 2. Feature Motivation

$$
x \rightarrow z \rightarrow y
$$

Mapping to feature space can enable linear separation.

---

## 3. Image Features

- Color histogram
- HOG
- Bag-of-Words

---

## 4. Perceptron

$$
y = f(w_1 x_1 + w_2 x_2)
$$

$$
f(a) =
\begin{cases}
1 & a > \theta \\
0 & a \le \theta
\end{cases}
$$

---

## 5. Multilayer Perceptron

$$
h = \sigma(W_1 x), \quad y = W_2 h
$$

---

## 6. Activation Functions

$$
\sigma(x) = \frac{1}{1 + e^{-x}}, \quad \text{ReLU}(x) = \max(0, x)
$$

---

## 7. Backpropagation

Loss:
$$
\mathcal{L} = (\hat{y} - y)^2
$$

Gradients:
$$
\frac{\partial \mathcal{L}}{\partial W_2} = 2(\hat{y}-y)h
$$

$$
\frac{\partial \mathcal{L}}{\partial W_1} = 2(\hat{y}-y)W_2 h(1-h)x
$$

---

## 8. Summary

Linear → Features → Perceptrons → MLP → Backprop
