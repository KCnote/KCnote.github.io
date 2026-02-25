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
    Input \rightarrow  FC \rightarrow  Activation \rightarrow  FC ... \rightarrow  Softmax \rightarrow  Output
    $$

- Can express non-linear seperable data like XOR

### 2. Why use Multi-layer Perceptrons(MLP)?
     Linear/Non-Linear Seperable Data

About Single Perceptron

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

</div>

![AND Decision Boundary](/assets/img/develop/AND-Decision-Boundary.png)

Seperating is <span style="color:blue">POSSIBLE</span> on linear data.

![XOR Decision Boundary](/assets/img/develop/XOR-Decision-Boundary.png)

<div style="width:100%;">

| x1 | x2 | y |
|----|----|---|
| 0  | 0  | 0 |
| 0  | 1  | 1 |
| 1  | 0  | 1 |
| 1  | 1  | 0 |

</div>

Seperating is <span style="color:red">IMPOSSIBLE</span> on non-linear data.

    BUT Change From Single to Multi-layer, it is POSSIBLE.

$$
y = f(w_5f(w_1x_1+w_2x_2) + w_6f(w_3x_1+w_4x_2))
$$

$$
h = \sigma(W_1 x), \quad y = W_2 h
$$

![XOR MLP](/assets/img/develop/XOR-MLP.png)


### 3. How use Multi-layer Perceptrons(MLP)?
    Fully-Connected Layer
$$\mathbf{y} = W\mathbf{x} + \mathbf{b}$$

* $ \mathbf{x} \in \mathbb{R}^{n} $ : input vector
* $ W \in \mathbb{R}^{m \times n} $ : weight matrix
* $ \mathbf{b} \in \mathbb{R}^{m} $ : bias vector
* $ \mathbf{y} \in \mathbb{R}^{m} $ : output vector

<b>Affine transformation</b> on the input vector, where every input neuron is connected to every output neuron.

$$ y_i = \sum_{j=1}^{n} w_{ij} x_j + b_i $$

Each output unit is connected to **all input units**.

<span style="color:blue"><b>Concept of Space Transformation</b></span>:

An FC layer linearly transforms the input feature space into another space:

$$
\mathbf{x} \rightarrow \text{feature transformation} \rightarrow \mathbf{y}
$$

    Acitivation Functions

<table>
<tr>
<td><img src="/assets/img/develop/Activation-Step.png" width="250" alt="Step activation function"></td>
<td><img src="/assets/img/develop/Activation-Sigmoid.png" width="250" alt="Sigmoid activation function"></td>
<td><img src="/assets/img/develop/Activation-Tanh.png" width="250" alt="Tanh activation function"></td>
</tr>

<tr>
<td><img src="/assets/img/develop/Activation-ReLU.png" width="250" alt="ReLU activation function"></td>
<td><img src="/assets/img/develop/Activation-Leaky-ReLU.png" width="250" alt="Leaky ReLU activation function"></td>
<td><img src="/assets/img/develop/Activation-ELU.png" width="250" alt="ELU activation function"></td>
</tr>
</table>

$$
\text{Step}(x) =
\begin{cases}
1, & x \ge 0 \\
0, & x < 0
\end{cases}
\:\:\:\:
\sigma(x) = \frac{1}{1 + e^{-x}}
\:\:\:\:
\tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}
$$
$$
\text{ReLU}(x) = \max(0, x)
$$
$$
\text{LeakyReLU}(x) =
\begin{cases}
x, & x > 0 \\
\alpha x, & x \le 0
\end{cases}

\:\:\:\:
\text{ELU}(x) =
\begin{cases}
x, & x > 0 \\
\alpha (e^x - 1), & x \le 0
\end{cases}
$$

    Backpropagation

The main target of backpropgation is <b>Minimize Loss Functions</b>.

#### Function Example:

$$
\hat{y} = W_2 \sigma(W_1 \mathbf{x} + \mathbf{b}_1) + b_2
$$

#### Loss(MSE):
$$
L = (\hat{y} - y)^2
$$

### Backpropagation Derivation
---

$$
\hat{y} = W_2 \mathbf{h}, \quad
\frac{\partial L}{\partial \hat{y}} = 2(\hat{y} - y), \quad 
\frac{\partial \hat{y}}{\partial W_2} = \mathbf{h}
$$

$$
\boxed{
\frac{\partial L}{\partial W_2}
=====
\frac{\partial L}{\partial \hat{y}}
\frac{\partial \hat{y}}{\partial W_2}
=====
2(\hat{y} - y)\mathbf{h}
}
$$


$$
\boxed{
\frac{\partial L}{\partial \mathbf{h}}
=====
\frac{\partial L}{\partial \hat{y}}
\frac{\partial \hat{y}}{\partial \mathbf{h}}
=====
2(\hat{y}-y) W_2
}
$$

---

$$
\mathbf{z}_1 = W_1 \mathbf{x}, \quad
\frac{\partial \mathbf{z}_1}{\partial W_1} = \mathbf{x}
$$

#### If sigmoid:

$$
\sigma'(z) = \sigma(z)(1-\sigma(z))
$$

$$
\boxed{
\frac{\partial L}{\partial \mathbf{z}_1}
=====
\frac{\partial L}{\partial \mathbf{h}}
\odot
\sigma'(\mathbf{z}_1)
=====
2(\hat{y}-y) W_2
\odot
\mathbf{h}(1-\mathbf{h})
}
$$

$$
\boxed{
\frac{\partial L}{\partial W_1}
=====
\frac{\partial L}{\partial \mathbf{z}_1}
\mathbf{x}^T
}
$$

#### Final:

$$
\boxed{
\frac{\partial L}{\partial W_1}
================

2(\hat{y}-y)
W_2
\odot
\mathbf{h}(1-\mathbf{h})
\mathbf{x}^T
}
$$


## 🔥 Core Idea

Forward:

$$
\mathbf{x} \rightarrow \mathbf{z} \rightarrow \mathbf{a} \rightarrow \hat{y}
$$

Backward:

$$
\frac{\partial L}{\partial \mathbf{x}}
====
\frac{\partial L}{\partial \mathbf{a}}
\frac{\partial a}{\partial \mathbf{z}}
\frac{\partial \mathbf{z}}{\partial \mathbf{x}}
====
\frac{\partial L}{\partial \mathbf{z}}
\frac{\partial \mathbf{z}}{\partial \mathbf{x}}
$$

### Backpropagation = Chain Rule applied in reverse order.